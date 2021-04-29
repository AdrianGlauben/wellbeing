# Exercise 1 — Data Exploration

:house_with_garden: [Home](./README.md)

:open_book: [`dplyr` documentation](https://dplyr.tidyverse.org/reference/index.html)

## Preparation

1. Install the `tidyverse` package by running `install.packages("tidyverse")`
   Tidyverse is a group of very popular packages (e.g., `dplyr`, `tidyr`, `ggplot2` or `stringr`). This installation needs to run only once.
1. Load `tidyverse` by typing `library(tidyverse)` and hitting `Enter ⏎`. You need to run this every time you open RStudio again.

## Group task

1. Who is the shortest character?

```R
starwars %>%
   arrange(height)
```

2. Who is the tallest character?

```R
starwars %>%
   arrange(desc(height))
```

3. Find all droids.

```R
starwars %>%
   filter(species == 'Droid')
```

4. How many droids are there?

```R
starwars %>%
   filter(species == 'Droid') %>%
   summarise(n())
```

5. What is the home world of Han Solo?

```R
starwars %>%
   select(name, homeworld) %>%
   filter(name == 'Han Solo')
```

6. How many characters are there per species? Sort by ascending count.

```R
starwars %>%
    group_by(species) %>%
    summarise(count = n()) %>%
    arrange(desc(count))
```

7. Change the height from `cm` to `m`.

```R
starwars %>%
   mutate(height = height / 100)
```

8. Filter out all characters that have missing values (`NA`) for their mass. How many are left?

```R
starwars %>%
   select(name, mass) %>%
   na.omit()
```
