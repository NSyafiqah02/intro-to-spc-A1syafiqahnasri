
  library(tidyverse)
  library(plotly)
  library(qcc)
  library(htmlwidgets)

  # Filter data for Machine 1, Temperature 338, Pressure 200
  df_cc1 <- X028...028 %>%
    filter(Machine == 1, Temperature == 338, Pressure == 200) %>%
    arrange(Timestamp)

  if (nrow(df_cc1) > 0) {
    qcc_obj1 <- qcc(df_cc1$PartLength, type = "xbar.one", plot = FALSE)

    data_points1 <- data.frame(
      Index = 1:length(qcc_obj1$data),
      PartLength = qcc_obj1$data
    )

    CL1 <- qcc_obj1$center
    UCL1 <- qcc_obj1$limits[2]
    LCL1 <- qcc_obj1$limits[1]

    p_cc1 <- ggplot(data_points1, aes(x = Index, y = PartLength)) +
      geom_line(color = "#0072B2", linewidth = 1) +
      geom_point(color = "#0072B2") +
      geom_hline(yintercept = CL1, linetype = "solid", color = "blue", linewidth = 1, alpha=0.7) +
      geom_hline(yintercept = UCL1, linetype = "dashed", color = "red", linewidth = 1, alpha=0.7) +
      geom_hline(yintercept = LCL1, linetype = "dashed", color = "red", linewidth = 1, alpha=0.7) +
      annotate("text", x = max(data_points1$Index), y = UCL1, label = "UCL", vjust = -0.5, hjust = 1, color = "red", size=6) +
      annotate("text", x = max(data_points1$Index), y = LCL1, label = "LCL", vjust = 1.5, hjust = 1, color = "red", size=6) +
      annotate("text", x = max(data_points1$Index), y = CL1, label = "CL", vjust = -0.5, hjust = 1, color = "blue", size=6) +
      labs(title = "Xbar Control Chart for Part Length (Machine 1, Temp 338, Pres 200)",
           x = "Observation Index",
           y = "Part Length") +
      theme_minimal() +
      theme(plot.title = element_text(size = 18),
            axis.title = element_text(size = 18),
            axis.text = element_text(size = 14),
            panel.background = element_rect(fill = "white", colour = NA),
            plot.background = element_rect(fill = "white", colour = NA))

    html_plot_cc1 <- ggplotly(p_cc1)
    saveWidget(html_plot_cc1, "media/plots/xbar_machine1_temp338_pres200.html", selfcontained = TRUE)
  } else {
    cat("No data found for Machine 1 with Temperature 338 and Pressure 200 to create control chart.
")
  }
  
