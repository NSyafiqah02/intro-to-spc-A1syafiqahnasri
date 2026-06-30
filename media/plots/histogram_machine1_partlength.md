
library(tidyverse)
library(plotly)
library(htmlwidgets)

# Filter data for Machine 1
df_machine1 <- X028...028 %>% filter(Machine == 1)

# Create histogram for PartLength
p1 <- ggplot(df_machine1, aes(x = PartLength)) +
  geom_histogram(binwidth = 0.5, fill = "#0072B2", color = "white") + # Okabe-Ito color
  labs(title = "Histogram of Part Length (Machine 1)",
       x = "Part Length",
       y = "Frequency") +
  theme_minimal() +
  theme(plot.title = element_text(size = 18),
        axis.title = element_text(size = 18),
        axis.text = element_text(size = 14),
        panel.background = element_rect(fill = "white", colour = NA),
        plot.background = element_rect(fill = "white", colour = NA))

# Convert to plotly
html_plot1 <- ggplotly(p1)

# Save as HTML widget
saveWidget(html_plot1, "media/plots/histogram_machine1_partlength.html", selfcontained = TRUE)

