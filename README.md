# Increasing Well-Being With Data Analytics

- [Exercise 1 — Data Exploration](./EXERCISE-1.md)

## Syllabus

- [R for Data Science](https://r4ds.had.co.nz)
- [Cheatsheet "Daten bändigen mit R"](../wellbeing/Schummelzettel%20–%20dplyr%20und%20tidyr.pdf)

## Helpful links

- [https://duckduckgo.com](https://duckduckgo.com)
- [https://www.google.com](https://www.google.com)
- [https://www.rdocumentation.org](https://www.rdocumentation.org)
- [https://www.tidyverse.org](https://www.tidyverse.org)
- [https://www.rstudio.com/resources/cheatsheets](https://www.rstudio.com/resources/cheatsheets)
- [https://www.r-bloggers.com](https://www.r-bloggers.com)
- [https://stackoverflow.com/questions/tagged/r](https://stackoverflow.com/questions/tagged/r)
- [https://towardsdatascience.com](https://towardsdatascience.com)

## 0. Prerequisites

Please prepare for the exercise by following these steps:

1. Download and install `R` from here: https://cloud.r-project.org/
1. Download and install `RStudio` from here: https://www.rstudio.com/products/rstudio/download/#download
1. Make sure that you can open RStudio and that it works by entering `sum(1, 1)` and pressing `Enter ⏎`. You should see an output of `2`.

Everything below will be covered during the exercise, but feel free to read ahead.

## 1. The most important skill you will need

First of all, the goal for this course is _not_ to teach you everything you will need to work with R. The goal here is to show you how some basics, give you an introduction to RStudio, and make you curious to explore by yourself. Depending on the wellness activity you choose, you will need different commands and approaches for the analysis. Therefore, we want to show you where to look for the answers and give you a glimpse into working with R. The goal is _most definitely not_ to make you memorize some random commands or functions because...

:trophy: **The single most important skill you need to use R is knowing how to use a search engine well.**

## 2. First steps in R

- Variables are usually in `snake_case`
- Variable assignments look like this: `days_in_year <- 365`
- Function calls look like this: `f(x, y)`. Example: `round(3.14158, digits = 2)`
- To inspect a variable type `days_in_year` and press `Enter ⏎`
- One important data structure is vectors, they look like this (for a string vector) (the `c` stands for `combine`): `c('Han Solo', 'Chewbacca')`
- Most commonly in our course, data is stored in `data.frame`s or `tibble`s. A `tibble` (or `tbl_df`) is [a modern reimagining of the `data.frame`](https://tibble.tidyverse.org), so fundamentally the same but better.
- The pipe: Instead of calling functions like this `f(x, y)`, you can also use a so-called pipe and put the first function parameter in front of it like this: `x %>% f(y)`. Example from above: `3.14157 %>% round(digits = 2)` is the same as `round(3.14157, digits = 2)`. Another example: `5 %>% sum(7)` is the same as `sum(5, 7)`.

## 3. Creating our first tibble

- First, put some example names in a string vector: `names <- c('Han Solo', 'Leia Organa', 'R2-D2')`
- Second, put some example data in a string vector: `species <- c('Human', 'Human', 'Droid')`
- Then, make it a data frame: `df <- data.frame(name = names, species = species)`
- Then turn it into a tibble: `tibble <- as_tibble(df)`

## 4. Subsetting

This is how you can get data out of a data frame:

- Get the entire column with `df['name']`
- Get the values of a column by name with `df[['name']]` or `df$name`
- Get the values of a column by index with `df[,1]`
- Get the values of a row by index with `df[1,]`
- Get the values of a cell by index of with `df[1,1]`

## 5. Working with `dplyr`

[`dplyr`](https://github.com/tidyverse/dplyr) is a very popular package for handling data. It has five basic verbs:

1.  `select()`: picks variables (columns) based on their names.
2.  `filter()`: picks cases (rows) based on their values.
3.  `arrange()`: changes the ordering of the rows.
4.  `mutate()`: adds new variables that are functions of existing variables
5.  `summarise()`: reduces multiple values down to a single summary.
6.  (and `group_by`: allows you to perform any operation “by group”.)

[Let's start with exercise 1!](./EXERCISE-1.md)
