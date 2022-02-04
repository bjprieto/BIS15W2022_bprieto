---
title: "Lab 9 Homework"
author: "Benjamin Prieto"
date: "2022-02-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- read_csv(here("lab9","data","ca_college_data.csv"))
```

```
## Rows: 341 Columns: 10
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

**1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.**

General Overview of Data

```r
glimpse(colleges)
```

```
## Rows: 341
## Columns: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "College…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Oxnard",…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3872", …
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.0000, …
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9281, 93…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.1704, …
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.2307, …
```

Check NA Representation and Summarize

```r
naniar::miss_var_summary(colleges)
```

```
## # A tibble: 10 × 3
##    variable      n_miss pct_miss
##    <chr>          <int>    <dbl>
##  1 SAT_AVG          276     80.9
##  2 ADM_RATE         240     70.4
##  3 C150_4_POOLED    221     64.8
##  4 COSTT4_A         124     36.4
##  5 PFTFTUG1_EF       53     15.5
##  6 PCIP26            35     10.3
##  7 INSTNM             0      0  
##  8 CITY               0      0  
##  9 STABBR             0      0  
## 10 ZIP                0      0
```
Clean Names for Ease

```r
colleges <- janitor::clean_names(colleges)
colleges
```

```
## # A tibble: 341 × 10
##    instnm     city   stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##    <chr>      <chr>  <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Grossmont… El Ca… CA     9202…       NA      NA 0.0016     7956        NA    
##  2 College o… Visal… CA     9327…       NA      NA 0.0066     8109        NA    
##  3 College o… San M… CA     9440…       NA      NA 0.0038     8278        NA    
##  4 Ventura C… Ventu… CA     9300…       NA      NA 0.0035     8407        NA    
##  5 Oxnard Co… Oxnard CA     9303…       NA      NA 0.0085     8516        NA    
##  6 Moorpark … Moorp… CA     9302…       NA      NA 0.0151     8577        NA    
##  7 Skyline C… San B… CA     9406…       NA      NA 0          8580         0.233
##  8 Glendale … Glend… CA     9120…       NA      NA 0.002      9181        NA    
##  9 Citrus Co… Glend… CA     9174…       NA      NA 0.0021     9281        NA    
## 10 Fresno Ci… Fresno CA     93741       NA      NA 0.0324     9370        NA    
## # … with 331 more rows, and 1 more variable: pftftug1_ef <dbl>
```

Yes, this data appears to be tidy—variables, observations, and values don't seem to be conflated with each other.


**2. Which cities in California have the highest number of colleges?**

```r
colleges %>% 
  filter(stabbr == "CA") %>% #filter for entries in California
  count(city) %>% #count number of colleges per city
  arrange(desc(n)) #arrange data to begin with most colleges
```

```
## # A tibble: 159 × 2
##    city              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # … with 149 more rows
```


**3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.**

```r
colleges %>% 
  filter(stabbr == "CA") %>% #filter for entries in California
  count(city) %>% #count number of colleges per city
  arrange(desc(n)) %>% #arrange data to begin with most colleges
  head(n = 10) %>% #keep only the top 10 results
  ggplot(aes(x = city, y = n))+ 
  geom_col()+ #produce bar graph
  coord_flip() #flip to resolve city names
```

![](lab9_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?**

```r
colleges %>% 
  select(city, costt4_a) %>% #focus on relevant vars
  group_by(city) %>% #group by city
  summarize(avg_cost = mean(costt4_a, na.rm = T)) %>% #calculate mean costs
  arrange(desc(avg_cost)) %>% #sort data so the greatest cost is on top
  head(n = 1) #keep only first result
```

```
## # A tibble: 1 × 2
##   city      avg_cost
##   <chr>        <dbl>
## 1 Claremont    66498
```

**5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).**

```r
colleges %>% 
  filter(city == "Claremont" | city == "Davis",costt4_a!="NA")  %>% #filter results for most expensive city and Davis
  select(instnm,city,costt4_a) %>% #focus on relevant vars
  ggplot(aes(x = instnm, y = costt4_a))+ 
  geom_col()+ #produce bar graph
  coord_flip() #flip graph to resolve college names
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

**6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?**

```r
colleges %>% 
  select(adm_rate,c150_4_pooled) %>% 
  ggplot(aes(x = adm_rate, y = c150_4_pooled))+
  geom_point()+ #produce scatter plot
  geom_smooth(method = lm) #add linear regression line to clarify correlation
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 251 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

As admission rate increases, four-year completion rate decreases. Optimistically, this could be due to a larger number of transfer students that are admitted who would not complete a full year's worth of courses at the institution. More pessimistically, a larger admission rate has a greater rate of students that cannot keep up with the institution's curriculum such that many more students are likely to drop out. Or perhaps it is the other way around—more cynically, the institution's curriculum is so difficult/unsatisfactory that many students drop out or transfer, and to compensate, the institution accepts more students to maintain funding.


**7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?**

```r
colleges %>% 
  select(costt4_a, c150_4_pooled) %>% #select relevant vars
  ggplot(aes(x = costt4_a, c150_4_pooled))+ 
  geom_point()+ #produce scatter plot
  geom_smooth(method = lm) #add linear regression line for trend clarity
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 225 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 225 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

There appears to be a positive correlation between cost and four-year completion rate. I'm not sure how the cost is measured since I couldn't find an answer by skimming the website sourcing the data some I'm not entirely sure, but perhaps the cost increases as four-year completion increases due to students paying more while attending the school for longer. If cost refers merely to tuition, then from a cynical standpoint, the correlation could be due to colleges wanting to maximize funding by increasing tuition knowing that many of their students will be retained for four years.


**8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.**

```r
uc <- colleges %>% 
  filter(grepl("University of California",instnm)) 
uc
```

```
## # A tibble: 10 × 10
##    instnm     city   stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##    <chr>      <chr>  <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Universit… La Jo… CA     92093    0.357    1324  0.216    31043         0.872
##  2 Universit… Irvine CA     92697    0.406    1206  0.107    31198         0.876
##  3 Universit… River… CA     92521    0.663    1078  0.149    31494         0.73 
##  4 Universit… Los A… CA     9009…    0.180    1334  0.155    33078         0.911
##  5 Universit… Davis  CA     9561…    0.423    1218  0.198    33904         0.850
##  6 Universit… Santa… CA     9506…    0.578    1201  0.193    34608         0.776
##  7 Universit… Berke… CA     94720    0.169    1422  0.105    34924         0.916
##  8 Universit… Santa… CA     93106    0.358    1281  0.108    34998         0.816
##  9 Universit… San F… CA     9410…   NA          NA NA           NA        NA    
## 10 Universit… San F… CA     9414…   NA          NA NA           NA        NA    
## # … with 1 more variable: pftftug1_ef <dbl>
```

*Note: I tried to find a way to filter for only UCs using the functions we had learned in class, but could not find any that I could use efficiently. I ended up looking up a good way to do it online while still using dplyr, which ended up being the `grepl` function.


**Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.**

```r
univ_calif_final <- uc %>% 
  filter(instnm!="University of California-Hastings College of Law",instnm!="University of California-San Francisco")
univ_calif_final
```

```
## # A tibble: 8 × 10
##   instnm      city   stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr>       <chr>  <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 University… La Jo… CA     92093    0.357    1324  0.216    31043         0.872
## 2 University… Irvine CA     92697    0.406    1206  0.107    31198         0.876
## 3 University… River… CA     92521    0.663    1078  0.149    31494         0.73 
## 4 University… Los A… CA     9009…    0.180    1334  0.155    33078         0.911
## 5 University… Davis  CA     9561…    0.423    1218  0.198    33904         0.850
## 6 University… Santa… CA     9506…    0.578    1201  0.193    34608         0.776
## 7 University… Berke… CA     94720    0.169    1422  0.105    34924         0.916
## 8 University… Santa… CA     93106    0.358    1281  0.108    34998         0.816
## # … with 1 more variable: pftftug1_ef <dbl>
```

**Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".**

```r
univ_calif_final_sep <- univ_calif_final %>% 
  separate(instnm,
           into = c("univ","campus"),
           sep = "-")
univ_calif_final_sep
```

```
## # A tibble: 8 × 11
##   univ  campus city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 Univ… San D… La J… CA     92093    0.357    1324  0.216    31043         0.872
## 2 Univ… Irvine Irvi… CA     92697    0.406    1206  0.107    31198         0.876
## 3 Univ… River… Rive… CA     92521    0.663    1078  0.149    31494         0.73 
## 4 Univ… Los A… Los … CA     9009…    0.180    1334  0.155    33078         0.911
## 5 Univ… Davis  Davis CA     9561…    0.423    1218  0.198    33904         0.850
## 6 Univ… Santa… Sant… CA     9506…    0.578    1201  0.193    34608         0.776
## 7 Univ… Berke… Berk… CA     94720    0.169    1422  0.105    34924         0.916
## 8 Univ… Santa… Sant… CA     93106    0.358    1281  0.108    34998         0.816
## # … with 1 more variable: pftftug1_ef <dbl>
```

**9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.**

Numerical Summary: UCs in order of descending admission rates

```r
univ_calif_final_sep %>% 
  select(campus,adm_rate) %>% 
  arrange(desc(adm_rate))
```

```
## # A tibble: 8 × 2
##   campus        adm_rate
##   <chr>            <dbl>
## 1 Riverside        0.663
## 2 Santa Cruz       0.578
## 3 Davis            0.423
## 4 Irvine           0.406
## 5 Santa Barbara    0.358
## 6 San Diego        0.357
## 7 Los Angeles      0.180
## 8 Berkeley         0.169
```

Plot: Admission Rate vs. UC Campus

```r
univ_calif_final_sep %>% 
  select(campus,adm_rate) %>% 
  ggplot(aes(x = campus, y = adm_rate))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

**10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.**

Numerical Summary: UCs in order of descending biological/biomedical degree conferrence

```r
univ_calif_final_sep %>% 
  select(campus,pcip26) %>% 
  arrange(desc(pcip26))
```

```
## # A tibble: 8 × 2
##   campus        pcip26
##   <chr>          <dbl>
## 1 San Diego      0.216
## 2 Davis          0.198
## 3 Santa Cruz     0.193
## 4 Los Angeles    0.155
## 5 Riverside      0.149
## 6 Santa Barbara  0.108
## 7 Irvine         0.107
## 8 Berkeley       0.105
```

Plot: Biological/Biomedical Degree Conferrence vs. UC Campus

```r
univ_calif_final_sep %>% 
  ggplot(aes(x = campus, y = pcip26))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
