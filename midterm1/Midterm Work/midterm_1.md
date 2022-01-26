---
title: "Midterm 1"
author: "Benjamin Prieto"
date: "2022-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

**1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.**

I feel that we have had ample practice in "extracting knowledge and insights from noisy, structured and unstructured data". For example, in the last assignment we had used `dplyr` functions, namely `group_by()`, `summarize()`, `filter()`, and `select()`, to extract relevant data from the `fisheries` dataset and answer a series of queries similar to "Which country caught the most cephalopods from 2001 to 2004?".


**2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?**  

The most helpful and interesting thing that I have learned in BIS 15L is how to use pipes in conjunction with `dplyr` to make my code much more clean and concise, and overall easier to parse after having not worked on it in a while. I think I need more practice utilizing all of the functions we've learned thus far just so that I can more easily recall them when I want to make a certain data transformation or analysis without having to consult my notebook for the best function to use—we've learned about many functions and arguments in class already and I think it's easy to forget the earlier functions taught as the class goes on.


In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**


```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1…
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"…
```


```r
class(elephants$Age)
```

```
## [1] "numeric"
```


```r
class(elephants$Height)
```

```
## [1] "numeric"
```


```r
class(elephants$Sex)
```

```
## [1] "character"
```


**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**


```r
elephants <- rename(elephants,
       age="Age",
       height="Height",
       sex="Sex")
names(elephants)
```

```
## [1] "age"    "height" "sex"
```


```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```


**5. (2 points) How many male and female elephants are represented in the data?**


```r
table(elephants$sex)
```

```
## 
##   F   M 
## 150 138
```


**6. (2 points) What is the average age all elephants in the data?**


```r
anyNA(elephants$age) #check for NAs before doing average calculation 
```

```
## [1] FALSE
```


```r
mean(elephants$age)
```

```
## [1] 10.97132
```


**7. (2 points) How does the average age and height of elephants compare by sex?**


```r
elephants %>% 
  group_by(sex) %>% #compare elephants by sex
  summarize(avg_age=mean(age),avg_height=mean(height)) #calculate averages of age and height
```

```
## # A tibble: 2 × 3
##   sex   avg_age avg_height
##   <fct>   <dbl>      <dbl>
## 1 F       12.8        190.
## 2 M        8.95       185.
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**  


```r
elephants %>% 
  group_by(sex) %>% #compare by sex
  filter(age>20) %>% #filter for individuals over 20 yrs old
  summarize(avg_height=mean(height),min_height=min(height),max_height=max(height),individuals=n()) #extract avg height, max, min, individuals
```

```
## # A tibble: 2 × 5
##   sex   avg_height min_height max_height individuals
##   <fct>      <dbl>      <dbl>      <dbl>       <int>
## 1 F           232.       193.       278.          37
## 2 M           270.       229.       304.          13
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
gabon <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(gabon)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, …
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N…
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8…
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8…
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56…
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1…
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92…
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, …
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92…
```


```r
gabon <- janitor::clean_names(gabon) #clean variable names for ease of use
names(gabon)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
gabon <- gabon %>% #change all entries to lowercase for ease of use
  mutate_all(tolower) 
head(gabon)
```

```
## # A tibble: 6 × 26
##   transect_id distance hunt_cat num_households land_use veg_rich veg_stems
##   <chr>       <chr>    <chr>    <chr>          <chr>    <chr>    <chr>    
## 1 1           7.14     moderate 54             park     16.67    31.2     
## 2 2           17.31    none     54             park     15.75    37.44    
## 3 2           18.32    none     29             park     16.88    32.33    
## 4 3           20.85    none     29             logging  12.44    29.39    
## 5 4           15.95    none     29             park     17.13    36       
## 6 5           17.47    none     29             park     16.5     29.22    
## # … with 19 more variables: veg_liana <chr>, veg_dbh <chr>, veg_canopy <chr>,
## #   veg_understory <chr>, ra_apes <chr>, ra_birds <chr>, ra_elephant <chr>,
## #   ra_monkeys <chr>, ra_rodent <chr>, ra_ungulate <chr>,
## #   rich_all_species <chr>, evenness_all_species <chr>,
## #   diversity_all_species <chr>, rich_bird_species <chr>,
## #   evenness_bird_species <chr>, diversity_bird_species <chr>,
## #   rich_mammal_species <chr>, evenness_mammal_species <chr>, …
```


```r
gabon$hunt_cat <- as.factor(gabon$hunt_cat) #change hunt_cat to factor
class(gabon$hunt_cat)
```

```
## [1] "factor"
```


```r
gabon$land_use <- as.factor(gabon$land_use) #change land_use to factor
class(gabon$land_use)
```

```
## [1] "factor"
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**


```r
levels(gabon$hunt_cat) #check hunt_cat levels for correct spelling and case
```

```
## [1] "high"     "moderate" "none"
```


```r
gabon$diversity_bird_species <- as.numeric(gabon$diversity_bird_species) #change diversity_bird_species to numeric to allow calculations
class(gabon$diversity_bird_species)
```

```
## [1] "numeric"
```


```r
gabon$diversity_mammal_species <- as.numeric(gabon$diversity_mammal_species) #change diversity_mammal_species to numeric to allow calculations
class(gabon$diversity_mammal_species)
```

```
## [1] "numeric"
```


```r
gabon %>% 
  filter(hunt_cat=="high" | hunt_cat=="moderate") %>% #filter for high or moderate hunting
  summarize(avg_dv_birds=mean(diversity_bird_species),avg_dv_mammals=mean(diversity_mammal_species)) #calculate avg diversity for each group
```

```
## # A tibble: 1 × 2
##   avg_dv_birds avg_dv_mammals
##          <dbl>          <dbl>
## 1         1.64           1.71
```


**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**


```r
names(gabon) #check variable names for correct spelling
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
gabon$distance <- as.numeric(gabon$distance) #set all relative abundances to numeric to allow for calculations
gabon$ra_apes <- as.numeric(gabon$ra_apes)
gabon$ra_birds <- as.numeric(gabon$ra_birds)
gabon$ra_elephant <- as.numeric(gabon$ra_elephant)
gabon$ra_monkeys <- as.numeric(gabon$ra_monkeys)
gabon$ra_rodent <- as.numeric(gabon$ra_rodent)
gabon$ra_ungulate <- as.numeric(gabon$ra_ungulate)
```


```r
gabon %>% 
  filter(distance<3) %>% #filter for distances from villages that are less than 3 km away
  summarize(across(starts_with("ra"),mean)) #calculate mean relative abundance for all animal groups
```

```
## # A tibble: 1 × 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    0.12     76.6       0.145       17.3      3.90        1.87
```


```r
gabon %>% 
  filter(distance>25) %>% #filter for distances from villages that are more than 25 km away
  summarize(across(starts_with("ra"),mean)) #calculate mean relative abundance for all animal groups
```

```
## # A tibble: 1 × 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    4.91     31.6           0       54.1      1.29        8.12
```


**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

At a glance, do the relative abundances of each of the studied animal groups seem to be greater on average in land used as parks compared to land used for logging?


```r
gabon %>% 
  group_by(land_use) %>% #compare by land use
  filter(land_use!="neither") %>% #exclude "neither" since it is not relevant to the query
  summarize(across(starts_with("ra"),mean)) #calculate relative abundance for each animal group
```

```
## # A tibble: 2 × 7
##   land_use ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##   <fct>      <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1 logging     2.10     62.7       0.425       28.4      3.22        3.10
## 2 park        2.50     44.0       0.614       42.0      2.87        8.06
```

