# Exercise 2 — Data Curation

- :house_with_garden: [Home](./README.md)
- :open_book: [`dplyr` documentation](https://dplyr.tidyverse.org/reference/index.html)
- :open_book: [`readr` documentation](https://readr.tidyverse.org/reference/index.html)
- :open_book: [`lubridate` documentation](https://lubridate.tidyverse.org/reference/index.html)
- :open_book: [`rmarkdown` cheatsheet](./rmarkdown-cheatsheet-2.0.pdf)

## Preparation

1. Install the `weights` package by running `install.packages('weights')`
1. Install the `tidyfst` package by running `install.packages('tidyfst')`
1. Install the `rmarkdown` package by running `install.packages('rmarkdown')`
   `

## 1. Group task

### 1.1. What is [Kaggle](https://www.kaggle.com/)?

- [Kaggle Competitions](https://www.kaggle.com/competitions) [(Docs)](https://www.kaggle.com/docs/competitions)

  > "Kaggle Competitions are designed to provide challenges for competitors at all different stages of their machine learning careers."

- [Kaggle Datasets](https://www.kaggle.com/datasets) [(Docs)](https://www.kaggle.com/docs/datasets)

  > "Explore, analyze, and share quality data"

- [Kaggle Notebooks](https://www.kaggle.com/code) [(Docs)](https://www.kaggle.com/docs/notebooks)

  > "Explore and run machine learning code with Kaggle Notebooks, a cloud computational environment that enables reproducible and collaborative analysis"

### 1.2. How to import data you downloaded from Kaggle

There are [a lot of packages in the `tidyverse` for importing data](https://www.tidyverse.org/packages/#import), but only care about [`readr`](https://readr.tidyverse.org/)&dagger; and [`readxl`](https://readxl.tidyverse.org/)&ddagger;.

- `read_csv()`&dagger; for comma-separated values (csv) files
- `read_csv2()`&dagger; for csv files that use semicolons
- `read_tsv()`&dagger; for csv files that use tabs
- `read_delim()`&dagger; for csv files that use anything else
- `read_excel()`&ddagger; for Excel files

As you can see, there are a lot of different functions with very specific use cases. To read a file, it's easiest if the file is in your current working directory (CWD). To find out what your CWD is, use `getwd()`.

### 1.3. Dummy variables

To get started with `.csv` files, we will now take a look at some examples. We uploaded two example files for you: `friends.csv` and `starwars_dd.csv`. To get the files, you can either download everything as `.zip` or create a new empty file and copy&paste the raw file content. (For those of you who know what that is, you can also, of course, clone the repository.)

- **Option 1**: Download as `.zip`:
  Go to [the `wellbeing` repository](https://github.com/tud-ise/wellbeing), click on the download button and click on "Download ZIP". Then put it into your CWD. Also see: ["How to download as ZIP?"](https://stackoverflow.com/questions/2751227/how-to-download-source-in-zip-format-from-github)
- **Option 2**: Create empty file and copy&paste:
  Open RStudio, type `write.csv('hello there', 'friends.csv')` and press `Enter ⏎`. You should now see a new file in RStudio that you can open and replace the content with [the raw content of the files in the repository](https://raw.githubusercontent.com/tud-ise/wellbeing/main/friends.csv).

Now that you have the file in your CWD, let's import and work with it.

1. Import `friends.csv` into a variable named `friends`.

```R
friends <- read_csv('friends.csv')
```

2. Write a function called `has_been_vaccinated()` that returns `1` if the passed parameter is defined and `0` if it is not.

```R
has_been_vaccinated <- function(vaccine) {
  is_vaccinated = ifelse(is.na(vaccine), 0, 1)
  return(is_vaccinated)
}
```

3. Use this function to dummify the column `vaccine` into a new column `is_vaccinated`. `1` means "vaccinated" and `0` means "not vaccinated". It doesn't matter which vaccine the character has, only **_if_** they have one.

```R
friends %>%
  mutate(is_vaccinated = has_been_vaccinated(vaccine))
```

4. Now, we **_do_** want to know which vaccine each character received. In theory, we could do this by checking for each possibility separately:

```R
has_received_biontech <- function(vaccine){
  return(ifelse(vaccine == 'BioNTech', 1, 0))
}

has_received_astrazeneca <- function(vaccine){
  return(ifelse(vaccine == 'AstraZeneca', 1, 0))
}
# [etc. ...]

friends %>%
  mutate(biontech = has_received_biontech(vaccine)) %>%
  mutate(astra = has_received_astrazeneca(vaccine)) %>%
# [etc. ...]
```

But, as you can see, this is quite tedious. Luckily, there is the function `dummify()` from the package `weights`. So load the package with `library('weights')` and then let's try to use `dummify()`:

```R
dummify(friends$vaccine)
```

This will show you an error: `variable needs to be a factor`. But what are factors?

> R uses factors to represent categorical variables that have a known set of possible values.&dagger;

&dagger; [Source](https://r4ds.had.co.nz/factors.html?q=fac#creating-factors)

So how do we create factors? In this case, it's simple. First, we remove all `NA` values:

```R
friends <- friends %>%
  mutate(vaccine = replace_na(vaccine, 'NONE'))
```

Then, we make the `vaccine` column a factor:

```R
friends_factorized <- friends %>%
  mutate(vaccine = as.factor(vaccine))
```

## 2. Task (break out session)

1. Install the `tidyfst` package: `install.packages("fastDummies")`
1. Download the `starwars_dd.csv` file from GitHub. For ease of use: place it in the folder which is shown in the `Files` section of RStudio
1. Import `starwars_dd.csv` into a variable named `starwars_dd`. The contents are different from the internal `starwars` dataset and simplify the next steps for you.
1. Import the `tidyfst` package. Then generate dummies for all films and store them in the variable `starwars_films_dd`. Inspect them in RStudio (using `View`) afterwards. You may use the [:open_book: `dummy_dt`](https://www.rdocumentation.org/packages/tidyfst/versions/0.9.9/topics/dummy_dt) function. Make sure to exclude rows which hold no additional information, we are focusing on the different films here (Hint: Each character has multiple rows for each film, vehicle and starship)
1. How many characters starred in `A New Hope`?
1. Which of those characters are female?
1. **Expert Problem** How many characters starred in `A New Hope` and in `The Force Awakens`?

   You are allowed to use the following snippets, which you may copy as-is and/or provide you with the idea how to tackle this problem:

   - Setting the `films_...` columns manually to `1` to fake the "correct" output

     ```R
     mutate(`films_A New Hope` = 1, `films_The Force Awakens` = 1)
     ```

   - Which is the most prevalent eye color on each planet? (From the last exercise)

     ```R
     > starwars %>%
     +       group_by(homeworld, eye_color) %>%
     +       mutate(rank = row_number()) %>%
     +       group_by(homeworld) %>%
     +       slice(which.max(rank)) %>%
     +       summarise(homeworld, eye_color, rank)
     # A tibble: 49 x 3
     ```

## 3. Group task

### 3.1. Working with dates

Because working with dates can be cumbersome, the `tidyverse` contains a very helpful package for that: `lubridate`.

1. If the data you are trying to work with contains a date like `3/14/21`, you can tell `lubridate` that this is a date:

```R
date <- parse_date_time('3/14/21', 'mdy')
```

The `'mdy'` represents `[m]onth [d]ay [y]ear`. The resulting `date` is a "POSIXct date-time object" (you can verify that with `is.instant(date)` or `class(date)`) that makes it very easy to work with it. For instance, you can find out which week the date refers to:

```R
week(date)
```

We can also do some calculations with timestamps:

```R
start <- now()
# wait...
end <- now()
elapsed <- start %--% end
# ... and find out how long you waited
as.duration(elapsed)
```

### 3.2. Working with R Markdown

If you want to generate output of the magic you created in R, there is another great package to do that: `rmarkdown`. ([What is Markdown?](https://en.wikipedia.org/wiki/Markdown))

To create a new file, click on `File > New File > R Markdown...`, name it whatever you want and click `OK`. This should open an untitled file, which you can save to your CWD via `File > Save As...`. If you have named it `report.Rmd`, you can turn this into an `.html` file by typing `render('report.Rmd')` and pressing `Enter ⏎`. This will create a new file in your CWD (bottom right in R Studio). Click on it and select `View in Web Browser` to see the generated HTML.
