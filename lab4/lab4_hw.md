---
title: "Lab 4 Homework"
author: "Benjamin Prieto"
date: "2022-01-16"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
**Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.**  

**Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!**  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
**For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.**  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**


```r
homerange <- readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double()
## )
## ℹ Use `spec()` for the full column specifications.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  


```r
dim(homerange)
```

```
## [1] 569  24
```


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fishe…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", "…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```


```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```


**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  


```r
homerange$taxon <- as.factor(homerange$taxon) #set taxon var as a factor to be compatible with class()
class(homerange$taxon) #check previous work
```

```
## [1] "factor"
```

```r
levels(homerange$taxon) #print levels
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```


```r
homerange$order <- as.factor(homerange$order) #repeat previous steps with 'order'
class(homerange$order)
```

```
## [1] "factor"
```

```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes\xa0" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```
In `homerange`, the taxa class, order, family, genus, and species are represented.


```r
taxa <- select(homerange,"taxon","common.name","class","order","family","genus","species")
names(taxa)
```

```
## [1] "taxon"       "common.name" "class"       "order"       "family"     
## [6] "genus"       "species"
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**


```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
tguild <- as.factor(homerange$trophic.guild) #set trophic.guild as factor to view levels for exact spelling of var for later
levels(tguild)
```

```
## [1] "carnivore" "herbivore"
```


```r
carni <- filter(homerange,trophic.guild=="carnivore") #filter data for only carnivores
cspecies <- as.factor(carni$species) #set species var as a factor to examine levels properly
glimpse(cspecies) #return number of levels
```

```
##  Factor w/ 322 levels "adspersus","aedon",..: 247 230 10 104 50 175 232 313 136 254 ...
```
According to `glimpse()`, there are 322 species represented among the carnivores in the data frame.


```r
herbi <- filter(homerange,trophic.guild=="herbivore")
hspecies <- as.factor(herbi$species)
glimpse(hspecies)
```

```
##  Factor w/ 212 levels "aberti","aegyptius",..: 111 113 199 22 174 20 50 31 211 16 ...
```
According to `glimpse()`, there are 212 species represented among the herbivores in the data frame.

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  


```r
carni <- filter(homerange,trophic.guild=="carnivore")
```


```r
herbi <- filter(homerange,trophic.guild=="herbivore")
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  


```r
anyNA(carni$mean.hra.m2) #check for NAs
```

```
## [1] FALSE
```

```r
carni.m2 <- carni[,13] #create vector of mean.hra.m2
carni.mean <- colMeans(carni.m2) #find mean
carni.mean #print results
```

```
## mean.hra.m2 
##    13039918
```

```r
anyNA(herbi$mean.hra.m2) #repeat previous steps with herbivore data
```

```
## [1] FALSE
```

```r
herbi.m2 <- herbi[,13]
herbi.mean <- colMeans(herbi.m2)
herbi.mean
```

```
## mean.hra.m2 
##    34137012
```

```r
carni.mean > herbi.mean
```

```
## mean.hra.m2 
##       FALSE
```

The herbivores have a larger` mean.hra.m2` on average than carnivores, i.e. 34,137,012 > 13,039,918.

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**


```r
deer <- filter(homerange,family=="cervidae") #filter data by only deer
deer <- select(deer,mean.mass.g,log10.mass,family,genus,species) #create a new data frame according to desired var
deer[order(deer$log10.mass,decreasing=TRUE),] #sort data by decreasing log10 mass
```

```
## # A tibble: 12 × 5
##    mean.mass.g log10.mass family   genus      species    
##          <dbl>      <dbl> <chr>    <chr>      <chr>      
##  1     307227.       5.49 cervidae alces      alces      
##  2     234758.       5.37 cervidae cervus     elaphus    
##  3     102059.       5.01 cervidae rangifer   tarandus   
##  4      87884.       4.94 cervidae odocoileus virginianus
##  5      71450.       4.85 cervidae dama       dama       
##  6      62823.       4.80 cervidae axis       axis       
##  7      53864.       4.73 cervidae odocoileus hemionus   
##  8      35000.       4.54 cervidae ozotoceros bezoarticus
##  9      29450.       4.47 cervidae cervus     nippon     
## 10      24050.       4.38 cervidae capreolus  capreolus  
## 11      13500.       4.13 cervidae muntiacus  reevesi    
## 12       7500.       3.88 cervidae pudu       puda
```


```r
largest.deer <- filter(homerange, genus=="alces",species=="alces") #filter data to return only the largest deer
largest.deer #print results
```

```
## # A tibble: 1 × 24
##   taxon   common.name class    order    family genus species primarymethod N    
##   <fct>   <chr>       <chr>    <fct>    <chr>  <chr> <chr>   <chr>         <chr>
## 1 mammals moose       mammalia artioda… cervi… alces alces   telemetry*    <NA> 
## # … with 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <chr>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```

According to mean mass (g) and log10 mass, the largest deer is *Alces alces*, whose common name is "moose".

**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    


```r
snakes <- filter(homerange,taxon=="snakes") #filter homerange to only snake data
which.min(snakes$mean.hra.m2) #index the snake with the smallest mean HRA
```

```
## [1] 29
```

```r
snake.min.hra <- snakes[29,] #recall the minimum and save as an object to view results
snake.min.hra #print results
```

```
## # A tibble: 1 × 24
##   taxon  common.name    class  order  family  genus species  primarymethod N    
##   <fct>  <chr>          <chr>  <fct>  <chr>   <chr> <chr>    <chr>         <chr>
## 1 snakes namaqua dwarf… repti… squam… viperi… bitis schneid… telemetry     11   
## # … with 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <chr>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```

```r
min(snakes$mean.hra.m2) #to check which.min() results
```

```
## [1] 200
```
The snake species with the smallest home range is the Namaqua Dwarf Adder with a mean home range of 200 meters squared. According to True Vipers: Natural History and Toxinology of Old World Vipers (2013), these snakes are considered the smallest vipers with an average length of 7-10 in. Their small average home range in relation to the other snakes in the given data frame is consistent with the original article's observation that home-range area is proportional to body size. Unfortunately, because of their small size and predator-dense habitat, they experience high rates of annual mortality (39-56%), and as such they have developed a higher reproductive rate relative to other viipers ("Population density and survival estimates of the African viperid, Bitis schneideri", 2012).

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
