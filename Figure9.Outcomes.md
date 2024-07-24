---
title: "Outcomes"
author: "Ken Field"
date: "Last compiled on 2024-07-24"
output:
  html_document:
    toc: true
    keep_md: yes
  pdf_document: default
---

IMPORTANT NOTE

This Rmd uses the deidentified results and is safe to share.



We wish to determine if the new core classes (CURE Lab and BIO Seminar) are helping students to succeed academically.

## Loading Results

I asked the registrar to help me answer the following questions:
1. What was the DFW rate for first-year students in BIOL 205 and 206 from SP 2017 to FA 2019?
2. What was the DFW rate for all students in BIOL 201 and 202 from FA 2021 to SP 2024?

They sent us the files located in the "Grade Outcomes" folder.
I have removed any identifying information for the students and instructors.

Note that the analysis 205 and 206 only includes first-year students in these Biology classes, even though there are other students in these classes.
This is to make it more comparable to 201 and 202 which are not taken by nearly as broad a spectrum of students.

Importing data for 205 and 206.


```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `across(Grade, str_replace, "D|F|W", "DFW")`.
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

```
## [1] "Semester"                   "Course"                    
## [3] "Gender"                     "First-Generation Indicator"
## [5] "Grade"                      "Race/Ethnicity"            
## [7] "Curriculum"
```

```
## BIOL205 BIOL206 
##     470     390
```

```
##  DFW Pass 
##  104  756
```

```
##   Fall Term 2017-2018   Fall Term 2018-2019   Fall Term 2019-2020 
##                   149                   159                   162 
## Spring Term 2016-2017 Spring Term 2017-2018 Spring Term 2018-2019 
##                   119                   123                   148
```

```
## Female   Male 
##    633    227
```

```
## nonWhite    White 
##      254      606
```

Importing data from 201 and 202.
The spreadsheets have a very awkward format and were checked carefully. 
Before I imported the data, I consolidated it within excel to make the three years have a consistent format.



Converting Totals into Passes by subtracting each D, F, or W.


```
##  [1] "Course"                "Semester"              "D_Male_White"         
##  [4] "F_Female_White"        "W_Female_White"        "W_Male_White"         
##  [7] "Female_Total_White"    "Male_Total_White"      "D_Male_nonWhite"      
## [10] "F_Female_nonWhite"     "W_Female_nonWhite"     "W_Male_nonWhite"      
## [13] "Female_Total_nonWhite" "Male_Total_nonWhite"
```

```
##  [1] "Course"                "Semester"              "D_Female_White"       
##  [4] "F_Female_White"        "W_Female_White"        "W_Male_White"         
##  [7] "Female_Total_White"    "Male_Total_White"      "D_Female_nonWhite"    
## [10] "F_Female_nonWhite"     "W_Female_nonWhite"     "W_Male_nonWhite"      
## [13] "Female_Total_nonWhite" "Male_Total_nonWhite"
```

```
##  [1] "Course"                "Semester"              "D_Female_White"       
##  [4] "D_Male_White"          "F_Female_White"        "F_Male_White"         
##  [7] "W_Female_White"        "Female_Total_White"    "Male_Total_White"     
## [10] "D_Female_nonWhite"     "D_Male_nonWhite"       "F_Female_nonWhite"    
## [13] "F_Male_nonWhite"       "W_Female_nonWhite"     "Female_Total_nonWhite"
## [16] "Male_Total_nonWhite"
```

Now pivoting the table and splitting the categories


``` r
names(DFW_21_22)
```

```
##  [1] "Course"               "Semester"             "D_Male_White"        
##  [4] "F_Female_White"       "W_Female_White"       "W_Male_White"        
##  [7] "D_Male_nonWhite"      "F_Female_nonWhite"    "W_Female_nonWhite"   
## [10] "W_Male_nonWhite"      "Pass_Female_White"    "Pass_Male_White"     
## [13] "Pass_Female_nonWhite" "Pass_Male_nonWhite"
```

``` r
DFW_21_22_long <- DFW_21_22 %>%
  pivot_longer(cols = D_Male_White:Pass_Male_nonWhite, 
               names_to = "Group", values_to = "Count") %>%
  separate_wider_delim(Group, delim = "_", names = c("Grade", "Gender", "Race/Ethnicity")) %>%
  mutate(across(Grade, str_replace, 'D|F|W', 'DFW')) %>%
  uncount(weights = Count) 
summary(as.factor(DFW_21_22_long$Grade))
```

```
##  DFW Pass 
##    9  320
```

``` r
names(DFW_22_23)
```

```
##  [1] "Course"               "Semester"             "D_Female_White"      
##  [4] "F_Female_White"       "W_Female_White"       "W_Male_White"        
##  [7] "D_Female_nonWhite"    "F_Female_nonWhite"    "W_Female_nonWhite"   
## [10] "W_Male_nonWhite"      "Pass_Female_White"    "Pass_Male_White"     
## [13] "Pass_Female_nonWhite" "Pass_Male_nonWhite"
```

``` r
DFW_22_23_long <- DFW_22_23 %>%
  pivot_longer(cols = D_Female_White:Pass_Male_nonWhite, 
               names_to = "Group", values_to = "Count") %>%
  separate_wider_delim(Group, delim = "_", names = c("Grade", "Gender", "Race/Ethnicity")) %>%
  mutate(across(Grade, str_replace, 'D|F|W', 'DFW')) %>%
  uncount(weights = Count) 
summary(as.factor(DFW_22_23_long$Grade))
```

```
##  DFW Pass 
##   10  272
```

``` r
names(DFW_23_24)
```

```
##  [1] "Course"               "Semester"             "D_Female_White"      
##  [4] "D_Male_White"         "F_Female_White"       "F_Male_White"        
##  [7] "W_Female_White"       "D_Female_nonWhite"    "D_Male_nonWhite"     
## [10] "F_Female_nonWhite"    "F_Male_nonWhite"      "W_Female_nonWhite"   
## [13] "Pass_Female_White"    "Pass_Male_White"      "Pass_Female_nonWhite"
## [16] "Pass_Male_nonWhite"
```

``` r
DFW_23_24_long <- DFW_23_24 %>%
  pivot_longer(cols = D_Female_White:Pass_Male_nonWhite, 
               names_to = "Group", values_to = "Count") %>%
  separate_wider_delim(Group, delim = "_", names = c("Grade", "Gender", "Race/Ethnicity")) %>%
  mutate(across(Grade, str_replace, 'D|F|W', 'DFW')) %>%
  uncount(weights = Count) 
summary(as.factor(DFW_23_24_long$Grade))
```

```
##  DFW Pass 
##   10  258
```

``` r
DFW_201_202 <- DFW_21_22_long %>%
  add_row(DFW_22_23_long) %>%
  add_row(DFW_23_24_long) %>%
  mutate(Curriculum = "New")
```

Combining the two datasets


``` r
All_DFW <- BIOL205_206 %>%
  select(names(DFW_201_202)) %>%
  add_row(DFW_201_202) %>%
  mutate_if(is.character, as.factor) %>%
  mutate(Curriculum = fct_relevel(Curriculum, c("Old", "New"))) %>%
  mutate(`Race/Ethnicity` = fct_relevel(`Race/Ethnicity`, c("White", "nonWhite")))

Only202_DFW <- All_DFW %>%
  filter(Course != "BIOL201")

summary(All_DFW)
```

```
##      Course                   Semester    Grade         Gender    
##  BIOL201:569   Spring 2022        :180   DFW : 133   Female:1251  
##  BIOL202:310   Fall Term 2019-2020:162   Pass:1606   Male  : 488  
##  BIOL205:470   Fall Term 2018-2019:159                            
##  BIOL206:390   Fall 2021          :149                            
##                Fall 2022          :149                            
##                Fall Term 2017-2018:149                            
##                (Other)            :791                            
##   Race/Ethnicity Curriculum
##  White   :1202   Old:860   
##  nonWhite: 537   New:879   
##                            
##                            
##                            
##                            
## 
```

``` r
summary(Only202_DFW)
```

```
##      Course                     Semester    Grade         Gender   
##  BIOL201:  0   Fall Term 2019-2020  :162   DFW : 108   Female:850  
##  BIOL202:310   Fall Term 2018-2019  :159   Pass:1062   Male  :320  
##  BIOL205:470   Fall Term 2017-2018  :149                           
##  BIOL206:390   Spring Term 2018-2019:148                           
##                Spring Term 2017-2018:123                           
##                Spring Term 2016-2017:119                           
##                (Other)              :310                           
##   Race/Ethnicity Curriculum
##  White   :832    Old:860   
##  nonWhite:338    New:310   
##                            
##                            
##                            
##                            
## 
```

``` r
All_DFW %>%
  group_by(Curriculum, Grade) %>%
  summarise(n = n())
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW      29
## 4 New        Pass    850
```

``` r
Only202_DFW %>%
  group_by(Curriculum, Grade) %>%
  summarise(n = n())
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW       4
## 4 New        Pass    306
```

``` r
Only202_DFW %>%
  group_by(Course, Grade) %>%
  summarise(n = n())
```

```
## `summarise()` has grouped output by 'Course'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 6 × 3
## # Groups:   Course [3]
##   Course  Grade     n
##   <fct>   <fct> <int>
## 1 BIOL202 DFW       4
## 2 BIOL202 Pass    306
## 3 BIOL205 DFW      77
## 4 BIOL205 Pass    393
## 5 BIOL206 DFW      27
## 6 BIOL206 Pass    363
```

## Modeling results with negative binomial glm

### Both 201 and 202


```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum, data = All_DFW, init.theta = 1340.369534, 
##     link = log)
## 
## Coefficients:
##               Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -2.11254    0.09806 -21.543  < 2e-16 ***
## CurriculumNew -1.29895    0.21000  -6.185 6.19e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1340.37) family taken to be 1)
## 
##     Null deviance: 683.72  on 1738  degrees of freedom
## Residual deviance: 637.19  on 1737  degrees of freedom
## AIC: 909.29
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1340 
##           Std. Err.:  7261 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -903.285
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum * Gender * `Race/Ethnicity`, 
##     data = All_DFW, init.theta = 1122.190149, link = log)
## 
## Coefficients:
##                                                   Estimate Std. Error z value
## (Intercept)                                       -2.29604    0.14745 -15.572
## CurriculumNew                                     -1.80068    0.40571  -4.438
## GenderMale                                         0.06468    0.29025   0.223
## `Race/Ethnicity`nonWhite                           0.38365    0.24537   1.564
## CurriculumNew:GenderMale                           0.47669    0.65354   0.729
## CurriculumNew:`Race/Ethnicity`nonWhite             0.91477    0.53517   1.709
## GenderMale:`Race/Ethnicity`nonWhite                0.26358    0.43038   0.612
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite -0.85157    0.90080  -0.945
##                                                   Pr(>|z|)    
## (Intercept)                                        < 2e-16 ***
## CurriculumNew                                     9.07e-06 ***
## GenderMale                                          0.8236    
## `Race/Ethnicity`nonWhite                            0.1179    
## CurriculumNew:GenderMale                            0.4658    
## CurriculumNew:`Race/Ethnicity`nonWhite              0.0874 .  
## GenderMale:`Race/Ethnicity`nonWhite                 0.5402    
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite   0.3445    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1122.19) family taken to be 1)
## 
##     Null deviance: 683.70  on 1738  degrees of freedom
## Residual deviance: 621.19  on 1731  degrees of freedom
## AIC: 905.3
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1122 
##           Std. Err.:  5328 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -887.304
```


```
## Start:  AIC=903.3
## DFW ~ Curriculum * Gender * `Race/Ethnicity`
## 
##                                      Df   AIC
## - Curriculum:Gender:`Race/Ethnicity`  1 902.2
## <none>                                  903.3
## 
## Step:  AIC=902.2
## DFW ~ Curriculum + Gender + `Race/Ethnicity` + Curriculum:Gender + 
##     Curriculum:`Race/Ethnicity` + Gender:`Race/Ethnicity`
## 
##                               Df    AIC
## - Curriculum:Gender            1 900.20
## - Gender:`Race/Ethnicity`      1 900.23
## <none>                           902.20
## - Curriculum:`Race/Ethnicity`  1 902.34
## 
## Step:  AIC=900.2
## DFW ~ Curriculum + Gender + `Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity`
## 
##                               Df    AIC
## - Gender:`Race/Ethnicity`      1 898.24
## <none>                           900.20
## - Curriculum:`Race/Ethnicity`  1 900.35
## 
## Step:  AIC=898.24
## DFW ~ Curriculum + Gender + `Race/Ethnicity` + Curriculum:`Race/Ethnicity`
## 
##                               Df    AIC
## - Gender                       1 897.22
## <none>                           898.24
## - Curriculum:`Race/Ethnicity`  1 898.40
## 
## Step:  AIC=897.22
## DFW ~ Curriculum + `Race/Ethnicity` + Curriculum:`Race/Ethnicity`
## 
##                               Df    AIC
## <none>                           897.22
## - Curriculum:`Race/Ethnicity`  1 897.31
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum + `Race/Ethnicity` + Curriculum:`Race/Ethnicity`, 
##     data = All_DFW, init.theta = 1155.721611, link = log)
## 
## Coefficients:
##                                        Estimate Std. Error z value Pr(>|z|)    
## (Intercept)                             -2.2797     0.1270 -17.950  < 2e-16 ***
## CurriculumNew                           -1.6256     0.3154  -5.154 2.54e-07 ***
## `Race/Ethnicity`nonWhite                 0.4801     0.1999   2.402   0.0163 *  
## CurriculumNew:`Race/Ethnicity`nonWhite   0.6130     0.4267   1.437   0.1509    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1155.722) family taken to be 1)
## 
##     Null deviance: 683.7  on 1738  degrees of freedom
## Residual deviance: 623.1  on 1735  degrees of freedom
## AIC: 899.22
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1156 
##           Std. Err.:  5607 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -889.217
```

A very big effect of the new curriculum and a small effect of the interaction between curriculum and race/ethnicity.


```
##          1          2          3          4 
## 0.10231023 0.02013423 0.16535433 0.06007067
```

```
## 
## Call:  glm.nb(formula = DFW ~ Curriculum + `Race/Ethnicity` + Curriculum:`Race/Ethnicity`, 
##     data = All_DFW, init.theta = 1155.721611, link = log)
## 
## Coefficients:
##                            (Intercept)                           CurriculumNew  
##                                -2.2797                                 -1.6256  
##               `Race/Ethnicity`nonWhite  CurriculumNew:`Race/Ethnicity`nonWhite  
##                                 0.4801                                  0.6130  
## 
## Degrees of Freedom: 1738 Total (i.e. Null);  1735 Residual
## Null Deviance:	    683.7 
## Residual Deviance: 623.1 	AIC: 899.2
```

```
## # Check for zero-inflation
## 
##    Observed zeros: 1606
##   Predicted zeros: 1614
##             Ratio: 1.01
```

```
## Model seems ok, ratio of observed and predicted zeros is within the
##   tolerance range (p = 0.512).
```

```
## # Overdispersion test
## 
##  dispersion ratio = 0.902
##           p-value = 0.312
```

```
## No overdispersion detected.
```

```
## # Indices of model performance
## 
## AIC     |    AICc |     BIC | Nagelkerke's R2 |  RMSE | Sigma | Score_log | Score_spherical
## -------------------------------------------------------------------------------------------
## 899.217 | 899.251 | 926.522 |           0.105 | 0.261 | 1.000 |    -0.257 |           0.023
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum + `Race/Ethnicity` + Curriculum:`Race/Ethnicity`, 
##     data = All_DFW, init.theta = 1155.721611, link = log)
## 
## Coefficients:
##                                        Estimate Std. Error z value Pr(>|z|)    
## (Intercept)                             -2.2797     0.1270 -17.950  < 2e-16 ***
## CurriculumNew                           -1.6256     0.3154  -5.154 2.54e-07 ***
## `Race/Ethnicity`nonWhite                 0.4801     0.1999   2.402   0.0163 *  
## CurriculumNew:`Race/Ethnicity`nonWhite   0.6130     0.4267   1.437   0.1509    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1155.722) family taken to be 1)
## 
##     Null deviance: 683.7  on 1738  degrees of freedom
## Residual deviance: 623.1  on 1735  degrees of freedom
## AIC: 899.22
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1156 
##           Std. Err.:  5607 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -889.217
```


```
## [1] 0.1023149
```

```
## [1] 0.140254
```

```
## [1] 0.07463841
```



```
## [1] 0.1967936
```

```
## [1] 0.2234428
```

```
## [1] 0.1733227
```

```
## [1] 5.081467
```

```
## [1] 4.475419
```

```
## [1] 5.769584
```

The model indicates that students under the old curriculum were 5.1-fold (4.4 - 5.8) more likely to earn a DFW in the first two Biology courses (p = 2.54e-07).

![](Figure9.Outcomes_files/figure-html/Combo 201 and 202-1.png)<!-- -->

### Only 202

Adding Fall vs Spring to the full model.


```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum, data = Only202_DFW, init.theta = 1072.208091, 
##     link = log)
## 
## Coefficients:
##               Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -2.11254    0.09806 -21.543  < 2e-16 ***
## CurriculumNew -2.23774    0.50953  -4.392 1.12e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1072.208) family taken to be 1)
## 
##     Null deviance: 514.56  on 1169  degrees of freedom
## Residual deviance: 474.12  on 1168  degrees of freedom
## AIC: 696.22
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1072 
##           Std. Err.:  5378 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -690.223
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`, 
##     data = Only202_DFW, init.theta = 2031.355587, link = log)
## 
## Coefficients:
##                                                                       Estimate
## (Intercept)                                                         -1.926e+00
## Fall_SpringSpring                                                   -1.190e+00
## CurriculumNew                                                       -2.005e+00
## GenderMale                                                           1.347e-01
## `Race/Ethnicity`nonWhite                                             3.059e-01
## Fall_SpringSpring:CurriculumNew                                     -3.196e+01
## Fall_SpringSpring:GenderMale                                        -1.827e-01
## CurriculumNew:GenderMale                                            -3.325e+01
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           4.468e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               4.070e-01
## GenderMale:`Race/Ethnicity`nonWhite                                 -1.671e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           3.334e+01
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             3.237e+01
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                1.090e+00
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    3.411e+01
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite -6.855e+01
##                                                                     Std. Error
## (Intercept)                                                          1.644e-01
## Fall_SpringSpring                                                    3.717e-01
## CurriculumNew                                                        1.013e+00
## GenderMale                                                           3.224e-01
## `Race/Ethnicity`nonWhite                                             2.874e-01
## Fall_SpringSpring:CurriculumNew                                      6.518e+06
## Fall_SpringSpring:GenderMale                                         7.405e-01
## CurriculumNew:GenderMale                                             1.205e+07
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           5.645e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               1.443e+00
## GenderMale:`Race/Ethnicity`nonWhite                                  5.202e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.750e+07
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             6.518e+06
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                9.915e-01
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    1.205e+07
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  2.555e+07
##                                                                     z value
## (Intercept)                                                         -11.718
## Fall_SpringSpring                                                    -3.201
## CurriculumNew                                                        -1.979
## GenderMale                                                            0.418
## `Race/Ethnicity`nonWhite                                              1.065
## Fall_SpringSpring:CurriculumNew                                       0.000
## Fall_SpringSpring:GenderMale                                         -0.247
## CurriculumNew:GenderMale                                              0.000
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                            0.792
## CurriculumNew:`Race/Ethnicity`nonWhite                                0.282
## GenderMale:`Race/Ethnicity`nonWhite                                  -0.321
## Fall_SpringSpring:CurriculumNew:GenderMale                            0.000
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite              0.000
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                 1.100
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                     0.000
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite   0.000
##                                                                     Pr(>|z|)
## (Intercept)                                                          < 2e-16
## Fall_SpringSpring                                                    0.00137
## CurriculumNew                                                        0.04783
## GenderMale                                                           0.67621
## `Race/Ethnicity`nonWhite                                             0.28709
## Fall_SpringSpring:CurriculumNew                                      1.00000
## Fall_SpringSpring:GenderMale                                         0.80509
## CurriculumNew:GenderMale                                             1.00000
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           0.42865
## CurriculumNew:`Race/Ethnicity`nonWhite                               0.77791
## GenderMale:`Race/Ethnicity`nonWhite                                  0.74807
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.00000
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             1.00000
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                0.27146
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    1.00000
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  1.00000
##                                                                        
## (Intercept)                                                         ***
## Fall_SpringSpring                                                   ** 
## CurriculumNew                                                       *  
## GenderMale                                                             
## `Race/Ethnicity`nonWhite                                               
## Fall_SpringSpring:CurriculumNew                                        
## Fall_SpringSpring:GenderMale                                           
## CurriculumNew:GenderMale                                               
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                             
## CurriculumNew:`Race/Ethnicity`nonWhite                                 
## GenderMale:`Race/Ethnicity`nonWhite                                    
## Fall_SpringSpring:CurriculumNew:GenderMale                             
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite               
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                  
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(2031.355) family taken to be 1)
## 
##     Null deviance: 514.60  on 1169  degrees of freedom
## Residual deviance: 437.11  on 1154  degrees of freedom
## AIC: 687.16
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  2031 
##           Std. Err.:  12507 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -653.163
```

Fall_Spring is playing a bigger role than Curriculum. 
We need to account for that.


```
## Start:  AIC=685.16
## DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`
## 
##                                                  Df    AIC
## - Fall_Spring:Curriculum:Gender:`Race/Ethnicity`  1 683.16
## <none>                                              685.16
## 
## Step:  AIC=683.16
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Curriculum + Fall_Spring:Gender + Curriculum:Gender + 
##     Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity` + Fall_Spring:Curriculum:Gender + 
##     Fall_Spring:Curriculum:`Race/Ethnicity` + Fall_Spring:Gender:`Race/Ethnicity` + 
##     Curriculum:Gender:`Race/Ethnicity`
## 
##                                           Df    AIC
## - Fall_Spring:Curriculum:`Race/Ethnicity`  1 682.14
## - Fall_Spring:Gender:`Race/Ethnicity`      1 682.41
## - Curriculum:Gender:`Race/Ethnicity`       1 682.53
## - Fall_Spring:Curriculum:Gender            1 682.70
## <none>                                       683.16
## 
## Step:  AIC=682.14
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Curriculum + Fall_Spring:Gender + Curriculum:Gender + 
##     Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity` + Fall_Spring:Curriculum:Gender + 
##     Fall_Spring:Gender:`Race/Ethnicity` + Curriculum:Gender:`Race/Ethnicity`
## 
##                                       Df    AIC
## - Curriculum:Gender:`Race/Ethnicity`   1 681.10
## - Fall_Spring:Gender:`Race/Ethnicity`  1 681.19
## - Fall_Spring:Curriculum:Gender        1 681.23
## <none>                                   682.14
## 
## Step:  AIC=681.1
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Curriculum + Fall_Spring:Gender + Curriculum:Gender + 
##     Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity` + Fall_Spring:Curriculum:Gender + 
##     Fall_Spring:Gender:`Race/Ethnicity`
## 
##                                       Df    AIC
## - Fall_Spring:Curriculum:Gender        1 679.97
## - Fall_Spring:Gender:`Race/Ethnicity`  1 680.03
## <none>                                   681.10
## - Curriculum:`Race/Ethnicity`          1 681.33
## 
## Step:  AIC=679.97
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Curriculum + Fall_Spring:Gender + Curriculum:Gender + 
##     Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity` + Fall_Spring:Gender:`Race/Ethnicity`
## 
##                                       Df    AIC
## - Curriculum:Gender                    1 678.19
## - Fall_Spring:Curriculum               1 678.51
## - Fall_Spring:Gender:`Race/Ethnicity`  1 678.77
## <none>                                   679.97
## - Curriculum:`Race/Ethnicity`          1 679.99
## 
## Step:  AIC=678.19
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Curriculum + Fall_Spring:Gender + Fall_Spring:`Race/Ethnicity` + 
##     Curriculum:`Race/Ethnicity` + Gender:`Race/Ethnicity` + Fall_Spring:Gender:`Race/Ethnicity`
## 
##                                       Df    AIC
## - Fall_Spring:Curriculum               1 676.77
## - Fall_Spring:Gender:`Race/Ethnicity`  1 677.00
## <none>                                   678.19
## - Curriculum:`Race/Ethnicity`          1 678.23
## 
## Step:  AIC=676.77
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Gender + Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity` + Fall_Spring:Gender:`Race/Ethnicity`
## 
##                                       Df    AIC
## - Fall_Spring:Gender:`Race/Ethnicity`  1 675.59
## - Curriculum:`Race/Ethnicity`          1 676.56
## <none>                                   676.77
## 
## Step:  AIC=675.59
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Gender + Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity` + 
##     Gender:`Race/Ethnicity`
## 
##                                Df    AIC
## - Gender:`Race/Ethnicity`       1 673.76
## - Fall_Spring:Gender            1 674.10
## - Curriculum:`Race/Ethnicity`   1 675.37
## <none>                            675.59
## - Fall_Spring:`Race/Ethnicity`  1 676.90
## 
## Step:  AIC=673.76
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:Gender + Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity`
## 
##                                Df    AIC
## - Fall_Spring:Gender            1 672.37
## - Curriculum:`Race/Ethnicity`   1 673.54
## <none>                            673.76
## - Fall_Spring:`Race/Ethnicity`  1 675.14
## 
## Step:  AIC=672.37
## DFW ~ Fall_Spring + Curriculum + Gender + `Race/Ethnicity` + 
##     Fall_Spring:`Race/Ethnicity` + Curriculum:`Race/Ethnicity`
## 
##                                Df    AIC
## - Gender                        1 671.00
## - Curriculum:`Race/Ethnicity`   1 672.19
## <none>                            672.37
## - Fall_Spring:`Race/Ethnicity`  1 673.87
## 
## Step:  AIC=671
## DFW ~ Fall_Spring + Curriculum + `Race/Ethnicity` + Fall_Spring:`Race/Ethnicity` + 
##     Curriculum:`Race/Ethnicity`
## 
##                                Df    AIC
## - Curriculum:`Race/Ethnicity`   1 670.77
## <none>                            671.00
## - Fall_Spring:`Race/Ethnicity`  1 672.38
## 
## Step:  AIC=670.77
## DFW ~ Fall_Spring + Curriculum + `Race/Ethnicity` + Fall_Spring:`Race/Ethnicity`
## 
##                                Df    AIC
## <none>                            670.77
## - Fall_Spring:`Race/Ethnicity`  1 672.43
## - Curriculum                    1 701.21
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring + Curriculum + `Race/Ethnicity` + 
##     Fall_Spring:`Race/Ethnicity`, data = Only202_DFW, init.theta = 1566.704185, 
##     link = log)
## 
## Coefficients:
##                                            Estimate Std. Error z value Pr(>|z|)
## (Intercept)                                 -1.9040     0.1409 -13.515  < 2e-16
## Fall_SpringSpring                           -1.2884     0.3213  -4.010 6.06e-05
## CurriculumNew                               -2.0715     0.5104  -4.059 4.94e-05
## `Race/Ethnicity`nonWhite                     0.3117     0.2326   1.340   0.1803
## Fall_SpringSpring:`Race/Ethnicity`nonWhite   0.8489     0.4472   1.898   0.0577
##                                               
## (Intercept)                                ***
## Fall_SpringSpring                          ***
## CurriculumNew                              ***
## `Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:`Race/Ethnicity`nonWhite .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1566.704) family taken to be 1)
## 
##     Null deviance: 514.59  on 1169  degrees of freedom
## Residual deviance: 444.70  on 1165  degrees of freedom
## AIC: 672.77
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1567 
##           Std. Err.:  8611 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -660.773
```



```
## 
## Call:  glm.nb(formula = DFW ~ Fall_Spring + Curriculum + `Race/Ethnicity` + 
##     Fall_Spring:`Race/Ethnicity`, data = Only202_DFW, init.theta = 1566.704185, 
##     link = log)
## 
## Coefficients:
##                                (Intercept)  
##                                    -1.9040  
##                          Fall_SpringSpring  
##                                    -1.2884  
##                              CurriculumNew  
##                                    -2.0715  
##                   `Race/Ethnicity`nonWhite  
##                                     0.3117  
## Fall_SpringSpring:`Race/Ethnicity`nonWhite  
##                                     0.8489  
## 
## Degrees of Freedom: 1169 Total (i.e. Null);  1165 Residual
## Null Deviance:	    514.6 
## Residual Deviance: 444.7 	AIC: 672.8
```

```
## # Check for zero-inflation
## 
##    Observed zeros: 1062
##   Predicted zeros: 1070
##             Ratio: 1.01
```

```
## Model seems ok, ratio of observed and predicted zeros is within the
##   tolerance range (p = 0.384).
```

```
## # Overdispersion test
## 
##  dispersion ratio = 0.868
##           p-value = 0.216
```

```
## No overdispersion detected.
```

```
## # Indices of model performance
## 
## AIC     |    AICc |     BIC | Nagelkerke's R2 |  RMSE | Sigma | Score_log | Score_spherical
## -------------------------------------------------------------------------------------------
## 672.773 | 672.845 | 703.162 |           0.163 | 0.281 | 1.000 |    -0.285 |           0.028
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring + Curriculum + `Race/Ethnicity` + 
##     Fall_Spring:`Race/Ethnicity`, data = Only202_DFW, init.theta = 1566.704185, 
##     link = log)
## 
## Coefficients:
##                                            Estimate Std. Error z value Pr(>|z|)
## (Intercept)                                 -1.9040     0.1409 -13.515  < 2e-16
## Fall_SpringSpring                           -1.2884     0.3213  -4.010 6.06e-05
## CurriculumNew                               -2.0715     0.5104  -4.059 4.94e-05
## `Race/Ethnicity`nonWhite                     0.3117     0.2326   1.340   0.1803
## Fall_SpringSpring:`Race/Ethnicity`nonWhite   0.8489     0.4472   1.898   0.0577
##                                               
## (Intercept)                                ***
## Fall_SpringSpring                          ***
## CurriculumNew                              ***
## `Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:`Race/Ethnicity`nonWhite .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1566.704) family taken to be 1)
## 
##     Null deviance: 514.59  on 1169  degrees of freedom
## Residual deviance: 444.70  on 1165  degrees of freedom
## AIC: 672.77
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1567 
##           Std. Err.:  8611 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -660.773
```

Fall semester:


```
## [1] 0.1489715
```

```
## [1] 0.1715124
```

```
## [1] 0.1293931
```

Spring semester:


```
## [1] 0.04107318
```

```
## [1] 0.04728794
```

```
## [1] 0.03567518
```

Curriculum effect:


```
## [1] 0.1259966
```

```
## [1] 0.209905
```

```
## [1] 0.07563017
```

```
## [1] 7.936719
```

```
## [1] 4.764059
```

```
## [1] 13.22224
```

The model indicates that students in CURE Lab were 7.9-fold (4.76 - 13.2) less likely to earn a DFW than in first two Biology courses of the prior curriculum (p = 4.94e-05), after controlling for the effect of semester. There was no significant interaction between curriculum and either race/ethnicity or gender identity.


```
## [1] 1.616236
```

```
## [1] 1.973878
```

```
## [1] 1.323394
```


```
## Warning: Curriculum and Race/Ethnicity and Fall_Spring are not included in an
## interaction with one another in the model.
```

![](Figure9.Outcomes_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


## Figure 9


```
## Warning: Race/Ethnicity and Curriculum and Fall_Spring are not included in an
## interaction with one another in the model.
```

![](Figure9.Outcomes_files/figure-html/Figure 9-1.png)<!-- -->![](Figure9.Outcomes_files/figure-html/Figure 9-2.png)<!-- -->

### Only 201


```
##      Course                   Semester    Grade         Gender    
##  BIOL201:569   Spring 2022        :180   DFW : 133   Female:1251  
##  BIOL202:310   Fall Term 2019-2020:162   Pass:1606   Male  : 488  
##  BIOL205:470   Fall Term 2018-2019:159                            
##  BIOL206:390   Fall 2021          :149                            
##                Fall 2022          :149                            
##                Fall Term 2017-2018:149                            
##                (Other)            :791                            
##   Race/Ethnicity Curriculum    DFW         
##  White   :1202   Old:860    Mode :logical  
##  nonWhite: 537   New:879    FALSE:1606     
##                             TRUE :133      
##                                            
##                                            
##                                            
## 
```

```
##      Course                     Semester    Grade         Gender    
##  BIOL201:569   Fall Term 2019-2020  :162   DFW : 129   Female:1034  
##  BIOL202:  0   Fall Term 2018-2019  :159   Pass:1300   Male  : 395  
##  BIOL205:470   Fall Term 2017-2018  :149                            
##  BIOL206:390   Spring Term 2018-2019:148                            
##                Spring Term 2017-2018:123                            
##                Spring 2022          :119                            
##                (Other)              :569                            
##   Race/Ethnicity Curriculum    DFW          Fall_Spring       
##  White   :976    Old:860    Mode :logical   Length:1429       
##  nonWhite:453    New:569    FALSE:1300      Class :character  
##                             TRUE :129       Mode  :character  
##                                                               
##                                                               
##                                                               
## 
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW      29
## 4 New        Pass    850
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW      25
## 4 New        Pass    544
```

```
## `summarise()` has grouped output by 'Course'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 6 × 3
## # Groups:   Course [3]
##   Course  Grade     n
##   <fct>   <fct> <int>
## 1 BIOL201 DFW      25
## 2 BIOL201 Pass    544
## 3 BIOL205 DFW      77
## 4 BIOL205 Pass    393
## 5 BIOL206 DFW      27
## 6 BIOL206 Pass    363
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Curriculum, data = Only201_DFW, init.theta = 1757.638637, 
##     link = log)
## 
## Coefficients:
##               Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -2.11254    0.09806 -21.543  < 2e-16 ***
## CurriculumNew -1.01246    0.22275  -4.545 5.49e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1757.639) family taken to be 1)
## 
##     Null deviance: 620.40  on 1428  degrees of freedom
## Residual deviance: 595.59  on 1427  degrees of freedom
## AIC: 859.67
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1758 
##           Std. Err.:  10847 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -853.667
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`, 
##     data = Only201_DFW, init.theta = 1387.966567, link = log)
## 
## Coefficients:
##                                                                       Estimate
## (Intercept)                                                         -1.926e+00
## Fall_SpringSpring                                                   -1.190e+00
## CurriculumNew                                                       -1.629e+00
## GenderMale                                                           1.347e-01
## `Race/Ethnicity`nonWhite                                             3.059e-01
## Fall_SpringSpring:CurriculumNew                                      2.563e-01
## Fall_SpringSpring:GenderMale                                        -1.827e-01
## CurriculumNew:GenderMale                                             1.949e-02
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           4.468e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               9.807e-01
## GenderMale:`Race/Ethnicity`nonWhite                                 -1.671e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.787e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            -2.280e-01
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                1.090e+00
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    2.339e-01
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite -3.698e+01
##                                                                     Std. Error
## (Intercept)                                                          1.644e-01
## Fall_SpringSpring                                                    3.717e-01
## CurriculumNew                                                        4.765e-01
## GenderMale                                                           3.224e-01
## `Race/Ethnicity`nonWhite                                             2.874e-01
## Fall_SpringSpring:CurriculumNew                                      1.157e+00
## Fall_SpringSpring:GenderMale                                         7.406e-01
## CurriculumNew:GenderMale                                             8.966e-01
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           5.645e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               6.703e-01
## GenderMale:`Race/Ethnicity`nonWhite                                  5.202e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.607e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             1.391e+00
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                9.915e-01
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    1.178e+00
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  1.205e+07
##                                                                     z value
## (Intercept)                                                         -11.717
## Fall_SpringSpring                                                    -3.201
## CurriculumNew                                                        -3.419
## GenderMale                                                            0.418
## `Race/Ethnicity`nonWhite                                              1.065
## Fall_SpringSpring:CurriculumNew                                       0.222
## Fall_SpringSpring:GenderMale                                         -0.247
## CurriculumNew:GenderMale                                              0.022
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                            0.792
## CurriculumNew:`Race/Ethnicity`nonWhite                                1.463
## GenderMale:`Race/Ethnicity`nonWhite                                  -0.321
## Fall_SpringSpring:CurriculumNew:GenderMale                            1.112
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             -0.164
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                 1.100
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                     0.199
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite   0.000
##                                                                     Pr(>|z|)
## (Intercept)                                                          < 2e-16
## Fall_SpringSpring                                                   0.001372
## CurriculumNew                                                       0.000629
## GenderMale                                                          0.676218
## `Race/Ethnicity`nonWhite                                            0.287100
## Fall_SpringSpring:CurriculumNew                                     0.824671
## Fall_SpringSpring:GenderMale                                        0.805090
## CurriculumNew:GenderMale                                            0.982655
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                          0.428650
## CurriculumNew:`Race/Ethnicity`nonWhite                              0.143420
## GenderMale:`Race/Ethnicity`nonWhite                                 0.748073
## Fall_SpringSpring:CurriculumNew:GenderMale                          0.266020
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            0.869805
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite               0.271463
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                   0.842570
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite 0.999998
##                                                                        
## (Intercept)                                                         ***
## Fall_SpringSpring                                                   ** 
## CurriculumNew                                                       ***
## GenderMale                                                             
## `Race/Ethnicity`nonWhite                                               
## Fall_SpringSpring:CurriculumNew                                        
## Fall_SpringSpring:GenderMale                                           
## CurriculumNew:GenderMale                                               
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                             
## CurriculumNew:`Race/Ethnicity`nonWhite                                 
## GenderMale:`Race/Ethnicity`nonWhite                                    
## Fall_SpringSpring:CurriculumNew:GenderMale                             
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite               
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                  
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1387.966) family taken to be 1)
## 
##     Null deviance: 620.38  on 1428  degrees of freedom
## Residual deviance: 552.85  on 1413  degrees of freedom
## AIC: 844.95
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1388 
##           Std. Err.:  6737 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -810.947
```



```
## Start:  AIC=842.95
## DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`
## 
##                                                  Df    AIC
## <none>                                              842.95
## - Fall_Spring:Curriculum:Gender:`Race/Ethnicity`  1 846.22
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`, 
##     data = Only201_DFW, init.theta = 1387.966567, link = log)
## 
## Coefficients:
##                                                                       Estimate
## (Intercept)                                                         -1.926e+00
## Fall_SpringSpring                                                   -1.190e+00
## CurriculumNew                                                       -1.629e+00
## GenderMale                                                           1.347e-01
## `Race/Ethnicity`nonWhite                                             3.059e-01
## Fall_SpringSpring:CurriculumNew                                      2.563e-01
## Fall_SpringSpring:GenderMale                                        -1.827e-01
## CurriculumNew:GenderMale                                             1.949e-02
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           4.468e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               9.807e-01
## GenderMale:`Race/Ethnicity`nonWhite                                 -1.671e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.787e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            -2.280e-01
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                1.090e+00
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    2.339e-01
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite -3.698e+01
##                                                                     Std. Error
## (Intercept)                                                          1.644e-01
## Fall_SpringSpring                                                    3.717e-01
## CurriculumNew                                                        4.765e-01
## GenderMale                                                           3.224e-01
## `Race/Ethnicity`nonWhite                                             2.874e-01
## Fall_SpringSpring:CurriculumNew                                      1.157e+00
## Fall_SpringSpring:GenderMale                                         7.406e-01
## CurriculumNew:GenderMale                                             8.966e-01
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           5.645e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               6.703e-01
## GenderMale:`Race/Ethnicity`nonWhite                                  5.202e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.607e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             1.391e+00
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                9.915e-01
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    1.178e+00
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  1.205e+07
##                                                                     z value
## (Intercept)                                                         -11.717
## Fall_SpringSpring                                                    -3.201
## CurriculumNew                                                        -3.419
## GenderMale                                                            0.418
## `Race/Ethnicity`nonWhite                                              1.065
## Fall_SpringSpring:CurriculumNew                                       0.222
## Fall_SpringSpring:GenderMale                                         -0.247
## CurriculumNew:GenderMale                                              0.022
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                            0.792
## CurriculumNew:`Race/Ethnicity`nonWhite                                1.463
## GenderMale:`Race/Ethnicity`nonWhite                                  -0.321
## Fall_SpringSpring:CurriculumNew:GenderMale                            1.112
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             -0.164
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                 1.100
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                     0.199
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite   0.000
##                                                                     Pr(>|z|)
## (Intercept)                                                          < 2e-16
## Fall_SpringSpring                                                   0.001372
## CurriculumNew                                                       0.000629
## GenderMale                                                          0.676218
## `Race/Ethnicity`nonWhite                                            0.287100
## Fall_SpringSpring:CurriculumNew                                     0.824671
## Fall_SpringSpring:GenderMale                                        0.805090
## CurriculumNew:GenderMale                                            0.982655
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                          0.428650
## CurriculumNew:`Race/Ethnicity`nonWhite                              0.143420
## GenderMale:`Race/Ethnicity`nonWhite                                 0.748073
## Fall_SpringSpring:CurriculumNew:GenderMale                          0.266020
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            0.869805
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite               0.271463
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                   0.842570
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite 0.999998
##                                                                        
## (Intercept)                                                         ***
## Fall_SpringSpring                                                   ** 
## CurriculumNew                                                       ***
## GenderMale                                                             
## `Race/Ethnicity`nonWhite                                               
## Fall_SpringSpring:CurriculumNew                                        
## Fall_SpringSpring:GenderMale                                           
## CurriculumNew:GenderMale                                               
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                             
## CurriculumNew:`Race/Ethnicity`nonWhite                                 
## GenderMale:`Race/Ethnicity`nonWhite                                    
## Fall_SpringSpring:CurriculumNew:GenderMale                             
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite               
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                  
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1387.966) family taken to be 1)
## 
##     Null deviance: 620.38  on 1428  degrees of freedom
## Residual deviance: 552.85  on 1413  degrees of freedom
## AIC: 844.95
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1388 
##           Std. Err.:  6737 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -810.947
```



```
## 
## Call:  glm.nb(formula = DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`, 
##     data = Only201_DFW, init.theta = 1387.966567, link = log)
## 
## Coefficients:
##                                                         (Intercept)  
##                                                            -1.92642  
##                                                   Fall_SpringSpring  
##                                                            -1.18957  
##                                                       CurriculumNew  
##                                                            -1.62893  
##                                                          GenderMale  
##                                                             0.13466  
##                                            `Race/Ethnicity`nonWhite  
##                                                             0.30593  
##                                     Fall_SpringSpring:CurriculumNew  
##                                                             0.25628  
##                                        Fall_SpringSpring:GenderMale  
##                                                            -0.18274  
##                                            CurriculumNew:GenderMale  
##                                                             0.01949  
##                          Fall_SpringSpring:`Race/Ethnicity`nonWhite  
##                                                             0.44684  
##                              CurriculumNew:`Race/Ethnicity`nonWhite  
##                                                             0.98074  
##                                 GenderMale:`Race/Ethnicity`nonWhite  
##                                                            -0.16709  
##                          Fall_SpringSpring:CurriculumNew:GenderMale  
##                                                             1.78720  
##            Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite  
##                                                            -0.22802  
##               Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite  
##                                                             1.09031  
##                   CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  
##                                                             0.23393  
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  
##                                                           -36.97626  
## 
## Degrees of Freedom: 1428 Total (i.e. Null);  1413 Residual
## Null Deviance:	    620.4 
## Residual Deviance: 552.9 	AIC: 844.9
```

```
## # Check for zero-inflation
## 
##    Observed zeros: 1300
##   Predicted zeros: 1308
##             Ratio: 1.01
```

```
## Model seems ok, ratio of observed and predicted zeros is within the
##   tolerance range (p = 0.400).
```

```
## # Overdispersion test
## 
##  dispersion ratio = 0.865
##           p-value = 0.136
```

```
## No overdispersion detected.
```

```
## # Indices of model performance
## 
## AIC     |    AICc |     BIC | Nagelkerke's R2 |  RMSE | Sigma | Score_log | Score_spherical
## -------------------------------------------------------------------------------------------
## 844.947 | 845.380 | 934.447 |           0.131 | 0.279 | 1.000 |    -0.286 |           0.025
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring * Curriculum * Gender * `Race/Ethnicity`, 
##     data = Only201_DFW, init.theta = 1387.966567, link = log)
## 
## Coefficients:
##                                                                       Estimate
## (Intercept)                                                         -1.926e+00
## Fall_SpringSpring                                                   -1.190e+00
## CurriculumNew                                                       -1.629e+00
## GenderMale                                                           1.347e-01
## `Race/Ethnicity`nonWhite                                             3.059e-01
## Fall_SpringSpring:CurriculumNew                                      2.563e-01
## Fall_SpringSpring:GenderMale                                        -1.827e-01
## CurriculumNew:GenderMale                                             1.949e-02
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           4.468e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               9.807e-01
## GenderMale:`Race/Ethnicity`nonWhite                                 -1.671e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.787e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            -2.280e-01
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                1.090e+00
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    2.339e-01
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite -3.698e+01
##                                                                     Std. Error
## (Intercept)                                                          1.644e-01
## Fall_SpringSpring                                                    3.717e-01
## CurriculumNew                                                        4.765e-01
## GenderMale                                                           3.224e-01
## `Race/Ethnicity`nonWhite                                             2.874e-01
## Fall_SpringSpring:CurriculumNew                                      1.157e+00
## Fall_SpringSpring:GenderMale                                         7.406e-01
## CurriculumNew:GenderMale                                             8.966e-01
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                           5.645e-01
## CurriculumNew:`Race/Ethnicity`nonWhite                               6.703e-01
## GenderMale:`Race/Ethnicity`nonWhite                                  5.202e-01
## Fall_SpringSpring:CurriculumNew:GenderMale                           1.607e+00
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             1.391e+00
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                9.915e-01
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                    1.178e+00
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite  1.205e+07
##                                                                     z value
## (Intercept)                                                         -11.717
## Fall_SpringSpring                                                    -3.201
## CurriculumNew                                                        -3.419
## GenderMale                                                            0.418
## `Race/Ethnicity`nonWhite                                              1.065
## Fall_SpringSpring:CurriculumNew                                       0.222
## Fall_SpringSpring:GenderMale                                         -0.247
## CurriculumNew:GenderMale                                              0.022
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                            0.792
## CurriculumNew:`Race/Ethnicity`nonWhite                                1.463
## GenderMale:`Race/Ethnicity`nonWhite                                  -0.321
## Fall_SpringSpring:CurriculumNew:GenderMale                            1.112
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite             -0.164
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                 1.100
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                     0.199
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite   0.000
##                                                                     Pr(>|z|)
## (Intercept)                                                          < 2e-16
## Fall_SpringSpring                                                   0.001372
## CurriculumNew                                                       0.000629
## GenderMale                                                          0.676218
## `Race/Ethnicity`nonWhite                                            0.287100
## Fall_SpringSpring:CurriculumNew                                     0.824671
## Fall_SpringSpring:GenderMale                                        0.805090
## CurriculumNew:GenderMale                                            0.982655
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                          0.428650
## CurriculumNew:`Race/Ethnicity`nonWhite                              0.143420
## GenderMale:`Race/Ethnicity`nonWhite                                 0.748073
## Fall_SpringSpring:CurriculumNew:GenderMale                          0.266020
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite            0.869805
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite               0.271463
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                   0.842570
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite 0.999998
##                                                                        
## (Intercept)                                                         ***
## Fall_SpringSpring                                                   ** 
## CurriculumNew                                                       ***
## GenderMale                                                             
## `Race/Ethnicity`nonWhite                                               
## Fall_SpringSpring:CurriculumNew                                        
## Fall_SpringSpring:GenderMale                                           
## CurriculumNew:GenderMale                                               
## Fall_SpringSpring:`Race/Ethnicity`nonWhite                             
## CurriculumNew:`Race/Ethnicity`nonWhite                                 
## GenderMale:`Race/Ethnicity`nonWhite                                    
## Fall_SpringSpring:CurriculumNew:GenderMale                             
## Fall_SpringSpring:CurriculumNew:`Race/Ethnicity`nonWhite               
## Fall_SpringSpring:GenderMale:`Race/Ethnicity`nonWhite                  
## CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite                      
## Fall_SpringSpring:CurriculumNew:GenderMale:`Race/Ethnicity`nonWhite    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1387.966) family taken to be 1)
## 
##     Null deviance: 620.38  on 1428  degrees of freedom
## Residual deviance: 552.85  on 1413  degrees of freedom
## AIC: 844.95
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1388 
##           Std. Err.:  6737 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -810.947
```

That is very messy.
Let's just add semester to the simple model.


```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring * Curriculum, data = Only201_DFW, 
##     init.theta = 1682.547563, link = log)
## 
## Coefficients:
##                                 Estimate Std. Error z value Pr(>|z|)    
## (Intercept)                      -1.8089     0.1140 -15.872  < 2e-16 ***
## Fall_SpringSpring                -0.8614     0.2237  -3.851 0.000118 ***
## CurriculumNew                    -1.1386     0.2680  -4.249 2.15e-05 ***
## Fall_SpringSpring:CurriculumNew   0.3871     0.4836   0.800 0.423438    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1682.548) family taken to be 1)
## 
##     Null deviance: 620.40  on 1428  degrees of freedom
## Residual deviance: 577.67  on 1425  degrees of freedom
## AIC: 845.75
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1683 
##           Std. Err.:  9501 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -835.746
```

```
## Start:  AIC=843.75
## DFW ~ Fall_Spring * Curriculum
## 
##                          Df    AIC
## - Fall_Spring:Curriculum  1 842.37
## <none>                      843.75
## 
## Step:  AIC=842.37
## DFW ~ Fall_Spring + Curriculum
## 
##               Df    AIC
## <none>           842.37
## - Fall_Spring  1 857.67
## - Curriculum   1 866.04
```

```
## 
## Call:
## glm.nb(formula = DFW ~ Fall_Spring + Curriculum, data = Only201_DFW, 
##     init.theta = 1535.114374, link = log)
## 
## Coefficients:
##                   Estimate Std. Error z value Pr(>|z|)    
## (Intercept)        -1.8297     0.1122 -16.314  < 2e-16 ***
## Fall_SpringSpring  -0.7837     0.1980  -3.957 7.59e-05 ***
## CurriculumNew      -1.0288     0.2228  -4.618 3.87e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1535.114) family taken to be 1)
## 
##     Null deviance: 620.39  on 1428  degrees of freedom
## Residual deviance: 578.29  on 1426  degrees of freedom
## AIC: 844.37
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1535 
##           Std. Err.:  8288 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -836.369
```



```
## [1] 0.1604617
```

```
## [1] 0.1795144
```

```
## [1] 0.1434312
```



```
## [1] 0.3574356
```

```
## [1] 0.4466411
```

```
## [1] 0.2860468
```

```
## [1] 2.797707
```

```
## [1] 2.238934
```

```
## [1] 3.495932
```

The model indicates that students in BIOL 201 were  2.80-fold (2.24 - 3.50) less likely to earn a DFW than in first two Biology courses of the prior curriculum (p = 3.87e-06), after controlling for the effect of semester.


```
## [1] 1.742986
```

```
## [1] 2.082356
```

```
## [1] 1.458925
```

![](Figure9.Outcomes_files/figure-html/unnamed-chunk-25-1.png)<!-- -->


## Figure 9 201 version


```
## Warning: Fall_Spring and Curriculum are not included in an interaction with one
## another in the model.
```

![](Figure9.Outcomes_files/figure-html/Figure 9 201-1.png)<!-- -->![](Figure9.Outcomes_files/figure-html/Figure 9 201-2.png)<!-- -->

## First-gen status

One of the studies of COVID impacts demonstrated that first-gen students were more highly impacted than other students.
We would like to see how that factor translates to our outcomes.


```
##      Course                     Semester      Gender    
##  BIOL201:514   Fall Term 2019-2020  :162   Female:1214  
##  BIOL202:310   Fall Term 2018-2019  :159   Male  : 470  
##  BIOL205:470   Fall Term 2017-2018  :149                
##  BIOL206:390   Fall Term 2021-2022  :149                
##                Fall Term 2022-2023  :148                
##                Spring Term 2018-2019:148                
##                (Other)              :769                
##  First-Generation Indicator  Grade      Curriculum Fall_Spring     DFW         
##  N:1466                     DFW : 132   Old:860    Fall  :911   Mode :logical  
##  Y: 218                     Pass:1552   New:824    Spring:773   FALSE:1552     
##                                                                 TRUE :132      
##                                                                                
##                                                                                
##                                                                                
## 
```

```
##      Course                     Semester      Gender   
##  BIOL201:  0   Fall Term 2019-2020  :162   Female:850  
##  BIOL202:310   Fall Term 2018-2019  :159   Male  :320  
##  BIOL205:470   Fall Term 2017-2018  :149               
##  BIOL206:390   Spring Term 2018-2019:148               
##                Spring Term 2017-2018:123               
##                Spring Term 2016-2017:119               
##                (Other)              :310               
##  First-Generation Indicator  Grade      Curriculum Fall_Spring     DFW         
##  N:1028                     DFW : 108   Old:860    Fall  :588   Mode :logical  
##  Y: 142                     Pass:1062   New:310    Spring:582   FALSE:1062     
##                                                                 TRUE :108      
##                                                                                
##                                                                                
##                                                                                
## 
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW      28
## 4 New        Pass    796
```

```
## `summarise()` has grouped output by 'Curriculum'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 4 × 3
## # Groups:   Curriculum [2]
##   Curriculum Grade     n
##   <fct>      <fct> <int>
## 1 Old        DFW     104
## 2 Old        Pass    756
## 3 New        DFW       4
## 4 New        Pass    306
```

```
## `summarise()` has grouped output by 'Course'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 6 × 3
## # Groups:   Course [3]
##   Course  Grade     n
##   <fct>   <fct> <int>
## 1 BIOL202 DFW       4
## 2 BIOL202 Pass    306
## 3 BIOL205 DFW      77
## 4 BIOL205 Pass    393
## 5 BIOL206 DFW      27
## 6 BIOL206 Pass    363
```


```
## 
## Call:
## glm.nb(formula = DFW ~ `First-Generation Indicator` * Curriculum, 
##     data = Only202_DFW_Firstgen, init.theta = 1259.411507, link = log)
## 
## Coefficients:
##                                               Estimate Std. Error z value
## (Intercept)                                 -2.176e+00  1.078e-01 -20.182
## `First-Generation Indicator`Y                4.417e-01  2.592e-01   1.704
## CurriculumNew                               -2.036e+00  5.115e-01  -3.980
## `First-Generation Indicator`Y:CurriculumNew -3.327e+01  1.061e+07   0.000
##                                             Pr(>|z|)    
## (Intercept)                                  < 2e-16 ***
## `First-Generation Indicator`Y                 0.0884 .  
## CurriculumNew                               6.89e-05 ***
## `First-Generation Indicator`Y:CurriculumNew   1.0000    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1259.411) family taken to be 1)
## 
##     Null deviance: 514.57  on 1169  degrees of freedom
## Residual deviance: 470.40  on 1166  degrees of freedom
## AIC: 696.48
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1259 
##           Std. Err.:  6750 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -686.483
```

```
## 
## Call:
## glm.nb(formula = DFW ~ `First-Generation Indicator` * Fall_Spring * 
##     Curriculum, data = Only202_DFW_Firstgen, init.theta = 946.8859954, 
##     link = log)
## 
## Coefficients:
##                                                                 Estimate
## (Intercept)                                                   -1.869e+00
## `First-Generation Indicator`Y                                  4.270e-01
## Fall_SpringSpring                                             -8.773e-01
## CurriculumNew                                                 -1.676e+00
## `First-Generation Indicator`Y:Fall_SpringSpring                7.897e-02
## `First-Generation Indicator`Y:CurriculumNew                   -3.392e+01
## Fall_SpringSpring:CurriculumNew                               -6.889e-01
## `First-Generation Indicator`Y:Fall_SpringSpring:CurriculumNew  1.487e+00
##                                                               Std. Error
## (Intercept)                                                    1.250e-01
## `First-Generation Indicator`Y                                  3.043e-01
## Fall_SpringSpring                                              2.472e-01
## CurriculumNew                                                  5.907e-01
## `First-Generation Indicator`Y:Fall_SpringSpring                5.814e-01
## `First-Generation Indicator`Y:CurriculumNew                    1.794e+07
## Fall_SpringSpring:CurriculumNew                                1.181e+00
## `First-Generation Indicator`Y:Fall_SpringSpring:CurriculumNew  2.225e+07
##                                                               z value Pr(>|z|)
## (Intercept)                                                   -14.954  < 2e-16
## `First-Generation Indicator`Y                                   1.403 0.160475
## Fall_SpringSpring                                              -3.550 0.000386
## CurriculumNew                                                  -2.838 0.004543
## `First-Generation Indicator`Y:Fall_SpringSpring                 0.136 0.891966
## `First-Generation Indicator`Y:CurriculumNew                     0.000 0.999998
## Fall_SpringSpring:CurriculumNew                                -0.583 0.559623
## `First-Generation Indicator`Y:Fall_SpringSpring:CurriculumNew   0.000 1.000000
##                                                                  
## (Intercept)                                                   ***
## `First-Generation Indicator`Y                                    
## Fall_SpringSpring                                             ***
## CurriculumNew                                                 ** 
## `First-Generation Indicator`Y:Fall_SpringSpring                  
## `First-Generation Indicator`Y:CurriculumNew                      
## Fall_SpringSpring:CurriculumNew                                  
## `First-Generation Indicator`Y:Fall_SpringSpring:CurriculumNew    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(946.886) family taken to be 1)
## 
##     Null deviance: 514.54  on 1169  degrees of freedom
## Residual deviance: 451.45  on 1162  degrees of freedom
## AIC: 685.56
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  947 
##           Std. Err.:  4098 
## Warning while fitting theta: alternation limit reached 
## 
##  2 x log-likelihood:  -667.56
```



```
## Start:  AIC=683.56
## DFW ~ `First-Generation Indicator` * Fall_Spring * Curriculum
## 
##                                                       Df    AIC
## - `First-Generation Indicator`:Fall_Spring:Curriculum  1 681.56
## <none>                                                   683.56
## 
## Step:  AIC=681.56
## DFW ~ `First-Generation Indicator` + Fall_Spring + Curriculum + 
##     `First-Generation Indicator`:Fall_Spring + `First-Generation Indicator`:Curriculum + 
##     Fall_Spring:Curriculum
## 
##                                            Df    AIC
## - `First-Generation Indicator`:Fall_Spring  1 679.58
## - Fall_Spring:Curriculum                    1 679.93
## - `First-Generation Indicator`:Curriculum   1 681.12
## <none>                                        681.56
## 
## Step:  AIC=679.58
## DFW ~ `First-Generation Indicator` + Fall_Spring + Curriculum + 
##     `First-Generation Indicator`:Curriculum + Fall_Spring:Curriculum
## 
##                                           Df    AIC
## - Fall_Spring:Curriculum                   1 677.97
## - `First-Generation Indicator`:Curriculum  1 679.14
## <none>                                       679.58
## 
## Step:  AIC=677.97
## DFW ~ `First-Generation Indicator` + Fall_Spring + Curriculum + 
##     `First-Generation Indicator`:Curriculum
## 
##                                           Df    AIC
## - `First-Generation Indicator`:Curriculum  1 677.55
## <none>                                       677.97
## - Fall_Spring                              1 694.48
## 
## Step:  AIC=677.55
## DFW ~ `First-Generation Indicator` + Fall_Spring + Curriculum
## 
##                                Df    AIC
## <none>                            677.55
## - `First-Generation Indicator`  1 677.75
## - Fall_Spring                   1 694.11
## - Curriculum                    1 709.01
```

```
## 
## Call:
## glm.nb(formula = DFW ~ `First-Generation Indicator` + Fall_Spring + 
##     Curriculum, data = Only202_DFW_Firstgen, init.theta = 1082.294948, 
##     link = log)
## 
## Coefficients:
##                               Estimate Std. Error z value Pr(>|z|)    
## (Intercept)                    -1.8571     0.1206 -15.397  < 2e-16 ***
## `First-Generation Indicator`Y   0.4003     0.2582   1.550    0.121    
## Fall_SpringSpring              -0.8936     0.2200  -4.061 4.89e-05 ***
## CurriculumNew                  -2.0975     0.5106  -4.108 3.99e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(1082.295) family taken to be 1)
## 
##     Null deviance: 514.56  on 1169  degrees of freedom
## Residual deviance: 453.45  on 1166  degrees of freedom
## AIC: 679.55
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  1082 
##           Std. Err.:  5014 
## Warning while fitting theta: iteration limit reached 
## 
##  2 x log-likelihood:  -669.554
```

That analysis showed that including first-generation status marginally improved the model, but it was very minor compared to semester and curriculum. This was true for both the effect size and the significance.

