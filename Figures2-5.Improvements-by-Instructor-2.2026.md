---
title: "Improvements by Instructor"
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

Loading in the results without instructor information:


``` r
NoDemographicsYear1 <- read_delim("Deidentified Surveys/Year1.NoDemographics.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE) %>%
  rename(Semester = Semester_pre)
```

```
## Rows: 85 Columns: 151
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (141): ResponseId_pre, Instructor, Semester_pre, Q1_pre, Q8_pre, Q9_1_pr...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoDemographicsYear2 <- read_delim("Deidentified Surveys/Year2.NoDemographics.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 77 Columns: 151
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (141): ResponseId_pre, Instructor, Semester, Q1_pre, Q8_pre, Q9_1_pre, Q...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoDemographicsYear3 <- read_delim("Deidentified Surveys/Year3.NoDemographics.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 63 Columns: 151
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (141): ResponseId_pre, Instructor, Semester, Q1_pre, Q8_pre, Q9_1_pre, Q...
## dbl  (10): Q19_1_pre, Q19_2_pre, Q19_3_pre, Q19_4_pre, Q19_5_pre, Q19_6_pre,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
NoDemographics <- bind_rows(NoDemographicsYear1, NoDemographicsYear2, NoDemographicsYear3)

NoDemographicsQuestions <- read_delim("Deidentified Surveys/Year3.NoDemographicsQuestions.tsv", 
    delim = "\t", escape_double = FALSE, 
    trim_ws = TRUE)
```

```
## Rows: 151 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (2): value, Question
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Cleaning up the factors and removing anyone who did not agree to the informed consent.


``` r
NoDemographics <- NoDemographics %>%
  mutate(Semester = factor(Semester, levels = c("Fall 2021", "Spring 2022",
                                                "Fall 2022", "Spring 2023",
                                                "Fall 2023", "Spring 2024"))) %>%
  mutate(across(Instructor, str_replace, 'Prof. ', ''))
```

```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `across(Instructor, str_replace, "Prof. ", "")`.
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

``` r
NoDemographics <- NoDemographics %>%
  filter(Q1_pre == "Agree")
```

We have hypothesized that instructors may be less successful in improving learning elements during the first time
that they have taught the class. To test this hypothesis, we will need to code a new variable, that I will call Rookie.
To do this, I logged into Cognos and ran a "Class Info by Term and Instructor" report. Note that I had to identify the 
instructor pseudonyms using the Deidentification Rmd, but I have not included that information here.

- 'McGonagall' 'Fall 2021'
- 'Dumbledore' 'Fall 2021'
- 'Hagrid' 'Spring 2022'
- 'Lupin' 'Fall 2021'
- 'Sinistra' 'Spring 2022'


``` r
NoDemographics <- NoDemographics %>%
  mutate(Rookie = case_when((Instructor == 'McGonagall') & (Semester == 'Fall 2021') ~ "Rookie", 
                     (Instructor == 'Dumbledore') & (Semester == 'Fall 2021') ~ "Rookie",
                     (Instructor == 'Hagrid') & (Semester == 'Spring 2022') ~ "Rookie",
                     (Instructor == 'Lupin') & (Semester == 'Fall 2021') ~ "Rookie",
                     (Instructor == 'Sinistra') & (Semester == 'Spring 2022') ~ "Rookie",
                     .default = "Veteran"))
```

## Improvement in learning elements

Comparing the responses to Pre10 and Post9:


```
## [1] "Please look over this inventory of elements that might be included in a course. For each element, give an estimate of your current level of ability before the course begins. Your current level of ability may be a result of courses in high school or college, or it may be a result of other experiences such as jobs or special programs. If students are expected to do the following course elements, what would be their level of expertise? - A scripted lab or project in which the students know the expected outcome"
```

```
##  [1] "A scripted lab or project in which the students know the expected outcome"                         
##  [2] "A lab or project in which only the instructor knows the outcome"                                   
##  [3] "A lab or project where no one knows the outcome"                                                   
##  [4] "At least one project that is assigned and structured by the instructor"                            
##  [5] "A project in which students have some input into the research process and/or what is being studied"
##  [6] "A project entirely of student design"                                                              
##  [7] "Work individually"                                                                                 
##  [8] "Work as a whole class"                                                                             
##  [9] "Work in small groups"                                                                              
## [10] "Become responsible for a part of the project"                                                      
## [11] "Read primary scientific literature"                                                                
## [12] "Write a research proposal"                                                                         
## [13] "Collect data"                                                                                      
## [14] "Analyze data"                                                                                      
## [15] "Present results orally"                                                                            
## [16] "Present results in written papers or reports"                                                      
## [17] "Present posters"                                                                                   
## [18] "Critique the work of other students"                                                               
## [19] "Listen to lectures"                                                                                
## [20] "Read a textbook"                                                                                   
## [21] "Work on problem sets"                                                                              
## [22] "Take tests in class"                                                                               
## [23] "Discuss reading materials in class"                                                                
## [24] "Maintain a lab notebook"                                                                           
## [25] "Computer modeling"
```



```
## [1] "Please rate how much learning you gained from each element you experienced in this course. The scale measuring your gain is from (no or very small gain) to (very large gain). Some elements may not have happened at all. If the item is not relevant or you prefer not to answer, please choose the \"\"not applicable\"\" option. If students were expected to do the following course elements, what would be their level of gained experience? - A scripted lab or project in which the students know the expected outcome"
```

```
##  [1] "A scripted lab or project in which the students know the expected outcome"                         
##  [2] "A lab or project in which only the instructor knows the outcome"                                   
##  [3] "A lab or project where no one knows the outcome"                                                   
##  [4] "At least one project that is assigned and structured by the instructor"                            
##  [5] "A project in which students have some input into the research process and/or what is being studied"
##  [6] "A project entirely of student design"                                                              
##  [7] "Work individually"                                                                                 
##  [8] "Work as a whole class"                                                                             
##  [9] "Work in small groups"                                                                              
## [10] "Become responsible for a part of the project"                                                      
## [11] "Read primary scientific literature"                                                                
## [12] "Write a research proposal"                                                                         
## [13] "Collect data"                                                                                      
## [14] "Analyze data"                                                                                      
## [15] "Present results orally"                                                                            
## [16] "Present results in written papers or reports"                                                      
## [17] "Present posters"                                                                                   
## [18] "Critique the work of other students"                                                               
## [19] "Listen to lectures"                                                                                
## [20] "Read a textbook"                                                                                   
## [21] "Work on problem sets"                                                                              
## [22] "Take tests in class"                                                                               
## [23] "Discuss reading materials in class"                                                                
## [24] "Maintain a lab notebook"                                                                           
## [25] "Computer modeling"
```

```
##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

Now to compare the pre and post responses for those questions:


```
## # A tibble: 6 × 53
##   Semester  Instructor Rookie Q10_1_pre Q10_2_pre Q10_3_pre Q10_4_pre Q10_5_pre
##   <fct>     <chr>      <chr>  <chr>     <chr>     <chr>     <chr>     <chr>    
## 1 Fall 2021 McGonagall Rookie Much      Some      None      Some      Extensive
## 2 Fall 2021 McGonagall Rookie Some      Much      None      Extensive Little   
## 3 Fall 2021 McGonagall Rookie Some      Much      None      Much      Some     
## 4 Fall 2021 McGonagall Rookie Little    Much      Little    Some      Some     
## 5 Fall 2021 McGonagall Rookie Some      Some      Little    Much      Much     
## 6 Fall 2021 McGonagall Rookie Some      Much      Little    Much      Some     
## # ℹ 45 more variables: Q10_6_pre <chr>, Q10_7_pre <chr>, Q10_8_pre <chr>,
## #   Q10_9_pre <chr>, Q10_10_pre <chr>, Q10_11_pre <chr>, Q10_12_pre <chr>,
## #   Q10_13_pre <chr>, Q10_14_pre <chr>, Q10_15_pre <chr>, Q10_16_pre <chr>,
## #   Q10_17_pre <chr>, Q10_18_pre <chr>, Q10_19_pre <chr>, Q10_20_pre <chr>,
## #   Q10_21_pre <chr>, Q10_22_pre <chr>, Q10_23_pre <chr>, Q10_24_pre <chr>,
## #   Q10_25_pre <chr>, Q9_1_post <chr>, Q9_2_post <chr>, Q9_3_post <chr>,
## #   Q9_4_post <chr>, Q9_5_post <chr>, Q9_6_post <chr>, Q9_7_post <chr>, …
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

## Sample Size

### Table S1


``` r
Q10Clean %>%
  group_by(Instructor, Semester) %>%
  count() %>%
  print()
```

```
## # A tibble: 14 × 3
## # Groups:   Instructor, Semester [14]
##    Instructor Semester        n
##    <chr>      <fct>       <int>
##  1 Dumbledore Fall 2021      15
##  2 Dumbledore Fall 2022      24
##  3 Dumbledore Spring 2023    11
##  4 Dumbledore Fall 2023      20
##  5 Hagrid     Spring 2022    16
##  6 Hagrid     Spring 2024    19
##  7 Lupin      Fall 2021      11
##  8 Lupin      Fall 2022      13
##  9 McGonagall Fall 2021       8
## 10 McGonagall Spring 2022    25
## 11 Sinistra   Spring 2022    10
## 12 Sinistra   Spring 2023    28
## 13 Sinistra   Fall 2023      10
## 14 Sinistra   Spring 2024    14
```

First let's just look at the contingency tables to see if everything looks right.


```
## [1] "Rows represents pre-survey response, Columns represent post-survey response."
```

```
## [1] "First for All sections then by Instructor."
```

```
## [1] "A scripted lab or project in which the students know the expected outcome"
```

```
##            
##             None Little Some Much Extensive
##   None         1      1    0    0         0
##   Little       1      3    8    8         5
##   Some         3      7   21   40        14
##   Much         3      5   17   33        28
##   Extensive    1      4    4    6         2
```

```
## [1] "By Instructor"
```

```
## [1] "A scripted lab or project in which the students know the expected outcome"
```

```
## , ,  = Dumbledore
## 
##            
##             None Little Some Much Extensive
##   None         1      1    0    0         0
##   Little       1      0    3    3         3
##   Some         1      2    8    7         7
##   Much         1      1    3    8        10
##   Extensive    0      1    2    1         2
## 
## , ,  = Hagrid
## 
##            
##             None Little Some Much Extensive
##   None         0      0    0    0         0
##   Little       0      0    2    1         0
##   Some         0      1   10    5         0
##   Much         1      0    1    5         2
##   Extensive    1      2    0    2         0
## 
## , ,  = Lupin
## 
##            
##             None Little Some Much Extensive
##   None         0      0    0    0         0
##   Little       0      1    1    2         0
##   Some         0      1    1    2         2
##   Much         0      2    2    8         0
##   Extensive    0      0    0    1         0
## 
## , ,  = McGonagall
## 
##            
##             None Little Some Much Extensive
##   None         0      0    0    0         0
##   Little       0      1    0    1         1
##   Some         0      1    1    9         1
##   Much         0      0    6    7         4
##   Extensive    0      1    0    0         0
## 
## , ,  = Sinistra
## 
##            
##             None Little Some Much Extensive
##   None         0      0    0    0         0
##   Little       0      1    2    1         1
##   Some         2      2    1   17         4
##   Much         1      2    5    5        12
##   Extensive    0      0    2    2         0
```

Balloon Plot

On these plots, the answers on the x axis are the pre-survey results and on the y-axis
are the post survey results. 
Responses above the "none" level indicate students who felt that they increased in this element.


The pre-survey questions were asked as:
Please look over this inventory of elements that might be included in a course. For each element, give an estimate of your current level of ability before the course begins. Your current level of ability may be a result of courses in high school or college, or it may be a result of other experiences such as jobs or special programs. If students are expected to do the following course elements, what would be their level of expertise? 


The post-survey questions were asked: 
Please rate how much learning you gained from each element you experienced in this course. The scale measuring your gain is from (no or very small gain) to (very large gain). Some elements may not have happened at all. If the item is not relevant or you prefer not to answer, please choose the "not applicable" option. If students were expected to do the following course elements, what would be their level of gained experience? 


```
## Dumbledore     Hagrid      Lupin McGonagall   Sinistra 
##         70         35         24         33         62
```


After feedback from the reviewers we have adopted a simpler approach to the analysis. 
We are dropping the interaction term between Rookie and Instructor as that was heavily confounded by semester.
We considered using a mixed effects model with semester taught as a random effect. 
This did not significantly change the outcome of the analysis and did not match our a priori analysis plan, so it was not included.
We are going to report F-test results for all variables (no model selection) and then use an alpha cutoff of 0.05 to determine which visualizations to show.
Further reading also suggested to use an F-test for testing Gaussian GLMs, rather than an LRT or Chi-squared test

## Q10 Figures  {.tabset .tabset-pills}

### Q10_1

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_1-1.png)<!-- -->

```
## [1] "Dumbledore" "Hagrid"     "Lupin"      "McGonagall" "Sinistra"
```

```
## 
## Call:
## glm(formula = Q9_1_post ~ Q10_1_pre + Instructor + Rookie, data = Q_1Clean)
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           3.542224   0.333345  10.626   <2e-16 ***
## Q10_1_pre             0.129947   0.086319   1.505   0.1337    
## InstructorHagrid     -0.545433   0.227678  -2.396   0.0175 *  
## InstructorLupin      -0.271429   0.256505  -1.058   0.2912    
## InstructorMcGonagall -0.003572   0.223378  -0.016   0.9873    
## InstructorSinistra    0.035753   0.186927   0.191   0.8485    
## RookieVeteran        -0.268776   0.170035  -1.581   0.1155    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.091505)
## 
##     Null deviance: 238.49  on 214  degrees of freedom
## Residual deviance: 227.03  on 208  degrees of freedom
## AIC: 637.85
## 
## Number of Fisher Scoring iterations: 2
```

```
## Linear mixed model fit by REML ['lmerMod']
## Formula: Q9_1_post ~ Q10_1_pre + Instructor + Rookie + (1 | Semester)
##    Data: Q_1Clean
## 
## REML criterion at convergence: 635.4
## 
## Scaled residuals: 
##     Min      1Q  Median      3Q     Max 
## -2.7450 -0.6202  0.2128  0.7165  1.6628 
## 
## Random effects:
##  Groups   Name        Variance Std.Dev.
##  Semester (Intercept) 0.01261  0.1123  
##  Residual             1.08536  1.0418  
## Number of obs: 215, groups:  Semester, 6
## 
## Fixed effects:
##                      Estimate Std. Error t value
## (Intercept)           3.57284    0.34394  10.388
## Q10_1_pre             0.12377    0.08630   1.434
## InstructorHagrid     -0.58275    0.24574  -2.371
## InstructorLupin      -0.25766    0.25922  -0.994
## InstructorMcGonagall -0.03093    0.24097  -0.128
## InstructorSinistra   -0.01689    0.19581  -0.086
## RookieVeteran        -0.25086    0.19000  -1.320
## 
## Correlation of Fixed Effects:
##             (Intr) Q10_1_ InstrH InstrL InstMG InstrS
## Q10_1_pre   -0.803                                   
## InstrctrHgr -0.267 -0.060                            
## InstrctrLpn -0.201 -0.050  0.284                     
## InstrctrMcG -0.156 -0.054  0.373  0.265              
## InstrctrSns -0.225 -0.047  0.425  0.297  0.405       
## RookieVetrn -0.370 -0.078  0.200  0.137 -0.063 -0.013
```

```
## # Comparison of Model Performance Indices
## 
## Name             |   Model | AIC (weights) | AICc (weights) | BIC (weights)
## ---------------------------------------------------------------------------
## imp_model_Q10_1  |     glm | 637.9 (0.807) |  638.6 (0.821) | 664.8 (0.958)
## lmer_model_Q10_1 | lmerMod | 640.7 (0.193) |  641.6 (0.179) | 671.1 (0.042)
## 
## Name             |  RMSE | Sigma |    R2 | R2 (cond.) | R2 (marg.) |   ICC
## --------------------------------------------------------------------------
## imp_model_Q10_1  | 1.028 | 1.045 | 0.048 |            |            |      
## lmer_model_Q10_1 | 1.022 | 1.042 |       |      0.056 |      0.045 | 0.011
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_1_post
## Error estimate based on Pearson residuals 
## 
##             Sum Sq  Df F values  Pr(>F)  
## Q10_1_pre    2.474   1   2.2663 0.13373  
## Instructor   8.682   4   1.9886 0.09749 .
## Rookie       2.727   1   2.4987 0.11546  
## Residuals  227.033 208                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## 
## Call:
## glm(formula = Q9_1_post ~ Q10_1_pre + Instructor + Rookie, data = Q_1Clean, 
##     contrasts = type3)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.25090    0.30593  10.626   <2e-16 ***
## Q10_1_pre    0.12995    0.08632   1.505    0.134    
## Instructor1  0.15694    0.12756   1.230    0.220    
## Instructor2 -0.38850    0.16292  -2.385    0.018 *  
## Instructor3 -0.11449    0.18699  -0.612    0.541    
## Instructor4  0.15336    0.16085   0.953    0.341    
## Rookie1      0.13439    0.08502   1.581    0.115    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.091505)
## 
##     Null deviance: 238.49  on 214  degrees of freedom
## Residual deviance: 227.03  on 208  degrees of freedom
## AIC: 637.85
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_1_post
## Error estimate based on Pearson residuals 
## 
##             Sum Sq  Df F values  Pr(>F)  
## Q10_1_pre    2.474   1   2.2663 0.13373  
## Instructor   8.682   4   1.9886 0.09749 .
## Rookie       2.727   1   2.4987 0.11546  
## Residuals  227.033 208                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Nothing met the alpha cutoff of 0.05

### Q10_2

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_2-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_2_post) ~ as.numeric(Q10_2_pre) + 
##     Instructor + Rookie, data = Q_2Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            4.039253   0.312766  12.915   <2e-16 ***
## as.numeric(Q10_2_pre) -0.062860   0.080667  -0.779   0.4367    
## InstructorHagrid      -0.478091   0.215539  -2.218   0.0276 *  
## InstructorLupin        0.009228   0.239845   0.038   0.9693    
## InstructorMcGonagall  -0.055219   0.214384  -0.258   0.7970    
## InstructorSinistra     0.010218   0.177923   0.057   0.9543    
## RookieVeteran         -0.193220   0.159466  -1.212   0.2270    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9872623)
## 
##     Null deviance: 212.96  on 214  degrees of freedom
## Residual deviance: 205.35  on 208  degrees of freedom
## AIC: 616.27
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_2_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_2_pre)   0.600   1   0.6072 0.4367
## Instructor              6.107   4   1.5464 0.1900
## Rookie                  1.449   1   1.4681 0.2270
## Residuals             205.351 208
```

Nothing met the cutoff

### Q10_3

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_3-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_3_post) ~ as.numeric(Q10_3_pre) + 
##     Instructor + Rookie, data = Q_3Clean)
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            4.24131    0.21269  19.942  < 2e-16 ***
## as.numeric(Q10_3_pre)  0.06987    0.06528   1.070 0.285616    
## InstructorHagrid      -0.74456    0.21278  -3.499 0.000567 ***
## InstructorLupin       -0.63399    0.24031  -2.638 0.008944 ** 
## InstructorMcGonagall  -0.14272    0.21437  -0.666 0.506279    
## InstructorSinistra    -0.16036    0.17696  -0.906 0.365830    
## RookieVeteran         -0.18606    0.15751  -1.181 0.238803    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.00173)
## 
##     Null deviance: 231.93  on 221  degrees of freedom
## Residual deviance: 215.37  on 215  degrees of freedom
## AIC: 639.28
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_3_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)   
## as.numeric(Q10_3_pre)   1.148   1   1.1459 0.2856   
## Instructor             16.135   4   4.0267 0.0036 **
## Rookie                  1.398   1   1.3954 0.2388   
## Residuals             215.372 215                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets alpha cutoff.

### Q10_4

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_4-1.png)<!-- -->

```
## 
## Call:
## glm(formula = Q9_4_post ~ Q10_4_pre + Instructor + Rookie, data = Q_4Clean)
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           4.30071    0.26422  16.277  < 2e-16 ***
## Q10_4_pre             0.01481    0.06583   0.225  0.82224    
## InstructorHagrid     -0.54058    0.16455  -3.285  0.00119 ** 
## InstructorLupin      -0.07156    0.18467  -0.388  0.69875    
## InstructorMcGonagall  0.11406    0.16282   0.701  0.48435    
## InstructorSinistra    0.02087    0.13442   0.155  0.87677    
## RookieVeteran        -0.14094    0.12047  -1.170  0.24331    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.5907875)
## 
##     Null deviance: 137.5  on 223  degrees of freedom
## Residual deviance: 128.2  on 217  degrees of freedom
## AIC: 526.68
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_4_post
## Error estimate based on Pearson residuals 
## 
##             Sum Sq  Df F values   Pr(>F)   
## Q10_4_pre    0.030   1   0.0506 0.822237   
## Instructor   9.121   4   3.8599 0.004736 **
## Rookie       0.809   1   1.3688 0.243307   
## Residuals  128.201 217                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets alpha cutoff.

### Final Figure 3A Q10_5

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_5-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_5_post) ~ as.numeric(Q10_5_pre) + 
##     Instructor + Rookie, data = Q_5Clean)
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            4.48267    0.16707  26.832  < 2e-16 ***
## as.numeric(Q10_5_pre)  0.08996    0.04453   2.020   0.0446 *  
## InstructorHagrid      -0.73932    0.13828  -5.346 2.28e-07 ***
## InstructorLupin       -0.71325    0.15657  -4.556 8.73e-06 ***
## InstructorMcGonagall  -0.20793    0.13891  -1.497   0.1359    
## InstructorSinistra    -0.03650    0.11457  -0.319   0.7504    
## RookieVeteran         -0.11488    0.10259  -1.120   0.2640    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4266137)
## 
##     Null deviance: 112.933  on 222  degrees of freedom
## Residual deviance:  92.149  on 216  degrees of freedom
## AIC: 451.77
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_5_post)
## Error estimate based on Pearson residuals 
## 
##                       Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_5_pre)  1.741   1   4.0809    0.0446 *  
## Instructor            19.286   4  11.3015 2.405e-08 ***
## Rookie                 0.535   1   1.2541    0.2640    
## Residuals             92.149 216                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Pre and Instructor meets alpha cutoff.

### Q10_6

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_6-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_6_post) ~ as.numeric(Q10_6_pre) + 
##     Instructor + Rookie, data = Q_6Clean)
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            4.57550    0.14779  30.958  < 2e-16 ***
## as.numeric(Q10_6_pre)  0.03204    0.03911   0.819    0.414    
## InstructorHagrid      -1.05492    0.14538  -7.256 7.22e-12 ***
## InstructorLupin       -1.14523    0.16586  -6.905 5.64e-11 ***
## InstructorMcGonagall  -0.06972    0.14709  -0.474    0.636    
## InstructorSinistra     0.15575    0.12170   1.280    0.202    
## RookieVeteran         -0.11579    0.10939  -1.059    0.291    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4731961)
## 
##     Null deviance: 155.83  on 220  degrees of freedom
## Residual deviance: 101.26  on 214  degrees of freedom
## AIC: 470.7
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_6_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)    
## as.numeric(Q10_6_pre)   0.318   1   0.6711 0.4136    
## Instructor             52.812   4  27.9020 <2e-16 ***
## Rookie                  0.530   1   1.1205 0.2910    
## Residuals             101.264 214                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets alpha cutoff.

### Q10_7

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_7-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_7_post) ~ as.numeric(Q10_7_pre) + 
##     Instructor + Rookie, data = Q_7Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            3.774283   0.309431  12.197   <2e-16 ***
## as.numeric(Q10_7_pre)  0.020782   0.069579   0.299   0.7655    
## InstructorHagrid      -0.553463   0.218064  -2.538   0.0119 *  
## InstructorLupin       -0.271686   0.248829  -1.092   0.2761    
## InstructorMcGonagall  -0.157586   0.219074  -0.719   0.4727    
## InstructorSinistra    -0.009061   0.181243  -0.050   0.9602    
## RookieVeteran         -0.082937   0.161656  -0.513   0.6084    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.067055)
## 
##     Null deviance: 238.72  on 222  degrees of freedom
## Residual deviance: 230.48  on 216  degrees of freedom
## AIC: 656.21
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_7_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_7_pre)   0.095   1   0.0892 0.7655
## Instructor              8.209   4   1.9234 0.1076
## Rookie                  0.281   1   0.2632 0.6084
## Residuals             230.484 216
```

Nothing meets cutoff.

### Q10_8

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_8-1.png)<!-- -->

```
## 
## Call:
## glm(formula = Q9_8_post ~ Q10_8_pre + Instructor + Rookie, data = Q_8Clean)
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           3.70229    0.29263  12.652  < 2e-16 ***
## Q10_8_pre             0.11847    0.06808   1.740  0.08323 .  
## InstructorHagrid     -0.87314    0.22168  -3.939  0.00011 ***
## InstructorLupin      -0.68531    0.25300  -2.709  0.00729 ** 
## InstructorMcGonagall -0.42748    0.22299  -1.917  0.05655 .  
## InstructorSinistra   -0.22594    0.18637  -1.212  0.22671    
## RookieVeteran        -0.38091    0.16491  -2.310  0.02184 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.109311)
## 
##     Null deviance: 265.98  on 223  degrees of freedom
## Residual deviance: 240.72  on 217  degrees of freedom
## AIC: 667.81
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_8_post
## Error estimate based on Pearson residuals 
## 
##             Sum Sq  Df F values   Pr(>F)   
## Q10_8_pre    3.360   1   3.0286 0.083227 . 
## Instructor  20.721   4   4.6697 0.001234 **
## Rookie       5.918   1   5.3349 0.021843 * 
## Residuals  240.721 217                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor and Rookie meet alpha cutoff.

### Q10_9

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_9-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_9_post) ~ as.numeric(Q10_9_pre) + 
##     Instructor + Rookie, data = Q_9Clean)
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            4.38943    0.26089  16.825  < 2e-16 ***
## as.numeric(Q10_9_pre)  0.08314    0.05955   1.396  0.16408    
## InstructorHagrid      -0.36545    0.13396  -2.728  0.00689 ** 
## InstructorLupin       -0.26498    0.15238  -1.739  0.08347 .  
## InstructorMcGonagall  -0.07334    0.13400  -0.547  0.58474    
## InstructorSinistra     0.02810    0.11082   0.254  0.80010    
## RookieVeteran         -0.11285    0.09890  -1.141  0.25510    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.3991157)
## 
##     Null deviance: 91.049  on 222  degrees of freedom
## Residual deviance: 86.209  on 216  degrees of freedom
## AIC: 436.91
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_9_post)
## Error estimate based on Pearson residuals 
## 
##                       Sum Sq  Df F values  Pr(>F)  
## as.numeric(Q10_9_pre)  0.778   1   1.9495 0.16408  
## Instructor             4.416   4   2.7662 0.02842 *
## Rookie                 0.520   1   1.3021 0.25510  
## Residuals             86.209 216                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_10

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_10-1.png)<!-- -->

```
## 
## Call:
## glm(formula = Q9_10_post ~ Q10_10_pre + Instructor + Rookie, 
##     data = Q_10Clean)
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           4.31532    0.21321  20.239  < 2e-16 ***
## Q10_10_pre            0.11776    0.05273   2.233 0.026550 *  
## InstructorHagrid     -0.46562    0.12918  -3.604 0.000389 ***
## InstructorLupin      -0.20946    0.14501  -1.444 0.150066    
## InstructorMcGonagall -0.12292    0.13053  -0.942 0.347413    
## InstructorSinistra    0.10861    0.10616   1.023 0.307423    
## RookieVeteran        -0.20079    0.09559  -2.101 0.036840 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.3637216)
## 
##     Null deviance: 86.955  on 221  degrees of freedom
## Residual deviance: 78.200  on 215  degrees of freedom
## AIC: 414.37
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_10_post
## Error estimate based on Pearson residuals 
## 
##            Sum Sq  Df F values   Pr(>F)    
## Q10_10_pre  1.814   1   4.9883 0.026550 *  
## Instructor  7.620   4   5.2374 0.000481 ***
## Rookie      1.605   1   4.4126 0.036840 *  
## Residuals  78.200 215                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor and Rookie meet cutoff.

### Q10_11

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_11-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_11_post) ~ as.numeric(Q10_11_pre) + 
##     Instructor + Rookie, data = Q_11Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.17305    0.17426  23.947  < 2e-16 ***
## as.numeric(Q10_11_pre)  0.12238    0.04814   2.542   0.0117 *  
## InstructorHagrid       -0.79692    0.14457  -5.512 1.01e-07 ***
## InstructorLupin        -0.14343    0.15956  -0.899   0.3697    
## InstructorMcGonagall   -0.02335    0.14274  -0.164   0.8702    
## InstructorSinistra     -0.12116    0.12048  -1.006   0.3157    
## RookieVeteran          -0.03077    0.10565  -0.291   0.7712    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4416128)
## 
##     Null deviance: 111.968  on 221  degrees of freedom
## Residual deviance:  94.947  on 215  degrees of freedom
## AIC: 457.45
## 
## Number of Fisher Scoring iterations: 2
```

```
## 
## Call:
## glm(formula = as.numeric(Q9_11_post) ~ as.numeric(Q10_11_pre) + 
##     Instructor + Rookie, data = Q_11Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.17305    0.17426  23.947  < 2e-16 ***
## as.numeric(Q10_11_pre)  0.12238    0.04814   2.542   0.0117 *  
## InstructorHagrid       -0.79692    0.14457  -5.512 1.01e-07 ***
## InstructorLupin        -0.14343    0.15956  -0.899   0.3697    
## InstructorMcGonagall   -0.02335    0.14274  -0.164   0.8702    
## InstructorSinistra     -0.12116    0.12048  -1.006   0.3157    
## RookieVeteran          -0.03077    0.10565  -0.291   0.7712    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4416128)
## 
##     Null deviance: 111.968  on 221  degrees of freedom
## Residual deviance:  94.947  on 215  degrees of freedom
## AIC: 457.45
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_11_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_11_pre)  2.854   1   6.4626   0.01172 *  
## Instructor             15.417   4   8.7278 1.513e-06 ***
## Rookie                  0.037   1   0.0848   0.77117    
## Residuals              94.947 215                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_12

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_12-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_12_post) ~ as.numeric(Q10_12_pre) + 
##     Instructor + Rookie, data = Q_12Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             3.75480    0.19708  19.052   <2e-16 ***
## as.numeric(Q10_12_pre)  0.10461    0.05521   1.895   0.0595 .  
## InstructorHagrid       -0.32318    0.19396  -1.666   0.0971 .  
## InstructorLupin        -0.29233    0.22010  -1.328   0.1855    
## InstructorMcGonagall   -0.01671    0.19514  -0.086   0.9318    
## InstructorSinistra      0.38299    0.16326   2.346   0.0199 *  
## RookieVeteran           0.12496    0.14572   0.858   0.3921    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8366148)
## 
##     Null deviance: 200.65  on 220  degrees of freedom
## Residual deviance: 179.04  on 214  degrees of freedom
## AIC: 596.63
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## as.numeric(Q9_12_post) ~ as.numeric(Q10_12_pre) + Instructor + 
##     Rookie
##                        Df Deviance    AIC scaled dev. Pr(>Chi)   
## <none>                      179.04 596.63                        
## as.numeric(Q10_12_pre)  1   182.04 598.31      3.6764 0.055189 . 
## Instructor              4   192.65 604.83     16.1932 0.002771 **
## Rookie                  1   179.65 595.39      0.7582 0.383903   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_12_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values   Pr(>F)   
## as.numeric(Q10_12_pre)   3.003   1   3.5897 0.059487 . 
## Instructor              13.611   4   4.0673 0.003369 **
## Rookie                   0.615   1   0.7354 0.392093   
## Residuals              179.036 214                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_13

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_13-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_13_post) ~ as.numeric(Q10_13_pre) + 
##     Instructor + Rookie, data = Q_13Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.47608    0.21579  20.743  < 2e-16 ***
## as.numeric(Q10_13_pre)  0.11686    0.04989   2.342 0.020076 *  
## InstructorHagrid       -0.78363    0.13645  -5.743 3.15e-08 ***
## InstructorLupin        -0.60128    0.15537  -3.870 0.000144 ***
## InstructorMcGonagall   -0.12205    0.13687  -0.892 0.373537    
## InstructorSinistra     -0.08739    0.11464  -0.762 0.446688    
## RookieVeteran          -0.20228    0.10231  -1.977 0.049301 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4195731)
## 
##     Null deviance: 111.279  on 221  degrees of freedom
## Residual deviance:  90.208  on 215  degrees of freedom
## AIC: 446.09
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_13_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_13_pre)  2.302   1   5.4867   0.02008 *  
## Instructor             18.070   4  10.7670 5.663e-08 ***
## Rookie                  1.640   1   3.9092   0.04930 *  
## Residuals              90.208 215                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor and Rookie meets cutoff

### Q10_14

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_14-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_14_post) ~ as.numeric(Q10_14_pre) + 
##     Instructor + Rookie, data = Q_14Clean)
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.076044   0.220608  18.476  < 2e-16 ***
## as.numeric(Q10_14_pre)  0.175277   0.050415   3.477 0.000614 ***
## InstructorHagrid       -0.715905   0.137369  -5.212 4.38e-07 ***
## InstructorLupin        -0.714757   0.156141  -4.578 7.95e-06 ***
## InstructorMcGonagall   -0.137486   0.137505  -1.000 0.318503    
## InstructorSinistra      0.002071   0.114792   0.018 0.985622    
## RookieVeteran          -0.061259   0.102929  -0.595 0.552367    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.4237034)
## 
##     Null deviance: 117.279  on 221  degrees of freedom
## Residual deviance:  91.096  on 215  degrees of freedom
## AIC: 448.26
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_2_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_2_pre)   0.600   1   0.6072 0.4367
## Instructor              6.107   4   1.5464 0.1900
## Rookie                  1.449   1   1.4681 0.2270
## Residuals             205.351 208
```

Nothing meets cutoff

### Q10_15

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_15-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_15_post) ~ as.numeric(Q10_15_pre) + 
##     Instructor + Rookie, data = Q_15Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             3.79082    0.21241  17.847  < 2e-16 ***
## as.numeric(Q10_15_pre)  0.16235    0.05438   2.986 0.003159 ** 
## InstructorHagrid       -0.25371    0.18154  -1.398 0.163696    
## InstructorLupin        -0.84378    0.20525  -4.111 5.62e-05 ***
## InstructorMcGonagall   -0.68594    0.18324  -3.743 0.000233 ***
## InstructorSinistra      0.16942    0.14944   1.134 0.258192    
## RookieVeteran          -0.01573    0.13329  -0.118 0.906184    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7172459)
## 
##     Null deviance: 185.70  on 220  degrees of freedom
## Residual deviance: 153.49  on 214  degrees of freedom
## AIC: 562.61
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_15_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_15_pre)   6.394   1   8.9145  0.003159 ** 
## Instructor              27.491   4   9.5821 3.808e-07 ***
## Rookie                   0.010   1   0.0139  0.906184    
## Residuals              153.491 214                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff

### Q10_16

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_16-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_16_post) ~ as.numeric(Q10_16_pre) + 
##     Instructor + Rookie, data = Q_16Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             3.97099    0.21420  18.539  < 2e-16 ***
## as.numeric(Q10_16_pre)  0.12244    0.05186   2.361  0.01913 *  
## InstructorHagrid       -0.51950    0.16241  -3.199  0.00159 ** 
## InstructorLupin        -0.54802    0.18443  -2.971  0.00330 ** 
## InstructorMcGonagall   -0.03891    0.16272  -0.239  0.81122    
## InstructorSinistra      0.10064    0.13475   0.747  0.45596    
## RookieVeteran          -0.05397    0.12032  -0.449  0.65422    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.5906496)
## 
##     Null deviance: 145.09  on 222  degrees of freedom
## Residual deviance: 127.58  on 216  degrees of freedom
## AIC: 524.32
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_2_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_2_pre)   0.600   1   0.6072 0.4367
## Instructor              6.107   4   1.5464 0.1900
## Rookie                  1.449   1   1.4681 0.2270
## Residuals             205.351 208
```

Nothing meets cutoff

### Q10_17

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_17-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_17_post) ~ as.numeric(Q10_17_pre) + 
##     Instructor + Rookie, data = Q_17Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.06476    0.22142  18.358  < 2e-16 ***
## as.numeric(Q10_17_pre)  0.04555    0.05377   0.847  0.39780    
## InstructorHagrid       -0.29657    0.17776  -1.668  0.09668 .  
## InstructorLupin        -0.53891    0.20200  -2.668  0.00821 ** 
## InstructorMcGonagall   -0.04281    0.18029  -0.237  0.81251    
## InstructorSinistra      0.28850    0.14747   1.956  0.05172 .  
## RookieVeteran          -0.09027    0.13237  -0.682  0.49599    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7107257)
## 
##     Null deviance: 168.20  on 222  degrees of freedom
## Residual deviance: 153.52  on 216  degrees of freedom
## AIC: 565.59
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_17_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_17_pre)   0.510   1   0.7178 0.3978039    
## Instructor              14.143   4   4.9748 0.0007435 ***
## Rookie                   0.331   1   0.4651 0.4959934    
## Residuals              153.517 216                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_18

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_18-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_18_post) ~ as.numeric(Q10_18_pre) + 
##     Instructor + Rookie, data = Q_18Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             3.96202    0.19471  20.348  < 2e-16 ***
## as.numeric(Q10_18_pre)  0.13812    0.05454   2.533    0.012 *  
## InstructorHagrid       -0.94203    0.17924  -5.256 3.56e-07 ***
## InstructorLupin        -0.91211    0.19959  -4.570 8.24e-06 ***
## InstructorMcGonagall   -0.10211    0.17794  -0.574    0.567    
## InstructorSinistra     -0.20153    0.14736  -1.368    0.173    
## RookieVeteran          -0.02412    0.13131  -0.184    0.854    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6880384)
## 
##     Null deviance: 180.78  on 220  degrees of freedom
## Residual deviance: 147.24  on 214  degrees of freedom
## AIC: 553.42
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_2_post)
## Error estimate based on Pearson residuals 
## 
##                        Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_2_pre)   0.600   1   0.6072 0.4367
## Instructor              6.107   4   1.5464 0.1900
## Rookie                  1.449   1   1.4681 0.2270
## Residuals             205.351 208
```

Nothing meets cutoff.

### Q10_19

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_19-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_19_post) ~ as.numeric(Q10_19_pre) + 
##     Instructor + Rookie, data = Q_19Clean)
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             2.986703   0.327333   9.124  < 2e-16 ***
## as.numeric(Q10_19_pre)  0.058648   0.072600   0.808 0.420101    
## InstructorHagrid        0.029507   0.242674   0.122 0.903340    
## InstructorLupin         0.250393   0.271019   0.924 0.356602    
## InstructorMcGonagall   -0.002791   0.247064  -0.011 0.990998    
## InstructorSinistra      0.672033   0.199586   3.367 0.000903 ***
## RookieVeteran          -0.146923   0.179929  -0.817 0.415103    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.266436)
## 
##     Null deviance: 285.77  on 216  degrees of freedom
## Residual deviance: 265.95  on 210  degrees of freedom
## AIC: 675.96
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_19_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values Pr(>F)   
## as.numeric(Q10_19_pre)   0.826   1   0.6526 0.4201   
## Instructor              18.243   4   3.6013 0.0073 **
## Rookie                   0.844   1   0.6668 0.4151   
## Residuals              265.952 210                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_20

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_20-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_20_post) ~ as.numeric(Q10_20_pre) + 
##     Instructor + Rookie, data = Q_20Clean)
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             1.928903   0.382953   5.037 1.11e-06 ***
## as.numeric(Q10_20_pre)  0.015416   0.079076   0.195   0.8456    
## InstructorHagrid        0.279209   0.277501   1.006   0.3156    
## InstructorLupin        -0.128520   0.307085  -0.419   0.6761    
## InstructorMcGonagall   -0.005815   0.285295  -0.020   0.9838    
## InstructorSinistra      0.518071   0.224456   2.308   0.0221 *  
## RookieVeteran           0.071371   0.211078   0.338   0.7356    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.469149)
## 
##     Null deviance: 286.34  on 193  degrees of freedom
## Residual deviance: 274.73  on 187  degrees of freedom
## AIC: 634.05
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_20_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values Pr(>F)
## as.numeric(Q10_20_pre)   0.056   1   0.0380 0.8456
## Instructor              11.276   4   1.9188 0.1090
## Rookie                   0.168   1   0.1143 0.7356
## Residuals              274.731 187
```

Nothing meets cutoff.

### Q10_21

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_21-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_21_post) ~ as.numeric(Q10_21_pre) + 
##     Instructor + Rookie, data = Q_21Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             2.50078    0.47387   5.277 3.74e-07 ***
## as.numeric(Q10_21_pre) -0.07181    0.10367  -0.693   0.4894    
## InstructorHagrid        0.33877    0.30662   1.105   0.2707    
## InstructorLupin        -0.23084    0.33437  -0.690   0.4909    
## InstructorMcGonagall   -0.18164    0.31079  -0.584   0.5596    
## InstructorSinistra      0.61294    0.24775   2.474   0.0143 *  
## RookieVeteran          -0.04347    0.23052  -0.189   0.8506    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.715395)
## 
##     Null deviance: 328.99  on 186  degrees of freedom
## Residual deviance: 308.77  on 180  degrees of freedom
## AIC: 640.46
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_21_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values  Pr(>F)  
## as.numeric(Q10_21_pre)   0.823   1   0.4798 0.48941  
## Instructor              18.993   4   2.7680 0.02886 *
## Rookie                   0.061   1   0.0356 0.85064  
## Residuals              308.771 180                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_22

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_22-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_22_post) ~ as.numeric(Q10_22_pre) + 
##     Instructor + Rookie, data = Q_22Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             2.02841    0.41057   4.940 1.75e-06 ***
## as.numeric(Q10_22_pre) -0.04083    0.08893  -0.459   0.6467    
## InstructorHagrid        0.44098    0.27907   1.580   0.1158    
## InstructorLupin        -0.27880    0.30722  -0.908   0.3653    
## InstructorMcGonagall    0.04492    0.28500   0.158   0.8749    
## InstructorSinistra      0.57764    0.22587   2.557   0.0114 *  
## RookieVeteran           0.06859    0.21153   0.324   0.7461    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.451597)
## 
##     Null deviance: 283.45  on 189  degrees of freedom
## Residual deviance: 265.64  on 183  degrees of freedom
## AIC: 618.87
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_22_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values  Pr(>F)  
## as.numeric(Q10_22_pre)   0.306   1   0.2108 0.64672  
## Instructor              16.964   4   2.9216 0.02252 *
## Rookie                   0.153   1   0.1052 0.74610  
## Residuals              265.642 183                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Q10_23

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_23-1.png)<!-- -->

```
## 
## Call:
## glm(formula = Q9_23_post ~ Q10_23_pre + Instructor + Rookie, 
##     data = Q_23Clean)
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           3.63363    0.31031  11.710  < 2e-16 ***
## Q10_23_pre            0.16734    0.06837   2.448   0.0152 *  
## InstructorHagrid     -1.43674    0.18925  -7.592  9.5e-13 ***
## InstructorLupin      -0.16943    0.21316  -0.795   0.4276    
## InstructorMcGonagall -0.10361    0.18986  -0.546   0.5858    
## InstructorSinistra   -0.06658    0.15531  -0.429   0.6686    
## RookieVeteran        -0.18250    0.13957  -1.308   0.1924    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7900713)
## 
##     Null deviance: 227.41  on 221  degrees of freedom
## Residual deviance: 169.87  on 215  degrees of freedom
## AIC: 586.59
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Q9_23_post
## Error estimate based on Pearson residuals 
## 
##             Sum Sq  Df F values    Pr(>F)    
## Q10_23_pre   4.733   1   5.9911   0.01518 *  
## Instructor  52.781   4  16.7013 6.175e-12 ***
## Rookie       1.351   1   1.7098   0.19241    
## Residuals  169.865 215                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Instructor meets cutoff.

### Final Figure 3B Q10_24

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 3 Q10_24-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_24_post) ~ as.numeric(Q10_24_pre) + 
##     Instructor + Rookie, data = Q_24Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             4.32854    0.20561  21.052  < 2e-16 ***
## as.numeric(Q10_24_pre)  0.13282    0.04868   2.728  0.00691 ** 
## InstructorHagrid       -2.17999    0.18290 -11.919  < 2e-16 ***
## InstructorLupin        -0.15903    0.19801  -0.803  0.42279    
## InstructorMcGonagall   -0.22559    0.17586  -1.283  0.20098    
## InstructorSinistra     -0.21544    0.14445  -1.491  0.13735    
## RookieVeteran          -0.17115    0.13170  -1.300  0.19520    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6786781)
## 
##     Null deviance: 254.48  on 216  degrees of freedom
## Residual deviance: 142.52  on 210  degrees of freedom
## AIC: 540.59
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_24_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_24_pre)   5.051   1   7.4427  0.006909 ** 
## Instructor             107.039   4  39.4291 < 2.2e-16 ***
## Rookie                   1.146   1   1.6887  0.195196    
## Residuals              142.522 210                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Pre and Instructor meet cutoff.

### Q10_25

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_25-1.png)<!-- -->

```
## 
## Call:
## glm(formula = as.numeric(Q9_25_post) ~ as.numeric(Q10_25_pre) + 
##     Instructor + Rookie, data = Q_25Clean)
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)             3.72720    0.23574  15.810  < 2e-16 ***
## as.numeric(Q10_25_pre)  0.16325    0.06696   2.438 0.015646 *  
## InstructorHagrid       -1.28376    0.23174  -5.540 9.52e-08 ***
## InstructorLupin        -0.65351    0.26402  -2.475 0.014152 *  
## InstructorMcGonagall   -0.83873    0.24615  -3.407 0.000794 ***
## InstructorSinistra     -0.05260    0.19240  -0.273 0.784856    
## RookieVeteran          -0.27850    0.18345  -1.518 0.130569    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.138992)
## 
##     Null deviance: 281.38  on 205  degrees of freedom
## Residual deviance: 226.66  on 199  degrees of freedom
## AIC: 620.29
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Q9_25_post)
## Error estimate based on Pearson residuals 
## 
##                         Sum Sq  Df F values    Pr(>F)    
## as.numeric(Q10_25_pre)   6.770   1   5.9439   0.01565 *  
## Instructor              47.560   4  10.4390 1.072e-07 ***
## Rookie                   2.625   1   2.3047   0.13057    
## Residuals              226.659 199                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Pre and Instructor meet cutoff.

## Summary

For each question, a generalized linear model with a Gaussian linking function determined if the student's post-survey response was dependent on (1) that student's pre-survey response, (2) which instructor they had, and (3) whether it was that instructor's first semester teaching in the CURE Lab (Rookie status).
An alpha value of 0.05 was used to define statistical significance.

Removing "Q10_02", "Q10_19", "Q10_20", "Q10_21", "Q10_22", "Q10_25" *See below (not emphasized by any instructors)

The glm showed dependence on *instructor* and *rookie* status, but not pre-survey response, for the following question:

8, 10, 13

-  [8] "Work as a whole class"   
-  [10] "Become responsible for a part of the project"  
-  [13] "Collect data" 

The following questions showed dependence on *pre-survey response* and *instructor*, but not rookie status:

5, 24, -25-

-  [5] "A project in which students have some input into the research process and/or what is being studied"
-  [24] "Maintain a lab notebook"   
-  [25] "Computer modeling" *Removed 

The following questions showed dependence on *instructor*, but not pre-survey response or rookie status:

3, 4, 6, 9, 11, 12, 15, 17, -19-, -21-, -22-, 23 

- [3] "A lab or project where no one knows the outcome"   
- [4] "At least one project that is assigned and structured by the instructor" 
- [6] "A project entirely of student design"  
- [9] "Work in small groups"  
- [11] "Read primary scientific literature" 
- [12] "Write a research proposal" 
- [15] "Present results orally"
- [17] "Present posters"                                                                                   
- [19] "Listen to lectures" *Removed                                                                                 
- [21] "Work on problem sets" *Removed                                                                               
- [22] "Take tests in class" *Removed 
- [23] "Discuss reading materials in class"  

And the responses to these survey questions were independent of pre-survey responses, the instructor, and the rookie status:

1, -2-, 7, 14, 16, 18, -20-

- [1] "A scripted lab or project in which the students know the expected outcome" 
- [2] "A lab or project in which only the instructor knows the outcome" *Removed                               
- [7] "Work individually" 
- [14] "Analyze data" 
- [16] "Present results in written papers or reports"
- [18] "Critique the work of other students" 
- [20] "Read a textbook" *Removed                                                                                    

It is interesting to note that for every question that was significantly impacted by rookie status, the veteran status had a negative estimate. 
This means that students in an instructor's first semester perceived larger gains than in subsequent semesters. 
This does not support our hypothesis that experience would make the instructors more proficient at assisting student improvement in the learning elements.

## Figures

For questions 4 and 23 we want to show the interaction between instructor and rookie.
Although for 4, the presurvey response wasn't important, so we will use a cat plot.

### Figure 4A 4B and 4C

Questions 8, 10, and 13 need a cat plot showing Instructor and Rookie


```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Catplot 8-1.png)<!-- -->

```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```


```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Catplot 10-1.png)<!-- -->

```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```


```
## Using data Q_13Clean from global environment. This could cause incorrect
## results if Q_13Clean has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Catplot 13-1.png)<!-- -->

```
## Using data Q_13Clean from global environment. This could cause incorrect
## results if Q_13Clean has been altered since the model was fit. You can
## manually provide the data to the "data =" argument.
```

```
## Warning: Instructor and Rookie are not included in an interaction with one another
## in the model.
```


### Removed Figure Q10_2

This was not emphasized

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_2 No instructor-1.png)<!-- -->

### Removed Figure Q10_21

This was not emphasized.

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Q10_21 No instructor-1.png)<!-- -->

For questions where the instructor was included in the final model, but not Rookie status or the pre-survey response (3, 6, and 17) and ALSO the question where it did not depend on any of the variables (7):


### Final Figure 2 v 2026

Depend on Instructor only:

3, 4, 6, 9, 11, 12, 15, 17, -19-, -21-, -22-, 23 

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Figure 2 Instructor Questions-1.png)<!-- -->

```
## [1] "A lab or project where no one knows the outcome"                       
## [2] "At least one project that is assigned and structured by the instructor"
## [3] "A project entirely of student design"                                  
## [4] "Work in small groups"                                                  
## [5] "Read primary scientific literature"                                    
## [6] "Write a research proposal"                                             
## [7] "Present results orally"                                                
## [8] "Present posters"                                                       
## [9] "Discuss reading materials in class"
```

Note that we lost "Figure 4" because it is now incorporated into Figure 2. 
I am instead going to convert Figure 3B (with 9 parts!) into Figure 4.

### Final Figure 4 v 2026

Figure 4 is the combined Cat plots.

-  [8] "Work as a whole class"   
-  [10] "Become responsible for a part of the project"  
-  [13] "Collect data" 

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Figure 4-1.png)<!-- -->



## Instructor Survey Results

We will now examine how the responses to the CURE Instructor Survey might correlate with the student perceptions of gain corresponding to each course element. 
We predict that instructors who placed a greater emphasis on a course element will demonstrate greater perceived gains due to that element.

The five instructors filled out the forms in the summer of 2022. 
Note that it is possible that instructor emphasis shifted slightly in later years, but none of those shifts were dramatic enough to affect these results. 
The forms were PDFs that were sent by email, so I will manually enter the results here.



First to visualize how much variability there is in each question.

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Instructor Emphasis-1.png)<!-- -->

I want to see if certain questions and instructors cluster together to help decide if there are questions that are more interesting to look at than others.

### Final Figure 5A

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 5A Emphasis Heatmap-1.png)<!-- -->

The following six questions have at least three instructors who selected NA/None for the Emphasis:


```
## # A tibble: 6 × 1
##   Question                                                       
##   <chr>                                                          
## 1 A lab or project in which only the instructor knows the outcome
## 2 Listen to lectures                                             
## 3 Read a textbook                                                
## 4 Work on problem sets                                           
## 5 Take tests in class                                            
## 6 Computer modeling
```

It is also interesting to note that the following four questions had Major Emphasis in all five sections:


```
## # A tibble: 4 × 1
##   Question                                       
##   <chr>                                          
## 1 A lab or project where no one knows the outcome
## 2 Read primary scientific literature             
## 3 Collect data                                   
## 4 Analyze data
```

Now to test whether student-perceived gains are correlated with instructor emphasis on each course element.

Looking at all the questions together, including those in the previous list where we did not emphasize them.


```
## tibble [5,433 × 4] (S3: tbl_df/tbl/data.frame)
##  $ Instructor: chr [1:5433] "McGonagall" "McGonagall" "McGonagall" "McGonagall" ...
##  $ Question  : chr [1:5433] "Q10_01" "Q10_02" "Q10_03" "Q10_04" ...
##  $ Gain      : Ord.factor w/ 5 levels "None"<"Little"<..: 4 4 3 4 5 5 3 2 4 5 ...
##  $ Emphasis  : num [1:5433] 1 1 3 0 3 3 1 2 3 3 ...
```

```
##      Instructor        Question           Gain         Emphasis    
##  Length   :5433   Length   :5433   None     : 304   Min.   :0.000  
##  N.unique :   5   N.unique :  25   Little   : 446   1st Qu.:1.000  
##  N.blank  :   0   N.blank  :   0   Some     : 893   Median :2.000  
##  Min.nchar:   5   Min.nchar:   6   Much     :1773   Mean   :1.896  
##  Max.nchar:  10   Max.nchar:   6   Extensive:2017   3rd Qu.:3.000  
##                                                     Max.   :3.000
```

```
## 
## Call:
## glm(formula = as.numeric(Gain) ~ Emphasis, data = Q10_post_merged)
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.99042    0.02630  113.71   <2e-16 ***
## Emphasis     0.46651    0.01178   39.61   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.049255)
## 
##     Null deviance: 7344.9  on 5432  degrees of freedom
## Residual deviance: 5698.5  on 5431  degrees of freedom
## AIC: 15683
## 
## Number of Fisher Scoring iterations: 2
```

```
## Single term deletions
## 
## Model:
## as.numeric(Gain) ~ Emphasis
##          Df Deviance   AIC scaled dev.  Pr(>Chi)    
## <none>        5698.5 15683                          
## Emphasis  1   7344.9 17060      1378.9 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Gain)
## Error estimate based on Pearson residuals 
## 
##           Sum Sq   Df F values    Pr(>F)    
## Emphasis  1646.4    1   1569.1 < 2.2e-16 ***
## Residuals 5698.5 5431                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Self-reported gain in these skills is highly dependent on the emphasis placed by the instructor (p < 2e-16).

Now to explore whether this also depended on the instructor.

### Final Figure 5B


```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Gain)
## Error estimate based on Pearson residuals 
## 
##                     Sum Sq   Df F values    Pr(>F)    
## Emphasis             612.3    1 611.3496 < 2.2e-16 ***
## Instructor            61.9    4  15.4565 1.412e-12 ***
## Emphasis:Instructor    7.9    4   1.9623   0.09743 .  
## Residuals           5431.4 5423                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: as.numeric(Gain)
## Error estimate based on Pearson residuals 
## 
##            Sum Sq   Df F values    Pr(>F)    
## Emphasis   1612.9    1 1609.288 < 2.2e-16 ***
## Instructor  259.2    4   64.662 < 2.2e-16 ***
## Residuals  5439.3 5427                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Using data Q10_post_merged from global environment. This could cause
## incorrect results if Q10_post_merged has been altered since the model was
## fit. You can manually provide the data to the "data =" argument.
```

```
## Warning: Emphasis and Instructor are not included in an interaction with one another
## in the model.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 5B Gain Emphasis Interaction-1.png)<!-- -->

```
## 
## Call:
## glm(formula = Gain ~ Emphasis + Instructor, data = Q10_post_refactored)
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           3.02666    0.03567  84.862  < 2e-16 ***
## Emphasis1             0.79693    0.04651  17.135  < 2e-16 ***
## Emphasis2             1.11544    0.04367  25.540  < 2e-16 ***
## Emphasis3             1.47465    0.03643  40.479  < 2e-16 ***
## InstructorHagrid     -0.57495    0.04227 -13.601  < 2e-16 ***
## InstructorLupin      -0.48829    0.04800 -10.174  < 2e-16 ***
## InstructorMcGonagall -0.16679    0.04301  -3.878 0.000107 ***
## InstructorSinistra    0.03248    0.03586   0.906 0.365155    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9916936)
## 
##     Null deviance: 7344.9  on 5432  degrees of freedom
## Residual deviance: 5379.9  on 5425  degrees of freedom
## AIC: 15383
## 
## Number of Fisher Scoring iterations: 2
```

```
## Analysis of Deviance Table (Type III tests)
## 
## Response: Gain
## Error estimate based on Pearson residuals 
## 
##            Sum Sq   Df F values    Pr(>F)    
## Emphasis   1672.3    3  562.090 < 2.2e-16 ***
## Instructor  297.0    4   74.867 < 2.2e-16 ***
## Residuals  5379.9 5425                       
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
## Warning: Emphasis and Instructor are not included in an interaction with one another
## in the model.
## Emphasis and Instructor are not included in an interaction with one another
## in the model.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/Fig 5B Gain Emphasis Interaction-2.png)<!-- -->

Just to show the interaction:


``` r
interact_plot(model = interact_model, pred = Emphasis, modx = Instructor, 
              interval = TRUE, x.label = "Instructor Emphasis",  
              y.label = "Student Percieved Gain")
```

```
## Using data Q10_post_merged from global environment. This could cause
## incorrect results if Q10_post_merged has been altered since the model was
## fit. You can manually provide the data to the "data =" argument.
```

![](Figures2-5.Improvements-by-Instructor-2.2026_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## Presurvey by Semester

Responses on the presurvey had an effect on the student perceived gain for several questions.
We want to check to see if the presurvey responses were significantly different in the two semesters,
because that could be a confounding factor.


```
## 
## Call:
## glm(formula = as.numeric(Q10_1_pre) ~ FallSpring, data = Q_1Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.29897    0.08399  39.278   <2e-16 ***
## FallSpringSpring  0.22645    0.11337   1.997   0.0471 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6842893)
## 
##     Null deviance: 148.48  on 214  degrees of freedom
## Residual deviance: 145.75  on 213  degrees of freedom
## AIC: 532.57
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_2_pre) ~ FallSpring, data = Q_2Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.15152    0.08484  37.146   <2e-16 ***
## FallSpringSpring  0.21917    0.11551   1.898   0.0591 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7126179)
## 
##     Null deviance: 154.35  on 214  degrees of freedom
## Residual deviance: 151.79  on 213  degrees of freedom
## AIC: 541.29
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_3_pre) ~ FallSpring, data = Q_3Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        2.0792     0.1020  20.376  < 2e-16 ***
## FallSpringSpring   0.4662     0.1382   3.373 0.000878 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.051665)
## 
##     Null deviance: 243.33  on 221  degrees of freedom
## Residual deviance: 231.37  on 220  degrees of freedom
## AIC: 645.18
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_4_pre) ~ FallSpring, data = Q_4Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.62376    0.07891  45.922   <2e-16 ***
## FallSpringSpring  0.17299    0.10649   1.624    0.106    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6289264)
## 
##     Null deviance: 141.28  on 223  degrees of freedom
## Residual deviance: 139.62  on 222  degrees of freedom
## AIC: 535.8
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_5_pre) ~ FallSpring, data = Q_5Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       2.88119    0.09865  29.205   <2e-16 ***
## FallSpringSpring  0.30734    0.13338   2.304   0.0221 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9829782)
## 
##     Null deviance: 222.46  on 222  degrees of freedom
## Residual deviance: 217.24  on 221  degrees of freedom
## AIC: 633.01
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_6_pre) ~ FallSpring, data = Q_6Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        2.5300     0.1203  21.037   <2e-16 ***
## FallSpringSpring   0.2634     0.1625   1.621    0.107    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.446323)
## 
##     Null deviance: 320.54  on 220  degrees of freedom
## Residual deviance: 316.74  on 219  degrees of freedom
## AIC: 712.72
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_7_pre) ~ FallSpring, data = Q_7Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.75248    0.10078  37.236   <2e-16 ***
## FallSpringSpring  0.04261    0.13625   0.313    0.755    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.025742)
## 
##     Null deviance: 226.79  on 222  degrees of freedom
## Residual deviance: 226.69  on 221  degrees of freedom
## AIC: 642.51
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_8_pre) ~ FallSpring, data = Q_8Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        3.5545     0.1055  33.692   <2e-16 ***
## FallSpringSpring  -0.2780     0.1424  -1.953   0.0521 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.124109)
## 
##     Null deviance: 253.84  on 223  degrees of freedom
## Residual deviance: 249.55  on 222  degrees of freedom
## AIC: 665.88
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_9_pre) ~ FallSpring, data = Q_9Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       4.03960    0.07204  56.072   <2e-16 ***
## FallSpringSpring  0.05056    0.09740   0.519    0.604    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.5242072)
## 
##     Null deviance: 115.99  on 222  degrees of freedom
## Residual deviance: 115.85  on 221  degrees of freedom
## AIC: 492.81
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_10_pre) ~ FallSpring, data = Q_10Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.82178    0.07838  48.762   <2e-16 ***
## FallSpringSpring  0.22780    0.10616   2.146    0.033 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.6204298)
## 
##     Null deviance: 139.35  on 221  degrees of freedom
## Residual deviance: 136.49  on 220  degrees of freedom
## AIC: 528.03
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_11_pre) ~ FallSpring, data = Q_11Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       2.89000    0.09173  31.506  < 2e-16 ***
## FallSpringSpring  0.70836    0.12374   5.725 3.37e-08 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8414076)
## 
##     Null deviance: 212.68  on 221  degrees of freedom
## Residual deviance: 185.11  on 220  degrees of freedom
## AIC: 595.66
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_12_pre) ~ FallSpring, data = Q_12Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        2.4100     0.1128  21.367   <2e-16 ***
## FallSpringSpring   0.3751     0.1524   2.461   0.0146 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.272161)
## 
##     Null deviance: 286.31  on 220  degrees of freedom
## Residual deviance: 278.60  on 219  degrees of freedom
## AIC: 684.36
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_13_pre) ~ FallSpring, data = Q_13Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.73267    0.08774  42.541   <2e-16 ***
## FallSpringSpring -0.03019    0.11885  -0.254      0.8    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7775974)
## 
##     Null deviance: 171.12  on 221  degrees of freedom
## Residual deviance: 171.07  on 220  degrees of freedom
## AIC: 578.16
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_14_pre) ~ FallSpring, data = Q_14Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.71287    0.08724  42.560   <2e-16 ***
## FallSpringSpring -0.03519    0.11816  -0.298    0.766    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7686501)
## 
##     Null deviance: 169.17  on 221  degrees of freedom
## Residual deviance: 169.10  on 220  degrees of freedom
## AIC: 575.59
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_15_pre) ~ FallSpring, data = Q_15Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        3.1600     0.1070  29.533   <2e-16 ***
## FallSpringSpring   0.1375     0.1446   0.951    0.343    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.144882)
## 
##     Null deviance: 251.76  on 220  degrees of freedom
## Residual deviance: 250.73  on 219  degrees of freedom
## AIC: 661.06
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_16_pre) ~ FallSpring, data = Q_16Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.32000    0.09988  33.241   <2e-16 ***
## FallSpringSpring -0.13301    0.13448  -0.989    0.324    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9975529)
## 
##     Null deviance: 221.43  on 222  degrees of freedom
## Residual deviance: 220.46  on 221  degrees of freedom
## AIC: 636.29
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_17_pre) ~ FallSpring, data = Q_17Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        3.3069     0.1048  31.542   <2e-16 ***
## FallSpringSpring  -0.2741     0.1417  -1.934   0.0544 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.110199)
## 
##     Null deviance: 249.51  on 222  degrees of freedom
## Residual deviance: 245.35  on 221  degrees of freedom
## AIC: 660.15
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_18_pre) ~ FallSpring, data = Q_18Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        2.6263     0.1028  25.542  < 2e-16 ***
## FallSpringSpring   0.5295     0.1384   3.826  0.00017 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.046633)
## 
##     Null deviance: 244.53  on 220  degrees of freedom
## Residual deviance: 229.21  on 219  degrees of freedom
## AIC: 641.23
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_19_pre) ~ FallSpring, data = Q_19Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        3.6837     0.1065  34.583   <2e-16 ***
## FallSpringSpring   0.3499     0.1438   2.433   0.0158 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.111904)
## 
##     Null deviance: 245.64  on 216  degrees of freedom
## Residual deviance: 239.06  on 215  degrees of freedom
## AIC: 642.83
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_20_pre) ~ FallSpring, data = Q_20Clean_Semester)
## 
## Coefficients:
##                   Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.835165   0.116069  33.042   <2e-16 ***
## FallSpringSpring -0.009922   0.159293  -0.062     0.95    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.225947)
## 
##     Null deviance: 235.39  on 193  degrees of freedom
## Residual deviance: 235.38  on 192  degrees of freedom
## AIC: 594.06
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_21_pre) ~ FallSpring, data = Q_21Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.87500    0.09964   38.89   <2e-16 ***
## FallSpringSpring -0.01641    0.13695   -0.12    0.905    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.8737578)
## 
##     Null deviance: 161.66  on 186  degrees of freedom
## Residual deviance: 161.65  on 185  degrees of freedom
## AIC: 509.44
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_22_pre) ~ FallSpring, data = Q_22Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        4.1236     0.1052  39.186   <2e-16 ***
## FallSpringSpring  -0.1830     0.1443  -1.268    0.206    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9855533)
## 
##     Null deviance: 186.87  on 189  degrees of freedom
## Residual deviance: 185.28  on 188  degrees of freedom
## AIC: 540.42
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_23_pre) ~ FallSpring, data = Q_23Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.95050    0.08739  45.208   <2e-16 ***
## FallSpringSpring  0.02471    0.11837   0.209    0.835    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7712641)
## 
##     Null deviance: 169.71  on 221  degrees of freedom
## Residual deviance: 169.68  on 220  degrees of freedom
## AIC: 576.34
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_24_pre) ~ FallSpring, data = Q_24Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       3.32673    0.11569  28.755   <2e-16 ***
## FallSpringSpring -0.02501    0.15824  -0.158    0.875    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.351895)
## 
##     Null deviance: 290.69  on 216  degrees of freedom
## Residual deviance: 290.66  on 215  degrees of freedom
## AIC: 685.24
## 
## Number of Fisher Scoring iterations: 2
```


```
## 
## Call:
## glm(formula = as.numeric(Q10_25_pre) ~ FallSpring, data = Q_25Clean_Semester)
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       2.15054    0.11653  18.455   <2e-16 ***
## FallSpringSpring  0.04415    0.15733   0.281    0.779    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.262791)
## 
##     Null deviance: 257.71  on 205  degrees of freedom
## Residual deviance: 257.61  on 204  degrees of freedom
## AIC: 636.66
## 
## Number of Fisher Scoring iterations: 2
```


``` r
Q10TextClean$Question[3] # p = 0.000878, Estimate = 0.4662
```

```
## [1] "A lab or project where no one knows the outcome"
```

``` r
Q10TextClean$Question[4] # p = 0.106, Estimate = 0.17299
```

```
## [1] "At least one project that is assigned and structured by the instructor"
```

``` r
Q10TextClean$Question[6] # p = 0.107, Estimate = 0.2634
```

```
## [1] "A project entirely of student design"
```

``` r
Q10TextClean$Question[11] # p = 3.37e-08, Estimate = 0.70836
```

```
## [1] "Read primary scientific literature"
```

``` r
Q10TextClean$Question[12] # p = 0.0146, Estimate = 0.3751
```

```
## [1] "Write a research proposal"
```

``` r
Q10TextClean$Question[18] # p = 0.00017, Estimate = 0.5295
```

```
## [1] "Critique the work of other students"
```

``` r
Q10TextClean$Question[19] # p = 0.0158, Estimate =  0.3499
```

```
## [1] "Listen to lectures"
```

After discussion with Moria, we would like to show, as a supplemental table the questions that have p.adjust < 0.05.

## Table S4


``` r
stats_table <- tibble(Question = Q10TextClean$Question[11], 
                      Estimate = Q_11_pre_model$coefficients[[2]],
                      p = coef(summary(Q_11_pre_model))[,4][[2]],
                      `p.adj` = p.adjust(coef(summary(Q_11_pre_model))[,4][[2]], n = 25))
stats_table <- stats_table %>%
  add_row(Question = Q10TextClean$Question[18], 
          Estimate = Q_18_pre_model$coefficients[[2]], 
          p = coef(summary(Q_18_pre_model))[,4][[2]], 
          `p.adj` = p.adjust(coef(summary(Q_18_pre_model))[,4][[2]], n = 25))
stats_table <- stats_table %>%
  add_row(Question = Q10TextClean$Question[3], 
          Estimate = Q_3_pre_model$coefficients[[2]], 
          p = coef(summary(Q_3_pre_model))[,4][[2]], 
          `p.adj` = p.adjust(coef(summary(Q_3_pre_model))[,4][[2]], n = 25))

kable(stats_table, "simple")
```



Question                                            Estimate           p       p.adj
------------------------------------------------  ----------  ----------  ----------
Read primary scientific literature                 0.7083607   0.0000000   0.0000008
Critique the work of other students                0.5294751   0.0001699   0.0042470
A lab or project where no one knows the outcome    0.4662466   0.0008776   0.0219398


