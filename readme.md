# Instruction Manual: Maintaining Data Analyses and Reports

## Overview

This manual explains how to maintain and update the analytics reports and Excel tools developed for The Step Two Policy Project.

All analyses use open datasets and are processed using R Markdown files that generate HTML reports and CSV files. These files are stored in a private GitHub repository: <https://github.com/steptwopolicy/s2p_data_analyses>.

While most updates are automated, updating the Excel‑based tools requires a few manual steps.

------------------------------------------------------------------------

## Table of Contents

1.  System Requirements
2.  Installing R and RStudio
3.  Setting Up the RStudio Project
4.  Rendering the R Markdown Files
5.  Updating the Excel Workbooks
6.  Committing and Pushing Changes to GitHub
7.  Maintenance Schedule
8.  Troubleshooting & FAQ
9.  Additional Notes
10. Structure and Purpose of the R Markdown Files
11. Interactive Excel Workbooks

------------------------------------------------------------------------

## 1. System Requirements

-   **R** (version ≥ 4.3.2)

-   **RStudio** (version ≥ 2024.12)

-   **Git** installed and configured

-   **GitHub account** with access to the private repository

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

1.  Open RStudio.
2.  Go to **File \> New Project \> Version Control \> Git**.
3.  Paste the repository URL (<https://github.com/steptwopolicy/s2p_data_analyses>).
4.  Choose a local folder to clone the repo into.
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

1.  **Open the `.Rmd` file in RStudio.**

    <img src="images/Screenshot 2025-04-05 at 3.49.46 PM.png" width="500"/>

2.  **Click the “Knit” button**

    <img src="images/Screenshot 2025-04-05 at 3.53.35 PM.png" width="500"/>

3.  **Locate the Generated Files**

    -   HTML report: `/report-name.html`
    -   CSVs:
        -   `/*_table_data.csv`
        -   `/*_projection_data.csv`

------------------------------------------------------------------------

## 5. Updating the Excel Workbooks

Each of the two Excel workbooks uses two CSVs:

-   `*_table_data.csv` → Table sheet (visible first sheet)
-   `*_projection_data.csv` → Projections sheet (hidden third sheet)

> **Note on Frequency:** Although a new row in the Projections sheet is added only once per fiscal year, because the enrollment data source makes retrospective updates to prior months, **update the Projections sheet every month** using the latest CSV—even if you don’t add a new row that month.

### A. Unlock and Unhide Sheets

1.  Open the Excel workbook.
2.  Unhide the **Projections** sheet:
    -   Right‑click any sheet tab → **Unhide** → select **Projections**.
3.  Unprotect **Table** and **Projections** sheets:
    -   Go to **Review \> Unprotect Sheet** (no password required).

### B. Insert Rows and Paste Data

1.  **Table Sheet**
    -   Insert new rows at the top of the data table.
    -   Copy contents from `*_table_data.csv` and paste **Values Only** (right‑click → Paste Special → Values).
2.  **Projections Sheet**
    -   Insert rows into the named range `table1` (Actual data only).
    -   Paste **Values Only** from `*_projection_data.csv` into those rows.
3.  **Monthly Retrospective Updates**
    -   Even without adding a new row, clear and re‑paste the `*_projection_data.csv` monthly to capture any adjustments.

### C. Reprotect and Hide Table & Projections Sheets

1.  Protect both sheets: **Review \> Protect Sheet** (leave password blank).
2.  Hide the **Projections** sheet: Right‑click tab → **Hide**.

### D. Update the Graph Sheet’s Bar Colors

1.  Unprotect **Graph** sheet:
    -   Right‑click any tab → **Unhide** → select **Graph**.
    -   **Review \> Unprotect Sheet**.
2.  Adjust bar-fill colors:
    -   Select the chart.
    -   Click on the “Actual” bars and set their **Fill Color** to the Actual-data color.
    -   Click on the “Projected” bars (last five years) and set their **Fill Color** to the Projection-data color.
3.  Reprotect **Graph** sheet:
    -   **Review \> Protect Sheet** (leave password blank).
    -   Right‑click tab → **Hide**.

### E. Save the Workbook

-   Ensure the filename remains unchanged, then save.

------------------------------------------------------------------------

## 6. Committing and Pushing Changes to GitHub

1.  In RStudio’s **Git** tab, check updated HTML, CSV, and Excel files.

    <img src="images/Screenshot 2025-04-05 at 4.02.40 PM.png" width="500"/>

2.  Click **Commit** (the button with the checkmark), enter a message (e.g., “Update reports & Excel – May 2025”), and click **Commit**.

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

-   Automated updates of HTML and CSV files are handled by the Isaac’s daily script.
-   Excel workbooks require manual updates only as described above.
-   If formulas are accidentally deleted, restore from the previous Git commit.
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

## 11. Structure and Purpose of the R Markdown Files

### Workbook 1: MLTC Enrollment and Spending Projection Workbook

This workbook is designed to generate transparent, reproducible
projections of enrollment and spending in New York's Managed Long-Term
Care (MLTC) program. It allows users to adjust key growth-rate
assumptions and observe how those adjustments propagate through
enrollment, per-member spending, and total expenditure estimates across
multiple future fiscal years. Although the tool is implemented entirely
in Excel, its logic mirrors standard forecasting approaches used in
health policy analysis, enabling policy researchers to evaluate
alternative scenarios without requiring statistical software.

The workbook is organized into two primary worksheets. The **Table**
sheet contains the core projection model and the fields in which a user
enters updated growth-rate assumptions. The **Graph** sheet summarizes
growth-rate inputs and generates the visualization used in the
workbook's reporting outputs. Formulas on both sheets pull from these
user-specified parameters to produce internally consistent projections
that link enrollment growth, spending growth, per-member-per-month
(PMPM) dynamics, and fiscal impacts.

#### Workbook Structure and Calculation Logic

The projection model begins with a historical baseline year. Baseline
enrollment, PMPM spending, total spending, and related values for the
most recent year populate the first row of the model. All calculated
fields in subsequent years reference this baseline and each preceding
year so that changes compound over time.

##### 1. Enrollment Projections

Enrollment for each projected fiscal year is calculated by applying a
user-defined annual enrollment growth rate. For example, in row 4 the
projection formula:

```         
=G5*(1+K4)
```

takes the prior year's enrollment (cell G5) and applies the growth rate
identified in cell K4. The result becomes the next year's projected
enrollment. This pattern repeats across all rows so that changes
accumulate year over year.

##### 2. PMPM Spending Projections

Future PMPM spending is derived similarly. The formula:

```         
=(1+Q4)*O5
```

takes the previous year's PMPM spending (O5) and multiplies it by the
spending growth rate (Q4). The projected PMPM values then feed directly
into total spending calculations.

##### 3. Total Annual Spending

Total annual spending is calculated by multiplying the projected
enrollment by the PMPM amount and then annualizing the figure. For
example:

```         
=G4*12
```

converts the projected monthly spending into an annual total. Subsequent
formulas additionally compute year-over-year changes, differences from
baseline, and the proportionate changes implied by the PMPM and
enrollment trajectories.

##### 4. Growth Rates and Assumptions

Growth-rate assumptions come from two places:

-   Direct user entry into growth-rate fields within the **Table**
    sheet.
-   Cross-sheet references to parameter cells in the **Graph** sheet
    (e.g., `Graph!$P$20` for enrollment growth and `Graph!$R$20` for
    PMPM growth when generating multi-year projections).

The model is designed so that modifying these inputs immediately
recalculates all dependent fields, enabling quick sensitivity testing
and scenario analysis.

#### Conceptual Flow of the Projection Model

1.  **User sets annual growth assumptions** for enrollment and PMPM
    spending.\
2.  **Enrollment is projected**, compounding annually.\
3.  **PMPM spending is projected** using the corresponding spending
    growth rate.\
4.  **Total spending is calculated** by multiplying projected enrollment
    by projected PMPM, then annualizing.\
5.  **Differences and percent changes** are computed automatically
    relative to prior years and baseline conditions.

This consistent logic means all projected values can be traced to a
small set of user-controlled parameters, ensuring transparency and
reproducibility.

------------------------------------------------------------------------

### Walkthrough Example

The following walkthrough tracks a single projected year to illustrate
how formulas interact.

Assume the most recent fiscal year with observed data is located in row
5, and projections begin in row 4 (representing the next fiscal year).
Each field in row 4 pulls from row 5 and from user-defined growth
assumptions.

##### Step 1: Enrollment

Cell **G4** calculates projected enrollment:

```         
=G5*(1+K4)
```

If the user inputs an annual enrollment growth rate of 3% in **K4**, and
enrollment in the baseline year (G5) is 100,000 members, the model
produces:

```         
=100,000 * (1 + 0.03) = 103,000 members
```

##### Step 2: PMPM Spending

Projected PMPM spending in **O4** is calculated as:

```         
=(1+Q4)*O5
```

If the baseline PMPM spending is \$4,000 and the user specifies a
spending growth rate of 5% in **Q4**, the result is:

```         
= $4,000 * (1 + 0.05) = $4,200 PMPM
```

##### Step 3: Total Annual Spending

Total annual spending in **F4** is calculated using the projected PMPM
multiplied by enrollment and annualized:

```         
=G4*12
```

If the projected PMPM (after compounding) is placed in the preceding
field (G4), the annualization yields the year’s total spending.

##### Step 4: Year-over-Year Differences

The change in spending relative to the baseline is calculated in **J4**:

```         
=F4-F5
```

This provides the spending increase attributable to the combined effect
of enrollment and PMPM growth.

##### Step 5: Percent Change

Percent change in spending is shown in **N4**, which divides the change
in spending by the baseline:

```         
=M4/L5
```

This highlights the proportional magnitude of the fiscal change.

------------------------------------------------------------------------

### **Interpretation and Use**

The workbook allows the think tank team to assess how changes in
enrollment growth, PMPM spending growth, or both jointly influence MLTC
program spending. Because the model is designed for clarity and
reproducibility, each projection can be traced to specific user-defined
assumptions, enabling transparent communication with policymakers,
analysts, and other stakeholders.

------------------------------------------------------------------------

### Workbook 2: MMC Enrollment and Spending Projection Workbook

This workbook is designed to generate transparent, reproducible projections of enrollment and spending in the Medicaid Managed Care (MMC) program. It allows users to adjust key growth-rate assumptions and observe how those adjustments propagate through enrollment, per-capita spending, and total expenditure estimates across multiple future fiscal years. Although the tool is implemented entirely in Excel, its logic mirrors standard forecasting approaches used in health policy analysis, enabling policy researchers to evaluate alternative scenarios without requiring statistical software.

The workbook is organized into two primary worksheets. The Table sheet contains the core projection model and the historical data. The Graph sheet summarizes growth-rate inputs and generates the visualization used in the workbook's reporting outputs. Formulas on the Table sheet pull from user-specified parameters on the Graph sheet to produce internally consistent projections that link enrollment growth, per-capita spending dynamics, and fiscal impacts.

#### Workbook Structure and Calculation Logic

The projection model begins with a historical baseline year. Baseline enrollment, per-capita spending, total spending, and related values for the most recent actuals populate the lower rows of the model. All calculated fields in subsequent future years (located in the rows above) reference the preceding year so that changes compound over time.

##### 1. Enrollment Projections

Enrollment for each projected fiscal year is calculated by applying a user-defined annual enrollment growth rate. For example, if the projection for FY30 is in row 4 and FY29 is in row 5, the formula in the Average Annual Enrollment column (Column D) would be:

```
=D5 * (1 + 'Graph'!$Ref_Cell)
```

This takes the prior year's enrollment (cell **D5**) and applies the growth rate identified in the Graph sheet. The result becomes the next year's projected enrollment. This pattern repeats across all rows so that changes accumulate year over year.

##### 2. Per Capita Spending Projections

Future spending per person is derived similarly. The formula in the Average Annual Spending per Enrollee column (Column K) is:

```
=K5 * (1 + 'Graph'!$Ref_Cell)
```

This takes the previous year's average annual cost per enrollee (**K5**) and multiplies it by the spending growth rate defined in the Graph sheet. The projected per-capita values then feed directly into total spending calculations.

##### 3. Total Annual Spending

Total annual spending is calculated by multiplying the projected Average Annual Enrollment by the projected Average Annual Spending per Enrollee. Unlike some models that require monthly annualization (PMPM * 12), this workbook utilizes annualized per-capita figures directly. The formula in the State Spending column (Column H) is:

```
=D4 * K4
```

This yields the total projected state expenditure for that fiscal year. Subsequent formulas additionally compute year-over-year changes and percentage growth rates.

##### 4. Growth Rates and Assumptions

Growth-rate assumptions come from two specific input fields located on the Graph sheet:

-   **Enrollment Growth Input:** Controls the annual percentage increase/decrease in the covered population.
-   **Per Capita Spending Input:** Controls the annual percentage increase/decrease in cost per enrollee (proxy for rate adjustments and acuity changes).

The model is designed so that modifying these inputs immediately recalculates all dependent fields in the Table sheet, enabling quick sensitivity testing and scenario analysis.

#### Conceptual Flow of the Projection Model

1. **User sets annual growth assumptions** for enrollment and per-capita spending on the Graph sheet.

2. **Enrollment is projected** on the Table sheet, compounding annually from the baseline.

3. **Per-capita spending is projected** using the corresponding cost growth rate.

4. **Total spending is calculated** by multiplying the projected enrollment volume by the projected per-capita cost.

5. **Differences and percent changes** are computed automatically relative to prior years.

This consistent logic means all projected values can be traced to a small set of user-controlled parameters, ensuring transparency and reproducibility.

#### Walkthrough Example

The following walkthrough tracks a single projected year (FY30) to illustrate how formulas interact.

Assume FY30 (the forecast year) is located in Row 4, and FY29 (the prior year) is located in Row 5. Each field in Row 4 pulls from Row 5 and from user-defined growth assumptions.

##### Step 1: Enrollment

Cell **D4** calculates projected Average Annual Enrollment:

```
=D5 * (1 + Growth_Rate)
```

If the user inputs an annual enrollment growth rate of 1.81% (0.0181), and enrollment in the prior year (**D5**) is 4,757,976, the model produces:

4,757,976 * (1 + 0.0181) = 4,844,095 members

##### Step 2: Per Capita Spending

Projected Average Annual Spending per Enrollee in **K4** is calculated as:

```
=K5 * (1 + Cost_Growth_Rate)
```

If the prior year's per-capita spending is $4,709.08 and the user specifies a spending growth rate of 1.43% (0.0143), the result is:

$4,709.08 * (1 + 0.0143) = $4,776.42 per enrollee

##### Step 3: Total Annual Spending

Total annual spending in **H4** is calculated using the projected enrollment multiplied by the projected per-capita cost:

```
=D4 * K4
```

Using the values derived above:

4,844,095 members * $4,776.42 = $23,137,442,491

##### Step 4: Year-over-Year Differences

The change in spending relative to the prior year is calculated in **I4**:

```
=H4 - H5
```

This provides the spending increase (in dollars) attributable to the combined effect of enrollment volume and price/acuity changes.

##### Step 5: Percent Change

Percent change in spending is shown in **J4**, which divides the change in spending by the prior year's total:

```
=I4 / H5
```

This highlights the proportional magnitude of the fiscal change.

#### Interpretation and Use

The workbook allows the think tank team to assess how changes in **Enrollment Growth** (Volume) and **Per Capita Spending Growth** (Price) jointly influence MMC program spending. Because the model is designed for clarity and reproducibility, each projection can be traced to specific user-defined assumptions, enabling transparent communication with policymakers, analysts, and other stakeholders.

------------------------------------------------------------------------
