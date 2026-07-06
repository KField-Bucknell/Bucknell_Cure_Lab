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
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q01_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.06993    0.08946  23.138   <2e-16 ***
## ClassYearnot First            -0.56993    0.38866  -1.466    0.144    
## GenderMale                    -0.27363    0.17087  -1.601    0.111    
## ClassYearnot First:GenderMale  0.60697    0.60248   1.007    0.315    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.144412)
## 
##     Null deviance: 241.83  on 210  degrees of freedom
## Residual deviance: 236.89  on 207  degrees of freedom
## AIC: 633.22
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                236.89 633.22                     
## ClassYear:Gender  1   238.06 632.25       1.032   0.3097
```

### Q10_02


```
## [1] "Skill in the interpretation of results"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q02_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.80137    0.06569  42.646   <2e-16 ***
## ClassYearnot First            -0.17637    0.28821  -0.612    0.541    
## GenderMale                     0.18045    0.12558   1.437    0.152    
## ClassYearnot First:GenderMale  0.02789    0.44668   0.062    0.950    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6299994)
## 
##     Null deviance: 134.62  on 214  degrees of freedom
## Residual deviance: 132.93  on 211  degrees of freedom
## AIC: 516.77
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                132.93 516.77                     
## ClassYear:Gender  1   132.93 514.77   0.0039711   0.9498
```

### Q10_03


```
## [1] "Tolerance for obstacles faced in the research process"
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
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q04_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.88356    0.07256  39.739   <2e-16 ***
## ClassYearnot First            -0.63356    0.31837  -1.990   0.0479 *  
## GenderMale                    -0.06538    0.13872  -0.471   0.6379    
## ClassYearnot First:GenderMale  0.31538    0.49341   0.639   0.5234    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7687316)
## 
##     Null deviance: 165.97  on 214  degrees of freedom
## Residual deviance: 162.20  on 211  degrees of freedom
## AIC: 559.56
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                162.20 559.56                     
## ClassYear:Gender  1   162.52 557.97      0.4159    0.519
```

### Q10_05


```
## [1] "Understanding how knowledge is constructed"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q05_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.65972    0.07132  37.291   <2e-16 ***
## ClassYearnot First            -0.40972    0.31089  -1.318    0.189    
## GenderMale                     0.08573    0.13567   0.632    0.528    
## ClassYearnot First:GenderMale  0.49760    0.48172   1.033    0.303    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7325172)
## 
##     Null deviance: 155.0  on 212  degrees of freedom
## Residual deviance: 153.1  on 209  degrees of freedom
## AIC: 544.13
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                153.10 544.13                     
## ClassYear:Gender  1   153.88 543.21      1.0847   0.2977
```

### Q10_06


```
## [1] "Understanding of the research process in your field"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q06_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.93793    0.06709  43.788   <2e-16 ***
## ClassYearnot First            -0.31293    0.29342  -1.066    0.287    
## GenderMale                     0.09843    0.12794   0.769    0.443    
## ClassYearnot First:GenderMale  0.10990    0.45470   0.242    0.809    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6527475)
## 
##     Null deviance: 138.43  on 213  degrees of freedom
## Residual deviance: 137.08  on 210  degrees of freedom
## AIC: 521.98
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                137.08 521.98                     
## ClassYear:Gender  1   137.12 520.04    0.059522   0.8073
```

### Q10_07


```
## [1] "Ability to integrate theory and practice"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q07_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.59722    0.07520  34.537   <2e-16 ***
## ClassYearnot First            -0.22222    0.32779  -0.678    0.499    
## GenderMale                     0.22096    0.14304   1.545    0.124    
## ClassYearnot First:GenderMale -0.09596    0.50791  -0.189    0.850    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8143335)
## 
##     Null deviance: 172.88  on 212  degrees of freedom
## Residual deviance: 170.20  on 209  degrees of freedom
## AIC: 566.68
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                170.20 566.68                     
## ClassYear:Gender  1   170.22 564.72    0.036374   0.8487
```

### Q10_08


```
## [1] "Understanding of how scientists work on real problems"
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
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q09_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    3.10274    0.07271  42.671   <2e-16 ***
## ClassYearnot First            -0.22774    0.31903  -0.714    0.476    
## GenderMale                    -0.17547    0.13900  -1.262    0.208    
## ClassYearnot First:GenderMale  0.46713    0.49444   0.945    0.346    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7719257)
## 
##     Null deviance: 164.44  on 214  degrees of freedom
## Residual deviance: 162.88  on 211  degrees of freedom
## AIC: 560.45
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                162.88 560.45                     
## ClassYear:Gender  1   163.56 559.36     0.90761   0.3407
```

### Q10_10


```
## [1] "Ability to analyze data and other information"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q10_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    3.07534    0.06799  45.232   <2e-16 ***
## ClassYearnot First            -0.45034    0.29831  -1.510    0.133    
## GenderMale                     0.08829    0.12998   0.679    0.498    
## ClassYearnot First:GenderMale  0.45337    0.46232   0.981    0.328    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6749139)
## 
##     Null deviance: 144.49  on 214  degrees of freedom
## Residual deviance: 142.41  on 211  degrees of freedom
## AIC: 531.57
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                142.41 531.57                     
## ClassYear:Gender  1   143.06 530.55     0.97766   0.3228
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                142.41 531.57                     
## ClassYear:Gender  1   143.06 530.55     0.97766   0.3228
```

### Q10_11


```
## [1] "Understanding science"
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
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q12_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.68276    0.09063  29.601   <2e-16 ***
## ClassYearnot First            -0.68276    0.39634  -1.723   0.0864 .  
## GenderMale                    -0.08276    0.17282  -0.479   0.6325    
## ClassYearnot First:GenderMale  0.58276    0.61420   0.949   0.3438    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.190985)
## 
##     Null deviance: 253.84  on 213  degrees of freedom
## Residual deviance: 250.11  on 210  degrees of freedom
## AIC: 650.67
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                250.11 650.67                     
## ClassYear:Gender  1   251.18 649.59     0.91543   0.3387
```

### Q10_13


```
## [1] "Learning laboratory techniques"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q13_select)
## 
## Coefficients:
##                                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    3.301370   0.066613  49.561   <2e-16 ***
## ClassYearnot First            -0.176370   0.292262  -0.603    0.547    
## GenderMale                     0.007721   0.127342   0.061    0.952    
## ClassYearnot First:GenderMale  0.533946   0.452955   1.179    0.240    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6478366)
## 
##     Null deviance: 137.74  on 214  degrees of freedom
## Residual deviance: 136.69  on 211  degrees of freedom
## AIC: 522.77
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                136.69 522.77                     
## ClassYear:Gender  1   137.59 522.18      1.4113   0.2348
```

### Q10_14


```
## [1] "Ability to read and understand primary literature"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q14_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.93836    0.07437  39.510   <2e-16 ***
## ClassYearnot First            -0.31336    0.32630  -0.960    0.338    
## GenderMale                     0.09868    0.14313   0.689    0.491    
## ClassYearnot First:GenderMale -0.05701    0.50598  -0.113    0.910    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8075213)
## 
##     Null deviance: 171.33  on 213  degrees of freedom
## Residual deviance: 169.58  on 210  degrees of freedom
## AIC: 567.52
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                169.58 567.52                     
## ClassYear:Gender  1   169.59 565.53    0.012939   0.9094
```

### Q10_15


```
## [1] "Skill in how to give an effective oral presentation"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q15_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.63448    0.08136  32.381   <2e-16 ***
## ClassYearnot First            -0.63448    0.35580  -1.783    0.076 .  
## GenderMale                    -0.03071    0.15725  -0.195    0.845    
## ClassYearnot First:GenderMale  0.69738    0.55197   1.263    0.208    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9598085)
## 
##     Null deviance: 202.72  on 211  degrees of freedom
## Residual deviance: 199.64  on 208  degrees of freedom
## AIC: 598.9
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                199.64 598.90                     
## ClassYear:Gender  1   201.17 598.52      1.6207    0.203
```

### Q10_16


```
## [1] "Skill in science writing"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q16_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.99315    0.07216  41.479   <2e-16 ***
## ClassYearnot First             0.13185    0.31660   0.416    0.678    
## GenderMale                     0.07958    0.13795   0.577    0.565    
## ClassYearnot First:GenderMale -0.37124    0.49068  -0.757    0.450    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7602397)
## 
##     Null deviance: 160.96  on 214  degrees of freedom
## Residual deviance: 160.41  on 211  degrees of freedom
## AIC: 557.17
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                160.41 557.17                     
## ClassYear:Gender  1   160.85 555.75     0.58249   0.4453
```

### Q10_17


```
## [1] "Self-confidence"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q17_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.65068    0.08795  30.140   <2e-16 ***
## ClassYearnot First            -0.15068    0.38586  -0.391    0.697    
## GenderMale                     0.04022    0.16812   0.239    0.811    
## ClassYearnot First:GenderMale  0.12644    0.59801   0.211    0.833    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.129212)
## 
##     Null deviance: 238.53  on 214  degrees of freedom
## Residual deviance: 238.26  on 211  degrees of freedom
## AIC: 642.23
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                238.26 642.23                     
## ClassYear:Gender  1   238.31 640.28    0.045549    0.831
```

### Q10_18


```
## [1] "Understanding of how scientists think"
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
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q19_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.56849    0.08873  28.949   <2e-16 ***
## ClassYearnot First            -0.06849    0.38928  -0.176    0.861    
## GenderMale                    -0.16849    0.16962  -0.993    0.322    
## ClassYearnot First:GenderMale  0.16849    0.60332   0.279    0.780    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.149361)
## 
##     Null deviance: 243.66  on 214  degrees of freedom
## Residual deviance: 242.52  on 211  degrees of freedom
## AIC: 646.04
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                242.51 646.04                     
## ClassYear:Gender  1   242.60 644.11    0.079458    0.778
```

### Q10_20


```
## [1] "Becoming part of a learning community"
```

```
## 
## Call:
## glm(formula = 5 - as.numeric(Response) ~ ClassYear * Gender, 
##     data = Q20_select)
## 
## Coefficients:
##                               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                    2.93836    0.07429  39.551   <2e-16 ***
## ClassYearnot First            -0.31336    0.32596  -0.961    0.337    
## GenderMale                    -0.01108    0.14202  -0.078    0.938    
## ClassYearnot First:GenderMale  0.38608    0.50518   0.764    0.446    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.805826)
## 
##     Null deviance: 170.81  on 214  degrees of freedom
## Residual deviance: 170.03  on 211  degrees of freedom
## AIC: 569.69
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## 5 - as.numeric(Response) ~ ClassYear * Gender
##                  Df Deviance    AIC scaled dev. Pr(>Chi)
## <none>                170.03 569.69                     
## ClassYear:Gender  1   170.50 568.28     0.59434   0.4407
```

### Q10_21


```
## [1] "Confidence in my potential to be a teacher of science"
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

## Stats Summary Table


``` r
# 1. Automatically gather all 21 models into a single named list
# This looks for Q01_model, Q02_model, ... Q21_model in your environment
model_names <- sprintf("Q%02d_model", 1:21)
model_list <- mget(model_names, envir = .GlobalEnv)

# 2. Loop through the models, run ANOVA, and bind the results
anova_table <- map_dfr(model_list, function(model) {
  
  # Run the Type III ANOVA
  res <- car::Anova(model, type = "3", test.statistic = "F")
  
  # Convert the ANOVA matrix to a data frame and grab the row names
  as.data.frame(res) %>% 
    rownames_to_column(var = "Variable")
  
}, .id = "Question") %>% 
  
  # 3. Filter down to the specific variables you care about
  filter(Variable %in% c("Semester", "Gender", "`Race/Ethnicity`")) %>% 
  
  # 4. Keep only the columns you need and rename them for convenience
  select(Question, Variable, F_value = `F values`, p_value = `Pr(>F)`) %>% 
  
  # 5. Pivot the table wider so each variable gets its own F and p columns
  pivot_wider(
    names_from = Variable,
    values_from = c(F_value, p_value),
    names_glue = "{Variable}_{.value}"
  ) |>
  select(
    Question,
    Semester_F_value, Semester_p_value,
    Gender_F_value, Gender_p_value,
    `\`Race/Ethnicity\`_F_value`, `\`Race/Ethnicity\`_p_value`
  )

# View your final consolidated table

write_tsv(anova_table, file = "Post_Benefits_ANOVA.tsv")
kable(anova_table)
```



|Question  | Semester_F_value| Semester_p_value| Gender_F_value| Gender_p_value| `Race/Ethnicity`_F_value| `Race/Ethnicity`_p_value|
|:---------|----------------:|----------------:|--------------:|--------------:|------------------------:|------------------------:|
|Q01_model |        0.0003201|        0.9857436|      2.1506681|      0.1441438|                1.2769571|                0.2812422|
|Q02_model |        5.0879411|        0.0251976|      1.3113549|      0.2535459|                0.7772933|                0.4610601|
|Q03_model |        0.7873877|        0.3759788|      0.2190534|      0.6402821|                0.5105429|                0.6009656|
|Q04_model |        3.7565011|        0.0540391|      0.5156932|      0.4735398|                0.6731045|                0.5112976|
|Q05_model |        0.2830412|        0.5953235|      0.5821790|      0.4463863|                0.4523577|                0.6367958|
|Q06_model |        0.0023071|        0.9617398|      0.4957148|      0.4822290|                0.2585671|                0.7724215|
|Q07_model |        2.6937803|        0.1023607|      2.6790975|      0.1032945|                0.1023563|                0.9027566|
|Q08_model |        0.1005177|        0.7515467|      0.9624443|      0.3277812|                0.1731622|                0.8411297|
|Q09_model |        0.1988485|        0.6561441|      1.9936572|      0.1595447|                0.7483461|                0.4744954|
|Q10_model |        0.0075906|        0.9306620|      0.4795491|      0.4894463|                0.1487440|                0.8618869|
|Q11_model |        0.4488833|        0.5036543|      0.6106927|      0.4354701|                0.3999345|                0.6709098|
|Q12_model |        0.1297587|        0.7190722|      0.2121924|      0.6455677|                0.2743617|                0.7603500|
|Q13_model |        9.7517947|        0.0020628|      0.0412775|      0.8392141|                0.8012015|                0.4502537|
|Q14_model |        6.4574832|        0.0118269|      0.1341562|      0.7145582|                1.0499072|                0.3519400|
|Q15_model |        3.7296169|        0.0549206|      0.0021565|      0.9630091|                1.4588520|                0.2350552|
|Q16_model |        1.7652225|        0.1855192|      0.2529579|      0.6155647|                0.0664799|                0.9357028|
|Q17_model |        1.9253883|        0.1668394|      0.0129750|      0.9094273|                0.0180088|                0.9821541|
|Q18_model |        0.7510732|        0.3871964|      0.3839829|      0.5361990|                0.0095865|                0.9904598|
|Q19_model |        0.3554143|        0.5517520|      0.9696124|      0.3259908|                0.3518867|                0.7038033|
|Q20_model |        0.3164103|        0.5744157|      0.0102498|      0.9194625|                0.0527659|                0.9486156|
|Q21_model |        2.3620263|        0.1259584|      0.0062754|      0.9369419|                1.0839571|                0.3403045|

