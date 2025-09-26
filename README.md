# Activity 03 :: CS301 Fall 2025

## Creating Publishable Documents with Quarto and R

**Assigned**: Friday, 26 September 2025

**Due**: End of class (Please commit your work by the end of class!!!)

![graphics/quarto_logo.png](graphics/quarto_logo.png)

Welcome to Activity 03! In this activity, you'll learn how to create beautiful, publishable documents that seamlessly combine narrative text with R code and data visualizations using Quarto.

## Why Quarto Matters for Data Scientists

As a data scientist, your work doesn't end with analyzing data—you need to communicate your findings effectively. Quarto is a powerful publishing system that lets you combine code, results, and narrative text into professional documents. Whether you're creating reports, presentations, or scientific papers, Quarto helps you tell compelling data stories that are both reproducible and visually appealing.

## Meet Quarto: Your Publishing Powerhouse

Quarto builds on the success of R Markdown but takes it further, supporting multiple languages (R, Python, Julia, and more) and output formats (HTML, PDF, Word, presentations, websites, and books). With Quarto, you can embed R code directly in your documents, automatically generate plots and tables, and even create interactive visualizations—all while maintaining a professional appearance.

## Learning Objectives

In this activity, you will:

* Learn the basics of Quarto document creation
* Embed R code and visualizations in publishable documents
* Work with the Palmer Penguins dataset using the tidyverse
* Create interactive elements for dynamic document experiences
* Feel great about knowing how to use a tool which is widely adopted in the data science community!

## Getting Started

Follow the tutorials in this folder to set up your Quarto environment and start creating your first publishable data document. Each tutorial will guide you step-by-step through loading data, creating visualizations, and building professional documents.

Tutorials to complete:

* [tutorial_01.md](./tutorials/tutorial_01.md): Creating Your First Quarto Document with Data Visualizations
* [tutorial_02.md](./tutorials/tutorial_02.md): Interactive Quarto Documents with Dynamic Content

Explore the power of combining code and narrative! Try out the penguin data analysis examples to see how Quarto makes your data science work publication-ready.

![Penguin Analysis Preview](graphics/penguin_analysis.png)

* Check out the example Quarto document: `src/penguin_analysis.qmd`

## Deliverables

* Your completed code for the two tutorials above
* Write your reflections in the file `writing/reflection.md`

## Assessment

This is a check mark activity. Please complete the tutorials and commit your work to your GitHub repository by the end of class.

## GatorGrade

You can check the baseline writing and commit requirements for this lab assignment by running department's assignment checking `gatorgrade` tool. To use `gatorgrade`, you first need to make sure you have Python3 installed (type `python --version` or `python3 --version` to check). If you do not have Python installed, please see:

* [Setting Up Python on Windows](https://realpython.com/lessons/python-windows-setup/)
* [Python 3 Installation and Setup Guide](https://realpython.com/installing-python/)
* [How to Install Python 3 and Set Up a Local Programming Environment on Windows 10](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-windows-10)

Then, if you have not done so already, you need to install `gatorgrade`:

* First, [install `pipx`](https://pypa.github.io/pipx/installation/)
* Then, install `gatorgrade` with `pipx install gatorgrade`

Finally, you can run `gatorgrade`:

`gatorgrade --config config/gatorgrade.yml`

## Seeking Assistance

* Extra resources for using markdown include:
  * [Markdown Tidbits](https://www.youtube.com/watch?v=cdJEUAy5IyA)
  * [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* Additional Quarto resources:
  * [Quarto Guide](https://quarto.org/docs/guide/)
  * [Quarto with R](https://quarto.org/docs/computations/r.html)
* Do not forget to use the above git commands to push your work to the cloud for the instructor to grade your assignment. You can go to your GitHub repository using your browser to verify that your files have been submitted. Please see the TL's or the instructor if you have any questions about assignment submission.

If you have questions, please ask the Technical Leaders or the instructor.
