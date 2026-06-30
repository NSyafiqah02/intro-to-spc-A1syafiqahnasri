
  library(tidyverse)
  library(plotly)
  library(qcc)
  library(htmlwidgets)

  # Filter data for Machine 2, Temperature 338, Pressure 200
  df_cc2 <- X028...028 %>%
    filter(Machine == 2, Temperature == 338, Pressure == 200) %>%
    arrange(Timestamp)

  if (nrow(df_cc2) > 0) {
    qcc_obj2 <- qcc(df_cc2$PartLength, type = "xbar.one", plot = FALSE)

    data_points2 <- data.frame(
      Index = 1:length(qcc_obj2$data),
      PartLength = qcc_obj2$data
    )

    CL2 <- qcc_obj2$center
    UCL2 <- qcc_obj2$limits[2]
    LCL2 <- qcc_obj2$limits[1]

    p_cc2 <- ggplot(data_points2, aes(x = Index, y = PartLength)) +
      geom_line(color = "#009E73", linewidth = 1) +
      geom_point(color = "#009E73") +
      geom_hline(yintercept = CL2, linetype = "solid", color = "blue", linewidth = 1, alpha=0.7) +
      geom_hline(yintercept = UCL2, linetype = "dashed", color = "red", linewidth = 1, alpha=0.7) +
      geom_hline(yintercept = LCL2, linetype = "dashed", color = "red", linewidth = 1, alpha=0.7) +
      annotate("text", x = max(data_points2$Index), y = UCL2, label = "UCL", vjust = -0.5, hjust = 1, color = "red", size=6) +
      annotate("text", x = max(data_points2$Index), y = LCL2, label = "LCL", vjust = 1.5, hjust = 1, color = "red", size=6) +
      annotate("text", x = max(data_points2$Index), y = CL2, label = "CL", vjust = -0.5, hjust = 1, color = "blue", size=6) +
      labs(title = "Xbar Control Chart for Part Length (Machine 2, Temp 338, Pres 200)",
           x = "Observation Index",
           y = "Part Length") +
      theme_minimal() +
      theme(plot.title = element_text(size = 18),
            axis.title = element_text(size = 18),
            axis.text = element_text(size = 14),
            panel.background = element_rect(fill = "white", colour = NA),
            plot.background = element_rect(fill = "white", colour = NA))

    html_plot_cc2 <- ggplotly(p_cc2)
    saveWidget(html_plot_cc2, "media/plots/xbar_machine2_temp338_pres200.html", selfcontained = TRUE)
  } else {
    cat("No data found for Machine 2 with Temperature 338 and Pressure 200 to create control chart.
")
  }
  
