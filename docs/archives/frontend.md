The frontend code is located in the [archives-web](https://github.com/thestanforddaily/archives-web) repository. 

You can find an introductory slide deck [here](https://docs.google.com/presentation/d/1NwBSJjKjSDomaCoDW--owvon-N3aJZNCX9yaF9KNygk/edit?usp=sharing)

The front end is built using several technologies:

- React
- Next.js
  - For server side rendering, allows for faster page loading and better caching. 
  - There should be a writeup about Next.js somewhere...
- [OpenSeadragon](https://openseadragon.github.io/)
  - This is used to display the images of archived newspapers
  - Doesn't mesh super well with Next.js in my experience, so may need to disable SSR when using
- [react-big-calendar](https://github.com/jquense/react-big-calendar)
  - This is used to display the monthly calendar under the calendar page
- [react-jsonschema-form](https://github.com/rjsf-team/react-jsonschema-form)
  - This is used to create the search form under search
- [Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html)
  - Lucene is a query language, and is used to interface with CloudSearch on the backend. 

## Workflow

  1. make branch
  2. make changes
  3. save, see changes
  4. debug, go back to step 2 or continue
  5. make PR
  6. approve and merge PR
  7. pull checkout master 
  8. `yarn deploy` at master.

## Gotchas

 - `moment` is used to calculate strings for dates throughout the frontend. However, `moment` is too "smart" in that it by default factors in different time zones. As a result, you may encounter the situation where a date is correct when developing on your local machine, but then when you deploy, the date is incorrect in production. This is likely due to `moment`/timezones. To fix this, use `moment.utc` to convert all `moment`s to the same time zone.
   - You can find an example of this [here](https://github.com/TheStanfordDaily/archives-web/blob/d317d8d347dd04099ba5620d0333c8e0da12064d/components/PaperView.jsx#L424)
  

## Backend interactions

The frontend interacts with the backend in several ways:

- fetching search results
- fetching article text
- fetching issue images
- submitting corrections to articles

## Fetching search results
We interact with CloudSearch to do this. We submit queries to an endpoint and the endpoint responds with search results. the way we create these queries is with lucene syntax.

## Fetching article text
This is done in `helpers/papers.js`. We're just using github's api here, and the important thing is how to correctly structure queries. 

## Fetching issue images
This is handled by openseadragon. openseadragon makes requests to an endpoint for images when a user is trying to view the images for an article.

## Submitting corrections to articles
This is handled by google forms. When you click the submit button in the google form, it will send the request to the correct endpoint for us. One thing we do is use url parameters to prefill the google forms. You can read more [here](https://trevorfox.com/2015/06/dynamically-pre-fill-google-forms-with-mailchimp-merge-tags/).
