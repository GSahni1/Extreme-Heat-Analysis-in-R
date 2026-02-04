# Halifax Heat Analysis 📊

**Simple analysis of extreme heat days in Halifax (Shearwater) using daily maximum temperature records in R.**

---

## Overview 🔍
This project loads daily temperature data, normalizes temperatures by year, and analyzes extreme heat days (summary by year). It includes data processing and analysis scripts written in R.

## Files 📁
- `shearwater_rcs_daily_raw_2009_present.csv` — raw daily temperature data
- `extremdaysbyyear.csv` — summary of extreme heat days by year (output)
- `LoadDataAndNormalizedTempByYear.R` — data loading, and visualizing normalized values
- `ExtremHeatAnalysis.R` — main analysis and plotting
- `Halifax_Heat_Analysis.Rproj` — RStudio project file
- `ExtremeHeatReport.pbix`- Small Power BI report pertaining to number of extreme heat days every year

## Requirements ⚙️
- R (recommended >= 4.0)
- Common packages: `tidyverse` (includes `dplyr`, `ggplot2`, `readr`), `lubridate`

Install packages in R:

```r
install.packages(c("tidyverse", "lubridate"))
```

## Usage ▶️
1. Open the `Halifax_Heat_Analysis.Rproj` in RStudio (optional).
2. Run `LoadDataAndNormalizedTempByYear.R` to load and visualize trend of temperature data by year normalized for each day of the year to remove seasonal variation
3. Run `ExtremHeatAnalysis.R` to look at 95th percentile heat days and generate plot by year.

You can also source the scripts from an R session:

```r
source("LoadDataAndNormalizedTempByYear.R")
source("ExtremHeatAnalysis.R")
```

## Output ✅
- Summary CSV (`extremdaysbyyear.csv`) with counts of extreme heat days per year
- Plots illustrating trends in extreme heat (if the scripts save figures)
