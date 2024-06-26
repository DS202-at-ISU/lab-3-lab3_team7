
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
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

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

``` r
#gavins<-tally(group_by(av,Death1,Death2,Death3,Death4,Death5, sort = FALSE))

deaths <- av %>% pivot_longer(cols = starts_with("Death"), names_to = "Time", values_to = "Death")
head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <chr>,
    ## #   Death <chr>

``` r
#deaths$Time <- parse_number(deaths$Time)
#head(deaths)

gavin4<-deaths%>%group_by(Name.Alias, URL)%>%count(Death)

gavin5 = filter(gavin4, Death == "YES")
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

``` r
#doing the same thing we did for deaths with returns now

returns <- av %>% pivot_longer(cols = starts_with("Return"), names_to = "Time2", values_to = "return")
head(returns)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time2 <chr>,
    ## #   return <chr>

``` r
#deaths$Time <- parse_number(deaths$Time)
#head(deaths)

gavin6<-returns%>%group_by(Name.Alias, URL)%>%count(return)

returns = filter(gavin6, return == "YES")



# merging our two dataframes into one

combined<- merge(gavin5,gavin6, by=c("Name.Alias", "URL"))
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
deaths2 <- filter(deaths, deaths$Death == "YES")
deaths2$Time <- parse_number(deaths2$Time)
head(deaths2)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## 3 http://marvel.wiki… "Anthony …        3068 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Robert B…        2089 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

``` r
mean(deaths2$Time, na.rm = TRUE)
```

    ## [1] 1.303371

**The mean number of deaths an Avenger suffers is 1.3.**

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

## Individually - Emily Maruska

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> “There’s a 2-in-3 chance that a member of the Avengers returned from
> their first stint in the afterlife”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
dead <- av %>% pivot_longer(cols = starts_with("Death"), names_to = "Time", values_to = "Death")
dead$Time <- parse_number(dead$Time)

numReturned <- dead %>% filter(dead$Return1 == "YES")
nrow(numReturned)
```

    ## [1] 230

``` r
totalDied <- dead %>% filter(dead$Return1 != "")
nrow(totalDied)
```

    ## [1] 345

``` r
chanceToReturnFirstTime <- nrow(numReturned) / nrow(totalDied)
chanceToReturnFirstTime
```

    ## [1] 0.6666667

### Include your answer

**The data in the article is really accurate considering I counted all
the avengers that returned the first time, then counted all the avengers
regardless of if they returned or died and took the number returned over
the total died and got 0.6666 which translates directly to a “2-in-3
chance”**

## Individually - Gavin Anderson

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> “Of the nine Avengers we see on screen — Iron Man, Hulk, Captain
> America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver and
> The Vision — every single one of them has died at least once in the
> course of their time Avenging in the comics. In fact, Hawkeye died
> twice!”

### Include the code

``` r
combined2<-combined


#made a dataframe with only mcu avengers included
MCU_only<-av[c(3,4,5,7,8,9,16,10,14),-c(4:10)]

head(MCU_only)
```

    ##                                                       URL
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 7       http://marvel.wikia.com/Steven_Rogers_(Earth-616)
    ## 8        http://marvel.wikia.com/Clint_Barton_(Earth-616)
    ## 9     http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)
    ##                    Name.Alias Appearances Death1 Return1 Death2 Return2 Death3
    ## 3 Anthony Edward "Tony" Stark        3068    YES     YES                      
    ## 4         Robert Bruce Banner        2089    YES     YES                      
    ## 5                Thor Odinson        2402    YES     YES    YES      NO       
    ## 7               Steven Rogers        3458    YES     YES                      
    ## 8      Clinton Francis Barton        1456    YES     YES    YES     YES       
    ## 9             Pietro Maximoff         769    YES     YES                      
    ##   Return3 Death4 Return4 Death5 Return5
    ## 3                                      
    ## 4                                      
    ## 5                                      
    ## 7                                      
    ## 8                                      
    ## 9                                      
    ##                                                                                                                                                                              Notes
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 7                                                                                                                                 Dies at the end of Civil War. Later comes back. 
    ## 8                       Dies in exploding Kree ship in Averngers Vol. 1  Issue 502. Brought back by Scarlet Witch. Dies again in House of M Vol 1 Issue 7. Is later brought back. 
    ## 9                                                                                                                               Dies in House of M Vol 1 Issue 7. Later comes back

``` r
#and verified that every single current mcu character died at least once 
```

### Include your answer

Looking at the data frame “MCU_only” we can see that all of the onscreen
Avengers have indeed died at least once.

## Individually - Carolyn Jones

### FiveThirtyEight Statement

> “Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team. That’s about 40 percent”

### Include the code

``` r
deaths$Time <- parse_number(deaths$Time)
deaths <- filter(deaths, deaths$Death == "YES" & deaths$Time == 1)
nrow(deaths) # 69
```

    ## [1] 69

``` r
69/173 # percentage of deaths
```

    ## [1] 0.3988439

### Include your answer

After filtering the deaths dataset to only contain avengers who died at
least 1 time, the number of remaining avengers was 69, which is accurate
to what the article said. I also divided this number by the total number
of avengers to confirm that it was about 40%

## Individually - John Nesnidal

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> “Given the Avengers’ 53 years in operation and overall mortality rate,
> fans of the comics can expect one current or former member to die
> every seven months or so, with a permanent death occurring once every
> 20 months.”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
totalDeaths <- nrow(deaths2)
print(totalDeaths)
```

    ## [1] 89

``` r
monthsPerDeath <- (53 * 12) / totalDeaths
print(monthsPerDeath)
```

    ## [1] 7.146067

``` r
# Months per death: 7.1

deaths3 <- deaths2 %>%
  filter(Return1 == "NO" | Return2 == "NO" | Return3 == "NO" |
           Return4 == "NO" | Return5 == "NO") %>%
  group_by(URL) %>%
  summarise(
    numDeaths = max(Time)
  )

totalPermDeaths <- nrow(deaths3)
print(totalPermDeaths)
```

    ## [1] 32

``` r
monthsPerPermDeath <- (53 * 12) / totalPermDeaths
print(monthsPerPermDeath)
```

    ## [1] 19.875

``` r
# Months per permanent death: 19.9
```

### Include your answer

**The data in the article is accurate, rounded to the nearest whole
number. For the first statement (one death every 7 months), I took 53 \*
12 and divided it by the number of rows in the deaths table. For the
second statement (one permanent death every 20 months), I took 53 \* 12
and divided it by the number of individual Avengers who had a NO
reported in any of the return columns. These numbers come out to 7.1
months per death and 19.9 months per permanent death.**
