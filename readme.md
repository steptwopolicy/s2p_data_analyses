# Instruction Manual: Maintaining Data Analyses and Reports

## Overview

This manual explains how to maintain and update the analytics reports and Excel tools developed for The Step Two Policy Project.

All analyses use open datasets and are processed using R Markdown files that generate HTML reports and CSV files. These files are stored in a private GitHub repository: <https://github.com/steptwopolicy/s2p_data_analyses>.

While most updates are automated, updating the Excel‑based tools requires a few manual steps.

------------------------------------------------------------------------

## Table of Contents

1.  System Requirements\
2.  Installing R and RStudio\
3.  Setting Up the RStudio Project\
4.  Rendering the R Markdown Files\
5.  Updating the Excel Workbooks\
6.  Committing and Pushing Changes to GitHub\
7.  Maintenance Schedule\
8.  Troubleshooting & FAQ\
9.  Additional Notes

------------------------------------------------------------------------

## 1. System Requirements

-   **R** (version ≥ 4.3.2

-   **RStudio** (version ≥ 2024.12)\

-   **Git** installed and configured\

-   **GitHub account** with access to the private repository\

-   **Microsoft Excel** (for workbooks)

------------------------------------------------------------------------

## 2. Installing R and RStudio

### Step 1: Install R

Download and install R from the [CRAN website](https://cran.r-project.org/).

### Step 2: Install RStudio

Download and install RStudio from [RStudio's website](https://posit.co/download/rstudio-desktop/).

------------------------------------------------------------------------

## 3. Setting Up the RStudio Project

### Step 1: Clone the GitHub Repo

1.  Open RStudio.\
2.  Go to **File \> New Project \> Version Control \> Git**.\
3.  Paste the repository URL (<https://github.com/steptwopolicy/s2p_data_analyses>).\
4.  Choose a local folder to clone the repo into.\
5.  Click **Create Project**.

### Step 2: Install Required R Packages

Open the R Console and run:

``` r
required_packages <- c("tidyverse", "furrr", "scales", "gt", "pdftools", "httr", "downloadthis", "janitor", "knitr")

install.packages(required_packages)
```

------------------------------------------------------------------------

## 4. Rendering the R Markdown Files

Each of the four R Markdown (`.Rmd`) files includes a YAML header with a `project_directory` parameter. **Before knitting**, update this parameter to point to your local Dropbox folder:

``` yaml
params:
  project_directory: "/path/to/your/MLTC_MMC_NH_data/"
```

### To Manually Render

1.  **Open the `.Rmd` file in RStudio.**\
    ![](images/Screenshot%202025-04-05%20at%203.49.46%20PM.jpg){width="500"}

2.  **Click the “Knit” button**\
    ![](images/Screenshot%202025-04-05%20at%203.53.35%20PM.jpg){width="500"}

3.  **Locate the Generated Files**

    -   HTML report: `/report-name.html`\
    -   CSVs:
        -   `/*_table_data.csv`\
        -   `/*_projection_data.csv`

------------------------------------------------------------------------

## 5. Updating the Excel Workbooks

Each of the two Excel workbooks uses two CSVs:

-   `*_table_data.csv` → Table sheet (visible first sheet)\
-   `*_projection_data.csv` → Projections sheet (hidden third sheet)

> **Note on Frequency:**\
> Although a new row in the Projections sheet is added only once per fiscal year, because the enrollment data source makes retrospective updates to prior months, **update the Projections sheet every month** using the latest CSV—even if you don’t add a new row that month.

### A. Unlock and Unhide Sheets

1.  Open the Excel workbook.\
2.  Unhide the **Projections** sheet:
    -   Right‑click any sheet tab → **Unhide** → select **Projections**.\
3.  Unprotect **Table** and **Projections** sheets:
    -   Go to **Review \> Unprotect Sheet** (no password required).

### B. Insert Rows and Paste Data

1.  **Table Sheet**
    -   Insert new rows at the top of the data table.\
    -   Copy contents from `*_table_data.csv` and paste **Values Only** (right‑click → Paste Special → Values).
2.  **Projections Sheet**
    -   Insert rows into the named range `table1` (Actual data only).\
    -   Paste **Values Only** from `*_projection_data.csv` into those rows.
3.  **Monthly Retrospective Updates**
    -   Even without adding a new row, clear and re‑paste the `*_projection_data.csv` monthly to capture any adjustments.

### C. Reprotect and Hide Table & Projections Sheets

1.  Protect both sheets: **Review \> Protect Sheet** (leave password blank).\
2.  Hide the **Projections** sheet: Right‑click tab → **Hide**.

### D. Update the Graph Sheet’s Bar Colors

1.  Unprotect **Graph** sheet:
    -   Right‑click any tab → **Unhide** → select **Graph**.\
    -   **Review \> Unprotect Sheet**.
2.  Adjust bar-fill colors:
    -   Select the chart.\
    -   Click on the “Actual” bars and set their **Fill Color** to the Actual-data color.\
    -   Click on the “Projected” bars (last five years) and set their **Fill Color** to the Projection-data color.
3.  Reprotect **Graph** sheet:
    -   **Review \> Protect Sheet** (leave password blank).\
    -   Right‑click tab → **Hide**.

### E. Save the Workbook

-   Ensure the filename remains unchanged, then save.

------------------------------------------------------------------------

## 6. Committing and Pushing Changes to GitHub

1.  In RStudio’s **Git** tab, check updated HTML, CSV, and Excel files.\
    ![](images/Screenshot%202025-04-05%20at%204.02.40%20PM.jpg){width="500"}
2.  Click **Commit** (the button with the checkmark), enter a message (e.g., “Update reports & Excel – May 2025”), and click **Commit**.\
3.  Click **Push** (the green arrow) to upload changes.

> Once pushed, the website will automatically publish the updated reports and data.

------------------------------------------------------------------------

## 7. Maintenance Schedule

| Task                                  | Frequency         |
|---------------------------------------|-------------------|
| Review/edit Rmd files                 | As needed         |
| Render Rmd files manually             | As needed         |
| Update Excel workbooks (table)        | Monthly           |
| Add new Projections row               | Annually          |
| Update Projections for retrospectives | Monthly           |
| Update Graph sheet bar colors         | Annually          |
| Push updated files to GitHub          | After each update |

------------------------------------------------------------------------

## 8. Troubleshooting & FAQ

### Q1: Package installation error “not available for R version 4.x”

**A:** Check `R.version.string`, update R via CRAN, or install an archived version with `devtools::install_version()`.

### Q2: “Error: pandoc version not found”

**A:** Verify Pandoc in **Tools \> Global Options \> R Markdown**, or install from pandoc.org.

### Q3: Excel formulas break or rows misalign

**A:** Always insert rows **before** pasting and use **Paste Special → Values Only**. If `table1` errors, adjust via **Formulas \> Name Manager**.

### Q4: HTML report differs from last month

**A:** Review underlying data changes and recent Rmd edits. To revert, right‑click in Git pane → **Revert**.

------------------------------------------------------------------------

## 9. Additional Notes

-   Automated updates of HTML and CSV files are handled by the Isaac’s daily script.\
-   Excel workbooks require manual updates only as described above.\
-   If formulas are accidentally deleted, restore from the previous Git commit.\
-   For further support or to convert this manual into PDF/Word, please contact Isaac.

------------------------------------------------------------------------

## 10. Structure and Purpose of the R Markdown Files

Each `.Rmd` file in this project is designed to process publicly available Medicaid data, perform a standardized analysis, and output a clean, easy-to-read HTML report along with companion CSV files. All four `.Rmd` files follow the same basic structure, with a consistent set of code chunks that automate key steps in data processing, analysis, and visualization. Below is a high-level overview of what each chunk does:

### `setup_options`

This chunk sets global knitr options that control how R code and output appear in the final HTML report. For example, it may hide code chunks or control figure dimensions.

### `setup_libraries`

Loads the R packages used throughout the analysis. These typically include `tidyverse` for data wrangling and plotting, `readxl` for Excel file reading, `lubridate` for date handling, and others.

### `isaac_step2_graph_theme`

Defines a consistent graph theme (colors, fonts, axis styling, etc.) that is applied to all plots. This ensures visual consistency across reports and makes the graphs easier to interpret.

### `setup_medicaid_enrollment_data_ingestion`

Ingests Medicaid enrollment data. By default, this chunk retrieves updated enrollment data from Health Data NY. However, the user can set the parameter (`use_local_data = TRUE`) to instead load a previously downloaded version from a local file path. This helps preserve reproducibility when working offline or verifying past analyses.

### `setup_medicaid_spending_data_ingestion`

Ingests Medicaid spending data. By default, this chunk extracts spending values from Global Cap Reports (available as PDFs online). As with enrollment data, the user can override the default and load a local copy of the data using the parameter (`use_local_data = TRUE`). This is helpful when reviewing historical analyses or troubleshooting.

### `setup_medicaid_enrollment_vs_spending_analysis`

Performs the core analysis. This chunk:

-   Aggregates the enrollment data and the spending data into aligned time frames
-   Joins them into a single dataset
-   Calculates new fields
-   Generates a formatted data table
-   Prepares a graph of trends over time

### `table`

Prints the formatted data table directly into the HTML report.

### `graph`

Prints the time trend graph of enrollment and/or spending over time. The graph follows the styling defined in the `isaac_step2_graph_theme` chunk and reflects both actual and projected values.

### Supplemental chunks

Some `.Rmd` files include additional chunks at the end, which provide secondary analyses.

------------------------------------------------------------------------
