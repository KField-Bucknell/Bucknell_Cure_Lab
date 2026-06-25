---
title: "Post Benefits Question"
author: "Ken Field"
date: "Last compiled on 25 June 2026"
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
##    Semester                          Gender                        Ethnicity  
##  Length:224         Female              :158   White                    :171  
##  Class :character   Male                : 64   Asian American           : 15  
##  Mode  :character   Non-binary          :  1   Two or more races        : 11  
##                     Prefer not to answer:  1   Black or African American:  8  
##                                                Hispanic/Latino          :  8  
##                                                Prefer not to answer     :  7  
##                                                (Other)                  :  4  
##   ClassYear                        Q10_1_post                      Q10_2_post 
##  First :209   Very large gain           :21   Very large gain           : 42  
##  Second: 14   Large gain                :43   Large gain                :118  
##  Third :  1   Moderate gain             :85   Moderate gain             : 53  
##               Small gain                :51   Small gain                :  9  
##               No gain or very small gain:20   No gain or very small gain:  2  
##               NA's                      : 4                                   
##                                                                               
##                       Q10_3_post                       Q10_4_post
##  Very large gain           : 57   Very large gain           :55  
##  Large gain                :104   Large gain                :95  
##  Moderate gain             : 53   Moderate gain             :62  
##  Small gain                :  8   Small gain                :10  
##  No gain or very small gain:  1   No gain or very small gain: 2  
##  NA's                      :  1                                  
##                                                                  
##                       Q10_5_post                       Q10_6_post 
##  Very large gain           : 33   Very large gain           : 54  
##  Large gain                :106   Large gain                :115  
##  Moderate gain             : 65   Moderate gain             : 44  
##  Small gain                : 16   Small gain                :  9  
##  No gain or very small gain:  2   No gain or very small gain:  1  
##  NA's                      :  2   NA's                      :  1  
##                                                                   
##                       Q10_7_post                      Q10_8_post 
##  Very large gain           :36   Very large gain           : 66  
##  Large gain                :96   Large gain                :104  
##  Moderate gain             :66   Moderate gain             : 42  
##  Small gain                :22   Small gain                : 10  
##  No gain or very small gain: 2   No gain or very small gain:  2  
##  NA's                      : 2                                   
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
##                                   NA's                      : 1  
##                                                                  
##                      Q10_13_post                      Q10_14_post
##  Very large gain           :108   Very large gain           :68  
##  Large gain                : 84   Large gain                :92  
##  Moderate gain             : 25   Moderate gain             :48  
##  Small gain                :  5   Small gain                :14  
##  No gain or very small gain:  1   No gain or very small gain: 1  
##  NA's                      :  1   NA's                      : 1  
##                                                                  
##                      Q10_15_post                     Q10_16_post 
##  Very large gain           :42   Very large gain           : 67  
##  Large gain                :90   Large gain                :108  
##  Moderate gain             :55   Moderate gain             : 37  
##  Small gain                :32   Small gain                :  9  
##  No gain or very small gain: 2   No gain or very small gain:  3  
##  NA's                      : 3                                   
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
##  NA's                      : 3  
## 
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

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

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 Bar-1.png)<!-- -->

Because of the small numbers, we will not look at each question by both Gender and Race. 
This needs to be revisited now that we have more data.


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



![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Gender-1.png)<!-- -->

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Race-1.png)<!-- -->

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Class Year-1.png)<!-- -->

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By 6 Semesters-1.png)<!-- -->

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


![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Semester-1.png)<!-- -->

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

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction Q10_03-1.png)<!-- -->



```
## Using data Q08_select from global environment. This could cause incorrect
## results if Q08_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction Q10_08-1.png)<!-- -->


```
## Using data Q11_select from global environment. This could cause incorrect
## results if Q11_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction Q10_11-1.png)<!-- -->


```
## Using data Q18_select from global environment. This could cause incorrect
## results if Q18_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction Q10_18-1.png)<!-- -->


```
## Using data Q21_select from global environment. This could cause incorrect
## results if Q21_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction Q10_32-1.png)<!-- -->


## Analysis by Semester and Demo {.tabset .tabset-pills}

### Q10_01


```
## [1] "Clarification of a career path"
```

```
## Start:  AIC=591.32
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   215.17 588.44
## - Semester                 1   213.96 589.33
## <none>                         213.95 591.32
## 
## Step:  AIC=588.44
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - Semester          1   215.17 586.44
## - `Race/Ethnicity`  2   218.03 587.04
## <none>                  215.17 588.44
## - Gender            1   217.58 588.63
## 
## Step:  AIC=586.44
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   218.06 585.07
## <none>                  215.17 586.44
## - Gender            1   217.60 586.65
## 
## Step:  AIC=585.07
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## <none>        218.06 585.07
## - Gender  1   221.00 585.70
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender, data = Q01_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.06993    0.08843   23.41   <2e-16 ***
## GenderMale  -0.27363    0.16890   -1.62    0.107    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.118256)
## 
##     Null deviance: 220.99  on 196  degrees of freedom
## Residual deviance: 218.06  on 195  degrees of freedom
## AIC: 585.07
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      218.06 585.07                     
## Gender  1   221.00 585.70      2.6338   0.1046
```

### Q10_02


```
## [1] "Skill in the interpretation of results"
```

```
## Start:  AIC=481.96
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   120.52 479.61
## <none>                         119.54 481.96
## - Semester                 1   122.43 484.77
## 
## Step:  AIC=479.61
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   121.48 477.20
## - Gender            1   121.33 478.95
## <none>                  120.52 479.61
## - Semester          1   123.65 482.76
## 
## Step:  AIC=477.2
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   122.48 476.85
## <none>          121.48 477.20
## - Semester  1   124.22 479.68
## 
## Step:  AIC=476.85
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          122.48 476.85
## - Semester  1   125.52 479.78
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q02_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     2.98876    0.08316  35.940   <2e-16 ***
## SemesterSpring -0.24769    0.11140  -2.223   0.0273 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6154766)
## 
##     Null deviance: 125.52  on 200  degrees of freedom
## Residual deviance: 122.48  on 199  degrees of freedom
## AIC: 476.85
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>        122.48 476.85                       
## Semester  1   125.52 479.78      4.9321  0.02636 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_03


```
## [1] "Tolerance for obstacles faced in the research process"
```

```
## Start:  AIC=506.52
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   135.37 502.97
## - Semester                 1   135.68 505.42
## <none>                         135.07 506.52
## 
## Step:  AIC=502.97
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   136.08 500.01
## - Gender            1   135.53 501.19
## - Semester          1   135.92 501.77
## <none>                  135.37 502.97
## 
## Step:  AIC=500.01
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   136.23 498.23
## - Semester  1   136.65 498.85
## <none>          136.08 500.01
## 
## Step:  AIC=498.23
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## - Semester  1   136.76 497.01
## <none>          136.23 498.23
## 
## Step:  AIC=497.01
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q03_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.96517    0.05833   50.84   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6837811)
## 
##     Null deviance: 136.76  on 200  degrees of freedom
## Residual deviance: 136.76  on 200  degrees of freedom
## AIC: 497.01
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      136.76 497.01
```

```
## # A tibble: 5 × 2
##   Response                       n
##   <ord>                      <int>
## 1 Very large gain               54
## 2 Large gain                    96
## 3 Moderate gain                 42
## 4 Small gain                     8
## 5 No gain or very small gain     1
```

### Q10_04


```
## [1] "Readiness for more demanding research"
```

```
## Start:  AIC=520.05
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## <none>                         144.48 520.05
## - Gender:`Race/Ethnicity`  2   147.65 520.41
## - Semester                 1   146.78 521.23
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender * 
##     `Race/Ethnicity`, data = Q04_select)
## 
## Coefficients:
##                                                    Estimate Std. Error t value
## (Intercept)                                         3.02949    0.11200  27.049
## SemesterSpring                                     -0.21904    0.12456  -1.759
## GenderMale                                         -0.04530    0.15040  -0.301
## `Race/Ethnicity`Asian Students                     -0.02156    0.26323  -0.082
## `Race/Ethnicity`Other Students of Color            -0.11566    0.20585  -0.562
## GenderMale:`Race/Ethnicity`Asian Students          -1.14994    0.57857  -1.988
## GenderMale:`Race/Ethnicity`Other Students of Color  0.17528    0.45528   0.385
##                                                    Pr(>|t|)    
## (Intercept)                                          <2e-16 ***
## SemesterSpring                                       0.0802 .  
## GenderMale                                           0.7636    
## `Race/Ethnicity`Asian Students                       0.9348    
## `Race/Ethnicity`Other Students of Color              0.5748    
## GenderMale:`Race/Ethnicity`Asian Students            0.0483 *  
## GenderMale:`Race/Ethnicity`Other Students of Color   0.7007    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7447324)
## 
##     Null deviance: 151.37  on 200  degrees of freedom
## Residual deviance: 144.48  on 194  degrees of freedom
## AIC: 520.05
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
##                         Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>                       144.48 520.05                       
## Semester                 1   146.78 521.23      3.1788   0.0746 .
## Gender:`Race/Ethnicity`  2   147.65 520.41      4.3578   0.1132  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_05


```
## [1] "Understanding how knowledge is constructed"
```

```
## Start:  AIC=505.98
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Semester                 1   137.05 504.52
## <none>                         136.68 505.98
## - Gender:`Race/Ethnicity`  2   139.99 506.74
## 
## Step:  AIC=504.52
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
## 
##                           Df Deviance    AIC
## <none>                         137.05 504.52
## - Gender:`Race/Ethnicity`  2   140.19 505.03
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + 
##     Gender:`Race/Ethnicity`, data = Q05_select)
## 
## Coefficients:
##                                                    Estimate Std. Error t value
## (Intercept)                                         2.61261    0.07998  32.664
## GenderMale                                          0.15334    0.14665   1.046
## `Race/Ethnicity`Asian Students                      0.38739    0.25607   1.513
## `Race/Ethnicity`Other Students of Color             0.10167    0.20053   0.507
## GenderMale:`Race/Ethnicity`Asian Students          -1.15334    0.56337  -2.047
## GenderMale:`Race/Ethnicity`Other Students of Color  0.13237    0.44424   0.298
##                                                    Pr(>|t|)    
## (Intercept)                                          <2e-16 ***
## GenderMale                                            0.297    
## `Race/Ethnicity`Asian Students                        0.132    
## `Race/Ethnicity`Other Students of Color               0.613    
## GenderMale:`Race/Ethnicity`Asian Students             0.042 *  
## GenderMale:`Race/Ethnicity`Other Students of Color    0.766    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7101222)
## 
##     Null deviance: 141.06  on 198  degrees of freedom
## Residual deviance: 137.05  on 193  degrees of freedom
## AIC: 504.52
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
##                         Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                       137.05 504.52                     
## Gender:`Race/Ethnicity`  2   140.19 505.03      4.5033   0.1052
```

### Q10_06


```
## [1] "Understanding of the research process in your field"
```

```
## Start:  AIC=482.25
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Semester                 1   120.52 480.27
## <none>                         120.51 482.25
## - Gender:`Race/Ethnicity`  2   124.04 484.03
## 
## Step:  AIC=480.27
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
## 
##                           Df Deviance    AIC
## <none>                         120.52 480.27
## - Gender:`Race/Ethnicity`  2   124.04 482.03
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + 
##     Gender:`Race/Ethnicity`, data = Q06_select)
## 
## Coefficients:
##                                                    Estimate Std. Error t value
## (Intercept)                                         2.93750    0.07448  39.442
## GenderMale                                          0.16888    0.13698   1.233
## `Race/Ethnicity`Asian Students                      0.14583    0.23941   0.609
## `Race/Ethnicity`Other Students of Color            -0.08036    0.18743  -0.429
## GenderMale:`Race/Ethnicity`Asian Students          -1.25222    0.52689  -2.377
## GenderMale:`Race/Ethnicity`Other Students of Color -0.02603    0.41544  -0.063
##                                                    Pr(>|t|)    
## (Intercept)                                          <2e-16 ***
## GenderMale                                           0.2191    
## `Race/Ethnicity`Asian Students                       0.5431    
## `Race/Ethnicity`Other Students of Color              0.6686    
## GenderMale:`Race/Ethnicity`Asian Students            0.0184 *  
## GenderMale:`Race/Ethnicity`Other Students of Color   0.9501    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6212303)
## 
##     Null deviance: 124.76  on 199  degrees of freedom
## Residual deviance: 120.52  on 194  degrees of freedom
## AIC: 480.27
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
##                         Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>                       120.52 480.27                       
## Gender:`Race/Ethnicity`  2   124.04 482.03        5.76  0.05614 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: 5 - as.numeric(Response)
##                         LR Chisq Df Pr(>Chisq)  
## Gender                    1.5200  1    0.21762  
## `Race/Ethnicity`          0.6290  2    0.73014  
## Gender:`Race/Ethnicity`   5.6684  2    0.05876 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_07


```
## [1] "Ability to integrate theory and practice"
```

```
## Start:  AIC=528.94
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   156.29 528.66
## <none>                         153.39 528.94
## - Semester                 1   155.91 530.18
## 
## Step:  AIC=528.66
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   156.45 524.87
## <none>                  156.29 528.66
## - Gender            1   158.45 529.39
## - Semester          1   158.46 529.40
## 
## Step:  AIC=524.87
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## <none>          156.45 524.87
## - Gender    1   158.75 525.78
## - Semester  1   158.82 525.86
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester + Gender, data = Q07_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      2.4687     0.1054  23.415   <2e-16 ***
## SemesterSpring   0.2204     0.1280   1.722   0.0866 .  
## GenderMale       0.2413     0.1421   1.698   0.0911 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7982312)
## 
##     Null deviance: 160.76  on 198  degrees of freedom
## Residual deviance: 156.45  on 196  degrees of freedom
## AIC: 524.87
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester + Gender
##          Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>        156.45 524.87                       
## Semester  1   158.82 525.86      2.9886  0.08385 .
## Gender    1   158.75 525.78      2.9064  0.08823 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_08


```
## [1] "Understanding of how scientists work on real problems"
```

```
## Start:  AIC=506.15
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   134.94 502.33
## - Semester                 1   134.91 504.27
## <none>                         134.82 506.15
## 
## Step:  AIC=502.33
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   135.18 498.68
## - Semester          1   135.01 500.43
## - Gender            1   135.61 501.31
## <none>                  134.94 502.33
## 
## Step:  AIC=498.68
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Semester  1   135.24 496.76
## - Gender    1   135.89 497.73
## <none>          135.18 498.68
## 
## Step:  AIC=496.76
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   135.98 495.86
## <none>        135.24 496.76
## 
## Step:  AIC=495.86
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q08_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.00995    0.05816   51.75   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6799005)
## 
##     Null deviance: 135.98  on 200  degrees of freedom
## Residual deviance: 135.98  on 200  degrees of freedom
## AIC: 495.86
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      135.98 495.86
```

### Q10_09


```
## [1] "Understanding that scientific assertions require supporting evidence"
```

```
## Start:  AIC=521.14
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Semester                 1   145.35 519.26
## - Gender:`Race/Ethnicity`  2   147.97 520.85
## <none>                         145.27 521.14
## 
## Step:  AIC=519.26
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   148.12 519.06
## <none>                         145.35 519.26
## 
## Step:  AIC=519.06
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   149.17 516.47
## - Gender            1   149.55 518.99
## <none>                  148.12 519.06
## 
## Step:  AIC=516.47
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   150.40 516.12
## <none>        149.17 516.47
## 
## Step:  AIC=516.12
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q09_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.05473    0.06117   49.94   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.75199)
## 
##     Null deviance: 150.4  on 200  degrees of freedom
## Residual deviance: 150.4  on 200  degrees of freedom
## AIC: 516.12
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>       150.4 516.12
```

### Q10_10


```
## [1] "Ability to analyze data and other information"
```

```
## Start:  AIC=508.5
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   137.48 506.06
## - Semester                 1   136.43 506.52
## <none>                         136.41 508.50
## 
## Step:  AIC=506.06
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   137.69 502.37
## - Semester          1   137.48 504.07
## - Gender            1   137.81 504.55
## <none>                  137.48 506.06
## 
## Step:  AIC=502.37
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Semester  1   137.70 500.39
## - Gender    1   137.98 500.80
## <none>          137.69 502.37
## 
## Step:  AIC=500.39
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   138.01 498.84
## <none>        137.70 500.39
## 
## Step:  AIC=498.84
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q10_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.09950    0.05859    52.9   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6900498)
## 
##     Null deviance: 138.01  on 200  degrees of freedom
## Residual deviance: 138.01  on 200  degrees of freedom
## AIC: 498.84
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      138.01 498.84
```

### Q10_11


```
## [1] "Understanding science"
```

```
## Start:  AIC=532.12
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   153.71 528.49
## - Semester                 1   153.82 530.64
## <none>                         153.42 532.12
## 
## Step:  AIC=528.49
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   154.33 525.31
## - Semester          1   154.06 526.95
## - Gender            1   154.18 527.12
## <none>                  153.71 528.49
## 
## Step:  AIC=525.31
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   154.71 523.81
## - Semester  1   154.82 523.95
## <none>          154.33 525.31
## 
## Step:  AIC=523.81
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## - Semester  1   155.28 522.54
## <none>          154.71 523.81
## 
## Step:  AIC=522.54
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q11_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.94030    0.06215   47.31   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7764179)
## 
##     Null deviance: 155.28  on 200  degrees of freedom
## Residual deviance: 155.28  on 200  degrees of freedom
## AIC: 522.54
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      155.28 522.54
```

### Q10_12


```
## [1] "Learning ethical conduct in your field"
```

```
## Start:  AIC=607.83
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Semester                 1   225.94 605.97
## - Gender:`Race/Ethnicity`  2   229.75 607.31
## <none>                         225.79 607.83
## 
## Step:  AIC=605.97
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity` + Gender:`Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   229.90 605.44
## <none>                         225.94 605.97
## 
## Step:  AIC=605.44
## 5 - as.numeric(Response) ~ Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   230.61 602.05
## - Gender            1   230.12 603.63
## <none>                  229.90 605.44
## 
## Step:  AIC=602.05
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   230.88 600.29
## <none>        230.61 602.05
## 
## Step:  AIC=600.29
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q12_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.66000    0.07616   34.92   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.160201)
## 
##     Null deviance: 230.88  on 199  degrees of freedom
## Residual deviance: 230.88  on 199  degrees of freedom
## AIC: 600.29
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      230.88 600.29
```

### Q10_13


```
## [1] "Learning laboratory techniques"
```

```
## Start:  AIC=475.91
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   117.68 474.81
## <none>                         115.99 475.91
## - Semester                 1   122.39 484.71
## 
## Step:  AIC=474.81
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   118.64 472.45
## - Gender            1   117.71 472.86
## <none>                  117.68 474.81
## - Semester          1   123.54 482.57
## 
## Step:  AIC=472.45
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   118.67 470.49
## <none>          118.64 472.45
## - Semester  1   124.48 480.11
## 
## Step:  AIC=470.49
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          118.67 470.49
## - Semester  1   124.49 478.11
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q13_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     3.49438    0.08185  42.690  < 2e-16 ***
## SemesterSpring -0.34260    0.10966  -3.124  0.00205 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.5963157)
## 
##     Null deviance: 124.49  on 200  degrees of freedom
## Residual deviance: 118.67  on 199  degrees of freedom
## AIC: 470.49
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)   
## <none>        118.67 470.49                        
## Semester  1   124.49 478.11      9.6251 0.001919 **
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_14


```
## [1] "Ability to read and understand primary literature"
```

```
## Start:  AIC=525.61
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   150.33 522.48
## <none>                         149.68 525.61
## - Semester                 1   154.81 530.35
## 
## Step:  AIC=522.48
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - Gender            1   150.43 520.61
## - `Race/Ethnicity`  2   151.95 520.62
## <none>                  150.33 522.48
## - Semester          1   155.31 526.99
## 
## Step:  AIC=520.61
## 5 - as.numeric(Response) ~ Semester + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   152.13 518.86
## <none>                  150.43 520.61
## - Semester          1   155.61 525.38
## 
## Step:  AIC=518.86
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          152.13 518.86
## - Semester  1   156.75 522.85
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q14_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     3.13483    0.09291  33.739   <2e-16 ***
## SemesterSpring -0.30600    0.12472  -2.454    0.015 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7683322)
## 
##     Null deviance: 156.76  on 199  degrees of freedom
## Residual deviance: 152.13  on 198  degrees of freedom
## AIC: 518.86
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>        152.13 518.86                       
## Semester  1   156.75 522.85        5.99  0.01439 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_15


```
## [1] "Skill in how to give an effective oral presentation"
```

```
## Start:  AIC=557.35
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   178.64 553.53
## <none>                         178.48 557.35
## - Semester                 1   182.00 559.21
## 
## Step:  AIC=553.53
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - Gender            1   178.64 551.53
## - `Race/Ethnicity`  2   181.34 552.50
## <none>                  178.64 553.53
## - Semester          1   182.09 555.32
## 
## Step:  AIC=551.53
## 5 - as.numeric(Response) ~ Semester + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   181.34 550.50
## <none>                  178.64 551.53
## - Semester          1   182.11 553.33
## 
## Step:  AIC=550.5
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          181.34 550.50
## - Semester  1   184.34 551.75
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q15_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      2.4886     0.1025  24.271   <2e-16 ***
## SemesterSpring   0.2477     0.1376   1.801   0.0733 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9252203)
## 
##     Null deviance: 184.34  on 197  degrees of freedom
## Residual deviance: 181.34  on 196  degrees of freedom
## AIC: 550.5
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>        181.34 550.50                       
## Semester  1   184.34 551.75       3.249  0.07147 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

### Q10_16


```
## [1] "Skill in science writing"
```

```
## Start:  AIC=520.97
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   145.18 517.02
## - Semester                 1   146.40 520.70
## <none>                         145.14 520.97
## 
## Step:  AIC=517.02
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   145.28 513.15
## - Gender            1   145.37 515.28
## - Semester          1   146.49 516.82
## <none>                  145.18 517.02
## 
## Step:  AIC=513.15
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   145.44 511.38
## - Semester  1   146.70 513.12
## <none>          145.28 513.15
## 
## Step:  AIC=511.38
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          145.44 511.38
## - Semester  1   146.96 511.46
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q16_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)     3.11236    0.09062   34.35   <2e-16 ***
## SemesterSpring -0.17486    0.12140   -1.44    0.151    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7308488)
## 
##     Null deviance: 146.96  on 200  degrees of freedom
## Residual deviance: 145.44  on 199  degrees of freedom
## AIC: 511.38
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>        145.44 511.38                     
## Semester  1   146.96 511.46      2.0847   0.1488
```

### Q10_17


```
## [1] "Self-confidence"
```

```
## Start:  AIC=593.48
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   208.88 590.14
## <none>                         208.19 593.48
## - Semester                 1   210.35 593.55
## 
## Step:  AIC=590.14
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   208.91 586.18
## - Gender            1   208.89 588.15
## - Semester          1   210.93 590.10
## <none>                  208.88 590.14
## 
## Step:  AIC=586.18
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   208.93 584.19
## - Semester  1   210.93 586.11
## <none>          208.91 586.18
## 
## Step:  AIC=584.19
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## - Semester  1   211.00 584.17
## <none>          208.93 584.19
## 
## Step:  AIC=584.17
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q17_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.66169    0.07245   36.74   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.054975)
## 
##     Null deviance: 211  on 200  degrees of freedom
## Residual deviance: 211  on 200  degrees of freedom
## AIC: 584.17
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>         211 584.17
```

### Q10_18


```
## [1] "Understanding of how scientists think"
```

```
## Start:  AIC=531.44
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   153.02 527.59
## - Semester                 1   153.53 530.27
## <none>                         152.90 531.44
## 
## Step:  AIC=527.59
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   153.03 523.61
## - Gender            1   153.32 525.98
## - Semester          1   153.60 526.36
## <none>                  153.02 527.59
## 
## Step:  AIC=523.61
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   153.33 522.00
## - Semester  1   153.64 522.40
## <none>          153.03 523.61
## 
## Step:  AIC=522
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## - Semester  1   154.01 520.89
## <none>          153.33 522.00
## 
## Step:  AIC=520.89
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q18_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   2.7662     0.0619   44.69   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7700498)
## 
##     Null deviance: 154.01  on 200  degrees of freedom
## Residual deviance: 154.01  on 200  degrees of freedom
## AIC: 520.89
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      154.01 520.89
```

### Q10_19


```
## [1] "Learning to work independently"
```

```
## Start:  AIC=601.77
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   217.70 598.46
## - Semester                 1   217.46 600.23
## <none>                         216.96 601.77
## 
## Step:  AIC=598.46
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   218.48 595.18
## - Semester          1   218.10 596.82
## - Gender            1   218.78 597.45
## <none>                  217.70 598.46
## 
## Step:  AIC=595.18
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Semester  1   219.01 593.67
## - Gender    1   219.74 594.33
## <none>          218.48 595.18
## 
## Step:  AIC=593.67
## 5 - as.numeric(Response) ~ Gender
## 
##          Df Deviance    AIC
## - Gender  1   220.15 592.70
## <none>        219.01 593.67
## 
## Step:  AIC=592.7
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q19_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    2.522      0.074   34.09   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.100746)
## 
##     Null deviance: 220.15  on 200  degrees of freedom
## Residual deviance: 220.15  on 200  degrees of freedom
## AIC: 592.7
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance   AIC scaled dev. Pr(>Chi)
## <none>      220.15 592.7
```

### Q10_20


```
## [1] "Becoming part of a learning community"
```

```
## Start:  AIC=536.38
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   157.78 533.75
## - Semester                 1   157.00 534.76
## <none>                         156.70 536.38
## 
## Step:  AIC=533.75
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   157.86 529.85
## - Gender            1   157.78 531.76
## - Semester          1   158.03 532.07
## <none>                  157.78 533.75
## 
## Step:  AIC=529.85
## 5 - as.numeric(Response) ~ Semester + Gender
## 
##            Df Deviance    AIC
## - Gender    1   157.88 527.87
## - Semester  1   158.15 528.23
## <none>          157.86 529.85
## 
## Step:  AIC=527.87
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## - Semester  1   158.16 526.23
## <none>          157.88 527.87
## 
## Step:  AIC=526.23
## 5 - as.numeric(Response) ~ 1
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ 1, data = Q20_select)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.93532    0.06272    46.8   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.790796)
## 
##     Null deviance: 158.16  on 200  degrees of freedom
## Residual deviance: 158.16  on 200  degrees of freedom
## AIC: 526.23
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ 1
##        Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>      158.16 526.23
```

### Q10_21


```
## [1] "Confidence in my potential to be a teacher of science"
```

```
## Start:  AIC=612.96
## 5 - as.numeric(Response) ~ Semester + Gender * `Race/Ethnicity`
## 
##                           Df Deviance    AIC
## - Gender:`Race/Ethnicity`  2   238.15 610.46
## <none>                         236.36 612.96
## - Semester                 1   239.46 613.54
## 
## Step:  AIC=610.46
## 5 - as.numeric(Response) ~ Semester + Gender + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - Gender            1   238.16 608.46
## - `Race/Ethnicity`  2   240.82 608.67
## <none>                  238.15 610.46
## - Semester          1   241.06 610.86
## 
## Step:  AIC=608.46
## 5 - as.numeric(Response) ~ Semester + `Race/Ethnicity`
## 
##                    Df Deviance    AIC
## - `Race/Ethnicity`  2   240.83 606.67
## <none>                  238.16 608.46
## - Semester          1   241.12 608.91
## 
## Step:  AIC=606.67
## 5 - as.numeric(Response) ~ Semester
## 
##            Df Deviance    AIC
## <none>          240.83 606.67
## - Semester  1   244.51 607.68
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ Semester, data = Q21_select)
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      2.0562     0.1175  17.500   <2e-16 ***
## SemesterSpring   0.2741     0.1584   1.731   0.0851 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.22872)
## 
##     Null deviance: 244.51  on 197  degrees of freedom
## Residual deviance: 240.83  on 196  degrees of freedom
## AIC: 606.67
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ Semester
##          Df Deviance    AIC scaled dev. Pr(>Chi)  
## <none>        240.83 606.67                       
## Semester  1   244.51 607.68      3.0034  0.08309 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


## Summary

**UPDATED TO INDICATE THE EFFECT OF SEPARATING ASIAN STUDENTS**

*Indicates stayed the same*

Note that only first-year students are included in this analysis.

Most of the responses showed a dependence on the semester, although the significance was sometimes marginal. 
This needs to be interpreted carefully as the responses in the Spring semester, if different, were always less positive than in the Fall. 
That is, the students in the Spring perceived less of a gain in these measures due to the CURE Lab than those in the Fall semester. 
This is consistent with the hypothesis that BIO Seminar provided some gain in these measures and, therefore, the gains due to the CURE Lab were smaller. 

Questions that showed dependence on the Semester, but not Race or Gender:

*2*   
4 <- NOW INTERACTION AND SEMESTER   
*13*    
*14*    
*15*    
*16*    

Question that showed dependence on Gender and Race, but not Semester:

1 <- NOW JUST GENDER

Question that showed dependence on Semester and Race, but not Gender: 

21 <- NOW JUST SEMESTER

Question that showed dependence on Semester and Gender, but not Race: 

*7*

Questions that showed dependence on Race, Gender, and the interaction between Race and Gender, but not Semester:

*6*   
9 <- NOW NONE
12 <- NOW NONE

Questions that did not depend on Semester, Race, or Gender:

*3*   
5   <- NOW INTERACTION BUT NOT SEMESTER
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


### New Final Figure 6

2   13    14    15    16  21

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Semester big-1.png)<!-- -->

### New Final Figure 7A

NO QUESTION IN THIS CATEGORY



### New Final Figure 7C

3      8   10    11    17    18    19    20

9 <- NOW NONE
12 <- NOW NONE

not 5

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By None-1.png)<!-- -->

### New Final Figure 7B

7

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Semester and gender-1.png)<!-- -->


### New Final Figure 7D

1 <- NOW JUST GENDER

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By Race and gender-1.png)<!-- -->


### New Figure 7E

For this I need some cat plots.

5 and 6 show dependence Race, Gender, and the interaction between Race and Gender, but not Semester

4 shows dependence Race, Gender, and the interaction between Race and Gender, AND Semester

Question that showed dependence on Gender and Race, but not Semester: 1
Questions that showed dependence on Race, Gender, and the interaction between Race and Gender, but not Semester: 6   9   12

![](Figures6-8.Post-Benefits.2026_files/figure-html/Q10 By All-1.png)<!-- -->


```
## Using data Q04_select from global environment. This could cause incorrect
## results if Q04_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction 04-1.png)<!-- -->

```
## Using data Q04_select from global environment. This could cause incorrect
## results if Q04_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```


```
## Using data Q05_select from global environment. This could cause incorrect
## results if Q05_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction 05-1.png)<!-- -->

```
## Using data Q05_select from global environment. This could cause incorrect
## results if Q05_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```



```
## Using data Q06_select from global environment. This could cause incorrect
## results if Q06_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

![](Figures6-8.Post-Benefits.2026_files/figure-html/Interaction 06-1.png)<!-- -->

```
## Using data Q06_select from global environment. This could cause incorrect
## results if Q06_select has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```







![](Figures6-8.Post-Benefits.2026_files/figure-html/Combined Interaction Plots-1.png)<!-- -->

