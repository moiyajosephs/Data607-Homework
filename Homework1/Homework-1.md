607 Homework 1
================
Moiya Josephs
2/3/2022

## Load libraries

``` r
library(ggplot2)
library(curl)
```

    ## Using libcurl 7.64.1 with Schannel

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.1.2     v forcats 0.5.1
    ## v purrr   0.3.4

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter()     masks stats::filter()
    ## x dplyr::lag()        masks stats::lag()
    ## x readr::parse_date() masks curl::parse_date()

# Introduction

The article [*“Joining the Avengers Is As Deadly As Jumping Off A
Four-Story Building”* by Walt
Hickey](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
describes how characters in the popular Marvel comic have experienced
death at a rate comparable to jumping off of a four-story building.
According to the article, there is a 50% death rate if you jump off a
building four stories high. Hickey claims they found that if a character
joins the Avengers, they have a death rate of 40%. Since these are comic
book characters, and characters tend to be resurrected, the data set
also included each resurrection and death for the character. It capped
the number of deaths at 5. The author mentions a criterion for what does
not count as a death, such as if a character fakes their demise. To be
counted as a death, the death had to have been shown in the comics, or
other characters had to sincerely believe that the character is dead. By
these standards, Hickey compiled a CSV file that I will be using for
this assignment.

The source of the article is included under the References header.

## Import file as CSV

``` r
avengers_dataset_url <- "https://raw.githubusercontent.com/moiyajosephs/Data607-Homework/main/Homework1/avengers.csv"
avengers <- read_csv(curl(avengers_dataset_url))
```

    ## Rows: 173 Columns: 21

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (18): URL, Name/Alias, Current?, Gender, Probationary Introl, Full/Reser...
    ## dbl  (3): Appearances, Year, Years since joining

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(avengers)
```

    ## # A tibble: 6 x 21
    ##   URL            `Name/Alias`     Appearances `Current?` Gender `Probationary I~
    ##   <chr>          <chr>                  <dbl> <chr>      <chr>  <chr>           
    ## 1 http://marvel~ "Henry Jonathan~        1269 YES        MALE   <NA>            
    ## 2 http://marvel~ "Janet van Dyne"        1165 YES        FEMALE <NA>            
    ## 3 http://marvel~ "Anthony Edward~        3068 YES        MALE   <NA>            
    ## 4 http://marvel~ "Robert Bruce B~        2089 YES        MALE   <NA>            
    ## 5 http://marvel~ "Thor Odinson"          2402 YES        MALE   <NA>            
    ## 6 http://marvel~ "Richard Milhou~         612 YES        MALE   <NA>            
    ## # ... with 15 more variables: Full/Reserve Avengers Intro <chr>, Year <dbl>,
    ## #   Years since joining <dbl>, Honorary <chr>, Death1 <chr>, Return1 <chr>,
    ## #   Death2 <chr>, Return2 <chr>, Death3 <chr>, Return3 <chr>, Death4 <chr>,
    ## #   Return4 <chr>, Death5 <chr>, Return5 <chr>, Notes <chr>

## Subset the data

The avengers data set has 21 columns, some not necessary to view if
joining the Avengers is as deadly as falling off a four story building.
I selected the characters’ name, if they are a current member, the type
of member they are, Honorary/Full member and if they died. I also
renamed the column “Current?” to “Current Member” and “Honorary” to
“Honorary Member” to make the data set more understandable.

``` r
avengers_subset <- avengers[c(2,4,7,10:11,13,15,17,19)]
names(avengers_subset)[names(avengers_subset) == 'Current?'] <- 'Current Member'
names(avengers_subset)[names(avengers_subset) == 'Honorary'] <- 'Honorary Member'

head(avengers_subset)
```

    ## # A tibble: 6 x 9
    ##   `Name/Alias`  `Current Member` `Full/Reserve A~ `Honorary Membe~ Death1 Death2
    ##   <chr>         <chr>            <chr>            <chr>            <chr>  <chr> 
    ## 1 "Henry Jonat~ YES              Sep-63           Full             YES    <NA>  
    ## 2 "Janet van D~ YES              Sep-63           Full             YES    <NA>  
    ## 3 "Anthony Edw~ YES              Sep-63           Full             YES    <NA>  
    ## 4 "Robert Bruc~ YES              Sep-63           Full             YES    <NA>  
    ## 5 "Thor Odinso~ YES              Sep-63           Full             YES    YES   
    ## 6 "Richard Mil~ YES              Sep-63           Honorary         NO     <NA>  
    ## # ... with 3 more variables: Death3 <chr>, Death4 <chr>, Death5 <chr>

## How many characters are current Avengers?

First I made a subset of only current members. Here I can see which
characters are members of the Avengers, whether or not they are full
members or honorary and if they have died at least once.

``` r
current_avengers <- subset(avengers_subset, `Current Member` == "YES")
current_avengers
```

    ## # A tibble: 82 x 9
    ##    `Name/Alias` `Current Member` `Full/Reserve A~ `Honorary Membe~ Death1 Death2
    ##    <chr>        <chr>            <chr>            <chr>            <chr>  <chr> 
    ##  1 "Henry Jona~ YES              Sep-63           Full             YES    <NA>  
    ##  2 "Janet van ~ YES              Sep-63           Full             YES    <NA>  
    ##  3 "Anthony Ed~ YES              Sep-63           Full             YES    <NA>  
    ##  4 "Robert Bru~ YES              Sep-63           Full             YES    <NA>  
    ##  5 "Thor Odins~ YES              Sep-63           Full             YES    YES   
    ##  6 "Richard Mi~ YES              Sep-63           Honorary         NO     <NA>  
    ##  7 "Steven Rog~ YES              Mar-64           Full             YES    <NA>  
    ##  8 "Clinton Fr~ YES              May-65           Full             YES    YES   
    ##  9 "Pietro Max~ YES              May-65           Full             YES    <NA>  
    ## 10 "Wanda Maxi~ YES              May-65           Full             YES    <NA>  
    ## # ... with 72 more rows, and 3 more variables: Death3 <chr>, Death4 <chr>,
    ## #   Death5 <chr>

According to the subset and the amount of rows there are 82 current
members, who are either full or honorary.

## How many times has a given character died?

I wanted to total the amount of times an individual character has died.
Since they can die and come back to life multiple times I had to combine
five rows into one sum. First I changed the character value of YES to 1
or 0 is NO so that I can better aggregate the data.

``` r
current_avengers$Death1<-ifelse(!is.na(current_avengers$Death1) & current_avengers$Death1=="YES",1,0)
current_avengers$Death2<-ifelse(!is.na(current_avengers$Death2) & current_avengers$Death2=="YES",1,0)
current_avengers$Death3<-ifelse(!is.na(current_avengers$Death3) & current_avengers$Death3=="YES",1,0)
current_avengers$Death4<-ifelse(!is.na(current_avengers$Death4) & current_avengers$Death4=="YES",1,0)
current_avengers$Death5<-ifelse(!is.na(current_avengers$Death5) & current_avengers$Death5=="YES",1,0)

current_avengers
```

    ## # A tibble: 82 x 9
    ##    `Name/Alias` `Current Member` `Full/Reserve A~ `Honorary Membe~ Death1 Death2
    ##    <chr>        <chr>            <chr>            <chr>             <dbl>  <dbl>
    ##  1 "Henry Jona~ YES              Sep-63           Full                  1      0
    ##  2 "Janet van ~ YES              Sep-63           Full                  1      0
    ##  3 "Anthony Ed~ YES              Sep-63           Full                  1      0
    ##  4 "Robert Bru~ YES              Sep-63           Full                  1      0
    ##  5 "Thor Odins~ YES              Sep-63           Full                  1      1
    ##  6 "Richard Mi~ YES              Sep-63           Honorary              0      0
    ##  7 "Steven Rog~ YES              Mar-64           Full                  1      0
    ##  8 "Clinton Fr~ YES              May-65           Full                  1      1
    ##  9 "Pietro Max~ YES              May-65           Full                  1      0
    ## 10 "Wanda Maxi~ YES              May-65           Full                  1      0
    ## # ... with 72 more rows, and 3 more variables: Death3 <dbl>, Death4 <dbl>,
    ## #   Death5 <dbl>

Next I used the **mutate** function from **dyplr** to add an extra row
showing the total amount of times a character has died.

``` r
current_avengers <- current_avengers %>% mutate(sum = rowSums(.[5:9]))
```

``` r
head(current_avengers)
```

    ## # A tibble: 6 x 10
    ##   `Name/Alias`  `Current Member` `Full/Reserve A~ `Honorary Membe~ Death1 Death2
    ##   <chr>         <chr>            <chr>            <chr>             <dbl>  <dbl>
    ## 1 "Henry Jonat~ YES              Sep-63           Full                  1      0
    ## 2 "Janet van D~ YES              Sep-63           Full                  1      0
    ## 3 "Anthony Edw~ YES              Sep-63           Full                  1      0
    ## 4 "Robert Bruc~ YES              Sep-63           Full                  1      0
    ## 5 "Thor Odinso~ YES              Sep-63           Full                  1      1
    ## 6 "Richard Mil~ YES              Sep-63           Honorary              0      0
    ## # ... with 4 more variables: Death3 <dbl>, Death4 <dbl>, Death5 <dbl>,
    ## #   sum <dbl>

## Which Avenger died the most times?

To find the maximum amount of times a given character died I used the
sum function on my new column, “sum”.

``` r
max(current_avengers$sum)
```

    ## [1] 5

There is a character that has died five times, the maximum amount a
character can achieve in this data set. This is interesting but it would
be nice to know which character this is.

``` r
current_avengers %>% slice_max(current_avengers$sum)
```

    ## # A tibble: 1 x 10
    ##   `Name/Alias` `Current Member` `Full/Reserve Av~ `Honorary Membe~ Death1 Death2
    ##   <chr>        <chr>            <chr>             <chr>             <dbl>  <dbl>
    ## 1 Jocasta      YES              Nov-88            Full                  1      1
    ## # ... with 4 more variables: Death3 <dbl>, Death4 <dbl>, Death5 <dbl>,
    ## #   sum <dbl>

According to the **slice_max** function the charter that has died the
most is Jocasta. This character was also mentioned in the article as the
character with the most amount of deaths.

# Conclusions

Through data transformation I found that there were 82 current members
of the Avengers. Out of this subset, Jocasta had the highest amount of
deaths out of any character. To further verify this data I would use a
data set or study that verifies the articles statistic of falling from a
four story building is 50% death rate. Then I would use this data set
and find the actual percentage from the sum of characters who were
Avengers vs those who were not Avengers. To extend this article and see
if this trend continues, it would be interesting to add any current
members that have been added from 2015 to now.

# References

[Joining The Avengers Is As Deadly As Jumping Off A Four-Story
Building](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
