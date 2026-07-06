# HIV Study Dashboard

A Quarto dashboard comparing viral load trends across treatment cohorts using data pulled from the public [LabKey HIV Study Tutorial](https://www.labkey.org/home/Demos/HIV%20Study%20Tutorial/project-begin.view).

## Requirements

-   [Quarto](https://quarto.org)
-   R packages: `httr2`, `dplyr`, `purrr`, `plotly`, `bslib`, `htmltools`, `readr`

## Running locally

``` bash
quarto preview index.qmd
```

## Data read strategy

Data is **streamed to disk** from LabKey's `query-exportRowsTsv.api` rather than loaded through the JSON `query-selectRows.api`.

-   **Streaming pull:** Responses are written to `data/*.tsv` as bytes arrive, then read back.
-   **Complete result:** `query.maxRows=-1` lifts LabKey's default 100-row cap, and `headerType=FieldKey` returns machine-friendly column names.
-   **Scaling limit:** While overkill for this tiny dataset, the read `readr::read_tsv()` loads all data into memory. For genuinely large data you'd query on disk with [Apache Arrow](https://arrow.apache.org/docs/r/) or [DuckDB](https://duckdb.org/), filtering and aggregating out-of-core.
-   **No stored data:** `data/` is runtime scratch (git-ignored), so each build is a **fresh pull** from the API.

## Publishing

Pushing to `main` renders the dashboard via GitHub Actions and publishes it to GitHub Pages (`gh-pages` branch). The workflow also runs daily (`cron: '0 6 * * *'`, UTC) and on manual dispatch. The daily schedule is here to demonstrate the auto-refresh pattern; this LabKey tutorial dataset is static, so scheduled rebuilds won't actually change the numbers.