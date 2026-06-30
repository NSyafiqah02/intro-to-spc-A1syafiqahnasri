
library(tidyverse)
library(plotly)
library(htmlwidgets)

# Filter data for Machine 2
df_machine2 <- X028...028 %>% filter(Machine == 2)

# Create histogram for PartLength
p2 <- ggplot(df_machine2, aes(x = PartLength)) +
  geom_histogram(binwidth = 0.5, fill = "#D55E00", color = "white") + # Okabe-Ito color
  labs(title = "Histogram of Part Length (Machine 2)",
       x = "Part Length",
       y = "Frequency") +
  theme_minimal() +
  theme(plot.title = element_text(size = 18),
        axis.title = element_text(size = 18),
        axis.text = element_text(size = 14),
        panel.background = element_rect(fill = "white", colour = NA),
        plot.background = element_rect(fill = "white", colour = NA))

# Convert to plotly
html_plot2 <- ggplotly(p2)

# Save as HTML widget
saveWidget(html_plot2, "media/plots/histogram_machine2_partlength.html", selfcontained = TRUE)

