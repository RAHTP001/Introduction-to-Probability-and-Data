---
title: "Exploring the BRFSS data"
output: 
  html_document: 
    fig_height: 4
    highlight: pygments
    keep_md: yes
    theme: spacelab
---

## Background
The Behavioral Risk Factor Surveillance System (BRFSS) is an annual telephone survey in the United States. The BRFSS is designed to identify so-called risk factors in the adult population and report emerging trends. For example, respondents are asked about their diet and weekly physical activity, their HIV/AIDS status, possible tobacco use, immunization, health status, healthy days - health-related quality of life, health care access, inadequate sleep, hypertension awareness, cholesterol awareness, chronic health conditions, alcohol consumption, fruits and vegetables consumption, arthritis burden, and seatbelt use etc.

##

### Load packages


```r
library(ggplot2)
library(dplyr)
```

### Loading data


```r
load("brfss2013.RData")
```


* * *

## Part 1: Data
The data were collected from United States' all 50 states, the District of Columbia, Puerto Rico, Guam and American Samoa, Federated States of Micronesia, and Palau, by conducting both landline telephone- and cellular telephone-based surveys. Disproportionate stratified sampling (DSS) has been used for the landline sample and the cellular telephone respondents are randomly selected with each having equal probability of selection. The dataset we are working on contains 330 variables for a total of 491, 775 samples in 2013. The missing values denoted by "NA"


```r
table(brfss2013$iyear)
```

```
## 
##   2013   2014 
## 486088   5682
```

## Generalizability and Causality.

Based on this information, I think that the results from the analysis done using the data collected, could be **generazible** to the population because the sample is done using in certain point a random selection, the size of the sample is large and because some of the possible bias in the sample is removed using the data weighting, but the relationship between the variables don´t describe **casuality**, because there is no random assignment of the people and because the study is done using a survey, in other words observing the characteristics of the U.S. residents.

##veriables

In order to investigate about this question, is neccesary to become familiar with the  variables,For this, I’m going to show the structure of each of these variables, with the following code

I will use the following vairables
<ol>
<li> physhlth = Number Of Days Physical Health Not Good</li>        
<li> poorhlth = Poor Physical Or Mental Health </li>           
<li> genhlth =  Corresponds to General Health </li>                  
<li> income2 =  Corresponds  Income Level </li> 
<li> hlthpln1= Have Any Health Care Coverage </li> 
<li> sex = Respondents Sex </li>
<li> iyear =  Interview Year </li>
</ol>



```r
brfss2013 %>% select(physhlth,poorhlth,genhlth,income2,sex,iyear,hlthpln1) %>% str()
```

```
## 'data.frame':	491775 obs. of  7 variables:
##  $ physhlth: int  30 0 3 2 10 0 1 5 0 0 ...
##  $ poorhlth: int  30 NA 0 0 0 NA 0 10 NA NA ...
##  $ genhlth : Factor w/ 5 levels "Excellent","Very good",..: 4 3 3 2 3 2 4 3 1 3 ...
##  $ income2 : Factor w/ 8 levels "Less than $10,000",..: 7 8 8 7 6 8 NA 6 8 4 ...
##  $ sex     : Factor w/ 2 levels "Male","Female": 2 2 2 2 1 2 2 2 1 2 ...
##  $ iyear   : Factor w/ 2 levels "2013","2014": 1 1 1 1 1 1 1 1 1 1 ...
##  $ hlthpln1: Factor w/ 2 levels "Yes","No": 1 1 1 1 1 1 1 1 1 1 ...
```


We could see that the variables physhlth and poorhlth are both numeric with integer values, and that the variable like
 <ul>
<li>genhlth is a factor with 6 levels."Excellent" "Very good" "Good" "Fair" "Poor</li>
<li>income2 is a factor with 8 levels,</li>
<li>hlthpln1 is factor with 2 levels "Yes" "No",</li>
<li>sex  is factor with 2 levels "Male"   "Female",</li>
<li>iyear is factor with 2 levels  "2013" "2014",</li>
</ul>

Let´s see a summary for these variables.


```r
brfss2013 %>% select(physhlth,poorhlth,genhlth,income2,sex,iyear,hlthpln1) %>% summary()
```

```
##     physhlth         poorhlth           genhlth      
##  Min.   : 0.000   Min.   :   0.0   Excellent: 85482  
##  1st Qu.: 0.000   1st Qu.:   0.0   Very good:159076  
##  Median : 0.000   Median :   0.0   Good     :150555  
##  Mean   : 4.353   Mean   :   5.3   Fair     : 66726  
##  3rd Qu.: 3.000   3rd Qu.:   5.0   Poor     : 27951  
##  Max.   :60.000   Max.   :7000.0   NA's     :  1985  
##  NA's   :10957    NA's   :243153                     
##               income2           sex          iyear        hlthpln1     
##  $75,000 or more  :115902   Male  :201313   2013:486088   Yes :434571  
##  Less than $75,000: 65231   Female:290455   2014:  5682   No  : 55300  
##  Less than $50,000: 61509   NA's  :     7   NA's:     5   NA's:  1904  
##  Less than $35,000: 48867                                              
##  Less than $25,000: 41732                                              
##  (Other)          : 87108                                              
##  NA's             : 71426
```

* * *

## Part 2: Research questions

**Research quesion 1:**
Does the distribution of the number of days in which physical and mental health was not good during the past 30 days differ by gender.


**Research quesion 2:**
Is there an association between the month in which a respondent was interviewed and the respondent’s self-reported health perception?

**Research quesion 3:**
Is there any association between income and health care coverage

* * *

## Part 3:A) Exploratory data analysis for "REsearch question 1"



```r
ggplot(aes(x= (physhlth), fill=sex), data = brfss2013[!is.na(brfss2013$sex), ]) +
  geom_histogram(bins=15, position = position_dodge()) + ggtitle('Number of Days Physical Health not Good in the Past 30 Days')
```

```
## Warning: Removed 10953 rows containing non-finite values (stat_bin).
```

![](intro_data_prob_project_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


```r
ggplot(aes(x=poorhlth, fill=sex), data=brfss2013[!is.na(brfss2013$sex), ]) +
  geom_histogram(bins=15, position = position_dodge()) + ggtitle('Number of Days with Poor Physical Or Mental Health in the Past 30 Days')
```

```
## Warning: Removed 243149 rows containing non-finite values (stat_bin).
```

![](intro_data_prob_project_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

**conclusion** 

The above two figures show the data distribution of how male and female responder to the number of days physical, mental and both health not good during the past 30 days. We can see that there were more number of female respondents than male respondents.

**Research quesion 2:**


```r
by_month <- brfss2013 %>% filter(iyear=='2013') %>% group_by(imonth,genhlth) %>% summarise(n=n())
ggplot(aes(x=imonth, y=n, fill = genhlth), data = by_month[!is.na(by_month$genhlth), ]) + geom_bar(stat = 'identity', position = position_dodge()) + ggtitle('Health Perception By Month')+theme(axis.text.x = element_text(angle = 60, hjust = 1))
```

![](intro_data_prob_project_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


```r
by_month1 <- brfss2013 %>% filter(iyear=='2013') %>% group_by(imonth) %>% summarise(n=n())
ggplot(aes(x=imonth, y=n), data=by_month1) + geom_bar(stat = 'identity',color="red",fill="green") + ggtitle('Number of Respondents by Month')+theme(axis.text.x = element_text(angle = 60, hjust = 1))
```

![](intro_data_prob_project_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**Conclusion** 
I was trying to find out any exited pattern whether people respond their health condition differently in the different month. For example, are people more likely to say they are in good health in the spring or summer? It appears from graph that there was no obvious pattern.

**Research quesion 3:**


```r
plot(brfss2013$income2, brfss2013$hlthpln1,col=c("blue","green"), xlab = 'Income Level', ylab = 'Health Care Coverage', main =
'Income Level versus Health Care Coverage')
```

![](intro_data_prob_project_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**Conclusion** 
Above graph clearly shows that higher income respondents are more likely to have health care coverage then those of lower income respondents


## Summary

When we analyze health survery data, we must be aware that self-reported prevalence may be biased because respondents may not be aware of their risk status There is no causation can be established as BRFSS is an observation study that can only establish correlation/association between variables.
