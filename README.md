
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
    ## ✔ forcats   1.0.1     ✔ stringr   1.5.1
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

# 1️⃣ Deesha’s Analysi

# Statement: “Most Avengers die at least once.”

``` r
num_heroes <- nrow(deaths_summary)
num_with_death <- sum(deaths_summary$total_deaths >= 1)
prop_with_death <- num_with_death / num_heroes

cat("\n----- Deesha -----\n")
```

    ## 
    ## ----- Deesha -----

``` r
cat("Number of Avengers:", num_heroes, "\n")
```

    ## Number of Avengers: 163

``` r
cat("Avengers who died at least once:", num_with_death, "\n")
```

    ## Avengers who died at least once: 64

``` r
cat("Proportion who died:", round(prop_with_death, 3), "\n")
```

    ## Proportion who died: 0.393

``` r
if (prop_with_death > 0.5) {
  cat("Conclusion: Supported — most Avengers die at least once.\n")
} else {
  cat("Conclusion: Not supported — less than half die at least once.\n")
}
```

    ## Conclusion: Not supported — less than half die at least once.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Deesha’s quote

“Most Avengers die at least once.”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Deesha’s code

``` r
num_heroes <- nrow(deaths_summary)
num_with_death <- sum(deaths_summary$total_deaths >= 1)
prop_with_death <- num_with_death / num_heroes

num_heroes
```

    ## [1] 163

``` r
num_with_death
```

    ## [1] 64

``` r
prop_with_death
```

    ## [1] 0.392638

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

### Deesha’s answer

There are r num_heroes Avengers in the dataset, and r num_with_death of
them have died at least once. That means about r round(prop_with_death
\* 100, 1) % of all Avengers have experienced death.

Upload your changes to the repository. Discuss and refine answers as a
team.
