# Exercise 1 — Data Exploration

- :house_with_garden: [Home](./README.md)
- :open_book: [`dplyr` documentation](https://dplyr.tidyverse.org/reference/index.html)

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

## Task 1

1. Find all characters with yellow eyes.

```R
> starwars %>%
+     select(name, eye_color) %>%
+     filter(eye_color == 'yellow')
# A tibble: 11 x 2
   name              eye_color
   <chr>             <chr>
 1 C-3PO             yellow
 2 Darth Vader       yellow
 3 Palpatine         yellow
 4 Watto             yellow
 5 Darth Maul        yellow
 6 Dud Bolt          yellow
 7 Ki-Adi-Mundi      yellow
 8 Yarael Poof       yellow
 9 Poggle the Lesser yellow
10 Zam Wesell        yellow
11 Dexter Jettster   yellow
```

2. Remove all Gungans. How many characters are left?

```R
> starwars %>%
+     filter(species != 'Gungan')
# A tibble: 83 x 14
```

3. What is the average mass of all droids?

```R
> starwars %>%
+     select(name, species, mass) %>%
+     filter(species == 'Droid') %>%
+     na.omit() %>%
+     summarise(mean(mass))
# A tibble: 1 x 1
  `mean(mass)`
         <dbl>
1         69.8
```

4. Calculate the BMI for all humans (`mass / ((height / 100) ^ 2)`)

```R
> starwars %>%
+     select(name, mass, height, species) %>%
+     filter(species == 'Human') %>%
+     filter(!is.na(mass)) %>%
+     filter(!is.na(height)) %>%
+     mutate(bmi = (mass / ((height / 100) ^ 2)))
# A tibble: 22 x 5
   name                mass height species   bmi
   <chr>              <dbl>  <int> <chr>   <dbl>
 1 Luke Skywalker        77    172 Human    26.0
 2 Darth Vader          136    202 Human    33.3
 3 Leia Organa           49    150 Human    21.8
 4 Owen Lars            120    178 Human    37.9
 5 Beru Whitesun lars    75    165 Human    27.5
 6 Biggs Darklighter     84    183 Human    25.1
 7 Obi-Wan Kenobi        77    182 Human    23.2
 8 Anakin Skywalker      84    188 Human    23.8
 9 Han Solo              80    180 Human    24.7
10 Wedge Antilles        77    170 Human    26.6
# … with 12 more rows
```

5. Which character has the longest name?

```R
> starwars %>%
+     select(name) %>%
+     arrange(desc(str_length(name)))
# A tibble: 87 x 1
   name
   <chr>
 1 Jabba Desilijic Tiure
 2 Wicket Systri Warrick
 3 Bail Prestor Organa
 4 Beru Whitesun lars
 5 Biggs Darklighter
 6 Poggle the Lesser
 7 Anakin Skywalker
 8 Jek Tono Porkins
 9 Lando Calrissian
10 Luminara Unduli
# … with 77 more rows
```

6. What is the earliest birth year for each species? (`birth_year` is measured in BBY = Before Battle of Yavin, so high values = earlier)

```R
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species) %>%
+     arrange(birth_year) %>%
+     summarise(max(birth_year))
# A tibble: 15 x 2
   species        `max(birth_year)`
   <chr>                      <dbl>
 1 Cerean                        92
 2 Droid                        112
 3 Ewok                           8
 4 Gungan                        52
 5 Human                        102
 6 Hutt                         600
 7 Kel Dor                       22
 8 Mirialan                      58
 9 Mon Calamari                  41
10 Rodian                        44
11 Trandoshan                    53
12 Twi'lek                       48
13 Wookiee                      200
14 Yoda's species               896
15 Zabrak                        54
```

## Task 2

1. What are the most populated planets?

```R
> starwars %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 49 x 2
   homeworld     n
   <chr>     <int>
 1 Naboo        11
 2 Tatooine     10
 3 NA           10
 4 Alderaan      3
 5 Coruscant     3
 6 Kamino        3
 7 Corellia      2
 8 Kashyyyk      2
 9 Mirial        2
10 Ryloth        2
# … with 39 more rows
```

2. What is the average height of all female humans?

```R
> starwars %>%
+     select(name, height, gender, species) %>%
+     filter(species == 'Human', gender == 'feminine') %>%
+     na.omit() %>%
+     summarise(mean(height))
# A tibble: 1 x 1
  `mean(height)`
           <dbl>
1           160.
```

3. Which planet has the most droids?

```R
> starwars %>%
+     filter(species == 'Droid') %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 3 x 2
  homeworld     n
  <chr>     <int>
1 NA            3
2 Tatooine      2
3 Naboo         1
```

4. Who is the oldest character of each species?

```R
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species, name) %>%
+     summarise(by = max(birth_year)) %>%
+     group_by(species) %>%
+     slice(which.max(by))
`summarise()` has grouped output by 'species'. You can override using the `.groups` argument.
# A tibble: 15 x 3
# Groups:   species [15]
   species        name                     by
   <chr>          <chr>                 <dbl>
 1 Cerean         Ki-Adi-Mundi             92
 2 Droid          C-3PO                   112
 3 Ewok           Wicket Systri Warrick     8
 4 Gungan         Jar Jar Binks            52
 5 Human          Dooku                   102
 6 Hutt           Jabba Desilijic Tiure   600
 7 Kel Dor        Plo Koon                 22
 8 Mirialan       Luminara Unduli          58
 9 Mon Calamari   Ackbar                   41
10 Rodian         Greedo                   44
11 Trandoshan     Bossk                    53
12 Twi'lek        Ayla Secura              48
13 Wookiee        Chewbacca               200
14 Yoda's species Yoda                    896
15 Zabrak         Darth Maul               54
```

5. Which is the most prevalent eye color on each planet?

```R
> starwars %>%
+       group_by(homeworld, eye_color) %>%
+       mutate(rank = row_number()) %>%
+       group_by(homeworld) %>%
+       slice(which.max(rank)) %>%
+       summarise(homeworld, eye_color, rank)
# A tibble: 49 x 3
   homeworld      eye_color  rank
   <chr>          <chr>     <int>
 1 Alderaan       brown         3
 2 Aleen Minor    unknown       1
 3 Bespin         blue          1
 4 Bestine IV     blue          1
 5 Cato Neimoidia red           1
 6 Cerea          yellow        1
 7 Champala       blue          1
 8 Chandrila      blue          1
 9 Concord Dawn   brown         1
10 Corellia       brown         1
```

6. How many unique eye colors are there?

```R
> starwars %>%
+       summarise(eye_color) %>%
+       distinct(eye_color)
# A tibble: 15 x 1
   eye_color
   <chr>
 1 blue
 2 yellow
 3 red
 4 brown
 5 blue-gray
 6 black
 7 orange
 8 hazel
 9 pink
10 unknown
11 red, blue
12 gold
13 green, yellow
14 white
15 dark
```

[Let's continue with exercise 2!](./EXERCISE-2.md)
