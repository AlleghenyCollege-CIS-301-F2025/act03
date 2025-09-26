# Tutorial 02: Interactive Quarto Documents with Dynamic Content

CS301 Fall 2025: Activity 03

In this tutorial, you will create an advanced Quarto document with interactive elements that allow users to explore data dynamically.

## Objective

In this tutorial, you will:

* Create interactive visualizations with user controls
* Use HTML widgets and plotly for dynamic graphics  
* Implement parameterized reports with user inputs
* Build dashboards with tabbed sections and sidebars

## What you Will Build

You will create an interactive penguin data explorer that includes:

1. **Interactive plots** with zoom, hover, and selection
2. **Dynamic filtering** based on user selections  
3. **Parameterized content** that changes based on inputs
4. **Professional dashboard layout** with multiple sections

## Step 1: Install Additional Packages

For interactive features, you will need additional R packages:

```r
# Install interactive packages
install.packages(c("plotly", "DT", "crosstalk", "htmlwidgets"))

# If you want dashboard layouts:
install.packages(c("flexdashboard", "reactable"))
```

## Step 2: Create Interactive Quarto Document

Create a new file called `interactive_penguin_explorer.qmd` and copy the following content:

````markdown
---
title: "Interactive Palmer Penguins Explorer"
author: "Your Name Here"
date: "`r Sys.Date()`"
format: 
  html:
    theme: cosmo
    toc: true
    toc-float: true
    code-fold: true
    code-summary: "Show/Hide Code"
editor: visual
---

```{r setup, include=FALSE}
# Load required libraries
library(palmerpenguins)
library(ggplot2)
library(dplyr)
library(plotly)
library(DT)
library(crosstalk)

# Set default options
knitr::opts_chunk$set(
  echo = TRUE, 
  warning = FALSE, 
  message = FALSE,
  fig.width = 10,
  fig.height = 6
)

# Load and prepare data
data(penguins)
penguins_clean <- penguins %>% 
  filter(!is.na(sex)) %>%
  filter(!is.na(body_mass_g))
```

## Interactive Data Overview

Let's start with an interactive table where you can search, sort, and filter the data:

```{r interactive-table}
# Create interactive data table
DT::datatable(
  penguins_clean,
  caption = "Interactive Palmer Penguins Dataset",
  options = list(
    scrollX = TRUE,
    pageLength = 10,
    searchHighlight = TRUE
  ),
  filter = 'top'
) %>%
  DT::formatRound(columns = c('bill_length_mm', 'bill_depth_mm', 
                              'flipper_length_mm', 'body_mass_g'), digits = 1)
```

## Interactive Scatter Plot

This scatter plot allows you to zoom, pan, and hover over points for details:

```{r interactive-scatter}
# Create interactive scatter plot
p1 <- ggplot(penguins_clean, 
             aes(x = bill_length_mm, 
                 y = bill_depth_mm,
                 color = species,
                 size = body_mass_g,
                 text = paste("Species:", species,
                             "<br>Island:", island,
                             "<br>Sex:", sex,
                             "<br>Bill Length:", bill_length_mm, "mm",
                             "<br>Bill Depth:", bill_depth_mm, "mm",
                             "<br>Body Mass:", body_mass_g, "g"))) +
  geom_point(alpha = 0.7) +
  labs(
    title = "Penguin Bill Dimensions (Interactive)",
    x = "Bill Length (mm)",
    y = "Bill Depth (mm)",
    color = "Species",
    size = "Body Mass (g)"
  ) +
  theme_minimal()

# Convert to interactive plotly
ggplotly(p1, tooltip = "text") %>%
  config(displayModeBar = TRUE) %>%
  layout(hovermode = "closest")
```

## Linked Visualizations

These plots are linked - selecting points in one will highlight them in the other:

```{r linked-plots}
# Create shared data object for linking
shared_penguins <- SharedData$new(penguins_clean)

# Create first linked plot
p2 <- shared_penguins %>%
  plot_ly(
    x = ~flipper_length_mm,
    y = ~body_mass_g,
    color = ~species,
    text = ~paste("Species:", species, "<br>Island:", island),
    hovertemplate = "%{text}<br>Flipper: %{x}mm<br>Mass: %{y}g<extra></extra>",
    type = "scatter",
    mode = "markers"
  ) %>%
  layout(
    title = "Flipper Length vs Body Mass",
    xaxis = list(title = "Flipper Length (mm)"),
    yaxis = list(title = "Body Mass (g)")
  )

# Create second linked plot  
p3 <- shared_penguins %>%
  plot_ly(
    x = ~bill_length_mm,
    y = ~flipper_length_mm,
    color = ~species,
    text = ~paste("Species:", species, "<br>Island:", island),
    hovertemplate = "%{text}<br>Bill Length: %{x}mm<br>Flipper: %{y}mm<extra></extra>",
    type = "scatter",
    mode = "markers"
  ) %>%
  layout(
    title = "Bill Length vs Flipper Length", 
    xaxis = list(title = "Bill Length (mm)"),
    yaxis = list(title = "Flipper Length (mm)")
  )

# Display plots side by side
subplot(p2, p3, nrows = 1, shareY = FALSE, titleX = TRUE, titleY = TRUE) %>%
  layout(
    showlegend = TRUE,
    annotations = list(
      list(text = "Select points in either plot to see highlighting in both!",
           xref = "paper", yref = "paper",
           x = 0.5, y = -0.1, showarrow = FALSE)
    )
  )
```

## Dynamic Species Comparison

Use the dropdown to compare different species:

```{r dynamic-comparison}
#| code-summary: "Species Comparison Code"

# Create function for dynamic plotting
create_species_plot <- function(selected_species = "Adelie") {
  
  species_data <- penguins_clean %>%
    filter(species == selected_species)
  
  p <- ggplot(species_data, aes(x = body_mass_g, fill = sex)) +
    geom_histogram(bins = 15, alpha = 0.7, position = "identity") +
    labs(
      title = paste("Body Mass Distribution -", selected_species, "Penguins"),
      x = "Body Mass (g)",
      y = "Count",
      fill = "Sex"
    ) +
    theme_minimal() +
    scale_fill_brewer(type = "qual", palette = "Set2")
  
  ggplotly(p) %>%
    config(displayModeBar = FALSE)
}

# Create plots for all species
adelie_plot <- create_species_plot("Adelie")
chinstrap_plot <- create_species_plot("Chinstrap") 
gentoo_plot <- create_species_plot("Gentoo")

# Display with tabs (simulated with headers)
cat("### Adelie Penguins\n")
adelie_plot

cat("### Chinstrap Penguins\n") 
chinstrap_plot

cat("### Gentoo Penguins\n")
gentoo_plot
```

## Summary Statistics Dashboard

Interactive summary by species and island:

```{r summary-dashboard}
# Create interactive summary table
summary_stats <- penguins_clean %>%
  group_by(species, island, sex) %>%
  summarise(
    Count = n(),
    Avg_Bill_Length = round(mean(bill_length_mm, na.rm = TRUE), 2),
    Avg_Bill_Depth = round(mean(bill_depth_mm, na.rm = TRUE), 2),
    Avg_Flipper_Length = round(mean(flipper_length_mm, na.rm = TRUE), 2),
    Avg_Body_Mass = round(mean(body_mass_g, na.rm = TRUE), 2),
    .groups = "drop"
  )

DT::datatable(
  summary_stats,
  caption = "Summary Statistics by Species, Island, and Sex",
  options = list(
    dom = 'Bfrtip',
    pageLength = 20,
    scrollX = TRUE,
    buttons = c('copy', 'csv', 'excel')
  ),
  extensions = 'Buttons'
) %>%
  DT::formatStyle(
    columns = "Count",
    background = styleColorBar(summary_stats$Count, 'lightblue')
  ) %>%
  DT::formatStyle(
    columns = "Avg_Body_Mass",
    background = styleColorBar(summary_stats$Avg_Body_Mass, 'lightcoral')
  )
```

## Interactive Correlation Plot

Explore correlations between numeric variables:

```{r correlation-plot}
# Create correlation matrix
numeric_vars <- penguins_clean %>%
  select(bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g)

correlation_matrix <- cor(numeric_vars, use = "complete.obs")

# Create interactive heatmap
plot_ly(
  z = ~correlation_matrix,
  x = colnames(correlation_matrix),
  y = colnames(correlation_matrix),
  type = "heatmap",
  colorscale = "RdBu",
  zmid = 0,
  text = round(correlation_matrix, 2),
  texttemplate = "%{text}",
  textfont = list(size = 12),
  hovertemplate = "Correlation: %{z:.3f}<extra></extra>"
) %>%
  layout(
    title = "Correlation Matrix of Penguin Measurements",
    xaxis = list(title = ""),
    yaxis = list(title = "")
  )
```

## Key Interactive Features

This document demonstrates several interactive capabilities:

::: {.callout-tip}
## Interactive Elements Used

1. **Zoomable/Pannable Plots**: Use mouse wheel and drag to explore
2. **Hover Information**: Detailed tooltips on data points  
3. **Linked Brushing**: Selections in one plot highlight points in others
4. **Searchable Tables**: Filter and sort data interactively
5. **Dynamic Content**: Plots that update based on data subsets
:::

## Conclusion

Interactive Quarto documents bridge the gap between static reports and full applications, providing engaging ways to explore and present data while maintaining reproducibility.

Try experimenting with:

- Different plotly chart types
- Adding filters and controls
- Creating animated transitions
- Building multi-page dashboards
````

## Step 3: Render Your Interactive Document

1. Save the file as `interactive_penguin_explorer.qmd`
2. Render using RStudio's "Render" button or command line:
   ```bash
   quarto render interactive_penguin_explorer.qmd
   ```
3. Open the HTML file in your browser and interact with the elements!

## Step 4: Explore Advanced Features

Try these enhancements:

### Add Parameter Controls

You can add parameters that users can modify when rendering:

```yaml
params:
  species_filter: "Adelie"
  min_body_mass: 3000
```

### Create Dashboard Layout

For a dashboard look, change your format to:

```yaml
format:
  html:
    theme: cosmo
    css: styles.css
    layout: sidebar
```

### Add Animation

```r
# Animated scatter plot
p %>% 
  add_markers(frame = ~year) %>%  # if you had temporal data
  animation_opts(1000, easing = "elastic", redraw = FALSE)
```

### Add More Animation to a Scatter plot.

You can compare year to year changes in the scatter plot with animation.

```r
{r animated-plot}
# Create an animated plot showing the relationship between variables
# This demonstrates time-series animation (simulated)
set.seed(42)
animated_data <- penguins_clean %>%
  mutate(year = sample(2007:2009, n(), replace = TRUE)) %>%
  arrange(year)

p_animated <- animated_data %>%
  plot_ly(
    x = ~bill_length_mm,
    y = ~body_mass_g,
    size = ~flipper_length_mm,
    color = ~species,
    frame = ~year,
    text = ~paste("Species:", species, "<br>Year:", year),
    hovertemplate = "%{text}<br>Bill Length: %{x}mm<br>Body Mass: %{y}g<extra></extra>",
    type = "scatter",
    mode = "markers"
  ) %>%
  layout(
    title = "Penguin Measurements Over Time (Simulated Animation)",
    xaxis = list(title = "Bill Length (mm)"),
    yaxis = list(title = "Body Mass (g)")
  ) %>%
  animation_opts(
    1000, easing = "elastic", redraw = FALSE
  ) %>%
  animation_button(
    x = 1, xanchor = "right", y = 0, yanchor = "bottom"
  ) %>%
  animation_slider(
    currentvalue = list(prefix = "Year ", font = list(color="red"))
  )

p_animated
```

## Challenges to Try

Using this, or other data (R has a lot to play with!), try to add the following types of functionalities to your document.

1. **Add Filtering Controls**: Create dropdown menus or checkboxes to filter data
2. **Build Tabs**: Organize content into tabbed sections using `{.tabset}`
3. **Create Animations**: Add animated transitions between different views
4. **Multi-Column Layouts**: Use Quarto's column layouts for better organization
5. **Custom Styling**: Add CSS to customize the appearance

## Troubleshooting

Some of the problems that you might encounter are the following.

**Problem**: Interactive plots don't render  
**Solution**: Ensure `plotly` package is installed and HTML format is used

**Problem**: Linked plots don't work  
**Solution**: Check that `crosstalk` package is installed and `SharedData` is used correctly

**Problem**: Tables aren't interactive  
**Solution**: Verify `DT` package is installed and `DT::datatable()` is used

---

**Congratulations!** You've created interactive, publication-ready documents with Quarto! Now, go on to complete your reflections in `writing/reflection.md`!

(Did you add your name to the top of your source files? You should!)
