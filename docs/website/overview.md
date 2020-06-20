# Website

The Stanford Daily website is hosted at [https://www.stanforddaily.com](https://www.stanforddaily.com).

It encompasses the following GitHub repositories:

- [https://github.com/TheStanfordDaily/stanforddaily](https://github.com/TheStanfordDaily/stanforddaily) - frontend React code
- [https://github.com/TheStanfordDaily/stanforddaily-wordpress](https://github.com/TheStanfordDaily/stanforddaily-wordpress) - backend WordPress code
- [https://github.com/TheStanfordDaily/StanfordDaily-DataViz](https://github.com/TheStanfordDaily/StanfordDaily-DataViz) - static site used to embed custom HTML for data visualizations / interactive pages

## Architecture

Our website is architected as a [headless CMS](https://en.wikipedia.org/wiki/Headless_content_management_system) system.

We use WordPress on the backend. For the frontend, a React static site built using Next JS actually renders the UI for the end user.

See [this slide deck](https://docs.google.com/presentation/d/11e9ghjY-W9Z67T1-RBDR4ZXVUpINJMg2oZKIXxlNqfc/edit#slide=id.p) for the architecture:

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vT6Kf9s09BVVAB8ivfsMnoRZw7zoenJOkaaO1j5oLAIqoOp1oGjrGCSedFkvbPgS7icO6LXpFtibMhn/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>