Welcome to the Stanford Daily developer documentation. This site contains information about our infrastructure and instructions for setting up our code locally, as well as how deployment works and common troubleshooting issues.

## Running the docs locally

```bash
git clone https://github.com/TheStanfordDaily/docs.git
cd docs
pip install -r requirements-docs.txt
mkdocs serve
```

Open up [http://localhost:8000](http://localhost:8000) in your browser.

## Editing the docs
To create a new item in the nav bar on the left: 

1. Open `mkdocs.yml`
2. Add  list member `- [title of nav bar item]:` under key `nav:`.
   
!!! note
      nav bar item will be displayed in whatever position you place the list member relative to the rest of the list members under `nav:`.

To create a new sub-item in the nav bar on the left:

1. First, locate which item the sub-item should live over (e.g. `Website`). 
2. Then, find the sub-directory which the existing sub-items exist (e.g. `docs/website`). Or, if a sub-directory doesn't exist, then create one.

!!! note
      by convention, a sub-items of the `X` item in the nav bar will live in the sub-directory `docs/X`

[//]: <> (unfortunately, doesn't seem we like can continue ordered lists after notes)

&nbsp;&nbsp;&nbsp;&nbsp;3\. Then, create a new markdown document in that sub-directory (e.g. `docs/website/new_subitem_page.md`)

&nbsp;&nbsp;&nbsp;&nbsp;4\. Finally, in `mkdocs.yml` under the list item in `nav:` which corresponds to the item which your sub-item lives in, add a key value pair which corresponds to the new sub-item you're creating (e.g.       `Data Viz: website/data-viz.md`)

!!! note
      the key which you add this under will be the displayed name for the nav bar sub-item, and the value should be the path to the sub-item's markdown document with the `docs` prefix removed. 