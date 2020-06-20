# Website

The Stanford Daily website is hosted at [https://www.stanforddaily.com](https://www.stanforddaily.com).

It encompasses the following GitHub repositories:

- [https://github.com/TheStanfordDaily/stanforddaily](https://github.com/TheStanfordDaily/stanforddaily) - frontend React code
- [https://github.com/TheStanfordDaily/stanforddaily-wordpress](https://github.com/TheStanfordDaily/stanforddaily-wordpress) - backend WordPress code
- [https://github.com/TheStanfordDaily/StanfordDaily-DataViz](https://github.com/TheStanfordDaily/StanfordDaily-DataViz) - static site used to embed custom HTML for data visualizations / interactive pages

## Architecture

Our website is architected as a [headless CMS](https://en.wikipedia.org/wiki/Headless_content_management_system) system.

We use WordPress on the backend. For the frontend, a React static site built using Next.js actually renders the UI for the end user.
