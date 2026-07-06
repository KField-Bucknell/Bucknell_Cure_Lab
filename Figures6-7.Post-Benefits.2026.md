---
title: "Post Benefits Question"
author: "Ken Field"
date: "Last compiled on 06 July 2026"
output:
  html_document:
    toc: true
    keep_md: yes
  pdf_document: default
---

IMPORTANT NOTE

This Rmd uses the deidentified results and is safe to share.



## Loading Results

Loading in the results without instructor information. 
Note that because we want to analyze demographics for this question, we cannot look at instructor because that would lead to individuals being identifiable. 
We also need to be careful about looking at both gender and race at the same time.


``` r
NoInstructorYear1 <- read_delim("Deidentified Surveys/Year1.NoInstructor.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE) %>%
  rename(Semester = Semester_pre)
```

```
## Rows: 85 Columns: 156
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (146): ResponseId_pre, Semester_pre, Q1_pre, Q8_pre, Q9_1_pre, Q9_2_pre,...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoInstructorYear2 <- read_delim("Deidentified Surveys/Year2.NoInstructor.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 77 Columns: 156
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (146): ResponseId_pre, Semester, Q1_pre, Q8_pre, Q9_1_pre, Q9_2_pre, Q9_...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoInstructorYear3 <- read_delim("Deidentified Surveys/Year3.NoInstructor.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 63 Columns: 156
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (146): ResponseId_pre, Semester, Q1_pre, Q8_pre, Q9_1_pre, Q9_2_pre, Q9_...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoInstructor <- bind_rows(NoInstructorYear1, NoInstructorYear2, NoInstructorYear3)

NoInstructorQuestions <- read_delim("Deidentified Surveys/Year3.NoInstructorQuestions.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 156 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (2): value, Question
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Removing any student who did not agree with the informed consent question:



## Benefits of research experience

This question (post-survey question 10) was only in the post-survey.
I am going to try to keep the demographics for this analysis later.

Note that pre-survey question 10 matched with post-survey question 9, while pre-survey question 11 matched with post-survey question 13. Post-survey question 10 was not matched with a pre-survey question.


```
## [1] "In this section of the survey you will be asked to consider a variety of possible benefits you may have gained from your research experience. If for any reason you prefer not to answer, or consider the question irrelevant to you, please choose the \"\"Not applicable / Prefer not to answer\"\" option - Clarification of a career path"
```

```
##  [1] "Clarification of a career path"                                      
##  [2] "Skill in the interpretation of results"                              
##  [3] "Tolerance for obstacles faced in the research process"               
##  [4] "Readiness for more demanding research"                               
##  [5] "Understanding how knowledge is constructed"                          
##  [6] "Understanding of the research process in your field"                 
##  [7] "Ability to integrate theory and practice"                            
##  [8] "Understanding of how scientists work on real problems"               
##  [9] "Understanding that scientific assertions require supporting evidence"
## [10] "Ability to analyze data and other information"                       
## [11] "Understanding science"                                               
## [12] "Learning ethical conduct in your field"                              
## [13] "Learning laboratory techniques"                                      
## [14] "Ability to read and understand primary literature"                   
## [15] "Skill in how to give an effective oral presentation"                 
## [16] "Skill in science writing"                                            
## [17] "Self-confidence"                                                     
## [18] "Understanding of how scientists think"                               
## [19] "Learning to work independently"                                      
## [20] "Becoming part of a learning community"                               
## [21] "Confidence in my potential to be a teacher of science"
```

Now to see the responses:


```
## # A tibble: 6 × 25
##   Semester  Gender Ethnicity      ClassYear Q10_1_post     Q10_2_post Q10_3_post
##   <chr>     <chr>  <chr>          <chr>     <chr>          <chr>      <chr>     
## 1 Fall 2021 Female White          First     Moderate gain  Large gain Large gain
## 2 Fall 2021 Female White          First     Very large ga… Very larg… Very larg…
## 3 Fall 2021 Female White          First     Small gain     Moderate … Moderate …
## 4 Fall 2021 Female White          First     Moderate gain  Very larg… Large gain
## 5 Fall 2021 Female Asian American First     Moderate gain  Large gain Moderate …
## 6 Fall 2021 Female White          First     Moderate gain  Large gain Moderate …
## # ℹ 18 more variables: Q10_4_post <chr>, Q10_5_post <chr>, Q10_6_post <chr>,
## #   Q10_7_post <chr>, Q10_8_post <chr>, Q10_9_post <chr>, Q10_10_post <chr>,
## #   Q10_11_post <chr>, Q10_12_post <chr>, Q10_13_post <chr>, Q10_14_post <chr>,
## #   Q10_15_post <chr>, Q10_16_post <chr>, Q10_17_post <chr>, Q10_18_post <chr>,
## #   Q10_19_post <chr>, Q10_20_post <chr>, Q10_21_post <chr>
```

```
## Warning: `funs()` was deprecated in dplyr 0.8.0.
## ℹ Please use a list of either functions or lambdas:
## 
## # Simple named list: list(mean = mean, median = median)
## 
## # Auto named with `tibble::lst()`: tibble::lst(mean, median)
## 
## # Using lambdas list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
##       Semester                    Gender                        Ethnicity  
##  Length   :224   Female              :158   White                    :171  
##  N.unique :  6   Male                : 64   Asian American           : 15  
##  N.blank  :  0   Non-binary          :  1   Two or more races        : 11  
##  Min.nchar:  9   Prefer not to answer:  1   Black or African American:  8  
##  Max.nchar: 11                              Hispanic/Latino          :  8  
##                                             Prefer not to answer     :  7  
##                                             (Other)                  :  4  
##   ClassYear                        Q10_1_post                      Q10_2_post 
##  First :209   Very large gain           :21   Very large gain           : 42  
##  Second: 14   Large gain                :43   Large gain                :118  
##  Third :  1   Moderate gain             :85   Moderate gain             : 53  
##               Small gain                :51   Small gain                :  9  
##               No gain or very small gain:20   No gain or very small gain:  2  
##               NAs                       : 4                                   
##                                                                               
##                       Q10_3_post                       Q10_4_post
##  Very large gain           : 57   Very large gain           :55  
##  Large gain                :104   Large gain                :95  
##  Moderate gain             : 53   Moderate gain             :62  
##  Small gain                :  8   Small gain                :10  
##  No gain or very small gain:  1   No gain or very small gain: 2  
##  NAs                       :  1                                  
##                                                                  
##                       Q10_5_post                       Q10_6_post 
##  Very large gain           : 33   Very large gain           : 54  
##  Large gain                :106   Large gain                :115  
##  Moderate gain             : 65   Moderate gain             : 44  
##  Small gain                : 16   Small gain                :  9  
##  No gain or very small gain:  2   No gain or very small gain:  1  
##  NAs                       :  2   NAs                       :  1  
##                                                                   
##                       Q10_7_post                      Q10_8_post 
##  Very large gain           :36   Very large gain           : 66  
##  Large gain                :96   Large gain                :104  
##  Moderate gain             :66   Moderate gain             : 42  
##  Small gain                :22   Small gain                : 10  
##  No gain or very small gain: 2   No gain or very small gain:  2  
##  NAs                       : 2                                   
##                                                                  
##                       Q10_9_post                     Q10_10_post 
##  Very large gain           :76   Very large gain           : 75  
##  Large gain                :98   Large gain                :103  
##  Moderate gain             :36   Moderate gain             : 38  
##  Small gain                :13   Small gain                :  7  
##  No gain or very small gain: 1   No gain or very small gain:  1  
##                                                                  
##                                                                  
##                      Q10_11_post                      Q10_12_post
##  Very large gain           : 61   Very large gain           :54  
##  Large gain                :104   Large gain                :80  
##  Moderate gain             : 46   Moderate gain             :54  
##  Small gain                : 10   Small gain                :26  
##  No gain or very small gain:  3   No gain or very small gain: 9  
##                                   NAs                       : 1  
##                                                                  
##                      Q10_13_post                      Q10_14_post
##  Very large gain           :108   Very large gain           :68  
##  Large gain                : 84   Large gain                :92  
##  Moderate gain             : 25   Moderate gain             :48  
##  Small gain                :  5   Small gain                :14  
##  No gain or very small gain:  1   No gain or very small gain: 1  
##  NAs                       :  1   NAs                       : 1  
##                                                                  
##                      Q10_15_post                     Q10_16_post 
##  Very large gain           :42   Very large gain           : 67  
##  Large gain                :90   Large gain                :108  
##  Moderate gain             :55   Moderate gain             : 37  
##  Small gain                :32   Small gain                :  9  
##  No gain or very small gain: 2   No gain or very small gain:  3  
##  NAs                       : 3                                   
##                                                                  
##                      Q10_17_post                     Q10_18_post
##  Very large gain           :57   Very large gain           :47  
##  Large gain                :66   Large gain                :93  
##  Moderate gain             :74   Moderate gain             :67  
##  Small gain                :20   Small gain                :14  
##  No gain or very small gain: 7   No gain or very small gain: 3  
##                                                                 
##                                                                 
##                      Q10_19_post                     Q10_20_post
##  Very large gain           :45   Very large gain           :63  
##  Large gain                :74   Large gain                :99  
##  Moderate gain             :64   Moderate gain             :45  
##  Small gain                :32   Small gain                :15  
##  No gain or very small gain: 9   No gain or very small gain: 2  
##                                                                 
##                                                                 
##                      Q10_21_post
##  Very large gain           :32  
##  Large gain                :62  
##  Moderate gain             :65  
##  Small gain                :45  
##  No gain or very small gain:17  
##  NAs                       : 3  
## 
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```
## [1] "In this section of the survey you will be asked to consider a variety of possible benefits you may have gained from your research experience. If for any reason you prefer not to answer, or consider the question irrelevant to you, please choose the Not applicable / Prefer not to answer option - "
```

```
## # A tibble: 21 × 2
##    value       Question                                                         
##    <chr>       <chr>                                                            
##  1 Q10_1_post  Clarification of a career path                                   
##  2 Q10_2_post  Skill in the interpretation of results                           
##  3 Q10_3_post  Tolerance for obstacles faced in the research process            
##  4 Q10_4_post  Readiness for more demanding research                            
##  5 Q10_5_post  Understanding how knowledge is constructed                       
##  6 Q10_6_post  Understanding of the research process in your field              
##  7 Q10_7_post  Ability to integrate theory and practice                         
##  8 Q10_8_post  Understanding of how scientists work on real problems            
##  9 Q10_9_post  Understanding that scientific assertions require supporting evid…
## 10 Q10_10_post Ability to analyze data and other information                    
## # ℹ 11 more rows
```

## Cleaning Demographics 

Note that we decided to simplify the race/ethnicity variable in the hopes of keeping a large enough
sample size for analysis. This is probably still necessary even though we have 3 years of results.


``` r
Q10 <- Q10Clean 
names(Q10) <- gsub("Q10_", "Q", names(Q10))
names(Q10) <- gsub("_post", "", names(Q10))

# Remove Prefer not to answer and recode to white and other race/ethnicity.
Q10_demo <- Q10 %>%
  filter(Ethnicity != "Prefer not to answer") %>% 
  mutate(`Race/Ethnicity` = recode(Ethnicity, 
                                   "White" = "White Students", "Filipino" = "Asian Students",
                                   "Asian American" = "Asian Students",
                                   .default = "Other Students of Color")) %>%
  mutate(`Race/Ethnicity` = factor(`Race/Ethnicity`, levels = c("White Students", "Asian Students", "Other Students of Color"))) %>%
  mutate(ClassYear = as.factor(recode(ClassYear, 
                                      "First" = "First", "Second" = "not First", "Third" = "not First"))) %>%
  select(-Ethnicity) %>%
  rename(Q01=Q1, Q02=Q2, Q03=Q3, Q04=Q4, Q05=Q5, Q06=Q6, Q07=Q7, Q08=Q8, Q09=Q9)

table(Q10$Ethnicity, Q10$Gender)
```

```
##                            
##                             Female Male Non-binary Prefer not to answer
##   Asian American                12    3          0                    0
##   Black or African American      6    2          0                    0
##   Filipino                       1    0          0                    0
##   Hispanic/Latino                5    1          1                    1
##   Other                          2    1          0                    0
##   Prefer not to answer           4    3          0                    0
##   Two or more races              9    2          0                    0
##   White                        119   52          0                    0
```

``` r
table(Q10$Ethnicity)
```

```
## 
##            Asian American Black or African American                  Filipino 
##                        15                         8                         1 
##           Hispanic/Latino                     Other      Prefer not to answer 
##                         8                         3                         7 
##         Two or more races                     White 
##                        11                       171
```

``` r
table(Q10$Gender)
```

```
## 
##               Female                 Male           Non-binary 
##                  158                   64                    1 
## Prefer not to answer 
##                    1
```

``` r
table(Q10_demo$`Race/Ethnicity`)
```

```
## 
##          White Students          Asian Students Other Students of Color 
##                     171                      16                      30
```

## Bar plots

![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 Bar-1.png)<!-- -->

With only one year of data, we did not look at each question by both Gender and Race. 
This was revisited when we got more data.


```
##                          
##                           Female Male Non-binary Prefer not to answer
##   White Students             119   52          0                    0
##   Asian Students              13    3          0                    0
##   Other Students of Color     22    6          1                    1
```

We will need to remove the Non-binary and Prefer not to answer students, but we can keep 
the Male and Female students separate.


```
##                          
##                           Female Male Non-binary Prefer not to answer
##   White Students             119   52          0                    0
##   Asian Students              13    3          0                    0
##   Other Students of Color     22    6          0                    0
```

The numbers of non-white students who are not first years is quite small (only 3).
We should not look at both class year and race together.


```
##                          
##                           First not First
##   White Students            160        11
##   Asian Students             15         1
##   Other Students of Color    26         2
```

Prepare the data to look at all of the questions at once.



![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 By Gender-1.png)<!-- -->

![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 By Race-1.png)<!-- -->

![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 By Class Year-1.png)<!-- -->

![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 By 6 Semesters-1.png)<!-- -->

Simplifying the semesters to compare Fall to Spring.


```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `across(Semester, str_replace, " 2021| 2022| 2023| 2024", "")`.
## Caused by warning:
## ! The `...` argument of `across()` is deprecated as of dplyr 1.1.0.
## Supply arguments directly to `.fns` through an anonymous function instead.
## 
##   # Previously
##   across(a:b, mean, na.rm = TRUE)
## 
##   # Now
##   across(a:b, \(x) mean(x, na.rm = TRUE))
```


![](Figures6-7.Post-Benefits.2026_files/figure-html/Q10 By Semester-1.png)<!-- -->

## Statistical models 

For each question, testing whether gender or race were significant.
I am also including semester because we expect students who have had a prior semester of the new Biology core to also have had some influence on their responses.

For this analysis we will consider all Fall and Spring semesters together.
We will also look at the first-year students versus the others. 

Should the sophomores and junior  be put into "Spring" for the previous experience analysis? 
(probably, but just leaving them out of the semester analysis is much easier)

Note that the dependent variable in the model is calculated as "5-as.numeric(Response)" because the responses range from 1=Very large gain to 5=No gain or very small gain. 
This converts the response variable into a numeric value from 0 to 4 with a positive estimate meaning an improvement in the response on the qualitative scale.

## Analysis by ClassYear and Gender {.tabset .tabset-pills}

### Q10_01


```
## [1] "Clarification of a career path"
```

```
## Start:  AIC=633.22
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   238.06 632.25
## <none>                  236.89 633.22
## 
## Step:  AIC=632.25
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   239.36 631.40
## - Gender     1   240.21 632.15
## <none>           238.06 632.25
## 
## Step:  AIC=631.4
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## <none>        239.36 631.40
## - Gender  1   241.83 631.57
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender, data = Q01_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.03974    0.08709  23.421   <2e-16 ***
## GenderMale  -0.23974    0.16332  -1.468    0.144    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.145271)
## 
##     Null deviance: 241.83  on 210  degrees of freedom
## Residual deviance: 239.36  on 209  degrees of freedom
## AIC: 631.4
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      239.36 631.40                     
## Gender  1   241.83 631.57      2.1643   0.1413
```

### Q10_02


```
## [1] "Skill in the interpretation of results"
```

```
## Start:  AIC=516.77
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   132.93 514.77
## <none>                  132.93 516.77
## 
## Step:  AIC=514.77
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   133.28 513.34
## <none>           132.93 514.77
## - Gender     1   134.38 515.10
## 
## Step:  AIC=513.34
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## <none>        133.28 513.34
## - Gender  1   134.62 513.49
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender, data = Q02_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.79221    0.06374  43.803   <2e-16 ***
## GenderMale   0.17501    0.11967   1.462    0.145    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6257515)
## 
##     Null deviance: 134.62  on 214  degrees of freedom
## Residual deviance: 133.29  on 213  degrees of freedom
## AIC: 513.34
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      133.28 513.34                     
## Gender  1   134.62 513.49      2.1478   0.1428
```

### Q10_03


```
## [1] "Tolerance for obstacles faced in the research process"
```

```
## Start:  AIC=527.11
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## <none>                  139.48 527.11
## - ClassYear:Gender  1   144.17 532.22
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q03_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.97945    0.06729  44.279  < 2e-16 ***
## ClassYearnot First            -0.97945    0.29523  -3.318  0.00107 ** 
## GenderMale                    -0.05218    0.12863  -0.406  0.68542    
## ClassYearnot First:GenderMale  1.21885    0.45755   2.664  0.00832 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6610464)
## 
##     Null deviance: 147.09  on 214  degrees of freedom
## Residual deviance: 139.48  on 211  degrees of freedom
## AIC: 527.11
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)   
## <none>                139.48 527.11                        
## ClassYear:Gender  1   144.17 532.22      7.1117 0.007658 **
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## # A tibble: 5 × 2
##   Response                       n
##   <ord>                      <int>
## 1 Very large gain               56
## 2 Large gain                    99
## 3 Moderate gain                 51
## 4 Small gain                     8
## 5 No gain or very small gain     1
```

### Q10_04


```
## [1] "Readiness for more demanding research"
```

```
## Start:  AIC=559.56
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   162.52 557.97
## <none>                  162.20 559.56
## 
## Step:  AIC=557.97
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   162.59 556.07
## <none>           162.52 557.97
## - ClassYear  1   165.79 560.27
## 
## Step:  AIC=556.07
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## <none>           162.59 556.07
## - ClassYear  1   165.97 558.50
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear, data = Q04_select)
## 
## Coefficients:
##                    Estimate Std. Error t value Pr(>|t|)    
## (Intercept)         2.86567    0.06162  46.502   <2e-16 ***
## ClassYearnot First -0.50853    0.24150  -2.106   0.0364 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7633212)
## 
##     Null deviance: 165.97  on 214  degrees of freedom
## Residual deviance: 162.59  on 213  degrees of freedom
## AIC: 556.07
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear
##           Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>         162.59 556.07                       
## ClassYear  1   165.97 558.50      4.4298  0.03532 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_05


```
## [1] "Understanding how knowledge is constructed"
```

```
## Start:  AIC=544.13
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   153.88 543.21
## <none>                  153.10 544.13
## 
## Step:  AIC=543.21
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   154.41 541.95
## - Gender     1   154.56 542.15
## <none>           153.88 543.21
## 
## Step:  AIC=541.95
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   155.00 540.76
## <none>        154.41 541.95
## 
## Step:  AIC=540.76
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q05_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.67136    0.05859    45.6   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7311099)
## 
##     Null deviance: 155  on 212  degrees of freedom
## Residual deviance: 155  on 212  degrees of freedom
## AIC: 540.76
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>         155 540.76
```

### Q10_06


```
## [1] "Understanding of the research process in your field"
```

```
## Start:  AIC=521.98
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   137.12 520.04
## <none>                  137.08 521.98
## 
## Step:  AIC=520.04
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   137.61 518.82
## - ClassYear  1   138.04 519.48
## <none>           137.12 520.04
## 
## Step:  AIC=518.82
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   138.44 518.09
## <none>           137.61 518.82
## 
## Step:  AIC=518.09
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q06_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.94860    0.05511    53.5   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6499276)
## 
##     Null deviance: 138.43  on 213  degrees of freedom
## Residual deviance: 138.43  on 213  degrees of freedom
## AIC: 518.09
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      138.44 518.09
```

### Q10_07


```
## [1] "Ability to integrate theory and practice"
```

```
## Start:  AIC=566.68
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   170.22 564.72
## <none>                  170.20 566.68
## 
## Step:  AIC=564.72
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   171.12 563.83
## <none>           170.22 564.72
## - Gender     1   172.19 565.17
## 
## Step:  AIC=563.83
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## <none>        171.12 563.83
## - Gender  1   172.88 564.02
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender, data = Q07_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.58553    0.07304  35.397   <2e-16 ***
## GenderMale   0.20136    0.13649   1.475    0.142    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8109842)
## 
##     Null deviance: 172.88  on 212  degrees of freedom
## Residual deviance: 171.12  on 211  degrees of freedom
## AIC: 563.83
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      171.12 563.83                     
## Gender  1   172.88 564.02      2.1857   0.1393
```

### Q10_08


```
## [1] "Understanding of how scientists work on real problems"
```

```
## Start:  AIC=534.81
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## <none>                  144.57 534.81
## - ClassYear:Gender  1   146.10 535.08
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q08_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                     2.9726     0.0685  43.393   <2e-16 ***
## ClassYearnot First             -0.4726     0.3006  -1.572    0.117    
## GenderMale                      0.1365     0.1310   1.042    0.299    
## ClassYearnot First:GenderMale   0.6968     0.4658   1.496    0.136    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6851621)
## 
##     Null deviance: 148.00  on 214  degrees of freedom
## Residual deviance: 144.57  on 211  degrees of freedom
## AIC: 534.81
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                144.57 534.81                     
## ClassYear:Gender  1   146.10 535.08      2.2683    0.132
```

### Q10_09


```
## [1] "Understanding that scientific assertions require supporting evidence"
```

```
## Start:  AIC=560.45
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   163.56 559.36
## <none>                  162.88 560.45
## 
## Step:  AIC=559.36
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   163.58 557.38
## - Gender     1   164.40 558.45
## <none>           163.56 559.36
## 
## Step:  AIC=557.38
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   164.44 556.50
## <none>        163.58 557.38
## 
## Step:  AIC=556.5
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q09_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.05116    0.05978   51.04   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7683982)
## 
##     Null deviance: 164.44  on 214  degrees of freedom
## Residual deviance: 164.44  on 214  degrees of freedom
## AIC: 556.5
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance   AIC scaled dev. Pr(>Chi)
## <none>      164.44 556.5
```

### Q10_10


```
## [1] "Ability to analyze data and other information"
```

```
## Start:  AIC=531.57
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   143.06 530.55
## <none>                  142.41 531.57
## 
## Step:  AIC=530.55
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   143.72 529.55
## - ClassYear  1   143.94 529.88
## <none>           143.06 530.55
## 
## Step:  AIC=529.55
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   144.49 528.70
## <none>           143.72 529.55
## 
## Step:  AIC=528.7
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q10_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.08372    0.05604   55.03   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.675201)
## 
##     Null deviance: 144.49  on 214  degrees of freedom
## Residual deviance: 144.49  on 214  degrees of freedom
## AIC: 528.7
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance   AIC scaled dev. Pr(>Chi)
## <none>      144.49 528.7
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance   AIC scaled dev. Pr(>Chi)
## <none>      144.49 528.7
```

### Q10_11


```
## [1] "Understanding science"
```

```
## Start:  AIC=563.44
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## <none>                  165.16 563.44
## - ClassYear:Gender  1   167.23 564.12
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q11_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.91096    0.07322  39.756   <2e-16 ***
## ClassYearnot First            -0.66096    0.32125  -2.057   0.0409 *  
## GenderMale                     0.10722    0.13997   0.766   0.4445    
## ClassYearnot First:GenderMale  0.80944    0.49789   1.626   0.1055    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7827375)
## 
##     Null deviance: 169.66  on 214  degrees of freedom
## Residual deviance: 165.16  on 211  degrees of freedom
## AIC: 563.44
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                165.16 563.44                     
## ClassYear:Gender  1   167.23 564.12      2.6765   0.1018
```

### Q10_12


```
## [1] "Learning ethical conduct in your field"
```

```
## Start:  AIC=650.67
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   251.18 649.59
## <none>                  250.11 650.67
## 
## Step:  AIC=649.59
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   251.24 647.64
## <none>           251.18 649.59
## - ClassYear  1   253.69 649.72
## 
## Step:  AIC=647.64
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## <none>           251.24 647.64
## - ClassYear  1   253.84 647.84
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear, data = Q12_select)
## 
## Coefficients:
##                    Estimate Std. Error t value Pr(>|t|)    
## (Intercept)         2.66000    0.07698  34.556   <2e-16 ***
## ClassYearnot First -0.44571    0.30096  -1.481     0.14    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.185081)
## 
##     Null deviance: 253.84  on 213  degrees of freedom
## Residual deviance: 251.24  on 212  degrees of freedom
## AIC: 647.64
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear
##           Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>         251.24 647.64                     
## ClassYear  1   253.84 647.84      2.2027   0.1378
```

### Q10_13


```
## [1] "Learning laboratory techniques"
```

```
## Start:  AIC=522.77
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   137.59 522.18
## <none>                  136.69 522.77
## 
## Step:  AIC=522.18
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   137.62 520.22
## - Gender     1   137.70 520.35
## <none>           137.59 522.18
## 
## Step:  AIC=520.22
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   137.74 518.41
## <none>        137.62 520.22
## 
## Step:  AIC=518.41
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q13_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.30698    0.05471   60.44   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6436427)
## 
##     Null deviance: 137.74  on 214  degrees of freedom
## Residual deviance: 137.74  on 214  degrees of freedom
## AIC: 518.41
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      137.74 518.41
```

### Q10_14


```
## [1] "Ability to read and understand primary literature"
```

```
## Start:  AIC=567.52
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   169.59 565.53
## <none>                  169.58 567.52
## 
## Step:  AIC=565.53
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   169.97 564.01
## - ClassYear  1   171.06 565.38
## <none>           169.59 565.53
## 
## Step:  AIC=564.01
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   171.33 563.71
## <none>           169.97 564.01
## 
## Step:  AIC=563.71
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q14_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.94393    0.06131   48.02   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8043526)
## 
##     Null deviance: 171.33  on 213  degrees of freedom
## Residual deviance: 171.33  on 213  degrees of freedom
## AIC: 563.71
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      171.33 563.71
```

### Q10_15


```
## [1] "Skill in how to give an effective oral presentation"
```

```
## Start:  AIC=598.9
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   201.17 598.52
## <none>                  199.64 598.90
## 
## Step:  AIC=598.52
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   201.20 596.55
## - ClassYear  1   202.71 598.13
## <none>           201.17 598.52
## 
## Step:  AIC=596.55
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   202.72 596.14
## <none>           201.20 596.55
## 
## Step:  AIC=596.14
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q15_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.60377    0.06732   38.68   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.960744)
## 
##     Null deviance: 202.72  on 211  degrees of freedom
## Residual deviance: 202.72  on 211  degrees of freedom
## AIC: 596.14
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      202.72 596.14
```

### Q10_16


```
## [1] "Skill in science writing"
```

```
## Start:  AIC=557.17
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   160.85 555.75
## <none>                  160.41 557.17
## 
## Step:  AIC=555.75
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   160.85 553.76
## - Gender     1   160.96 553.90
## <none>           160.85 555.75
## 
## Step:  AIC=553.76
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   160.96 551.90
## <none>        160.85 553.76
## 
## Step:  AIC=551.9
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q16_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.01395    0.05915   50.96   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7521408)
## 
##     Null deviance: 160.96  on 214  degrees of freedom
## Residual deviance: 160.96  on 214  degrees of freedom
## AIC: 551.9
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance   AIC scaled dev. Pr(>Chi)
## <none>      160.96 551.9
```

### Q10_17


```
## [1] "Self-confidence"
```

```
## Start:  AIC=642.23
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   238.31 640.28
## <none>                  238.26 642.23
## 
## Step:  AIC=640.28
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   238.42 638.38
## - ClassYear  1   238.44 638.39
## <none>           238.31 640.28
## 
## Step:  AIC=638.38
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   238.53 636.47
## <none>           238.42 638.38
## 
## Step:  AIC=636.47
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q17_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    2.656      0.072   36.88   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.114627)
## 
##     Null deviance: 238.53  on 214  degrees of freedom
## Residual deviance: 238.53  on 214  degrees of freedom
## AIC: 636.47
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      238.53 636.47
```

### Q10_18


```
## [1] "Understanding of how scientists think"
```

```
## Start:  AIC=562.6
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## <none>                  164.51 562.60
## - ClassYear:Gender  1   166.43 563.08
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q18_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.73973    0.07308  37.491   <2e-16 ***
## ClassYearnot First            -0.61473    0.32062  -1.917   0.0566 .  
## GenderMale                     0.09664    0.13970   0.692   0.4899    
## ClassYearnot First:GenderMale  0.77836    0.49691   1.566   0.1188    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7796771)
## 
##     Null deviance: 168.44  on 214  degrees of freedom
## Residual deviance: 164.51  on 211  degrees of freedom
## AIC: 562.6
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                164.51 562.60                     
## ClassYear:Gender  1   166.43 563.08      2.4857   0.1149
```

### Q10_19


```
## [1] "Learning to work independently"
```

```
## Start:  AIC=646.04
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   242.60 644.11
## <none>                  242.51 646.04
## 
## Step:  AIC=644.11
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - ClassYear  1   242.60 642.11
## - Gender     1   243.65 643.04
## <none>           242.60 644.11
## 
## Step:  AIC=642.11
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   243.66 641.04
## <none>        242.60 642.11
## 
## Step:  AIC=641.04
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q19_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.52093    0.07277   34.64   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.138579)
## 
##     Null deviance: 243.66  on 214  degrees of freedom
## Residual deviance: 243.66  on 214  degrees of freedom
## AIC: 641.04
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      243.66 641.04
```

### Q10_20


```
## [1] "Becoming part of a learning community"
```

```
## Start:  AIC=569.69
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## - ClassYear:Gender  1   170.50 568.28
## <none>                  170.03 569.69
## 
## Step:  AIC=568.28
## 5 - as.numeric(Response) ~ ClassYear + Gender
## 
##             Df Deviance    AIC
## - Gender     1   170.52 566.31
## - ClassYear  1   170.80 566.67
## <none>           170.50 568.28
## 
## Step:  AIC=566.31
## 5 - as.numeric(Response) ~ ClassYear
## 
##             Df Deviance    AIC
## - ClassYear  1   170.81 564.67
## <none>           170.52 566.31
## 
## Step:  AIC=564.67
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q20_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.92558    0.06093   48.02   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7981743)
## 
##     Null deviance: 170.81  on 214  degrees of freedom
## Residual deviance: 170.81  on 214  degrees of freedom
## AIC: 564.67
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      170.81 564.67
```

### Q10_21


```
## [1] "Confidence in my potential to be a teacher of science"
```

```
## Start:  AIC=660.7
## 5 - as.numeric(Response) ~ ClassYear * Gender
## 
##                    Df Deviance    AIC
## <none>                  267.21 660.70
## - ClassYear:Gender  1   274.02 664.03
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q21_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.20979    0.09478  23.314   <2e-16 ***
## ClassYearnot First            -0.83479    0.41179  -2.027   0.0439 *  
## GenderMale                    -0.00979    0.17984  -0.054   0.9566    
## ClassYearnot First:GenderMale  1.46812    0.63800   2.301   0.0224 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.284686)
## 
##     Null deviance: 275.07  on 211  degrees of freedom
## Residual deviance: 267.21  on 208  degrees of freedom
## AIC: 660.7
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>                267.21 660.70                       
## ClassYear:Gender  1   274.02 664.03      5.3295  0.02097 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## Summary of Analysis by ClassYear and Gender

**This analysis was not changed by separating Asian Students**

Most of the questions depended on Gender alone, but that question is better dealt in Model 2.

The following questions showed dependence on Class Year alone:

- Q10_04: "Readiness for more demanding research" Estimate -0.508 for not First Year.
- Q10_12: "Learning ethical conduct in your field" Estimate -0.446 for not First Year.

The following questions showed dependence on Class Year and also an interaction with Gender:

- Q10_03: "Tolerance for obstacles faced in the research process" Estimate -0.979 for not First Year.
- Q10_08: "Understanding of how scientists work on real problems" Estimate -0.473 for not First Year.
- Q10_11: "Understanding science" Estimate -0.660 for not First Year.
- Q10_18: "Understanding of how scientists think" Estimate -0.615 for not First Year.
- Q10_21: "Confidence in my potential to be a teacher of science" Estimate -0.835 for not First Year.

For each of these questions that maintained the interaction during model selection, 
the effect size of the interaction was always at least as large (and usually larger) than that of Class Year alone. 
This means that male students who were not first-year students showed similar perceived benefits as first-year students, 
while female students showed significantly less perceived benefits. 

Here are the cat_plots showing these interactions:


```
## Using data Q03_select from global environment. This could cause incorrect
## results if Q03_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/Interaction Q10_03-1.png)<!-- -->



```
## Using data Q08_select from global environment. This could cause incorrect
## results if Q08_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/Interaction Q10_08-1.png)<!-- -->


```
## Using data Q11_select from global environment. This could cause incorrect
## results if Q11_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/Interaction Q10_11-1.png)<!-- -->


```
## Using data Q18_select from global environment. This could cause incorrect
## results if Q18_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/Interaction Q10_18-1.png)<!-- -->


```
## Using data Q21_select from global environment. This could cause incorrect
## results if Q21_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-7.Post-Benefits.2026_files/figure-html/Interaction Q10_32-1.png)<!-- -->


## Analysis by Semester and Demo {.tabset .tabset-pills}

For the new and improved analysis, we have removed the interaction between gender and race/ethnicity, separated asian students from other students of color, and also changed to using an alpha value cutoff of 0.05 instead of AIC for model selection.

### Q10_01


```
## [1] "Clarification of a career path"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q01_select)
## 
## Coefficients:
##                                          Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.001531   0.135627  14.758   <2e-16
## SemesterSpring                           0.002742   0.153265   0.018    0.986
## GenderMale                              -0.249747   0.170300  -1.467    0.144
## `Race/Ethnicity`Asian Students           0.380472   0.287263   1.324    0.187
## `Race/Ethnicity`Other Students of Color  0.237645   0.226409   1.050    0.295
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.120659)
## 
##     Null deviance: 220.99  on 196  degrees of freedom
## Residual deviance: 215.17  on 192  degrees of freedom
## AIC: 588.44
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.000   1   0.0003 0.9857
## Gender             2.410   1   2.1507 0.1441
## `Race/Ethnicity`   2.862   2   1.2770 0.2812
## Residuals        215.166 192
```

None of Semester, Gender, or Race/Ethnicity meet the 0.05 alpha threshold.

### Q10_02


```
## [1] "Skill in the interpretation of results"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q02_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                               2.9885     0.1002  29.835   <2e-16
## SemesterSpring                           -0.2542     0.1127  -2.256   0.0252
## GenderMale                                0.1433     0.1251   1.145   0.2535
## `Race/Ethnicity`Asian Students           -0.1652     0.2126  -0.777   0.4380
## `Race/Ethnicity`Other Students of Color  -0.1777     0.1675  -1.061   0.2898
##                                            
## (Intercept)                             ***
## SemesterSpring                          *  
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6149077)
## 
##     Null deviance: 125.52  on 200  degrees of freedom
## Residual deviance: 120.52  on 196  degrees of freedom
## AIC: 479.61
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)  
## Semester           3.129   1   5.0879 0.0252 *
## Gender             0.806   1   1.3114 0.2535  
## `Race/Ethnicity`   0.956   2   0.7773 0.4611  
## Residuals        120.522 196                  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Semester meets the cutoff.

### Q10_03


```
## [1] "Tolerance for obstacles faced in the research process"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q03_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.04299    0.10616  28.665   <2e-16
## SemesterSpring                          -0.10597    0.11943  -0.887    0.376
## GenderMale                              -0.06206    0.13260  -0.468    0.640
## `Race/Ethnicity`Asian Students          -0.18112    0.22530  -0.804    0.422
## `Race/Ethnicity`Other Students of Color  0.09070    0.17748   0.511    0.610
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6906844)
## 
##     Null deviance: 136.76  on 200  degrees of freedom
## Residual deviance: 135.37  on 196  degrees of freedom
## AIC: 502.97
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.544   1   0.7874 0.3760
## Gender             0.151   1   0.2191 0.6403
## `Race/Ethnicity`   0.705   2   0.5105 0.6010
## Residuals        135.374 196
```

None meet the cutoff.

### Q10_04


```
## [1] "Readiness for more demanding research"
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values  Pr(>F)  
## Semester           2.830   1   3.7565 0.05404 .
## Gender             0.388   1   0.5157 0.47354  
## `Race/Ethnicity`   1.014   2   0.6731 0.51130  
## Residuals        147.645 196                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

None meet the cutoff.

### Q10_05


```
## [1] "Understanding how knowledge is constructed"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q05_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                               2.5890     0.1095  23.655   <2e-16
## SemesterSpring                            0.0653     0.1227   0.532    0.595
## GenderMale                                0.1037     0.1358   0.763    0.446
## `Race/Ethnicity`Asian Students            0.1598     0.2304   0.694    0.489
## `Race/Ethnicity`Other Students of Color   0.1327     0.1816   0.731    0.466
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7215782)
## 
##     Null deviance: 141.06  on 198  degrees of freedom
## Residual deviance: 139.99  on 194  degrees of freedom
## AIC: 506.74
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.204   1   0.2830 0.5953
## Gender             0.420   1   0.5822 0.4464
## `Race/Ethnicity`   0.653   2   0.4524 0.6368
## Residuals        139.986 194
```

None meet the cutoff.

### Q10_06


```
## [1] "Understanding of the research process in your field"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q06_select)
## 
## Coefficients:
##                                          Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.964140   0.101917  29.084   <2e-16
## SemesterSpring                          -0.005513   0.114786  -0.048    0.962
## GenderMale                               0.089670   0.127359   0.704    0.482
## `Race/Ethnicity`Asian Students          -0.112834   0.216263  -0.522    0.602
## `Race/Ethnicity`Other Students of Color -0.094436   0.170378  -0.554    0.580
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6360954)
## 
##     Null deviance: 124.76  on 199  degrees of freedom
## Residual deviance: 124.04  on 195  degrees of freedom
## AIC: 484.03
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.001   1   0.0023 0.9617
## Gender             0.315   1   0.4957 0.4822
## `Race/Ethnicity`   0.329   2   0.2586 0.7724
## Residuals        124.039 195
```

None meet the cutoff.

### Q10_07


```
## [1] "Ability to integrate theory and practice"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q07_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.48836    0.11564  21.517   <2e-16
## SemesterSpring                           0.21286    0.12969   1.641    0.102
## GenderMale                               0.23496    0.14355   1.637    0.103
## `Race/Ethnicity`Asian Students          -0.03469    0.24350  -0.142    0.887
## `Race/Ethnicity`Other Students of Color -0.08514    0.19192  -0.444    0.658
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8056103)
## 
##     Null deviance: 160.76  on 198  degrees of freedom
## Residual deviance: 156.29  on 194  degrees of freedom
## AIC: 528.66
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           2.170   1   2.6938 0.1024
## Gender             2.158   1   2.6791 0.1033
## `Race/Ethnicity`   0.165   2   0.1024 0.9028
## Residuals        156.288 194
```

None meet the cutoff.

### Q10_08


```
## [1] "Understanding of how scientists work on real problems"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q08_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.00281    0.10599  28.331   <2e-16
## SemesterSpring                          -0.03780    0.11924  -0.317    0.752
## GenderMale                               0.12988    0.13239   0.981    0.328
## `Race/Ethnicity`Asian Students           0.05552    0.22494   0.247    0.805
## `Race/Ethnicity`Other Students of Color -0.08871    0.17719  -0.501    0.617
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6884917)
## 
##     Null deviance: 135.98  on 200  degrees of freedom
## Residual deviance: 134.94  on 196  degrees of freedom
## AIC: 502.33
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.069   1   0.1005 0.7515
## Gender             0.663   1   0.9624 0.3278
## `Race/Ethnicity`   0.238   2   0.1732 0.8411
## Residuals        134.944 196
```

None meet the cutoff.

### Q10_09


```
## [1] "Understanding that scientific assertions require supporting evidence"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q09_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.17771    0.11099  28.631   <2e-16
## SemesterSpring                          -0.05568    0.12486  -0.446    0.656
## GenderMale                              -0.19574    0.13863  -1.412    0.160
## `Race/Ethnicity`Asian Students          -0.17925    0.23555  -0.761    0.448
## `Race/Ethnicity`Other Students of Color -0.19343    0.18555  -1.042    0.298
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7549719)
## 
##     Null deviance: 150.40  on 200  degrees of freedom
## Residual deviance: 147.97  on 196  degrees of freedom
## AIC: 520.85
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.150   1   0.1988 0.6561
## Gender             1.505   1   1.9937 0.1595
## `Race/Ethnicity`   1.130   2   0.7483 0.4745
## Residuals        147.974 196
```

None meet the cutoff.

### Q10_10


```
## [1] "Ability to analyze data and other information"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q10_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.06648    0.10698  28.664   <2e-16
## SemesterSpring                          -0.01049    0.12035  -0.087    0.931
## GenderMale                               0.09253    0.13362   0.692    0.489
## `Race/Ethnicity`Asian Students           0.11991    0.22704   0.528    0.598
## `Race/Ethnicity`Other Students of Color  0.03555    0.17885   0.199    0.843
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7014088)
## 
##     Null deviance: 138.01  on 200  degrees of freedom
## Residual deviance: 137.48  on 196  degrees of freedom
## AIC: 506.06
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.005   1   0.0076 0.9307
## Gender             0.336   1   0.4795 0.4894
## `Race/Ethnicity`   0.209   2   0.1487 0.8619
## Residuals        137.476 196
```

None meet the cutoff.

### Q10_11


```
## [1] "Understanding science"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q11_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.93016    0.11312  25.904   <2e-16
## SemesterSpring                          -0.08526    0.12726  -0.670    0.504
## GenderMale                               0.11041    0.14129   0.781    0.435
## `Race/Ethnicity`Asian Students           0.08755    0.24007   0.365    0.716
## `Race/Ethnicity`Other Students of Color  0.16160    0.18911   0.855    0.394
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7842098)
## 
##     Null deviance: 155.28  on 200  degrees of freedom
## Residual deviance: 153.71  on 196  degrees of freedom
## AIC: 528.49
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.352   1   0.4489 0.5037
## Gender             0.479   1   0.6107 0.4355
## `Race/Ethnicity`   0.627   2   0.3999 0.6709
## Residuals        153.705 196
```

None meet the cutoff.

### Q10_12


```
## [1] "Learning ethical conduct in your field"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q12_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.69219    0.13980  19.258   <2e-16
## SemesterSpring                          -0.05641    0.15661  -0.360    0.719
## GenderMale                              -0.07990    0.17345  -0.461    0.646
## `Race/Ethnicity`Asian Students           0.21678    0.29441   0.736    0.462
## `Race/Ethnicity`Other Students of Color  0.03935    0.23202   0.170    0.865
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.178187)
## 
##     Null deviance: 230.88  on 199  degrees of freedom
## Residual deviance: 229.75  on 195  degrees of freedom
## AIC: 607.31
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.153   1   0.1298 0.7191
## Gender             0.250   1   0.2122 0.6456
## `Race/Ethnicity`   0.646   2   0.2744 0.7604
## Residuals        229.747 195
```

None meet the cutoff.

### Q10_13


```
## [1] "Learning laboratory techniques"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q13_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.50542    0.09898  35.416  < 2e-16
## SemesterSpring                          -0.34773    0.11135  -3.123  0.00206
## GenderMale                              -0.02512    0.12363  -0.203  0.83921
## `Race/Ethnicity`Asian Students           0.19521    0.21006   0.929  0.35387
## `Race/Ethnicity`Other Students of Color -0.12270    0.16547  -0.742  0.45926
##                                            
## (Intercept)                             ***
## SemesterSpring                          ** 
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6004204)
## 
##     Null deviance: 124.49  on 200  degrees of freedom
## Residual deviance: 117.68  on 196  degrees of freedom
## AIC: 474.81
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values   Pr(>F)   
## Semester           5.855   1   9.7518 0.002063 **
## Gender             0.025   1   0.0413 0.839214   
## `Race/Ethnicity`   0.962   2   0.8012 0.450254   
## Residuals        117.682 196                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Semester meets the cutoff.

### Q10_14


```
## [1] "Ability to read and understand primary literature"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q14_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.16677    0.11225  28.212   <2e-16
## SemesterSpring                          -0.32143    0.12649  -2.541   0.0118
## GenderMale                               0.05168    0.14109   0.366   0.7146
## `Race/Ethnicity`Asian Students          -0.02710    0.23804  -0.114   0.9095
## `Race/Ethnicity`Other Students of Color -0.27148    0.18752  -1.448   0.1493
##                                            
## (Intercept)                             ***
## SemesterSpring                          *  
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7709127)
## 
##     Null deviance: 156.76  on 199  degrees of freedom
## Residual deviance: 150.33  on 195  degrees of freedom
## AIC: 522.48
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values  Pr(>F)  
## Semester           4.978   1   6.4575 0.01183 *
## Gender             0.103   1   0.1342 0.71456  
## `Race/Ethnicity`   1.619   2   1.0499 0.35194  
## Residuals        150.328 195                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Semester meets the cutoff.

### Q10_15


```
## [1] "Skill in how to give an effective oral presentation"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q15_select)
## 
## Coefficients:
##                                          Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.439140   0.123260  19.789   <2e-16
## SemesterSpring                           0.268802   0.139188   1.931   0.0549
## GenderMale                               0.007226   0.155605   0.046   0.9630
## `Race/Ethnicity`Asian Students          -0.099360   0.260959  -0.381   0.7038
## `Race/Ethnicity`Other Students of Color  0.330361   0.205633   1.607   0.1098
##                                            
## (Intercept)                             ***
## SemesterSpring                          .  
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9255938)
## 
##     Null deviance: 184.34  on 197  degrees of freedom
## Residual deviance: 178.64  on 193  degrees of freedom
## AIC: 553.53
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values  Pr(>F)  
## Semester           3.452   1   3.7296 0.05492 .
## Gender             0.002   1   0.0022 0.96301  
## `Race/Ethnicity`   2.701   2   1.4589 0.23506  
## Residuals        178.640 193                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

None meet the cutoff.

### Q10_16


```
## [1] "Skill in science writing"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q16_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              3.07625    0.10994  27.982   <2e-16
## SemesterSpring                          -0.16432    0.12368  -1.329    0.186
## GenderMale                               0.06906    0.13731   0.503    0.616
## `Race/Ethnicity`Asian Students           0.05329    0.23331   0.228    0.820
## `Race/Ethnicity`Other Students of Color  0.05691    0.18379   0.310    0.757
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7407023)
## 
##     Null deviance: 146.96  on 200  degrees of freedom
## Residual deviance: 145.18  on 196  degrees of freedom
## AIC: 517.02
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           1.308   1   1.7652 0.1855
## Gender             0.187   1   0.2530 0.6156
## `Race/Ethnicity`   0.098   2   0.0665 0.9357
## Residuals        145.178 196
```

None meet the cutoff.

### Q10_17


```
## [1] "Self-confidence"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q17_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.77792    0.13187  21.066   <2e-16
## SemesterSpring                          -0.20585    0.14835  -1.388    0.167
## GenderMale                               0.01876    0.16471   0.114    0.909
## `Race/Ethnicity`Asian Students          -0.01895    0.27986  -0.068    0.946
## `Race/Ethnicity`Other Students of Color -0.04060    0.22045  -0.184    0.854
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.065699)
## 
##     Null deviance: 211.00  on 200  degrees of freedom
## Residual deviance: 208.88  on 196  degrees of freedom
## AIC: 590.14
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           2.052   1   1.9254 0.1668
## Gender             0.014   1   0.0130 0.9094
## `Race/Ethnicity`   0.038   2   0.0180 0.9822
## Residuals        208.877 196
```

None meet the cutoff.

### Q10_18


```
## [1] "Understanding of how scientists think"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q18_select)
## 
## Coefficients:
##                                          Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.801488   0.112865  24.822   <2e-16
## SemesterSpring                          -0.110040   0.126972  -0.867    0.387
## GenderMale                               0.087356   0.140973   0.620    0.536
## `Race/Ethnicity`Asian Students           0.032393   0.239529   0.135    0.893
## `Race/Ethnicity`Other Students of Color -0.002501   0.188687  -0.013    0.989
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7806991)
## 
##     Null deviance: 154.01  on 200  degrees of freedom
## Residual deviance: 153.02  on 196  degrees of freedom
## AIC: 527.59
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.586   1   0.7511 0.3872
## Gender             0.300   1   0.3840 0.5362
## `Race/Ethnicity`   0.015   2   0.0096 0.9905
## Residuals        153.017 196
```

None meet the cutoff.

### Q10_19


```
## [1] "Learning to work independently"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q19_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.58918    0.13462  19.233   <2e-16
## SemesterSpring                          -0.09029    0.15145  -0.596    0.552
## GenderMale                              -0.16558    0.16815  -0.985    0.326
## `Race/Ethnicity`Asian Students           0.21940    0.28571   0.768    0.443
## `Race/Ethnicity`Other Students of Color  0.09624    0.22506   0.428    0.669
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.110718)
## 
##     Null deviance: 220.15  on 200  degrees of freedom
## Residual deviance: 217.70  on 196  degrees of freedom
## AIC: 598.46
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.395   1   0.3554 0.5518
## Gender             1.077   1   0.9696 0.3260
## `Race/Ethnicity`   0.782   2   0.3519 0.7038
## Residuals        217.701 196
```

None meet the cutoff.

### Q10_20


```
## [1] "Becoming part of a learning community"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q20_select)
## 
## Coefficients:
##                                          Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.971865   0.114606  25.931   <2e-16
## SemesterSpring                          -0.072525   0.128932  -0.563    0.574
## GenderMale                              -0.014493   0.143149  -0.101    0.919
## `Race/Ethnicity`Asian Students          -0.001789   0.243226  -0.007    0.994
## `Race/Ethnicity`Other Students of Color  0.061605   0.191599   0.322    0.748
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8049833)
## 
##     Null deviance: 158.16  on 200  degrees of freedom
## Residual deviance: 157.78  on 196  degrees of freedom
## AIC: 533.75
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           0.255   1   0.3164 0.5744
## Gender             0.008   1   0.0102 0.9195
## `Race/Ethnicity`   0.085   2   0.0528 0.9486
## Residuals        157.777 196
```

None meet the cutoff.

### Q10_21


```
## [1] "Confidence in my potential to be a teacher of science"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender + 
##     `Race/Ethnicity`, data = Q21_select)
## 
## Coefficients:
##                                         Estimate Std. Error t value Pr(>|t|)
## (Intercept)                              2.13524    0.14206  15.030   <2e-16
## SemesterSpring                           0.24648    0.16037   1.537    0.126
## GenderMale                              -0.01408    0.17770  -0.079    0.937
## `Race/Ethnicity`Asian Students          -0.31411    0.30136  -1.042    0.299
## `Race/Ethnicity`Other Students of Color -0.27527    0.23747  -1.159    0.248
##                                            
## (Intercept)                             ***
## SemesterSpring                             
## GenderMale                                 
## `Race/Ethnicity`Asian Students             
## `Race/Ethnicity`Other Students of Color    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.233925)
## 
##     Null deviance: 244.51  on 197  degrees of freedom
## Residual deviance: 238.15  on 193  degrees of freedom
## AIC: 610.46
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
## Error estimate based on Pearson residuals 
## 
##                   Sum Sq  Df F values Pr(>F)
## Semester           2.915   1   2.3620 0.1260
## Gender             0.008   1   0.0063 0.9369
## `Race/Ethnicity`   2.675   2   1.0840 0.3403
## Residuals        238.148 193
```

None meet the cutoff. 

## Summary

**UPDATED TO INDICATE THE EFFECT OF SEPARATING ASIAN STUDENTS**

The reanalysis shows that neither Gender or Race/Ethnicity were significant in any of the models.

*Indicates stayed the same*

Note that only first-year students are included in this analysis.

Most of the responses showed a dependence on the semester, although the significance was sometimes marginal. 
This needs to be interpreted carefully as the responses in the Spring semester, if different, were always less positive than in the Fall. 
That is, the students in the Spring perceived less of a gain in these measures due to the CURE Lab than those in the Fall semester. 
This is consistent with the hypothesis that BIO Seminar provided some gain in these measures and, therefore, the gains due to the CURE Lab were smaller. 

Questions that showed dependence on the Semester, but not Race or Gender:

*2*   
4 <- NOW NONE 
*13*    
*14*    
15  <- NOW NONE   
16   <- NOW NONE  

Question that showed dependence on Gender and Race, but not Semester:

1 <- NOW NONE

Question that showed dependence on Semester and Race, but not Gender: 

21 <- NOW NONE

Question that showed dependence on Semester and Gender, but not Race: 

7 <- NOW NONE

Questions that showed dependence on Race, Gender, and the interaction between Race and Gender, but not Semester:

6 <- NOW NONE
9 <- NOW NONE
12 <- NOW NONE

Questions that did not depend on Semester, Race, or Gender:

*3*   
*5* 
*8*   
*10*    
*11*    
*17*    
*18*    
*19*    
*20*

Here is the list of questions for reference:

Preface: In this section of the survey you will be asked to consider a variety of possible benefits you may have gained from your research experience. If for any reason you prefer not to answer, or consider the question irrelevant to you, please choose the "Not applicable / Prefer not to answer" option 

 [1] "Clarification of a career path"                                      
 [2] "Skill in the interpretation of results"                              
 [3] "Tolerance for obstacles faced in the research process"               
 [4] "Readiness for more demanding research"                               
 [5] "Understanding how knowledge is constructed"                          
 [6] "Understanding of the research process in your field"                 
 [7] "Ability to integrate theory and practice"                            
 [8] "Understanding of how scientists work on real problems"               
 [9] "Understanding that scientific assertions require supporting evidence"
[10] "Ability to analyze data and other information"                       
[11] "Understanding science"                                               
[12] "Learning ethical conduct in your field"                              
[13] "Learning laboratory techniques"                                      
[14] "Ability to read and understand primary literature"                   
[15] "Skill in how to give an effective oral presentation"                 
[16] "Skill in science writing"                                            
[17] "Self-confidence"                                                     
[18] "Understanding of how scientists think"                               
[19] "Learning to work independently"                                      
[20] "Becoming part of a learning community"                               
[21] "Confidence in my potential to be a teacher of science"

## Final Figures


### 2026 Final Figure 6

*2*   
*13*    
*14*  

![](Figures6-7.Post-Benefits.2026_files/figure-html/Figure 6-1.png)<!-- -->

### 2026 Final Figure 7 (Was 8)

Everything except 
*2*   
*13*    
*14*  

![](Figures6-7.Post-Benefits.2026_files/figure-html/Figure 7-1.png)<!-- -->
