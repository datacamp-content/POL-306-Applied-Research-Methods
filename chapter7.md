---
title: Regression
description: ""
---

## Linear Regression

```yaml
type: NormalExercise
key: 4293374fdf
xp: 100
```

The most important function you will use for estimating linear regression models is `lm()`. This will expect a formula and will output something called a model object that you will want to save so you can then manipulate it. 

For example, if you have a dataframe called `data.frame` and three variables named: `var1`, `var2`, and `var3`. If you want to estimate a linear regression with `var1` as the dependent variable and the other two as independent variables you would write:

```
mod <- lm(var1 ~ var2 + var3, data=data.frame)
```

This won't output anything but you can look at the results using `summary()` like this:

`summary(mod)`

`@instructions`
We will start by just estimating a linear regression of the number of protests someone attended (the DV) as a function of the person's age and which convention they were at. The variables are all in the `df` dataset and are: `marches`, `age`, and `convention`. 

You should save the linear model as `mod` (like in the example above) and then output the results using the `summary()` function.

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())
```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 
```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df)

mod <- lm(marches~age + convention, data=df)
summary(mod)
```

`@sct`
```{r}
ex() %>% {
  check_function(., "lm" ) %>%
    check_arg("formula") %>% check_equal()
  check_function(., "summary")
}
```

---

## Linear Regression Extended

```yaml
type: NormalExercise
key: 4b9aec584a
xp: 100
```

The reason that we save the `lm()` object is that there are a lot of things we can do with it after we are done. Here are a few things:

- `coef(mod)` will create a vector of just the coefficients, which can be useful if we want to do something to them (as we will with the logistic regression). 
- `mod$residuals` is a vector of all the residuals for the regression. We could easily plot them with `plot(mod$residuals)` and one of the simpler ways to get the number of observations is with `length(mod$residuals)`.
- `mod$fitted.values` is a vector of the predictions from the regression.

`@instructions`
Using the `mod` we made in the last assignment (I included the code here), do the following:

1. Get the number of the observations included in the model. 
2. Plot the residuals.

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())
```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df)

mod <- lm(marches~age + convention, data=df)
summary(mod)


```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df)

mod <- lm(marches~age + convention, data=df)
summary(mod)

length(mod$residuals)
plot(mod$residuals)
```

`@sct`
```{r}
ex() %>% {
  check_function(., "length" ) %>%
    check_arg("x") %>% check_equal()
  check_function(., "plot") %>% 
    check_arg("x") %>% check_equal()
}
```

---

## Logistic Regression

```yaml
type: NormalExercise
key: 1842904957
xp: 100
```

Logistic regression is similar to linear regression but will use a slightly different function and needs to include a particular argument at the end. For logistic regression we use `glm()` which is the generalized linear model function and can be used to calculated a variety of models. To tell it to estimate a logistic model we include `family=binomial(link="logit")` as an argument at the end of the function call. All together this is:

```
glm(var1~var2 + var3, data=data.frame, family=binomial(link="logit")
```

R will automatically assign a 1 and a 0 to your dependent variable. It does this by assigning 0 to the first attribute that appears if you call `levels(data.frame$var1)` and 1 to the second attribute. 

In addition, all the functions we saw for `lm()` work for `glm()`

`@instructions`
Estimate a logistic regression predicting which convention someone was at using age and their party identification (`party` in the data) as independent variables. 

After you estimate the model output the `summary()` and use the `coef()` function and `exp()` to calculate the odds ratios of the logistic regression.

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())
```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df)
levels(df$convention)


```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df)
levels(df$convention)

mod <- glm(convention~age+party, data=df, family=binomial(link="logit"))
summary(mod)
exp(coef(mod))
```

`@sct`
```{r}
ex() %>% {
  check_function(., "glm" ) %>% {
    check_arg(., "formula") %>% check_equal()
    check_arg(., "family") %>% check_equal()
  }
  check_function(., "summary")
  check_output_expr(., "exp(coef(mod))")
}
```
