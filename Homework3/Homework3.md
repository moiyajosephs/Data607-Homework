Homework 3
================
Moiya Josephs
2/17/2022

``` r
library(stringr)
```

# Question 1

Using the 173 majors listed in fivethirtyeight.com’s College Majors
dataset
\[<https://fivethirtyeight.com/features/the-economic-guide-to-picking-a-college-major/>\],
provide code that identifies the majors that contain either “DATA” or
“STATISTICS”

``` r
college_major <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/college-majors/majors-list.csv", header = TRUE)

college_major$Major %>% str_subset("DATA|STATISTICS")
```

    ## [1] "MANAGEMENT INFORMATION SYSTEMS AND STATISTICS"
    ## [2] "COMPUTER PROGRAMMING AND DATA PROCESSING"     
    ## [3] "STATISTICS AND DECISION SCIENCE"

# Question 2

Write code that transforms the data below:

``` r
character <- '[1] "bell pepper"  "bilberry"     "blackberry"   "blood orange"
[5] "blueberry"    "cantaloupe"   "chili pepper" "cloudberry"  
[9] "elderberry"   "lime"         "lychee"       "mulberry"    
[13] "olive"        "salal berry"'
```

``` r
#c2<- str_replace_all(character2, regex("\\s{2,}|\\[[1-9]\\]"), " ")
# c2<- str_replace_all(character, regex("\\[[1-9]\\]"), " ")
# c2<- str_replace_all(c2, regex("\\s{2,}"), ",")
# c2<- str_replace_all(c2,fixed(""),"")

c2<- unlist(str_extract_all(character,'[[A-Za-z]]+\\s[[A-Za-z]]+|[[A-Za-z]]+'))
c3<- dput(as.character(c2))
```

    ## c("bell pepper", "bilberry", "blackberry", "blood orange", "blueberry", 
    ## "cantaloupe", "chili pepper", "cloudberry", "elderberry", "lime", 
    ## "lychee", "mulberry", "olive", "salal berry")

``` r
c3
```

    ##  [1] "bell pepper"  "bilberry"     "blackberry"   "blood orange" "blueberry"   
    ##  [6] "cantaloupe"   "chili pepper" "cloudberry"   "elderberry"   "lime"        
    ## [11] "lychee"       "mulberry"     "olive"        "salal berry"

# Question 3 Describe, in words, what these expressions will match:

For R an additional backslash needs to be added. - “(.)\\1\\1” -
“(.)(.)\\2\\1”  
- (..)\\1  
- “(.).\\1.\\1”  
- “(.)(.)(.).\*\\3\\2\\1”

### A

Will match any character because of the . in the capturing group (except
for line terminators) at least three times.Once because of the capturing
group. The second and third string is because of the \\1 repeated twice.

``` r
p1<- "www"
str_detect(p1,"(.)\\1\\1")
```

    ## [1] TRUE

### B

Both (.) will match any two characters each belonging to one capturing
group. \\ Escapes the backslash so it would be included in the string as
well. 2 and 1 since it follows an escaped backslash will be required in
the string to match the regex.

Some examples include:

``` r
# original regex - "(.)(.)\\2\\1"  Added extra \ because of escapping
p2<- "weew"

str_detect(p2,"(.)(.)\\2\\1")
```

    ## [1] TRUE

As long as it has two chatacters a backslash then a 2 and then a
backslash and 1 it will match the regex.

### C

Since there are two periods in the capturing group this regex will match
with any two characters that repeat at least once. (Since there is no
termination specified). Some examples wwww wwwwww

``` r
p3<- "wwww"
str_detect(p3,"(..)\\1")
```

    ## [1] TRUE

### D

“(.).\\1.\\1”

(.) means any character. The . also means any character. The double
backslash 1 means the original character should be repeated. The etra
.means any character and the 1 refers to the original character.

``` r
p4<- "apa.a"
str_detect(p4,"(.).\\1.\\1")
```

    ## [1] TRUE

### E

“(.)(.)(.).\*\\3\\2\\1” This regex expression indicated the string must
have three of any character, with any other characters afterwards as
long as it ends with the original 3 backwards.

``` r
p4<- "abccba"
str_detect(p4,"(.)(.)(.).*\\3\\2\\1")
```

    ## [1] TRUE

#Question 4

Construct regular expressions to match words that: - Start and end with
the same character.

``` r
four_a <- "abbbba"
str_detect(four_a,"^(.).*\\1$")
```

    ## [1] TRUE

-   Contain a repeated pair of letters (e.g. “church” contains “ch”
    repeated twice.)

``` r
four_b <- "church"
str_detect(four_b,"(..).*\\1")
```

    ## [1] TRUE

-   Contain one letter repeated in at least three places (e.g. “eleven”
    contains three “e”s.)

``` r
four_c <- "eleven"
str_detect(four_c,"(.).*\\1.*\\1")
```

    ## [1] TRUE
