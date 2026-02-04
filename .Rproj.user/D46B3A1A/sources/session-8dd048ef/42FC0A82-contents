#95th percentile tmax days =extreme heat days
extreme_heat_threshold <- quantile(
  halifax_daily_clean$tmax,
  probs = 0.95
)

#adding extreme heat flag
halifax_extreme <- halifax_daily_clean %>%
  mutate(
    year = year(date),
    extreme_heat = if_else(
      tmax >= extreme_heat_threshold,
      1L,
      0L
    )
  )

#total extreme heat days by year
extreme_days_by_year <- halifax_extreme %>%
  group_by(year) %>%
  summarise(
    extreme_heat_days = sum(extreme_heat),
    .groups = "drop"
  ) %>% filter((year<2026))

#linear fit of such extreme heat days wrt year also shows general increase
ggplot(extreme_days_by_year, aes(x = year, y = extreme_heat_days)) +
  geom_line(linewidth = 1) +
  geom_point(size = 2) +
  geom_smooth(
    method = "lm",
    se = TRUE,
    color = "black"
  )+
  labs(
    title = "Annual Number of Extreme Heat Days",
    subtitle = "Shearwater RCS (95th percentile of daily maximum temperature)",
    x = "Year",
    y = "Number of Extreme Heat Days"
  ) +
  theme_minimal()

write_csv(extreme_days_by_year, "extremdaysbyyear.csv")
