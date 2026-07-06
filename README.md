# HIV Study Dashboard

A Quarto dashboard comparing viral load trends across treatment cohorts using data pulled from the public [LabKey HIV Study Tutorial](https://www.labkey.org/home/Demos/HIV%20Study%20Tutorial/project-begin.view).

## Requirements

-   [Quarto](https://quarto.org)
-   R packages: `httr2`, `dplyr`, `purrr`, `plotly`, `bslib`

## Running locally

``` bash
quarto preview index.qmd
```

## Publishing

Pushing to `main` triggers a GitHub Actions workflow that renders the dashboard and publishes it to GitHub Pages (`gh-pages` branch).
