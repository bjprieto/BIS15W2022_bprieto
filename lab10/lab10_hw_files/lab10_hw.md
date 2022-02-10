---
title: "Lab 10 Homework"
author: "Benjamin Prieto"
date: "2022-02-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?**

Data Organization

```r
deserts
```

```
## # A tibble: 34,786 × 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # … with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```

Variable Names and Classes

```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```

Check for NAs Across Variables

```r
miss_var_summary(deserts)
```

```
## # A tibble: 13 × 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```

Variable Classes, Spread, NAs

```r
summary(deserts)
```

```
##    record_id         month             day            year         plot_id     
##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
##                                                                                
##   species_id            sex            hindfoot_length     weight      
##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
##                                        Mean   :29.29   Mean   : 42.67  
##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
##                                        Max.   :70.00   Max.   :280.00  
##                                        NA's   :3348    NA's   :2503    
##     genus             species              taxa            plot_type        
##  Length:34786       Length:34786       Length:34786       Length:34786      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
## 
```

NAs are represented by NAs, and the data is tidy in its recording.


**2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?**

Names Check

```r
names(deserts)
```

```
##  [1] "record_id"       "month"           "day"             "year"           
##  [5] "plot_id"         "species_id"      "sex"             "hindfoot_length"
##  [9] "weight"          "genus"           "species"         "taxa"           
## [13] "plot_type"
```


Genera and Species Represented, Total Number of Observations

```r
deserts %>% 
  summarize(genera_represented = n_distinct(genus),
            species_represented = n_distinct(species),
            n_total = sum(n()))
```

```
## # A tibble: 1 × 3
##   genera_represented species_represented n_total
##                <int>               <int>   <int>
## 1                 26                  40   34786
```

Most Sampled Species

```r
deserts %>% 
  count(species) %>% 
  arrange(desc(n)) %>% 
  filter(n == max(n))
```

```
## # A tibble: 1 × 2
##   species      n
##   <chr>    <int>
## 1 merriami 10596
```

Least Sampled Species

```r
deserts %>% 
  count(species) %>% 
  arrange(desc(n)) %>% 
  filter(n == min(n))
```

```
## # A tibble: 6 × 2
##   species          n
##   <chr>        <int>
## 1 clarki           1
## 2 scutalatus       1
## 3 tereticaudus     1
## 4 tigris           1
## 5 uniparens        1
## 6 viridis          1
```

Plot: Species Sampling

```r
deserts %>% 
  count(species) %>% 
  ggplot(aes(x = reorder(species,n), y = n))+
  geom_col()+
  coord_flip()+
  labs(x = "Species",
       title = "Sampling of Species",)+
  theme(plot.title = element_text(hjust = 0.5)) 
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


**3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.**

Reference

```r
taxa_prop <- deserts %>% 
  tabyl(taxa)
taxa_prop
```

```
##     taxa     n      percent
##     Bird   450 0.0129362387
##   Rabbit    75 0.0021560398
##  Reptile    14 0.0004024608
##   Rodent 34247 0.9845052607
```

Plot

```r
taxa_prop %>% 
  ggplot(aes(x = "",y = percent, fill = taxa))+
  geom_col()+
  coord_polar(theta = "y")+ #arrange data as a pie chart
  labs(x = NULL,
       y = NULL,
       title = "Proportion of Taxa Among Deserts Animal Samples",)+
  scale_y_continuous(labels = scales::percent)+ #scale y to percent
  theme(plot.title = element_text(hjust = 0.5, size = 11)) 
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


**4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`**

Reference

```r
taxa_prop_plot <- deserts %>% 
  group_by(taxa, plot_type) %>% 
  count(taxa, species)
taxa_prop_plot
```

```
## # A tibble: 163 × 4
## # Groups:   taxa, plot_type [19]
##    taxa  plot_type                species             n
##    <chr> <chr>                    <chr>           <int>
##  1 Bird  Control                  bilineata          76
##  2 Bird  Control                  brunneicapillus    13
##  3 Bird  Control                  chlorurus           8
##  4 Bird  Control                  gramineus           3
##  5 Bird  Control                  melanocorys         2
##  6 Bird  Control                  savannarum          1
##  7 Bird  Control                  sp.                 5
##  8 Bird  Control                  squamata            2
##  9 Bird  Long-term Krat Exclosure bilineata          41
## 10 Bird  Long-term Krat Exclosure brunneicapillus     6
## # … with 153 more rows
```

Plot

```r
taxa_prop_plot %>% 
  ggplot(aes(x = taxa, y = n, fill = plot_type))+
  geom_col(position = position_fill())+
  scale_y_continuous(labels = scales::percent)+
  labs(x = "Taxa",
       y = "n",
       title = "Taxa Proportions of Desert Animals by Plot Type")+
  theme(plot.title = element_text(hjust = 0.5)) 
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


**5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.**

Reference

```r
deserts %>% 
  select(species, weight) %>% 
  group_by(species) %>% 
  filter(weight!=is.na(weight)) %>% 
  summarize(min_weight = min(weight),
            median_weight = median(weight),
            max_weight = max(weight))
```

```
## # A tibble: 22 × 4
##    species     min_weight median_weight max_weight
##    <chr>            <dbl>         <dbl>      <dbl>
##  1 albigula            30         164          280
##  2 baileyi             12          31           55
##  3 eremicus             8          22           40
##  4 flavus               4           8           25
##  5 fulvescens           9          13           20
##  6 fulviventer         24          50          199
##  7 hispidus            16          64.5        140
##  8 intermedius         17          19.5         21
##  9 leucogaster         10          32           56
## 10 leucopus             8          20           27
## # … with 12 more rows
```

Plot

```r
deserts %>% 
  ggplot(aes(x = species, y = weight))+
  geom_boxplot()+
  scale_y_log10()+
  coord_flip()+
  labs(x = "Species",
       y = "Log 10 Weight",
       title = "Desert Animal Weight by Species")+
  theme(plot.title = element_text(hjust = 0.5), 
        axis.text.y = element_text(size = 8))
```

```
## Warning: Removed 2503 rows containing non-finite values (stat_boxplot).
```

![](lab10_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


**6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.**

Reference

```r
deserts %>% 
  count(taxa, species) %>% 
  arrange(desc(species))
```

```
## # A tibble: 42 × 3
##    taxa    species          n
##    <chr>   <chr>        <int>
##  1 Reptile viridis          1
##  2 Reptile uniparens        1
##  3 Reptile undulatus        5
##  4 Rodent  torridus      2249
##  5 Reptile tigris           1
##  6 Rodent  tereticaudus     1
##  7 Rodent  taylori         46
##  8 Bird    squamata        16
##  9 Rodent  spilosoma      248
## 10 Rodent  spectabilis   2504
## # … with 32 more rows
```
Plot

```r
taxa_prop_plot %>% 
  ggplot(aes(x = taxa, y = n, fill = plot_type))+
  geom_col(position = position_fill())+
  labs(x = "Taxa",
       y = "n",
       title = "Taxa Proportions of Desert Animals by Plot Type")+
  theme(plot.title = element_text(hjust = 0.5))+
  geom_point()
```

![](lab10_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->



**7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?**

Reference

```r
merriami_by_year <- deserts %>% 
  filter(species == "merriami") %>% 
  group_by(year) %>% 
  count(species)
merriami_by_year
```

```
## # A tibble: 26 × 3
## # Groups:   year [26]
##     year species      n
##    <dbl> <chr>    <int>
##  1  1977 merriami   264
##  2  1978 merriami   389
##  3  1979 merriami   209
##  4  1980 merriami   493
##  5  1981 merriami   559
##  6  1982 merriami   609
##  7  1983 merriami   528
##  8  1984 merriami   396
##  9  1985 merriami   667
## 10  1986 merriami   406
## # … with 16 more rows
```

Plot

```r
merriami_by_year %>% 
  ggplot(aes(x = year, y = n))+
  geom_col()+
  labs(x = "Year",
       y = "n",
       title = "Dipodomys merriami Samples by Year")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->


**8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.**

Reference

```r
deserts %>% 
  select(weight, hindfoot_length)
```

```
## # A tibble: 34,786 × 2
##    weight hindfoot_length
##     <dbl>           <dbl>
##  1     NA              32
##  2     NA              33
##  3     NA              37
##  4     NA              36
##  5     NA              35
##  6     NA              14
##  7     NA              NA
##  8     NA              37
##  9     NA              34
## 10     NA              20
## # … with 34,776 more rows
```

Plot

```r
deserts %>% 
  ggplot(aes(x = weight, y = hindfoot_length))+
  geom_point(size = 0.5, alpha = 0.6)+
  geom_smooth(method = "lm")+
  scale_y_log10()+
  scale_x_log10()+
  labs(x = "Log 10 Weight",
       y = "Log 10 Hindfoot Length",
       title = "Hindfoot Length vs. Weight")+
  theme(plot.title = element_text(hjust = 0.5))
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 4048 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 4048 rows containing missing values (geom_point).
```

![](lab10_hw_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

Overplotting is not an issue since we are only concerned with the correlation of data and not necessarily the location/values of each point, especially given how dense the data is.


**9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.**

Species with the top two highest average weight

```r
deserts %>% 
  group_by(species) %>%
  summarize(avg_weight = mean(weight, na.rm = T)) %>% 
  top_n(avg_weight, n = 2)
```

```
## # A tibble: 2 × 2
##   species     avg_weight
##   <chr>            <dbl>
## 1 albigula          159.
## 2 spectabilis       120.
```

Reference

```r
top2_wt <- deserts %>% 
  group_by(species) %>%
  filter(species == "albigula" | species == "spectabilis",
         weight != is.na(weight),
         hindfoot_length != is.na(hindfoot_length)) %>% 
  select(species, sex, weight, hindfoot_length) %>% 
  mutate(wt_hindft_ratio = weight/hindfoot_length)
top2_wt
```

```
## # A tibble: 3,072 × 5
## # Groups:   species [2]
##    species     sex   weight hindfoot_length wt_hindft_ratio
##    <chr>       <chr>  <dbl>           <dbl>           <dbl>
##  1 spectabilis F        117              50            2.34
##  2 spectabilis F        121              51            2.37
##  3 spectabilis M        115              51            2.25
##  4 spectabilis F        120              48            2.5 
##  5 spectabilis F        118              48            2.46
##  6 spectabilis F        126              52            2.42
##  7 spectabilis M        132              50            2.64
##  8 spectabilis F        122              53            2.30
##  9 spectabilis F        107              48            2.23
## 10 spectabilis F        115              50            2.3 
## # … with 3,062 more rows
```

Reference 2

```r
top2_wt %>% 
  group_by(species, sex) %>% 
  summarize(min_wh_ratio = min(wt_hindft_ratio),
            median_wh_ratio = median(wt_hindft_ratio),
            max_wh_ratio = max(wt_hindft_ratio))
```

```
## `summarise()` has grouped output by 'species'. You can override using the `.groups` argument.
```

```
## # A tibble: 6 × 5
## # Groups:   species [2]
##   species     sex   min_wh_ratio median_wh_ratio max_wh_ratio
##   <chr>       <chr>        <dbl>           <dbl>        <dbl>
## 1 albigula    F            1                4.94         8.30
## 2 albigula    M            1                5.12         8.69
## 3 albigula    <NA>         2.79             2.79         2.79
## 4 spectabilis F            0.978            2.41         3.73
## 5 spectabilis M            0.235            2.55         3.37
## 6 spectabilis <NA>         1.98             2.29         2.57
```

Plot

```r
top2_wt %>% 
  ggplot(aes(x = species, y = wt_hindft_ratio, fill = sex))+
  geom_boxplot()+
  labs(x = "Species",
       y = "Weight to Hindfoot Ratio",
       title = "Weight to Hindfoot Ratio by Species")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-27-1.png)<!-- -->


**10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.**

What are the relative species abundances by taxa in the data?

Reference

```r
deserts %>% 
  group_by(taxa) %>% 
  count(species)
```

```
## # A tibble: 42 × 3
## # Groups:   taxa [4]
##    taxa  species             n
##    <chr> <chr>           <int>
##  1 Bird  bilineata         303
##  2 Bird  brunneicapillus    50
##  3 Bird  chlorurus          39
##  4 Bird  fuscus              5
##  5 Bird  gramineus           8
##  6 Bird  leucophrys          2
##  7 Bird  melanocorys        13
##  8 Bird  savannarum          2
##  9 Bird  sp.                12
## 10 Bird  squamata           16
## # … with 32 more rows
```
Plot

```r
deserts %>% 
  group_by(taxa) %>% 
  count(species) %>% 
  ggplot(aes(x = taxa, y = n, fill = species))+
  geom_col(position = position_fill())+
  scale_y_continuous(labels = scales::percent)+
  labs(x = "Taxa",
       y = "% Distinct Species",
       title = "Relative Species Abundance by Taxa Sampled")+
  theme(plot.title = element_text(hjust = 0.5),
        legend.position = "none")
```

![](lab10_hw_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
