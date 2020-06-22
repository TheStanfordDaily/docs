Several things power archives on the backend, and they're each located in different spots.

These things include:
- responding to frontend fetching search results
- responding to frontend fetching article text
- responding to frontend fetching issue images
- responding to frontend submitting corrections to articles

## Responding to Frontend Fetching Search Results
When the frontend makes a search request, they are directly interacting with an aws API Gateway. Read more about API Gateways below. The API Gateway then routes the frontend's request to the cloudsearch endpoint, at which point the server backing the cloudsearch endpoint will process the search query and then respond to API Gateway with the results. API Gateway then forwards this back to the frontend. You can read more about cloudsearch below.

## Responding to Frontend Fetching Article Text
There unfortunately isn't a single canonical source for article text on archives. There are two places that article text is stored. The first is on github. You can find the text for all the archived articles [here](https://github.com/thestanforddaily/archives-text). It's a massive repo and working with it is a pain (git doesn't scale super well, so actions like `git status` take > 10 min; it took me ~2 hours to submit corrections to all articles in the repo. Also, commit directly to master when working with archives-text). The other source for article text is in cloudsearch, as there is an article-text field in cloudsearch. Technically, the data in cloudsearch was derived from the archives-text repo, but they aren't connected in any other way; if you make changes to archive-text, they won't automatically sync to cloudsearch. There are many things to be improved here.

When fetching article text on the frontend (e.g. when you go to an article and see the corresponding text to that article in the side bar), this text comes from the archives-text repository. The backend is really all handled by github and their API. The only thing that concerns us here is ensuring that this repo's data is up to date.

## Responding to Frontend Fetching Issue Images
Issue images are fetched on the frontend by openseadragon. These images live in AWS s3 buckets, so the images are fetched from there. I don't know much about this tbh so I can't really document it well

## Responding to Frontend Submitting Corrections to Articles
The frontend corrections form is powered by google forms. We're using an embedded form. Checkout [this](https://trevorfox.com/2015/06/dynamically-pre-fill-google-forms-with-mailchimp-merge-tags/) for more details

## Cloudsearch
Cloudsearch is a search engine service by AWS which powers the archives search functionality. Cloudsearch is powered by Apache Solr. 

To "see" cloudsearch, go to `aws.amazon.com` and sign in. Then, in the find services search bar, search for cloudsearch. Once there, there should be a `archives-text-cloudsearch` domain. If you click on it, you can interact with the cloudsearch through aws's interface. Here you can search, and you can upload data. Note that you don't need to use AWS's own interface to do these two things. Amazon provides APIs which we can do these things programmatically. 

We interact with cloudsearch programmatically in two ways:

- Searching
- Uploading Data

### Searching
Searching is pretty simple. You just submit a query to an endpoint and the endpoint responds with the search results. 

### Uploading Data
We have around 10GB of article text data in the archives-text repository and we need to upload that data to CloudSearch so that it has the data which we are searching over. There are python scripts which upload this data to CloudSearch in the archives-scripts repository. You can find them [here](https://github.com/thestanforddaily/archives-scripts) and [here](https://github.com/TheStanfordDaily/archives-scripts/tree/master/cloudsearch). These two sources have a bit more documentation about CloudSearch, so go there to find out more.

## API Gateway
Unfortunately, CloudSearch doesn't allow cross origin requests to its endpoint. What this means is that we can't directly query the CloudSearch endpoint from the archives frontend. As a result, we need to setup an AWS API gateway for the CloudSearch endpoint. An API Gateway is essentially an endpoint which "wraps" around our CloudSearch endpoint. It offers several functions: you can "pre-process" and validate requests to CloudSearch and you can "post-process" requests to CloudSearch. AWS [has a lot more doc](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) for API Gateway. 

To access API Gateway, go to the AWS Console and search for "API Gateway." Then, there should be a gateway called `archives-cloudsearch-search-api`. Click on this. Then, click on the Get Method. You should see a flow chart with a test item, method request, integration request etc. If you click on method request and expand url query string parameter, you'll see a lot of the query string parameters which correspond to cloudsearch queries. The way gateway works is we need to explicitly tell the gateway which url queries are permitted to pass through the gateway. Once you add a parameter here, you need to go to integration request and add the same query string parameter to url query string parameters there. Name field should be the same as the query parameter, and mapped from should be whatever query parameter we're mapping from in the method request section. 

After you've made changes, you can test them in the test section. Then if you're satisfied, you need to deploy them to have the actually take effect. To do this, do to the actions dropdown and select deploy api, then select deployment stage (may need to create a new one) and click deploy. Now, the changes should take effect, and we can access the API gateway properly on the frontend. 
