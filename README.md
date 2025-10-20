
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.2
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)
```

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers. (readr)

``` r
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
glimpse(av)
```

    ## Rows: 173
    ## Columns: 21
    ## $ URL                         <chr> "http://marvel.wikia.com/Henry_Pym_(Earth-…
    ## $ Name.Alias                  <chr> "Henry Jonathan \"Hank\" Pym", "Janet van …
    ## $ Appearances                 <int> 1269, 1165, 3068, 2089, 2402, 612, 3458, 1…
    ## $ Current.                    <chr> "YES", "YES", "YES", "YES", "YES", "YES", …
    ## $ Gender                      <chr> "MALE", "FEMALE", "MALE", "MALE", "MALE", …
    ## $ Probationary.Introl         <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Full.Reserve.Avengers.Intro <chr> "Sep-63", "Sep-63", "Sep-63", "Sep-63", "S…
    ## $ Year                        <int> 1963, 1963, 1963, 1963, 1963, 1963, 1964, …
    ## $ Years.since.joining         <int> 52, 52, 52, 52, 52, 52, 51, 50, 50, 50, 50…
    ## $ Honorary                    <chr> "Full", "Full", "Full", "Full", "Full", "H…
    ## $ Death1                      <chr> "YES", "YES", "YES", "YES", "YES", "NO", "…
    ## $ Return1                     <chr> "NO", "YES", "YES", "YES", "YES", "", "YES…
    ## $ Death2                      <chr> "", "", "", "", "YES", "", "", "YES", "", …
    ## $ Return2                     <chr> "", "", "", "", "NO", "", "", "YES", "", "…
    ## $ Death3                      <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Return3                     <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Death4                      <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Return4                     <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Death5                      <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Return5                     <chr> "", "", "", "", "", "", "", "", "", "", ""…
    ## $ Notes                       <chr> "Merged with Ultron in Rage of Ultron Vol.…

``` r
death_cols <- names(av)[str_detect(names(av), regex("^death", ignore_case = TRUE))]

deaths <- av %>%
  select(Name.Alias, all_of(death_cols)) %>%
  pivot_longer(cols = all_of(death_cols),
               names_to = "Death_Time",
               values_to = "Death") %>%
  mutate(Time = parse_number(Death_Time)) %>%
  select(Name.Alias, Time, Death) %>%
  filter(Death != "")

head(deaths)
```

    ## # A tibble: 6 × 3
    ##   Name.Alias                       Time Death
    ##   <chr>                           <dbl> <chr>
    ## 1 "Henry Jonathan \"Hank\" Pym"       1 YES  
    ## 2 "Janet van Dyne"                    1 YES  
    ## 3 "Anthony Edward \"Tony\" Stark"     1 YES  
    ## 4 "Robert Bruce Banner"               1 YES  
    ## 5 "Thor Odinson"                      1 YES  
    ## 6 "Thor Odinson"                      2 YES

``` r
return_cols <- names(av)[str_detect(names(av), regex("^return", ignore_case = TRUE))]

returns <- av %>%
  select(Name.Alias, all_of(return_cols)) %>%
  pivot_longer(cols = all_of(return_cols),
               names_to = "Return_Time",
               values_to = "Return") %>%
  mutate(Time = parse_number(Return_Time)) %>%
  select(Name.Alias, Time, Return) %>%
  filter(Return != "")

head(returns)
```

    ## # A tibble: 6 × 3
    ##   Name.Alias                       Time Return
    ##   <chr>                           <dbl> <chr> 
    ## 1 "Henry Jonathan \"Hank\" Pym"       1 NO    
    ## 2 "Janet van Dyne"                    1 YES   
    ## 3 "Anthony Edward \"Tony\" Stark"     1 YES   
    ## 4 "Robert Bruce Banner"               1 YES   
    ## 5 "Thor Odinson"                      1 YES   
    ## 6 "Thor Odinson"                      2 NO

``` r
deaths_summary <- deaths %>%
  group_by(Name.Alias) %>%
  summarize(total_deaths = sum(Death == "YES", na.rm = TRUE))

avg_deaths <- mean(deaths_summary$total_deaths)
cat("Average number of deaths per Avenger:", avg_deaths, "\n")
```

    ## Average number of deaths per Avenger: 0.5460123

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

# Maya’s Analysis

### FiveThirtyEight Statement I’m Using: “Of the nine Avengers we see on screen — Iron Man, Hulk, Captain America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver and The Vision — every single one of them has died at least once in the comics. In fact, Hawkeye died twice!”

### My Code (Maya):

``` r
library(dplyr)
library(tidyr)
library(stringr)
library(tibble)
library(purrr)

#Identify columns
death_cols <- names(av)[str_detect(names(av), regex("^death\\d+$", ignore_case = TRUE))]
stopifnot(length(death_cols) > 0)


is_death_yes <- function(x) {
  y <- tolower(str_trim(as.character(x)))
  case_when(
    is.na(y) ~ FALSE,
    y %in% c("yes","y","true","1") ~ TRUE,
    str_detect(y, "deceas") ~ TRUE,   # "deceased"
    TRUE ~ FALSE
  )
}

#long deaths table straight from `av`
deaths_long <- av %>%
  select(Name.Alias, all_of(death_cols)) %>%
  pivot_longer(all_of(death_cols), names_to = "Time", values_to = "DeathRaw") %>%
  mutate(DeathYes = is_death_yes(DeathRaw))

#count deaths per alias row
deaths_by_alias <- deaths_long %>%
  group_by(Name.Alias) %>%
  summarise(total_deaths = sum(DeathYes, na.rm = TRUE), .groups = "drop") %>%
  mutate(alias_lc = tolower(Name.Alias))

#MCU hero, include codename + common civilian names 
#(using multiple simple "contains" terms per hero)
mcu_terms <- tribble(
  ~hero,             ~terms,
  "Iron Man",        c("iron man","tony stark","anthony stark"),
  "Hulk",            c("hulk","bruce banner","robert bruce banner"),
  "Captain America", c("captain america","steve rogers","steven rogers"),
  "Thor",            c("thor"),
  "Hawkeye",         c("hawkeye","clint barton"),
  "Black Widow",     c("black widow","natasha romanoff","natalia romanova"),
  "Scarlet Witch",   c("scarlet witch","wanda maximoff"),
  "Quicksilver",     c("quicksilver","pietro maximoff"),
  "Vision",          c("vision","the vision","victor shade")
)

# helper
contains_any <- function(text_vec, term_vec) {
  txt <- tolower(text_vec)
  map_lgl(txt, ~ any(str_detect(.x, fixed(term_vec, ignore_case = TRUE))))
}

#see which aliases matched for transparency
matched_aliases <- mcu_terms %>%
  mutate(
    aliases = map(terms, ~ deaths_by_alias %>%
                    filter(contains_any(alias_lc, .x)) %>%
                    arrange(alias_lc) %>%
                    pull(Name.Alias))
  )

# print matches
print(matched_aliases %>% select(hero, aliases))
```

    ## # A tibble: 9 × 2
    ##   hero            aliases  
    ##   <chr>           <list>   
    ## 1 Iron Man        <chr [0]>
    ## 2 Hulk            <chr [1]>
    ## 3 Captain America <chr [1]>
    ## 4 Thor            <chr [1]>
    ## 5 Hawkeye         <chr [0]>
    ## 6 Black Widow     <chr [0]>
    ## 7 Scarlet Witch   <chr [1]>
    ## 8 Quicksilver     <chr [1]>
    ## 9 Vision          <chr [1]>

``` r
#sum deaths for each hero across ALL matching alias rows
hero_deaths <- mcu_terms %>%
  mutate(
    total_deaths = map_dbl(terms, ~ deaths_by_alias %>%
                             filter(contains_any(alias_lc, .x)) %>%
                             summarise(n = sum(total_deaths, na.rm = TRUE)) %>%
                             pull(n) %>% { if(length(.)==0) 0 else . })
  ) %>%
  mutate(died_at_least_once = total_deaths >= 1)


all_nine_died  <- all(hero_deaths$died_at_least_once)
hawkeye_deaths <- hero_deaths %>% filter(hero == "Hawkeye") %>% pull(total_deaths)

print(hero_deaths %>% select(hero, total_deaths, died_at_least_once))
```

    ## # A tibble: 9 × 3
    ##   hero            total_deaths died_at_least_once
    ##   <chr>                  <dbl> <lgl>             
    ## 1 Iron Man                   0 FALSE             
    ## 2 Hulk                       1 TRUE              
    ## 3 Captain America            1 TRUE              
    ## 4 Thor                       2 TRUE              
    ## 5 Hawkeye                    0 FALSE             
    ## 6 Black Widow                0 FALSE             
    ## 7 Scarlet Witch              1 TRUE              
    ## 8 Quicksilver                1 TRUE              
    ## 9 Vision                     1 TRUE

``` r
cat("\n----- Summary (Maya) -----\n")
```

    ## 
    ## ----- Summary (Maya) -----

``` r
cat("All nine MCU Avengers died at least once? ", all_nine_died, "\n", sep = "")
```

    ## All nine MCU Avengers died at least once? FALSE

``` r
cat("Hawkeye total recorded deaths: ", hawkeye_deaths, "\n", sep = "")
```

    ## Hawkeye total recorded deaths: 0

``` r
died_heroes <- hero_deaths %>% filter(died_at_least_once) %>% pull(hero)
not_died    <- hero_deaths %>% filter(!died_at_least_once) %>% pull(hero)

cat("Died at least once:", paste(died_heroes, collapse = ", "), "\n")
```

    ## Died at least once: Hulk, Captain America, Thor, Scarlet Witch, Quicksilver, Vision

``` r
cat("No recorded deaths:", paste(not_died, collapse = ", "), "\n")
```

    ## No recorded deaths: Iron Man, Hawkeye, Black Widow

### My Conclusion (Maya)

After checking the data from the Avengers dataset, I found that the
statement isn’t fully supported. Out of the nine MCU Avengers listed,
only six — Hulk, Captain America, Thor, Scarlet Witch, Quicksilver, and
Vision — have at least one recorded death. Iron Man, Hawkeye, and Black
Widow don’t have any deaths shown in this dataset. The claim that
“Hawkeye died twice” also isn’t supported since the data records zero
deaths for him. This difference is likely because the dataset doesn’t
include every comic storyline or might list characters by their civilian
names, which can make it harder to match. Overall, while the article
says all nine have died at least once, the dataset we used doesn’t fully
back that up.

# Deesha’s Analysis

# Statement

Most Avengers die at least once”

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.x

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Deesha’s code

``` r
num_heroes <- nrow(deaths_summary)
num_with_death <- sum(deaths_summary$total_deaths >= 1)

prop_with_death <- num_with_death / num_heroes

cat("----- Deesha's Fact Check -----\n")
```

    ## ----- Deesha's Fact Check -----

``` r
cat("Total Avengers:", num_heroes, "\n")
```

    ## Total Avengers: 163

``` r
cat("Avengers who died at least once:", num_with_death, "\n")
```

    ## Avengers who died at least once: 64

``` r
cat("Proportion who died:", round(prop_with_death * 100, 1), "%\n")
```

    ## Proportion who died: 39.3 %

``` r
if (prop_with_death > 0.5) {
  cat("Conclusion: Supported — most Avengers die at least once.\n")
} else {
  cat("Conclusion: Not supported — less than half die at least once.\n")
}
```

    ## Conclusion: Not supported — less than half die at least once.

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

### Deesha’s answer

Based on the data, out of 163 Avengers in the dataset, 64 have died at
least once.  
That’s roughly 39.3% of all Avengers.  
Therefore, the FiveThirtyEight statement *“Most Avengers die at least
once”* is **not supported** by the dataset — fewer than half of all
Avengers have experienced death.

# Logans Anaylsis

### Statement:

“I counted 89 total deaths — some unlucky Avengers are basically Meat
Loaf with an E-ZPass.”

### Logans Code

``` r
total_deaths <- sum(deaths$Death == "YES", na.rm = TRUE)
total_deaths
```

    ## [1] 89

### Logans Conclusion

There are in fact 89 recorded deaths across all Avengers in the dataset
after checking. FiveThirtyEight did say 89 so he was correct after our
calcuations.

# Som’s Analysis

### FiveThirtyEight Statement

“On average, an Avenger dies about once.”

### What I checked (method)

Use the long `deaths` table to count deaths per character, then take the
mean across Avengers. Compare to ~1.

``` r
# If for any reason deaths_summary isn't in the environment, rebuild it safely
if (!exists("deaths_summary")) {
  deaths_summary <- deaths %>%
    dplyr::group_by(Name.Alias) %>%
    dplyr::summarize(total_deaths = sum(Death == "YES", na.rm = TRUE), .groups = "drop")
}

som_avg_deaths <- mean(deaths_summary$total_deaths, na.rm = TRUE)
som_n          <- nrow(deaths_summary)

cat("Avengers counted:", som_n, "\n")
```

    ## Avengers counted: 163

``` r
cat("Average deaths per Avenger:", round(som_avg_deaths, 3), "\n")
```

    ## Average deaths per Avenger: 0.546

``` r
# Simple verdict relative to ~1 death on average
tol <- 0.15  # tolerance band; adjust if your instructor prefers a different band
if (abs(som_avg_deaths - 1) <= tol) {
  cat("Conclusion: Supported (≈ 1 death on average within tolerance).\n")
} else {
  cat("Conclusion: Not supported (mean not close to 1 within tolerance).\n")
}
```

    ## Conclusion: Not supported (mean not close to 1 within tolerance).

### Som’s Conclusion

Using the long-form deaths table, the average deaths per Avenger is **r
round(som_avg_deaths, 3)** across **r som_n** Avengers. This does
**not** support the claim that “on average, an Avenger dies about once,”
given our estimate is well below 1. This mean is consistent with our
teammate’s count of total deaths: r round(som_avg_deaths, 3) × r som_n ≈
**r round(som_avg_deaths \* som_n)** total deaths, matching Logan’s
**89**.
