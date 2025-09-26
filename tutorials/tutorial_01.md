# Tutorial 01: Creating Your First Quarto Document with Data Visualizations

CS301 Fall 2025: Activity 03

Welcome to your first hands-on tutorial for creating publishable documents with embedded R code and data visualizations using Quarto!

## Objective

In this tutorial, you will:

* Set up a basic Quarto document
* Load and explore the Palmer Penguins dataset
* Create beautiful visualizations using the tidyverse
* Generate a professional HTML report with embedded code and graphics

## What You Will Learn

Quarto documents combine three key elements:

1. **Narrative text** (written in Markdown)
2. **R code** (in code chunks)
3. **Output** (tables, plots, and results)

## Step 1: Install Required Packages

Before we start, you will need to install the required R packages. Run these commands in your R console:

```r
# Install required packages (run this once)
install.packages(c("palmerpenguins", "tidyverse", "knitr"))

# Alternative if tidyverse is too large, install just what we need:
# install.packages(c("palmerpenguins", "ggplot2", "dplyr", "knitr"))
```

## Step 2: Create Your First Quarto Document

Create a new file called `penguin_report.qmd` in your project folder. Copy and paste the following content:

````markdown
---
title: "Palmer Penguins Data Analysis"
author: "Your Name Here"
date: "`r Sys.Date()`"
format: html
editor: visual
---

# Introduction

The Palmer Penguins dataset contains measurements for three penguin species observed on three islands in the Palmer Archipelago, Antarctica. In this report, we'll explore the relationships between different penguin characteristics.

```{r setup, include=FALSE}
# Load required libraries
library(palmerpenguins)
library(ggplot2)
library(dplyr)
library(knitr)

# Set default options for code chunks
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)
```

## Dataset Overview

Let's start by examining our data:

```{r data-overview}
# Load the penguins data
data(penguins)

# Display the first few rows
head(penguins)

# Get basic information about the dataset
str(penguins)

# Summary statistics
summary(penguins)
```

## Basic Statistics

Here are some basic statistics about our penguin friends:

```{r basic-stats}
# Count penguins by species
species_count <- penguins %>%
  count(species) %>%
  arrange(desc(n))

knitr::kable(species_count, 
             col.names = c("Species", "Count"),
             caption = "Number of Penguins by Species")

# Count by island
island_count <- penguins %>%
  count(island) %>%
  arrange(desc(n))

knitr::kable(island_count,
             col.names = c("Island", "Count"), 
             caption = "Number of Penguins by Island")
```

## Visualization 1: Penguin Body Mass Distribution

```{r body-mass-histogram}
# Create histogram of body mass
ggplot(penguins, aes(x = body_mass_g)) +
  geom_histogram(bins = 20, fill = "steelblue", alpha = 0.7) +
  labs(title = "Distribution of Penguin Body Mass",
       x = "Body Mass (grams)",
       y = "Frequency") +
  theme_minimal()
```

## Visualization 2: Bill Length vs. Bill Depth

```{r bill-measurements}
# Scatterplot of bill dimensions colored by species
ggplot(penguins, aes(x = bill_length_mm, y = bill_depth_mm, color = species)) +
  geom_point(size = 3, alpha = 0.8) +
  labs(title = "Penguin Bill Dimensions by Species",
       x = "Bill Length (mm)",
       y = "Bill Depth (mm)",
       color = "Species") +
  theme_minimal() +
  theme(legend.position = "bottom")
```

## Visualization 3: Flipper Length by Species

```{r flipper-boxplot}
# Box plot of flipper length by species
ggplot(penguins, aes(x = species, y = flipper_length_mm, fill = species)) +
  geom_boxplot(alpha = 0.7) +
  geom_jitter(width = 0.2, alpha = 0.5) +
  labs(title = "Flipper Length Distribution by Species",
       x = "Species",
       y = "Flipper Length (mm)") +
  theme_minimal() +
  theme(legend.position = "none") +
  scale_fill_brewer(type = "qual")
```

## Key Findings

Based on our analysis, we can observe:

1. **Adelie penguins** are the most numerous in our dataset
2. **Bill dimensions** show clear separation between species, with Gentoo penguins having the longest bills
3. **Flipper length** varies significantly between species, with Gentoo penguins having the longest flippers

## Conclusion

The Palmer Penguins dataset provides excellent examples of how different penguin species have evolved distinct physical characteristics. These measurements help researchers understand species identification and ecological adaptations.

## Data Source

Data were collected and made available by Dr. Kristen Gorman and the Palmer Station, Antarctica LTER, a member of the Long Term Ecological Research Network.
````

## Step 3: Render Your Document

1. Save the file as `penguin_report.qmd`
2. In RStudio, click the "Render" button (or use Ctrl+Shift+K)
3. Quarto will process your document and create an HTML file

If you don't have RStudio, you can render from the command line:

```bash
quarto render penguin_report.qmd
```

## Step 4: Explore the Results

Open the generated HTML file in your browser. Notice how:

* Code and output are nicely formatted
* Plots are embedded directly in the document
* Tables are professionally styled
* The document includes a table of contents

## Challenges to Try

1. **Customize the theme**: Add `theme: cosmo` or `theme: flatly` to the YAML header
2. **Hide code**: Add `echo: false` to the setup chunk to hide code in the final output  
3. **Add new visualizations**: Create a plot showing the relationship between body mass and flipper length
4. **Filter data**: Create visualizations for just one island or one species

## Troubleshooting

some common issues you might encounter are the following.

**Problem**: "Package not found" error  
**Solution**: Make sure you've installed all required packages using `install.packages()`

**Problem**: Rendering fails  
**Solution**: Check that your code chunks have proper syntax and all required libraries are loaded

**Problem**: Plots don't appear  
**Solution**: Ensure your code chunks include `{r chunk-name}` and plots are assigned to variables or called directly

---

**Next**: Try Tutorial 02 for interactive Quarto documents with dynamic content!
Cool, right?!
