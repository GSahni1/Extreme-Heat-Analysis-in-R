#wrt = with respect to
# Clear workspace
rm(list = ls())
# Set options
options(stringsAsFactors = FALSE)

# Load libraries
library(readr)
library(dplyr)
library(lubridate)
library(ggplot2)
station_id <- 47187  # Shearwater RCS
start_year <- 2009
end_year <- year(Sys.Date())


#Loading data 
dfs <- list()

for (yr in start_year:end_year) {
  
  url <- paste0(
    "https://climate.weather.gc.ca/climate_data/bulk_data_e.html",
    "?format=csv",
    "&stationID=", station_id,
    "&Year=", yr,
    "&Month=1",
    "&Day=1",
    "&timeframe=2"
  )
  
  df <- read_csv(
    url,
    col_types = cols(.default = "c"), 
    show_col_types = FALSE
  )
  
  dfs[[as.character(yr)]] <- df
}

halifax_daily <- bind_rows(dfs) %>%
  transmute(
    date  = as.Date(`Date/Time`),
    tmax  = as.numeric(`Max Temp (°C)`),
    tmin  = as.numeric(`Min Temp (°C)`),
    tmean = as.numeric(`Mean Temp (°C)`)
  )
halifax_daily <- halifax_daily %>%
  filter(date < as.Date("2026-01-29"))
halifax_daily <- halifax_daily %>%
  mutate(
    tmax_flag = case_when(
      is.na(tmax)            ~ "missing",
      TRUE                   ~ "valid"
    )
  )
write_csv(
  halifax_daily,
  "shearwater_rcs_daily_raw_2009_present.csv"
)

#Days with missing maximum temperature observations are excluded from the analysis.
halifax_daily_clean <- halifax_daily %>% filter(!is.na(tmax))

#day of year column for each day (for normalization)
halifax_doy <- halifax_daily_clean %>%
  mutate(
    year = year(date),
    doy  = yday(date)
  )

#normalizing each day wrt the same day for all the years, so that seasonal variation is taken away
halifax_norm <- halifax_doy %>%
  group_by(doy) %>%
  mutate(
    doy_mean = mean(tmax),
    doy_sd   = sd(tmax),
    tmax_norm = (tmax - doy_mean) / doy_sd
  ) %>%
  ungroup()


# Plot of such doy-normalized values wrt year shows an increase
ggplot(halifax_norm, aes(x = year, y = tmax_norm)) +
  geom_point(alpha = 0.25, size = 0.7) +
  geom_smooth(
    method = "lm",
    se = TRUE,
    color = "black"
  ) +
  labs(
    title = "Day-of-Year Normalized Daily Maximum Temperature",
    subtitle = "Shearwater RCS (2009–Present)",
    x = "Year",
    y = "Normalized Daily Max Temperature (z-score)"
  ) +
  theme_minimal()
