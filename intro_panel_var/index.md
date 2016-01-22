---
title       : Very Quick Introduction to Panel VAR for Political Scientists
subtitle    : Justin Murphy, University of Southampton
author      : jmrphy.net
job         : '@jmrphy'
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : mathjax            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

## Plan

1. What is VAR?
2. What is Panel VAR?
3. Example with code

---

## What is VAR?

> - "Vector autoregression" refers to an "*n*-equation, *n*-variable linear model in which each variable is in turn explained by its own lagged values, plus current and past values of the remaining n - 1 variables." (Stock and Watson 2001)

> - Standard tool of econometrics since the 1980s (Sims 1980).
    - Somewhat controversial because it's rather atheoretical
    
> - At a minimum, VAR is a powerful tool for revealing the "stylized facts" of covariation in time series data

---

## What is Panel VAR?

Pools the time-series of multiple units (Hood et. al 2008; Hurlin and Venet 2001):

$$ y_{i,t} = \alpha_{i} + \sum_{k=1}^p \gamma^{(k)} y_{i,t-k} + \sum_{k=0}^p \beta_i^{(k)} x_{i,t-k} + v_{i,t} $$

for each of the cross-sections *i* and for all *t* in [1,T], where

$$ \gamma^{(k)} $$ represents the autoregressive coefficients and
$$ \beta_i^{(k)} $$ represents the regression coefficients

---

## Stata Implementation

- In 2006, a Stata implementation was made available by Inessa Love at the World Bank

- Not integrated into Stata yet, so you have to use it manually

![Stata Program](https://dl.dropboxusercontent.com/u/20498362/Love_program.png)

---

## Basic Workflow
> 1. Time de-mean the variables
> 2. Use the "Helmert procedure" (Arellano and Bover 1995)
      - removes the mean of all future observations of the unit-year.
      - Keeps orthogonality between transformed variables and regressors so lagged regressors can be used as instruments in system GMM
> 3. Estimate panel VAR using system GMM
> 4. Graph impulse responses using Monte Carlo simulations

---

## Script

gen id=scode
xtset id year

foreach v of varlist var1 var2 var3 {  
bysort id: egen mean_`v'=mean(`v')  
gen demean_`v'=`v'-mean_`v'  
drop mean_`v'  
}

helm demean_var1 demean_var2 demean_var3

pvar demean_var1 demean_var2 demean_var3, lag(3) gmm monte 500

---

## Thank you!

These slides can be found at jmrphy.net/intro_panel_var

A reference list and all the code can be found at github.com/jmrphy/intro_panel_var

jmrphy.net  
@jmrphy



