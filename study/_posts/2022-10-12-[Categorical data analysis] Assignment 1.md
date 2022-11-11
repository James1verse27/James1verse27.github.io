---
layout: post
title:  "[Categorical data analysis] Assignment(1)"
category: Study
tag: [R, blog, jekyll, Data, Categorical data analysis, GLM]
toc: true
---
* this unordered seed list will be replaced by the toc
{:toc}

- 뒤로가기를 누르시면 목차로 되돌아옵니다. 😉

**패키지**
<details>
<summary>
설치된 패키지 접기/펼치기 버튼
</summary>

<div markdown="1">

``` r
#install.packages("kableExtra")
#install.packages("dplyr")
#install.packages("car")
#install.packages("moonBook")
library('kableExtra')
library('dplyr')
library('car')
library('moonBook')
```

</div>

</details>

# Q-1

![](/study/img/[Categorical data analysis] Assignment 1/Q-1a.jpg)

![](/study/img/[Categorical data analysis] Assignment 1/Q-1b.jpg)

## Q-1 1)

>**1) Is the Weibull distribution the exponential dispersion family?**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-1.jpg)

## Q-1 2)

>**2) Please find the score function and estimate $$\theta$$ when $$\lambda=2$$.**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-2.jpg)

``` r
y <- c(1051, 1337, 1389, 1921, 1942, 
       2322, 3629, 4006, 4012, 4063, 
       4921, 5445, 5620, 5817, 5905, 
       5956, 6068, 6121, 6473, 7501, 
       7886, 8108, 8546, 8666, 8831, 
       9106, 9711, 9806, 10205, 10396, 
       10861, 11026, 11214, 11362, 11604, 
       11608, 11745, 11762, 11895, 12044, 
       13520, 13670, 14110, 14496, 15395, 
       16179, 17092, 17568, 17568)
sqrt(sum(y^2)/length(y))
```

    ## [1] 9892.177

## Q-1 3)

>**3) Please derive the fisher information formula for $$\theta$$.**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-3.jpg)

## Q-1 4)

>**4) Use the Newton-Raphson formula to find $$\theta$$.**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-4.jpg)

## Q-1 5)

>**5) Suppose that $$\theta^{𝓀}$$ the $$𝓀^{th}$$ iterated value based on the NewtonRapson formmula.**
**Obtain the fifth iterated value $$\theta^{5}$$ given $$\lambda=2$$.**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-5.jpg)

``` r
a0 <- mean(y)

for( i in 0:5){
a0 <- a0+(a0*(sum(y^2)-length(y)*a0^2))/(3*sum(y^2)-length(y)*a0^2)
print(i)
print(a0)
}
```

    ## [1] 0
    ## [1] 9633.777
    ## [1] 1
    ## [1] 9875.898
    ## [1] 2
    ## [1] 9892.11
    ## [1] 3
    ## [1] 9892.177
    ## [1] 4
    ## [1] 9892.177
    ## [1] 5
    ## [1] 9892.177

> 초기값을 $$E(Y_i)$$로 설정하여 돌려본 결과 3번만의 MLE로 잘 추정하는 결과가 나왔다.

## Q-1 6)

>**Please construct 95% confidence interval vasd on Wald test for testing $$H_0 : \theta = 0 \ vs \ H_1 : \theta \neq 0$$.**

![](/study/img/[Categorical data analysis] Assignment 1/Q-1-6.jpg)

# Q-2

![](/study/img/[Categorical data analysis] Assignment 1/Q-2a.jpg){: width="600" height="700"}

![](/study/img/[Categorical data analysis] Assignment 1/Q-2b.jpg){: width="600" height="700"}

``` r
y <- c(2,3,6,7,8,9,10,12,15)
x <- c(-1,-1,0,0,0,0,1,1,1)
a <- list(y,x)
data <- data.frame(y,x)
data%>%
  kbl(caption = "Q-2 dataset") %>%
  kable_minimal() %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class=" lightable-minimal table table-striped table-hover table-condensed table-responsive" style="font-family: &quot;Trebuchet MS&quot;, verdana, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;">
<caption>
Q-2 dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
y
</th>
<th style="text-align:right;">
x
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
-1
</td>
</tr>
<tr>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
-1
</td>
</tr>
<tr>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
1
</td>
</tr>
</tbody>
</table>

## Q-2 1)

``` r
poi_reg <- glm(y~x,family=poisson(link=identity),data=data)
summary <- summary(glm(y~x,family=poisson(link=identity),data=data))
summary$coefficients%>%
  kbl(caption = "MLE") %>%
  kable_minimal() %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class=" lightable-minimal table table-striped table-hover table-condensed table-responsive" style="font-family: &quot;Trebuchet MS&quot;, verdana, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;">
<caption>
MLE
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
Estimate
</th>
<th style="text-align:right;">
Std. Error
</th>
<th style="text-align:right;">
z value
</th>
<th style="text-align:right;">
Pr(\>\|z\|)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
7.451633
</td>
<td style="text-align:right;">
0.8841222
</td>
<td style="text-align:right;">
8.428284
</td>
<td style="text-align:right;">
0.0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
x
</td>
<td style="text-align:right;">
4.935301
</td>
<td style="text-align:right;">
1.0891667
</td>
<td style="text-align:right;">
4.531263
</td>
<td style="text-align:right;">
5.9e-06
</td>
</tr>
</tbody>
</table>

![](/study/img/[Categorical data analysis] Assignment 1/Q-2-1.jpg)

``` r
a11 <- sum(1/(2+1*x))
a12 <- sum(x/(2+1*x))
a21 <- sum(x/(2+1*x))
a22 <- sum(x^2/(2+1*x))

a <- matrix(c(a11,a12,a21,a22),2,by=T)

b1 <- sum(y/(2+1*x))
b2 <- sum((x*y)/(2+1*x))
b <- matrix(c(b1,b2))
solve(a)%*%b
```

    ##          [,1]
    ## [1,] 7.452381
    ## [2,] 4.928571

> 초기값으로 $$\beta_1$$과 $$\beta_2$$에 어떠한 값을 넣더라도 $$\beta_1 \approx 7$$, $$\beta_2 \approx 5$$되므로

> 초기값으로$$\beta_1^{(1)} \approx 7$$, $$\beta_2^{(1)} \approx 5$$

``` r
b1 <- 7
b2 <- 5

for( i in 2:5){
J <- matrix(c(sum(1/(b1+b2*x)),sum(x/(b1+b2*x)),sum(x/(b1+b2*x)),sum(x^2/(b1+b2*x))),2,by=T)
z <- matrix(c(sum(y/(b1+b2*x)),sum((x*y)/(b1+b2*x))))
b1b2 <- solve(J)%*%z
b1 <- b1b2[1,1]
b2 <- b1b2[2,1]
print(i)
print(b1b2)
}
```

    ## [1] 2
    ##          [,1]
    ## [1,] 7.451389
    ## [2,] 4.937500
    ## [1] 3
    ##          [,1]
    ## [1,] 7.451632
    ## [2,] 4.935314
    ## [1] 4
    ##          [,1]
    ## [1,] 7.451633
    ## [2,] 4.935300
    ## [1] 5
    ##          [,1]
    ## [1,] 7.451633
    ## [2,] 4.935300

> $$\hat\beta_1= 7.45163$$, $$\hat \beta_2 = 4.93530$$

## Q-2 2)

``` r
vcov(summary)%>%
  kbl(caption = "Variance-covariance matrix") %>%
  kable_material(c("striped", "hover"))
```

<table class=" lightable-material lightable-striped lightable-hover" style="font-family: &quot;Source Sans Pro&quot;, helvetica, sans-serif; margin-left: auto; margin-right: auto;">
<caption>
Variance-covariance matrix
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
(Intercept)
</th>
<th style="text-align:right;">
x
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
0.7816721
</td>
<td style="text-align:right;">
0.4165711
</td>
</tr>
<tr>
<td style="text-align:left;">
x
</td>
<td style="text-align:right;">
0.4165711
</td>
<td style="text-align:right;">
1.1862840
</td>
</tr>
</tbody>
</table>

![](/study/img/[Categorical data analysis] Assignment 1/Q-2-2.jpg)

``` r
solve(J)
```

    ##           [,1]      [,2]
    ## [1,] 0.7816754 0.4165551
    ## [2,] 0.4165551 1.1863043

## Q-2 3)

![](/study/img/[Categorical data analysis] Assignment 1/Q-2-3.jpg)

``` r
confint(poi_reg)[2,1:2]%>% 
  t()%>%
  kbl(caption = "95% CI based on Wald test") %>%
  kable_minimal() %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class=" lightable-minimal table table-striped table-hover table-condensed table-responsive" style="font-family: &quot;Trebuchet MS&quot;, verdana, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;">
<caption>
95% CI based on Wald test
</caption>
<thead>
<tr>
<th style="text-align:right;">
2.5 %
</th>
<th style="text-align:right;">
97.5 %
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
2.697133
</td>
<td style="text-align:right;">
7.050466
</td>
</tr>
</tbody>
</table>

# Q-3

![](/study/img/[Categorical data analysis] Assignment 1/Q-3.jpg)

## Q-3 1,2)

![](/study/img/[Categorical data analysis] Assignment 1/Q-3-1-2.jpg)

# Q-4

![](/study/img/[Categorical data analysis] Assignment 1/Q-4.jpg)

## Crabs dataset

``` r
Crabs <- read.table("https://users.stat.ufl.edu/~aa/glm/data/Crabs.dat", header = T)
head(Crabs) %>%
  kbl(caption = "Crabs dataset") %>%
  kable_material(c("striped", "hover"))
```

<table class=" lightable-material lightable-striped lightable-hover" style="font-family: &quot;Source Sans Pro&quot;, helvetica, sans-serif; margin-left: auto; margin-right: auto;">
<caption>
Crabs dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
crab
</th>
<th style="text-align:right;">
y
</th>
<th style="text-align:right;">
weight
</th>
<th style="text-align:right;">
width
</th>
<th style="text-align:right;">
color
</th>
<th style="text-align:right;">
spine
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
3.05
</td>
<td style="text-align:right;">
28.3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.55
</td>
<td style="text-align:right;">
22.5
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
2.30
</td>
<td style="text-align:right;">
26.0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2.10
</td>
<td style="text-align:right;">
24.8
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.60
</td>
<td style="text-align:right;">
26.0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2.10
</td>
<td style="text-align:right;">
23.8
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3
</td>
</tr>
</tbody>
</table>

> satellites가 y값이며 빈도(count) 데이터이기 때문에 Poisson loglinear
> model을 적합시켰다.

``` r
summ <- summary(glm(y ~ width, data=Crabs, family=poisson(link=log))) 
summ_m <- data.frame(summ$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","z value"="z.value","p value"="Pr...z..") %>% 
  kable(caption = "Summary of glm",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
summ_m
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of glm
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
z value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
-3.3047572
</td>
<td style="text-align:right;">
0.5422416
</td>
<td style="text-align:right;">
-6.094622
</td>
<td style="text-align:right;">
1.1e-09
</td>
</tr>
<tr>
<td style="text-align:left;">
width
</td>
<td style="text-align:right;">
0.1640451
</td>
<td style="text-align:right;">
0.0199653
</td>
<td style="text-align:right;">
8.216491
</td>
<td style="text-align:right;">
< 2e-16
</td>
</tr>
</tbody>
</table>

> width를 predictor로 두었을 때 유의확률이 매우 significant한 것으로
> 보여진다.

``` r
summ2 <- summary(glm(y ~ weight + width, data=Crabs, family=poisson(link=log)))
summ2_m <- data.frame(summ2$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","z value"="z.value","p value"="Pr...z..") %>% 
  kable(caption = "Summary of glm add weight as a predictor",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
summ2_m
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of glm add weight as a predictor
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
z value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
-1.2952111
</td>
<td style="text-align:right;">
0.8988960
</td>
<td style="text-align:right;">
-1.4408910
</td>
<td style="text-align:right;">
0.1496155
</td>
</tr>
<tr>
<td style="text-align:left;">
weight
</td>
<td style="text-align:right;">
0.4469701
</td>
<td style="text-align:right;">
0.1586207
</td>
<td style="text-align:right;">
2.8178547
</td>
<td style="text-align:right;">
0.0048346
</td>
</tr>
<tr>
<td style="text-align:left;">
width
</td>
<td style="text-align:right;">
0.0460765
</td>
<td style="text-align:right;">
0.0467497
</td>
<td style="text-align:right;">
0.9855991
</td>
<td style="text-align:right;">
0.3243299
</td>
</tr>
</tbody>
</table>

> predictor로 weight가 추가되었을 때 유의확률이 non significant하게
> 결과가 나왔으며, width의 SE가 두배정도 커지고 Estimate값은 줄어드는
> 결과를 일으킨다.

> 왜 이러한 결과를 일으키는지 두 변수사이의 연관성을 통해 알아보고자
> 한다.

``` r
cor(Crabs$weight, Crabs$width) %>% data.frame() %>% 
  rename("Correlation coefficient"=".")%>% 
  kable(caption = "Correlation between weight and width",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Correlation between weight and width
</caption>
<thead>
<tr>
<th style="text-align:right;">
Correlation coefficient
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.8868715
</td>
</tr>
</tbody>
</table>

> 두 변수 사이의 연관성이 0.8이상의 값이 출력된 것으로 보아 width와
> weight 사이의 Correlation이 높음으로인해 위와 같은 문제가 발생한
> 것으로 보인다.

> 빈도자료에 대해서 poisson regression은 굉장히 strict한 모형이다.

>조금 더 확장된 모형으로 negative binomial distribution를 가정한 모형인 two-parameter distribution를 적용하는 것이 적절해 보인다.

``` r
cor <- cor(Crabs$weight, Crabs$width)
vif <- 1/(1-cor^2)
vif
```

    ## [1] 4.68474

> non significant해진 이유는 Correlation이 높았으며

> $$ VIF= \frac{1}{1-(0.887)^2}=4.68474 $$

> VIF=4.68474의 결과가 출력된다.

## Houses selling price dataset

### 데이터셋 설명

``` r
Houses <- read.table("https://users.stat.ufl.edu/~aa/glm/data/Houses.dat", header = T)
head(Houses)%>%
  kbl(caption = "Houses dataset") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

> Houses dataset으로 집의 가격을 예측하는 모델을 만들어 보겠다.


<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Houses dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
case
</th>
<th style="text-align:right;">
taxes
</th>
<th style="text-align:right;">
beds
</th>
<th style="text-align:right;">
baths
</th>
<th style="text-align:right;">
new
</th>
<th style="text-align:right;">
price
</th>
<th style="text-align:right;">
size
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3104
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
279.9
</td>
<td style="text-align:right;">
2048
</td>
</tr>
<tr>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1173
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
146.5
</td>
<td style="text-align:right;">
912
</td>
</tr>
<tr>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3076
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
237.7
</td>
<td style="text-align:right;">
1654
</td>
</tr>
<tr>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1608
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
200.0
</td>
<td style="text-align:right;">
2068
</td>
</tr>
<tr>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1454
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
159.9
</td>
<td style="text-align:right;">
1477
</td>
</tr>
<tr>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2997
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
499.9
</td>
<td style="text-align:right;">
3153
</td>
</tr>
</tbody>
</table>

- taxes : 연간 세금 고지서 (in dollars)

- beds : 침실 수 (2,3,4,5)

- baths : 욕실 수 (1,2,3,4)

- new : 새 집 여부 (1= yes,0= no)

- price (response variable) : 판매 가격 (in thousands of dollars)

- size : 집의 크기 (in square feet)

``` r
cor <- cor(cbind(Houses$price,Houses$size,Houses$taxes,Houses$beds,Houses$baths)) # correlation matrix
cor %>%
  kbl(caption = "Correlation matrix of House selling price dataset") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Correlation matrix of House selling price dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
price
</th>
<th style="text-align:right;">
size
</th>
<th style="text-align:right;">
taxes
</th>
<th style="text-align:right;">
beds
</th>
<th style="text-align:right;">
baths
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.8337848
</td>
<td style="text-align:right;">
0.8419802
</td>
<td style="text-align:right;">
0.3939570
</td>
<td style="text-align:right;">
0.5582533
</td>
</tr>
<tr>
<td style="text-align:right;">
0.8337848
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.8187958
</td>
<td style="text-align:right;">
0.5447831
</td>
<td style="text-align:right;">
0.6582247
</td>
</tr>
<tr>
<td style="text-align:right;">
0.8419802
</td>
<td style="text-align:right;">
0.8187958
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.4739287
</td>
<td style="text-align:right;">
0.5948543
</td>
</tr>
<tr>
<td style="text-align:right;">
0.3939570
</td>
<td style="text-align:right;">
0.5447831
</td>
<td style="text-align:right;">
0.4739287
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.4922224
</td>
</tr>
<tr>
<td style="text-align:right;">
0.5582533
</td>
<td style="text-align:right;">
0.6582247
</td>
<td style="text-align:right;">
0.5948543
</td>
<td style="text-align:right;">
0.4922224
</td>
<td style="text-align:right;">
1.0000000
</td>
</tr>
</tbody>
</table>

> price와 taxes, size 간에 강한 양의 상관관계가 있음으로 보여진다.

> Scatterplot matrix로 확인하여 보자.

``` r
pairs(cbind(Houses$price,Houses$size,Houses$taxes)) # scatterplot matrix for pairs of var’s
```

![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-17-1.png)

### - **Backward Elimination with House Selling Price Data**

> 모델을 선택하기 위해 backward elimination 을 사용하여 변수선택을
> 하도록 한다.

> 교호작용도 고려해야 하기에 second-order interaction과 third-order
> interaction을 추가한 모형들을 각각 비교해보도록 한다.

> 먼저 main effect만 존재하는 모형과 2차 교호작용을 추가한 모형을
> 비교해보도록 한다.

``` r
fit1 <- lm(price ~ size + new + baths + beds, data=Houses)
fit2 <- lm(price ~ (size + new + baths + beds)^2, data= Houses)
fit3 <- lm(price ~ (size + new + baths + beds)^3, data=Houses)
anova(fit1, fit2) %>% kable(caption = "Result of F test",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Result of F test
</caption>
<thead>
<tr>
<th style="text-align:right;">
Res.Df
</th>
<th style="text-align:right;">
RSS
</th>
<th style="text-align:right;">
Df
</th>
<th style="text-align:right;">
Sum of Sq
</th>
<th style="text-align:right;">
F
</th>
<th style="text-align:right;">
Pr(>F)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
95
</td>
<td style="text-align:right;">
279624.1
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
89
</td>
<td style="text-align:right;">
217916.4
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
61707.71
</td>
<td style="text-align:right;">
4.200378
</td>
<td style="text-align:right;">
0.0009128
</td>
</tr>
</tbody>
</table>

> main effect만 존재하는 모형과 2차 교호작용을 추가한 모형을 비교해본
> 결과 p value가 0.05보다 작아 두 모형에 차이가 존재함을 알 수 있다.

> 3차 교호작용이 필요한지 알아보기 위해 2차 교호작용이 포함된 모형과 3차
> 교호작용이 포함된 모델을 비교해보도록 한다.

``` r
anova(fit2, fit3)%>% kable(caption = "Result of F test",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Result of F test
</caption>
<thead>
<tr>
<th style="text-align:right;">
Res.Df
</th>
<th style="text-align:right;">
RSS
</th>
<th style="text-align:right;">
Df
</th>
<th style="text-align:right;">
Sum of Sq
</th>
<th style="text-align:right;">
F
</th>
<th style="text-align:right;">
Pr(>F)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
89
</td>
<td style="text-align:right;">
217916.4
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
85
</td>
<td style="text-align:right;">
199305.6
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
18610.81
</td>
<td style="text-align:right;">
1.984288
</td>
<td style="text-align:right;">
0.1041848
</td>
</tr>
</tbody>
</table>

> 2차 교호작용을 추가한 모형과 3차 교호작용을 추가한 모형을 비교해본 결과 p-value=0.104로 유의수준 0.05보다 크므로 두 모형에 차이가
> 존재하지 않음을 알 수 있다.

> 그러므로 3차 교호작용은 고려하지 않겠다.

> 다음은 main effect 모델과 2차 교호작용을 추가한 모형, 3차 교호작용을 추가한
> 모형의 걸정계수와 수정된 결정계수를 보인 결과이다.

``` r
r <- c(summary(fit1)$r.squared,summary(fit2)$r.squared,summary(fit3)$r.squared)
ar <- c(summary(fit1)$adj.r.squared,summary(fit2)$adj.r.squared,summary(fit3)$adj.r.squared)
name <- c("fit1","fit2","fit3")
data.frame(name,r,ar)%>% 
  rename("R square"="r","Adjusted R square"="ar")%>% 
  kable(caption = "R square and Adjuted R square",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
R square and Adjuted R square
</caption>
<thead>
<tr>
<th style="text-align:left;">
name
</th>
<th style="text-align:right;">
R square
</th>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
fit1
</td>
<td style="text-align:right;">
0.7245489
</td>
<td style="text-align:right;">
0.7129509
</td>
</tr>
<tr>
<td style="text-align:left;">
fit2
</td>
<td style="text-align:right;">
0.7853357
</td>
<td style="text-align:right;">
0.7612161
</td>
</tr>
<tr>
<td style="text-align:left;">
fit3
</td>
<td style="text-align:right;">
0.8036688
</td>
<td style="text-align:right;">
0.7713319
</td>
</tr>
</tbody>
</table>

> 결정계수값을 확인하여 보면 당연하게도 변수가 더 많이 들어간 모형이 더 큰 값을 갖게 된다.

> 모델들의 파라미터의 수를 비교하기에 수정된 결정계수에 집중하도록
> 해보겠다.

> 다음은 2차 교호작용을 추가한 모형의 변수들의 유의성을 보여준 결과이다.

``` r
sum_fit2 <- summary(fit2)
sum_fit2_m <- data.frame(sum_fit2$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of fit2",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit2_m
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit2
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
139.0577253
</td>
<td style="text-align:right;">
66.6233971
</td>
<td style="text-align:right;">
2.0872206
</td>
<td style="text-align:right;">
0.0397274
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
-0.0019477
</td>
<td style="text-align:right;">
0.0561268
</td>
<td style="text-align:right;">
-0.0347020
</td>
<td style="text-align:right;">
0.9723951
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
103.3107619
</td>
<td style="text-align:right;">
106.4540998
</td>
<td style="text-align:right;">
0.9704724
</td>
<td style="text-align:right;">
0.3344412
</td>
</tr>
<tr>
<td style="text-align:left;">
baths
</td>
<td style="text-align:right;">
14.2610985
</td>
<td style="text-align:right;">
47.0325661
</td>
<td style="text-align:right;">
0.3032175
</td>
<td style="text-align:right;">
0.7624317
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-59.5421033
</td>
<td style="text-align:right;">
33.3010781
</td>
<td style="text-align:right;">
-1.7879933
</td>
<td style="text-align:right;">
0.0771804
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.1052939
</td>
<td style="text-align:right;">
0.0306909
</td>
<td style="text-align:right;">
3.4307820
</td>
<td style="text-align:right;">
0.0009145
</td>
</tr>
<tr>
<td style="text-align:left;">
size:baths
</td>
<td style="text-align:right;">
-0.0030832
</td>
<td style="text-align:right;">
0.0159029
</td>
<td style="text-align:right;">
-0.1938756
</td>
<td style="text-align:right;">
0.8467151
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0327667
</td>
<td style="text-align:right;">
0.0166142
</td>
<td style="text-align:right;">
1.9722095
</td>
<td style="text-align:right;">
0.0516904
</td>
</tr>
<tr>
<td style="text-align:left;">
new:baths
</td>
<td style="text-align:right;">
-104.3218174
</td>
<td style="text-align:right;">
51.8093215
</td>
<td style="text-align:right;">
-2.0135724
</td>
<td style="text-align:right;">
0.0470750
</td>
</tr>
<tr>
<td style="text-align:left;">
new:beds
</td>
<td style="text-align:right;">
-10.4471405
</td>
<td style="text-align:right;">
40.1690392
</td>
<td style="text-align:right;">
-0.2600794
</td>
<td style="text-align:right;">
0.7954032
</td>
</tr>
<tr>
<td style="text-align:left;">
baths:beds
</td>
<td style="text-align:right;">
0.8135120
</td>
<td style="text-align:right;">
17.5004355
</td>
<td style="text-align:right;">
0.0464852
</td>
<td style="text-align:right;">
0.9630276
</td>
</tr>
</tbody>
</table>

> 결과를 보아 baths:beds 교호작용이 유의하지 않음을 확인할 수 있다.

> 이를 제거하고 모델을 피팅시켜보면 결과는 다음과 같다.

``` r
fit4 <- lm(price ~ size + new + beds + baths + size*new + size*baths + size*beds + new*baths + new*beds, data=Houses)
sum_fit4 <- summary(fit4)
sum_fit4_m <- data.frame(sum_fit4$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of fit4",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit4_m;sum_fit4$adj.r.squared%>% 
  data.frame()%>% 
  rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>%  
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit4
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
137.3347651
</td>
<td style="text-align:right;">
55.0536680
</td>
<td style="text-align:right;">
2.4945616
</td>
<td style="text-align:right;">
0.0144363
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
-0.0040249
</td>
<td style="text-align:right;">
0.0337734
</td>
<td style="text-align:right;">
-0.1191744
</td>
<td style="text-align:right;">
0.9054028
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
103.9437238
</td>
<td style="text-align:right;">
104.9927628
</td>
<td style="text-align:right;">
0.9900085
</td>
<td style="text-align:right;">
0.3248242
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-58.5224497
</td>
<td style="text-align:right;">
24.9170037
</td>
<td style="text-align:right;">
-2.3486953
</td>
<td style="text-align:right;">
0.0210288
</td>
</tr>
<tr>
<td style="text-align:left;">
baths
</td>
<td style="text-align:right;">
16.1307475
</td>
<td style="text-align:right;">
24.2446284
</td>
<td style="text-align:right;">
0.6653328
</td>
<td style="text-align:right;">
0.5075391
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.1052308
</td>
<td style="text-align:right;">
0.0304904
</td>
<td style="text-align:right;">
3.4512732
</td>
<td style="text-align:right;">
0.0008517
</td>
</tr>
<tr>
<td style="text-align:left;">
size:baths
</td>
<td style="text-align:right;">
-0.0027713
</td>
<td style="text-align:right;">
0.0143378
</td>
<td style="text-align:right;">
-0.1932839
</td>
<td style="text-align:right;">
0.8471722
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0332043
</td>
<td style="text-align:right;">
0.0136143
</td>
<td style="text-align:right;">
2.4389345
</td>
<td style="text-align:right;">
0.0166933
</td>
</tr>
<tr>
<td style="text-align:left;">
new:baths
</td>
<td style="text-align:right;">
-104.4784463
</td>
<td style="text-align:right;">
51.4122410
</td>
<td style="text-align:right;">
-2.0321706
</td>
<td style="text-align:right;">
0.0450842
</td>
</tr>
<tr>
<td style="text-align:left;">
new:beds
</td>
<td style="text-align:right;">
-10.4855587
</td>
<td style="text-align:right;">
39.9372837
</td>
<td style="text-align:right;">
-0.2625506
</td>
<td style="text-align:right;">
0.7934970
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7638635
</td>
</tr>
</tbody>
</table>

> 모델 피팅 결과 수정된 결정계수는 0.764이고 size:baths 교호작용이
> 유의하지 않아 이를 제거하고 다시 결과를 보기로 한다.

``` r
fit5 <- lm(price ~ size + new + beds + baths + size*new + size*beds + new*baths + new*beds, data=Houses)
sum_fit5 <- summary(fit5)
sum_fit5_m <- data.frame(sum_fit5$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..")%>% 
  kable(caption = "Summary of fit5",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit5_m
sum_fit5$adj.r.squared%>% 
  data.frame()%>%
  rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>% 
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit5
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
136.8890773
</td>
<td style="text-align:right;">
54.7136472
</td>
<td style="text-align:right;">
2.5019183
</td>
<td style="text-align:right;">
0.0141390
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
-0.0049199
</td>
<td style="text-align:right;">
0.0332771
</td>
<td style="text-align:right;">
-0.1478456
</td>
<td style="text-align:right;">
0.8827917
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
107.0270663
</td>
<td style="text-align:right;">
103.2234538
</td>
<td style="text-align:right;">
1.0368483
</td>
<td style="text-align:right;">
0.3025539
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-55.1543840
</td>
<td style="text-align:right;">
17.7159218
</td>
<td style="text-align:right;">
-3.1132664
</td>
<td style="text-align:right;">
0.0024734
</td>
</tr>
<tr>
<td style="text-align:left;">
baths
</td>
<td style="text-align:right;">
12.0963336
</td>
<td style="text-align:right;">
12.2682844
</td>
<td style="text-align:right;">
0.9859841
</td>
<td style="text-align:right;">
0.3267552
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.1060439
</td>
<td style="text-align:right;">
0.0300386
</td>
<td style="text-align:right;">
3.5302531
</td>
<td style="text-align:right;">
0.0006536
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0312882
</td>
<td style="text-align:right;">
0.0092818
</td>
<td style="text-align:right;">
3.3709137
</td>
<td style="text-align:right;">
0.0011010
</td>
</tr>
<tr>
<td style="text-align:left;">
new:baths
</td>
<td style="text-align:right;">
-107.9387245
</td>
<td style="text-align:right;">
47.9389790
</td>
<td style="text-align:right;">
-2.2515858
</td>
<td style="text-align:right;">
0.0267551
</td>
</tr>
<tr>
<td style="text-align:left;">
new:beds
</td>
<td style="text-align:right;">
-9.4895123
</td>
<td style="text-align:right;">
39.3933841
</td>
<td style="text-align:right;">
-0.2408910
</td>
<td style="text-align:right;">
0.8101815
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7663615
</td>
</tr>
</tbody>
</table>

> 모델 피팅 결과 수정된 결정계수는 0.766이고 new:beds 교호작용이
> 유의하지 않아 이를 제거하고 다시 결과를 보기로 한다.

``` r
fit6 <- lm(price ~ size + new + baths + beds + size*new+size*beds+new*baths, data=Houses)
sum_fit6 <- summary(fit6)
sum_fit6_m <- data.frame(sum_fit6$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of fit6",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit6_m;sum_fit6$adj.r.squared%>% data.frame() %>% 
  rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>%  
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit6
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
135.6459351
</td>
<td style="text-align:right;">
54.1901588
</td>
<td style="text-align:right;">
2.5031470
</td>
<td style="text-align:right;">
0.0140733
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
-0.0031778
</td>
<td style="text-align:right;">
0.0323150
</td>
<td style="text-align:right;">
-0.0983366
</td>
<td style="text-align:right;">
0.9218789
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
90.7242029
</td>
<td style="text-align:right;">
77.5413408
</td>
<td style="text-align:right;">
1.1700108
</td>
<td style="text-align:right;">
0.2450187
</td>
</tr>
<tr>
<td style="text-align:left;">
baths
</td>
<td style="text-align:right;">
12.2813031
</td>
<td style="text-align:right;">
12.1813868
</td>
<td style="text-align:right;">
1.0082024
</td>
<td style="text-align:right;">
0.3160017
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-55.0541117
</td>
<td style="text-align:right;">
17.6201276
</td>
<td style="text-align:right;">
-3.1245013
</td>
<td style="text-align:right;">
0.0023826
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.1039833
</td>
<td style="text-align:right;">
0.0286470
</td>
<td style="text-align:right;">
3.6298074
</td>
<td style="text-align:right;">
0.0004659
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0308547
</td>
<td style="text-align:right;">
0.0090589
</td>
<td style="text-align:right;">
3.4059907
</td>
<td style="text-align:right;">
0.0009790
</td>
</tr>
<tr>
<td style="text-align:left;">
new:baths
</td>
<td style="text-align:right;">
-111.5443866
</td>
<td style="text-align:right;">
45.3085824
</td>
<td style="text-align:right;">
-2.4618821
</td>
<td style="text-align:right;">
0.0156842
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7687537
</td>
</tr>
</tbody>
</table>

> 모델 피팅 결과 수정된 결정계수는 0.769로 조금 높아졌으며, interaction terms는 모두 유의하게 나왔다.

> new:baths를 제거한 모델을 fit7로 두고 결과를 확인하여 보면

``` r
fit7 <- lm(price ~ size + new + baths + beds + size*new+size*beds, data=Houses)
sum_fit7 <- summary(fit7)
sum_fit7_m <- data.frame(sum_fit7$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of fit7",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit7_m;sum_fit7$adj.r.squared%>% data.frame() %>% rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>%  
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit7
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
138.8243136
</td>
<td style="text-align:right;">
55.6292903
</td>
<td style="text-align:right;">
2.4955255
</td>
<td style="text-align:right;">
0.0143395
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.0054143
</td>
<td style="text-align:right;">
0.0329886
</td>
<td style="text-align:right;">
0.1641276
</td>
<td style="text-align:right;">
0.8699869
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-58.3989591
</td>
<td style="text-align:right;">
49.7104136
</td>
<td style="text-align:right;">
-1.1747832
</td>
<td style="text-align:right;">
0.2430807
</td>
</tr>
<tr>
<td style="text-align:left;">
baths
</td>
<td style="text-align:right;">
4.8119196
</td>
<td style="text-align:right;">
12.1142428
</td>
<td style="text-align:right;">
0.3972118
</td>
<td style="text-align:right;">
0.6921215
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-53.9900315
</td>
<td style="text-align:right;">
18.0877576
</td>
<td style="text-align:right;">
-2.9848936
</td>
<td style="text-align:right;">
0.0036245
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0550254
</td>
<td style="text-align:right;">
0.0211737
</td>
<td style="text-align:right;">
2.5987665
</td>
<td style="text-align:right;">
0.0108796
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0297523
</td>
<td style="text-align:right;">
0.0092908
</td>
<td style="text-align:right;">
3.2023436
</td>
<td style="text-align:right;">
0.0018666
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7561697
</td>
</tr>
</tbody>
</table>

> 수정된 결정계수가 0.756으로 조금 줄어들었다. 

> 따라서 효과가 그렇게 크진 않은 것을 볼 수 있다.

> interaction terms는 유의하지만 main effect size, new, baths가 유의하지 않으므로 우선 baths 변수를 제거하여보겠다.

> <span style="color: #2D3748; background-color:#fff5b1;"> main effect가 제거되었는데 interaction term이 존재할 수는 없다. </span>

``` r
fit8 <- update(fit7, .~. - baths, data=Houses)
sum_fit8 <- summary(fit8)
sum_fit8_m <- data.frame(sum_fit8$coefficients)%>%
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..")%>% 
  kable(caption = "Summary of fit8",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit8_m

sum_fit8$adj.r.squared%>% 
  data.frame()%>% 
  rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>% 
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit8
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
143.4709850
</td>
<td style="text-align:right;">
54.1411901
</td>
<td style="text-align:right;">
2.6499415
</td>
<td style="text-align:right;">
0.0094457
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.0068404
</td>
<td style="text-align:right;">
0.0326454
</td>
<td style="text-align:right;">
0.2095377
</td>
<td style="text-align:right;">
0.8344820
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-56.6857817
</td>
<td style="text-align:right;">
49.3005986
</td>
<td style="text-align:right;">
-1.1497991
</td>
<td style="text-align:right;">
0.2531436
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-53.6373359
</td>
<td style="text-align:right;">
17.9848343
</td>
<td style="text-align:right;">
-2.9823648
</td>
<td style="text-align:right;">
0.0036429
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0544088
</td>
<td style="text-align:right;">
0.0210219
</td>
<td style="text-align:right;">
2.5881984
</td>
<td style="text-align:right;">
0.0111785
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0300206
</td>
<td style="text-align:right;">
0.0092246
</td>
<td style="text-align:right;">
3.2544107
</td>
<td style="text-align:right;">
0.0015798
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7583544
</td>
</tr>
</tbody>
</table>

> fit8 결과, interaction terms는 모두 유의한 결과를 보이지만 main effect인 size와 new는 유의하지 않게 나왔다.

> interaction terms를 모두 제거하고 size와 new 변수만으로 모델을 만들어 수정된 결정계수값을 확인하여 보면 0.7168767의 값으로 줄어든다.

> 우선 fit8모형을 provisional model로 설정하여 해석하여 보겠다.

> 100 square foot 증가함에 따라서 집의 예상 판매 가격은 <br/>
older two-bedroom home의 경우 100[0.00684 + 0 + 2(0.03002)], 약 $6,688 증가한다.<br/>
older three-bedroom home의 경우 100[0.00684 + 0 +3(0.03002)], 약  $9,690 증가한다.<br/>
older four-bedroom home의 경우 100[0.00684 + 0 + 4(0.03002)], 약  $12,692 증가한다.<br/>
new home의 경우, 이 세 가지 값에 각각 +$5441달러가 된다.

> bedrooms 수에 따라 조정된 new 집의 예상 판매 가격에 대한 효과는 <br/>
−56.686+ 1000(0.0544),  약 −$2277, for a 1000-square-foot home,<br/>
−56.686+ 2000(0.0544), 약 $52,132, for a 2000- square-foot home,<br/>
−56.686+ 3000(0.0544), 약 $106,541 for a 3000-square- foot home.

> new 집에 따라 조정된 추가 bedrooms의 예상 판매 가격에 대한 효과는<br/>
−53.637+ 1000(0.0300),or−$23,616, for a 1000-square- foot home,<br/>
−53.637+ 2000(0.0300), or $6405, for a 2000-square-foot home,<br/>
−53.637+ 3000(0.0300), or$36,426, for a3000-square-foot home.

> 이를 시각적으로 표현하기 위해 QQ plot과 fitted value와 표준화 잔차의
> 그림 및 잔차플롯을 보이면 다음과 같다.

``` r
plot(fit8)
```

![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-27-1.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-27-2.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-27-3.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-27-4.png)

> fit8에서 beds변수를 제거한 모형을 fit9라고 하겠다. 

``` r
fit9 <- lm(formula = price ~ size + new + size:new, data=Houses)
sum_fit9 <- summary(fit9)
sum_fit9_m <- data.frame(sum_fit9$coefficients)%>%
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..")%>%
  kable(caption = "Summary of fit9",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_fit9_m

sum_fit9$adj.r.squared%>% 
  data.frame() %>% 
  rename("Adjusted R square"=".")%>% 
  kable(caption = "Adjusted R square",booktabs = TRUE, valign = 't')%>%  
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of fit9
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
-22.2278079
</td>
<td style="text-align:right;">
15.5211100
</td>
<td style="text-align:right;">
-1.432102
</td>
<td style="text-align:right;">
0.1553627
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.1044384
</td>
<td style="text-align:right;">
0.0094241
</td>
<td style="text-align:right;">
11.082080
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-78.5275023
</td>
<td style="text-align:right;">
51.0076419
</td>
<td style="text-align:right;">
-1.539524
</td>
<td style="text-align:right;">
0.1269661
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0619159
</td>
<td style="text-align:right;">
0.0216857
</td>
<td style="text-align:right;">
2.855149
</td>
<td style="text-align:right;">
0.0052716
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Adjusted R square
</caption>
<thead>
<tr>
<th style="text-align:right;">
Adjusted R square
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
0.7363181
</td>
</tr>
</tbody>
</table>


> adjusted R²=0.736으로 조금 작아진다.

> fit9는 모형이 단순화 되므로 해석하기에 편리해진다.

> $$ \hat{\mu} \ = \ −22.228 \ + \ 0.1044(size) \ −78.5275(new) \ + \ 0.0619(size∗new) $$

> 다음은 이전에 확인하여 본 fit1~fit9 중 BIC가 fit8에서 가장 작은지 코드를 작성해서 확인하여 보았다.

>fit1 <- lm(price ~ size + new + baths + beds) <br/>
fit2 <- lm(price ~ (size + new + baths + beds)^2)<br/>
fit3 <- lm(price ~ (size + new + baths + beds)^3)<br/>
fit4 <- lm(price ~ size + new + beds + baths + size:new + size:baths + size:beds + new:baths + new:beds)<br/>
fit5 <- lm(price ~ size + new + beds + baths + size:new + size:beds + new:baths + new:beds)<br/>
fit6 <- lm(price ~ size + new + baths + beds + size:new + size:beds + new:baths)<br/>
fit7 <- lm(price ~ size + new + baths + beds + size:new + size:beds)<br/>
fit8 <- lm(price ~ size + new + beds + size:new + size:beds)<br/>
fit9 <- lm(price ~ size + new + size:new)<br/>

``` r
bic <- c()
for (i in 1:9){
  bic <-c(bic,BIC(eval(parse(text=paste0("fit",c(1:9))[i]))))
}
data.frame(bic)%>%
  rename("BIC"="bic")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
BIC
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1105.022
</td>
</tr>
<tr>
<td style="text-align:right;">
1107.719
</td>
</tr>
<tr>
<td style="text-align:right;">
1117.213
</td>
</tr>
<tr>
<td style="text-align:right;">
1103.117
</td>
</tr>
<tr>
<td style="text-align:right;">
1098.553
</td>
</tr>
<tr>
<td style="text-align:right;">
1094.012
</td>
</tr>
<tr>
<td style="text-align:right;">
1095.786
</td>
</tr>
<tr>
<td style="text-align:right;">
1091.351
</td>
</tr>
<tr>
<td style="text-align:right;">
1092.973
</td>
</tr>
</tbody>
</table>

``` r
which.min(bic)
```

    ## [1] 8

``` r
min(bic)
```

    ## [1] 1091.351

> 위의 과정을 step() 함수를 사용하여 Backward Elimination을 계산할 수 있다.

> 여기서 중요한 것은 <span style="color: red"> step() 함수에서 출력되는 AIC 값은 TRUE 값이 아니다. <br/>
(상수값이 제대로 곱해져 있는 값이 아니다.)</span>

> <span style='background-color:#ffdce0'> R에서 AIC 함수를 사용하면 정확한 값을 준다. </span>

``` r
step(lm(price ~ (size + new + beds + baths)^2, data=Houses))
```

    ## Start:  AIC=790.67
    ## price ~ (size + new + beds + baths)^2
    ## 
    ##              Df Sum of Sq    RSS    AIC
    ## - beds:baths  1       5.3 217922 788.67
    ## - size:baths  1      92.0 218008 788.71
    ## - new:beds    1     165.6 218082 788.75
    ## <none>                    217916 790.67
    ## - size:beds   1    9523.7 227440 792.95
    ## - new:baths   1    9927.4 227844 793.12
    ## - size:new    1   28819.5 246736 801.09
    ## 
    ## Step:  AIC=788.67
    ## price ~ size + new + beds + baths + size:new + size:beds + size:baths + 
    ##     new:beds + new:baths
    ## 
    ##              Df Sum of Sq    RSS    AIC
    ## - size:baths  1      90.5 218012 786.71
    ## - new:beds    1     166.9 218089 786.75
    ## <none>                    217922 788.67
    ## - new:baths   1    9999.5 227921 791.16
    ## - size:beds   1   14403.2 232325 793.07
    ## - size:new    1   28841.4 246763 799.10
    ## 
    ## Step:  AIC=786.71
    ## price ~ size + new + beds + baths + size:new + size:beds + new:beds + 
    ##     new:baths
    ## 
    ##             Df Sum of Sq    RSS    AIC
    ## - new:beds   1       139 218151 784.78
    ## <none>                   218012 786.71
    ## - new:baths  1     12146 230158 790.13
    ## - size:beds  1     27223 245235 796.48
    ## - size:new   1     29857 247869 797.55
    ## 
    ## Step:  AIC=784.78
    ## price ~ size + new + beds + baths + size:new + size:beds + new:baths
    ## 
    ##             Df Sum of Sq    RSS    AIC
    ## <none>                   218151 784.78
    ## - new:baths  1     14372 232523 789.16
    ## - size:beds  1     27508 245659 794.65
    ## - size:new   1     31242 249393 796.16

    ## 
    ## Call:
    ## lm(formula = price ~ size + new + beds + baths + size:new + size:beds + 
    ##     new:baths, data = Houses)
    ## 
    ## Coefficients:
    ## (Intercept)         size          new         beds        baths     size:new  
    ##   1.356e+02   -3.178e-03    9.072e+01   -5.505e+01    1.228e+01    1.040e-01  
    ##   size:beds    new:baths  
    ##   3.085e-02   -1.115e+02

> 결과를 보면 fit6 에서 AIC 값이 가장 작은 값을 가지는 것을 확인할 수 있으며,

> BIC는 fit 8에서 가장 작은 값을 준다.

> <span style="color: red">위에서 주어진 AIC=784.78은 TRUE값이 아니다.</span>

``` r
aic <- AIC(lm(price ~ size+new+beds+baths+size:new+size:beds+new:baths, data=Houses))
bic <- BIC(lm(price ~ size + new + beds + size:new + size:beds, data=Houses)) # this is model with lowest BIC     
data.frame(aic,bic)%>%
  rename("AIC"="aic","BIC"="bic")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
AIC
</th>
<th style="text-align:right;">
BIC<br/>
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1070.565
</td>
<td style="text-align:right;">
1091.351
</td>
</tr>
</tbody>
</table>

> AIC(Akaike information criterion) 및 BIC(Baysian information criterion) 값을 구하여 모델이 얼마나 잘 적합이 되었는지 판단할 수 있다.

> AIC 여러개 비교해서 가장 작은 값을 주는 모형 선택, <br/> AIC는 복잡한 모형을 주는 경향이 있고 (AIC tends to produce ovefit models.)

> BIC는 덜 복잡한 모형을 주는 경향이 있다. (BIC tends to produce underfit models.)

> 집 값은 엄밀하게 말하면 대부분 positive 값을 가진다. <br/> 정규분포 가정한 일반선형모형은 적절하지 않을 수 있다.

``` r
ggplot(data = Houses, aes(x = price))+
  geom_histogram(aes(y=..density..),binwidth=10,color="#ffffb3", fill="#8dd3c7")+
  geom_density(alpha=0.3, color="#ffffb3", fill="#CED46A")
```

![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-3-1.png)


> 따라서 Gamma GLM을 적합시켜 보도록 하겠다.

### - **Gamma GLMs for House Selling Pride Data**

$$ \begin{align} f(y:k,\mu)=\frac{y^{k-1}}{\Gamma(k)(\frac{\mu}{k})^k}e^{-\frac{y}{\mu/k}}, \ y \geq 0, \ k>0 \\ for \ which \ E(y)= \mu, \ var(y) = \mu^2/k \end{align} $$

> 기존에 알던 감마분포와는 형태가 다르게 scale parameter가 $$E(y)= \mu$$되도록 reparametrization해주었다.

> Gamma GLMs usually assume k to be constant but unknown

> 관심있는 모수는 k가 아닌 $$\mu$$에 있기 때문에 k가 알려져 있지는 않지만 nuisance parameter로 보고 k는 constant라고 하겠다.

> 지수족의 형태는 다음과 같다.

$$f(y_i:\theta_i,\phi)=exp(\frac{y_i\theta_i-b(\theta_i)}{a(\phi)}+c(y_i,\phi))$$

> 위에 정의한 감마분포가 다음의 식으로 지수족을 따르는 것을 볼 수 있다.

$$f(y:k,\mu)=exp \bigg\{ \frac{-\frac{y}{\mu}-\big(-log(-\theta)\big)}{1/k}+c(y_i,\phi) \bigg\},  \ \theta=-\frac{1}{\mu}, \ b(\theta)=-log(-\theta), \ a(\phi)=\frac{1}{k}$$

> 감마분포를 가정한 상태에서 glm 모형을 적합시켜보도록 한다.

``` r
fit.gamma <- glm(price ~ size + new + beds + size:new + size:beds, family = Gamma(link = identity), data=Houses)
sum_g <- summary(fit.gamma)
sum_g_m<- data.frame(sum_g$coef)%>%
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
sum_g_m
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
44.3758986
</td>
<td style="text-align:right;">
48.5978445
</td>
<td style="text-align:right;">
0.9131248
</td>
<td style="text-align:right;">
0.3635130
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.0739797
</td>
<td style="text-align:right;">
0.0399989
</td>
<td style="text-align:right;">
1.8495416
</td>
<td style="text-align:right;">
0.0675219
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-60.0290457
</td>
<td style="text-align:right;">
65.7655129
</td>
<td style="text-align:right;">
-0.9127739
</td>
<td style="text-align:right;">
0.3636966
</td>
</tr>
<tr>
<td style="text-align:left;">
beds
</td>
<td style="text-align:right;">
-22.7131117
</td>
<td style="text-align:right;">
17.6312435
</td>
<td style="text-align:right;">
-1.2882308
</td>
<td style="text-align:right;">
0.2008276
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0538329
</td>
<td style="text-align:right;">
0.0375809
</td>
<td style="text-align:right;">
1.4324543
</td>
<td style="text-align:right;">
0.1553310
</td>
</tr>
<tr>
<td style="text-align:left;">
size:beds
</td>
<td style="text-align:right;">
0.0100000
</td>
<td style="text-align:right;">
0.0125590
</td>
<td style="text-align:right;">
0.7962412
</td>
<td style="text-align:right;">
0.4278982
</td>
</tr>
</tbody>
</table>

> 이전에 fit8을 피팅시킨 것과 동일한 조건을 주었으나 이전에는 interaction terms이 유의하였지만 Gamma GLM에서는 유의하지 않은 결과를 보여준다.

> 2차 교호작용이 추가된 모델과 원모델을 비교하여 차이가 있는지 검정해보면 결과는 다음과 같다.

``` r
fit.g1 <- glm(price ~ size + new + baths + beds, family=Gamma(link=identity),data=Houses)
fit.g2 <- glm(price~(size + new + baths + beds)^2,family=Gamma(link=identity),data=Houses)     
anova(fit.g1, fit.g2, test="F") %>% 
  kable(caption = "Result of F test",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Result of F test
</caption>
<thead>
<tr>
<th style="text-align:right;">
Resid. Df
</th>
<th style="text-align:right;">
Resid. Dev
</th>
<th style="text-align:right;">
Df
</th>
<th style="text-align:right;">
Deviance
</th>
<th style="text-align:right;">
F
</th>
<th style="text-align:right;">
Pr(>F)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
95
</td>
<td style="text-align:right;">
10.441719
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
89
</td>
<td style="text-align:right;">
9.872775
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
0.568944
</td>
<td style="text-align:right;">
0.8437596
</td>
<td style="text-align:right;">
0.5395703
</td>
</tr>
</tbody>
</table>

> 검정 결과 p-value=0.539로 유의수준 0.05보다 크므로 유의하지 않다.<br/> 그러므로 교호작용은 필요없다고 할 수 있다.

> size와 new의 교호작용만 넣었을 때의 모델을 Gamma glm 모형으로 적합했을
> 때와 일반 glm모형으로 적합한 경우의 결과를 비교해보면 다음과 같다.

``` r
fit_gam1 <- summary(glm(price ~ size + new + size:new, family=Gamma(link=identity), data=Houses))
fit_gam1_m <- data.frame(fit_gam1$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of Gamma glm",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))

fit_lm1 <- summary(lm(price ~ size + new + size:new,dat=Houses))
fit_lm1_m <- data.frame(fit_lm2$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary of normal linear glm",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))

fit_gam1_m; fit_lm1_m
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of Gamma glm
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
-7.4521938
</td>
<td style="text-align:right;">
12.9738404
</td>
<td style="text-align:right;">
-0.5744015
</td>
<td style="text-align:right;">
0.5670397
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.0944569
</td>
<td style="text-align:right;">
0.0100525
</td>
<td style="text-align:right;">
9.3963533
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-77.9033275
</td>
<td style="text-align:right;">
64.5827183
</td>
<td style="text-align:right;">
-1.2062566
</td>
<td style="text-align:right;">
0.2306829
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0649207
</td>
<td style="text-align:right;">
0.0367047
</td>
<td style="text-align:right;">
1.7687290
</td>
<td style="text-align:right;">
0.0801153
</td>
</tr>
</tbody>
</table>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary of normal linear glm
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
-22.2278079
</td>
<td style="text-align:right;">
15.5211100
</td>
<td style="text-align:right;">
-1.432102
</td>
<td style="text-align:right;">
0.1553627
</td>
</tr>
<tr>
<td style="text-align:left;">
size
</td>
<td style="text-align:right;">
0.1044384
</td>
<td style="text-align:right;">
0.0094241
</td>
<td style="text-align:right;">
11.082080
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
new
</td>
<td style="text-align:right;">
-78.5275023
</td>
<td style="text-align:right;">
51.0076419
</td>
<td style="text-align:right;">
-1.539524
</td>
<td style="text-align:right;">
0.1269661
</td>
</tr>
<tr>
<td style="text-align:left;">
size:new
</td>
<td style="text-align:right;">
0.0619159
</td>
<td style="text-align:right;">
0.0216857
</td>
<td style="text-align:right;">
2.855149
</td>
<td style="text-align:right;">
0.0052716
</td>
</tr>
</tbody>
</table>

``` r
fit_gam1_AIC <- AIC(glm(price ~ size + new + size:new, family=Gamma(link=identity), data=Houses))
fit_lm1_AIC <- AIC(lm(price ~ size + new + size:new,dat=Houses))

data.frame(fit_gam1_AIC,fit_lm1_AIC)%>%
  rename("AIC of Gamma glm"="fit_gam1_AIC","AIC of Normal linear glm"="fit_lm1_AIC")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
AIC of Gamma glm
</th>
<th style="text-align:right;">
AIC of Normal linear glm
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1047.935
</td>
<td style="text-align:right;">
1079.947
</td>
</tr>
</tbody>
</table>

> 감마분포를 가정한 모델 적합 결과를 시각적으로 보기위한 그림들은 다음과 같다.

``` r
plot(glm(price ~ size + new + size:new, family=Gamma(link=identity), data=Houses))
```

![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-37-1.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-37-2.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-37-3.png)
![](/study/img/[Categorical data analysis] Assignment 1/unnamed-chunk-37-4.png)

> 위의 결과를 보아 효과들은 전체적으로 비슷하지만 감마분포를 가정했을 때
> interaction term에서 더 큰 SE 값을 가지고 있는다.

> $$\phi$$가 추정되어야 k를 추정할 수 있으며 또한 분산을 추정할수 있기 때문에 $$\phi$$를 추정해보도록 한다.

> sas프로그램 같은 경우 dispersion parameter $$\phi$$를 MLE 방법으로 추정하는데 MLE를 사용하면 문제가 있을 수 있다.

> variance function이 정확하지 않으면 MLE값이 inconsistent하기 때문이다.

> (the ML estimator is inconsistent if the variance function is correct but the distribution is not truly the assumed one (McCullagh and Nelder 1989, p. 295))

> R 프로그램 같은 경우 $$\phi$$를 Pearson staistic을 이용해서 추정한다.

$$\hat{\phi}=\frac{X^2}{(n-p)}=\frac{1}{n-p} \sum^n_{i=1}\frac{(y_i-\hat{\mu_i})^2}{\hat{\mu_i}^2}$$

``` r
fit_gam1 <- summary(glm(price ~ size+new+size:new, family=Gamma(link=identity), data=Houses))
fit_gam1$dispersion
```

    ## [1] 0.1102068

> 이 감마 모델에서 dispersion parameter
> $$ \hatϕ=0.11021 $$
> 이며 그렇게 추정된 shape parameter
> $$ k=\frac{1}{\hat{ϕ}}=9.07 $$이다.

> 지수족에서 분산구하는 식은 다음과 같다.

$$var(y_i)=b^{''}(\theta_i)a(\phi)$$

> 위 식으로 추정된 standard deviation은 다음과 같이 구해진다.

$$ \hat \sigma  = \sqrt{\hatϕ}\hat\mu = 0.33197 \hat \mu  $$

> 식을 통해 $$\mu$$가 커짐에 따라 $$\sigma$$도 커지는 관계를 볼 수 있다.

> 따라서 $$E(y_i)$$에만 초점을 두는 것이 아닌 $$var(y_i)$$가 어떻게 변화되는지도 관심을 가져야 한다. 

``` r
aic1 <- AIC(lm(price ~ size + new + size:new, data=Houses))
aic2 <- AIC(lm(price ~ size +new +beds +baths +size:new +size:beds +new:baths, data=Houses))
aic3 <- AIC(glm(price ~ size + new + size:new,family=Gamma(link=identity), data=Houses))  

data.frame(aic1,aic2,aic3)%>% rename("size+new+size:new"="aic1","size +new +beds +baths +size:new +size:beds +new:baths"="aic2","size+new+size:new (Gamma)"="aic3")%>% 
  kable(caption = "AIC",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
AIC
</caption>
<thead>
<tr>
<th style="text-align:right;">
size+new+size:new
</th>
<th style="text-align:right;">
size +new +beds +baths +size:new +size:beds +new:baths
</th>
<th style="text-align:right;">
size+new+size:new (Gamma)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1079.947
</td>
<td style="text-align:right;">
1070.565
</td>
<td style="text-align:right;">
1047.935
</td>
</tr>
</tbody>
</table>

> AIC를 구한 결과 Gamma glm을 적합시켰을 때 모델의 AIC 값이 1047.9로써 앞서 구한 값들보다 작다는 것을 알 수 있다.

> link function을 log link를 사용한 결과도 한 번 확인하여 보자.

``` r
fit1 <- glm(price ~ size + new + baths + beds, family=Gamma(link=log), data=Houses)
fit2 <- glm(price ~ (size + new + baths + beds)^2, family=Gamma(link=log), data= Houses)
fit3 <- glm(price ~ (size + new + baths + beds)^3, family=Gamma(link=log), data=Houses)
fit4 <- glm(price ~ size + new + beds + baths + size*new + size*baths + size*beds + new*baths + new*beds, family=Gamma(link=log), data=Houses)
fit5 <- glm(price ~ size + new + beds + baths + size*new + size*beds + new*baths + new*beds, family=Gamma(link=log), data=Houses)
fit6 <- glm(price ~ size + new + beds + baths + size*new + size*beds + new*baths, family=Gamma(link=log), data=Houses)
fit7 <- glm(price ~ size + new + beds + baths + size*new + size*beds, family=Gamma(link=log), data=Houses)
fit8 <- update(fit7, .~. - baths, family=Gamma(link=log), data=Houses)
fit9 <- glm(formula = price ~ size + new + size:new, family=Gamma(link=log), data=Houses)

step(glm(price ~ (size + new + baths + beds)^2, family=Gamma(link=log), data= Houses), data=Houses)
```

    ## Start:  AIC=1059.01
    ## price ~ (size + new + baths + beds)^2
    ## 
    ##              Df Deviance    AIC
    ## - new:beds    1   10.268 1057.0
    ## - new:baths   1   10.270 1057.1
    ## - size:beds   1   10.289 1057.2
    ## - size:new    1   10.291 1057.3
    ## - baths:beds  1   10.354 1057.8
    ## <none>            10.264 1059.0
    ## - size:baths  1   10.691 1060.8
    ## 
    ## Step:  AIC=1057.06
    ## price ~ size + new + baths + beds + size:new + size:baths + size:beds + 
    ##     new:baths + baths:beds
    ## 
    ##              Df Deviance    AIC
    ## - new:baths   1   10.280 1055.2
    ## - size:beds   1   10.290 1055.2
    ## - size:new    1   10.291 1055.3
    ## - baths:beds  1   10.358 1055.9
    ## <none>            10.268 1057.1
    ## - size:baths  1   10.693 1058.9
    ## 
    ## Step:  AIC=1055.18
    ## price ~ size + new + baths + beds + size:new + size:baths + size:beds + 
    ##     baths:beds
    ## 
    ##              Df Deviance    AIC
    ## - size:new    1   10.291 1053.3
    ## - size:beds   1   10.308 1053.4
    ## - baths:beds  1   10.376 1054.0
    ## <none>            10.280 1055.2
    ## - size:baths  1   10.808 1058.0
    ## 
    ## Step:  AIC=1053.29
    ## price ~ size + new + baths + beds + size:baths + size:beds + 
    ##     baths:beds
    ## 
    ##              Df Deviance    AIC
    ## - size:beds   1   10.321 1051.6
    ## - baths:beds  1   10.379 1052.1
    ## <none>            10.291 1053.3
    ## - new         1   10.634 1054.5
    ## - size:baths  1   10.808 1056.0
    ## 
    ## Step:  AIC=1051.58
    ## price ~ size + new + baths + beds + size:baths + baths:beds
    ## 
    ##              Df Deviance    AIC
    ## <none>            10.321 1051.6
    ## - baths:beds  1   10.550 1051.7
    ## - new         1   10.644 1052.6
    ## - size:baths  1   10.811 1054.2

    ## 
    ## Call:  glm(formula = price ~ size + new + baths + beds + size:baths + 
    ##     baths:beds, family = Gamma(link = log), data = Houses)
    ## 
    ## Coefficients:
    ## (Intercept)         size          new        baths         beds   size:baths  
    ##   4.0413287    0.0010923    0.2023428    0.0088916   -0.3545010   -0.0002175  
    ##  baths:beds  
    ##   0.1465839  
    ## 
    ## Degrees of Freedom: 99 Total (i.e. Null);  93 Residual
    ## Null Deviance:       31.94 
    ## Residual Deviance: 10.32     AIC: 1052

> step() 함수 결과 <br/>
glm(price ~ size + new + baths + beds + size:baths + baths:beds, family = Gamma(link = log) <br/> 모형이 선택 되었다.

``` r
aic <- AIC(glm(formula = price ~ size + new + baths + beds + size:baths + 
        baths:beds, family = Gamma(link = log), data = Houses))
bic <- BIC(glm(formula = price ~ size + new + baths + beds + size:baths + 
          baths:beds, family = Gamma(link = log), data = Houses))
data.frame(aic,bic)%>% rename("AIC"="aic","BIC"="bic")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
AIC
</th>
<th style="text-align:right;">
BIC
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1051.582
</td>
<td style="text-align:right;">
1072.424
</td>
</tr>
</tbody>
</table>

> 이전의 identity link function을 사용했을 때보다 값이 큰 것을 볼 수 있다.

> fit1~fit9에서 AIC와 BIC의 최소값을 확인하여 보자.

``` r
#aic 구하기
aic <- c()
for (i in 1:9){
  aic <-c(aic,AIC(eval(parse(text=paste0("fit",c(1:9))[i]))))
}
data.frame(aic)%>% 
  rename("AIC"="aic")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
AIC
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1052.470
</td>
</tr>
<tr>
<td style="text-align:right;">
1059.012
</td>
</tr>
<tr>
<td style="text-align:right;">
1065.244
</td>
</tr>
<tr>
<td style="text-align:right;">
1057.909
</td>
</tr>
<tr>
<td style="text-align:right;">
1059.163
</td>
</tr>
<tr>
<td style="text-align:right;">
1057.180
</td>
</tr>
<tr>
<td style="text-align:right;">
1056.288
</td>
</tr>
<tr>
<td style="text-align:right;">
1055.296
</td>
</tr>
<tr>
<td style="text-align:right;">
1051.556
</td>
</tr>
</tbody>
</table>

``` r
which.min(aic)
```

    ## [1] 9

``` r
min(aic)
```

    ## [1] 1051.556

> AIC의 값은 fit9에서 가장 작은 값이 나왔으며,

``` r
#bic구하기
bic <- c()
for (i in 1:9){
  bic <-c(bic,BIC(eval(parse(text=paste0("fit",c(1:9))[i]))))
}
data.frame(bic)%>% rename("BIC"="bic")%>% 
  kable(caption = "",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
</caption>
<thead>
<tr>
<th style="text-align:right;">
BIC
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1068.101
</td>
</tr>
<tr>
<td style="text-align:right;">
1090.274
</td>
</tr>
<tr>
<td style="text-align:right;">
1106.926
</td>
</tr>
<tr>
<td style="text-align:right;">
1086.565
</td>
</tr>
<tr>
<td style="text-align:right;">
1085.215
</td>
</tr>
<tr>
<td style="text-align:right;">
1080.627
</td>
</tr>
<tr>
<td style="text-align:right;">
1077.130
</td>
</tr>
<tr>
<td style="text-align:right;">
1073.532
</td>
</tr>
<tr>
<td style="text-align:right;">
1064.582
</td>
</tr>
</tbody>
</table>

``` r
which.min(bic)
```

    ## [1] 9

``` r
min(bic)
```

    ## [1] 1064.582

> BIC도 fit9에서 가장 작은 값이 나왔다.

> 하지만 log link 보다 identity link가 AIC, BIC 둘 다 작은 값을 주는 것을 확인할 수 있다.

> 최종 모델식을 다음과 같이 구했다.

$$ E(price) = -7.4521938 + 0.0944569 * size -77.9033275 * new + 0.0649207	* (size:new) $$

# Q-5

![](/study/img/[Categorical data analysis] Assignment 1/Q-5.jpg)

### 데이터셋 설명

``` r
auto <- read.csv("C:/Biostat/Categorical data analysis/Assignment 1/Auto.csv")
head(auto)%>% 
  kable(caption = "Auto dataset",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Auto dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
mpg
</th>
<th style="text-align:right;">
cylinders
</th>
<th style="text-align:right;">
displacement
</th>
<th style="text-align:left;">
horsepower
</th>
<th style="text-align:right;">
weight
</th>
<th style="text-align:right;">
acceleration
</th>
<th style="text-align:right;">
year
</th>
<th style="text-align:right;">
origin
</th>
<th style="text-align:left;">
name
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
307
</td>
<td style="text-align:left;">
130
</td>
<td style="text-align:right;">
3504
</td>
<td style="text-align:right;">
12.0
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
chevrolet chevelle malibu
</td>
</tr>
<tr>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
350
</td>
<td style="text-align:left;">
165
</td>
<td style="text-align:right;">
3693
</td>
<td style="text-align:right;">
11.5
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
buick skylark 320
</td>
</tr>
<tr>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
318
</td>
<td style="text-align:left;">
150
</td>
<td style="text-align:right;">
3436
</td>
<td style="text-align:right;">
11.0
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
plymouth satellite
</td>
</tr>
<tr>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
304
</td>
<td style="text-align:left;">
150
</td>
<td style="text-align:right;">
3433
</td>
<td style="text-align:right;">
12.0
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
amc rebel sst
</td>
</tr>
<tr>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
302
</td>
<td style="text-align:left;">
140
</td>
<td style="text-align:right;">
3449
</td>
<td style="text-align:right;">
10.5
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
ford torino
</td>
</tr>
<tr>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
429
</td>
<td style="text-align:left;">
198
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:right;">
10.0
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:left;">
ford galaxie 500
</td>
</tr>
</tbody>
</table>

- mpg : 연비
- cylinders : 실린더수 
- displacement : 배기량
- horsepower: 출력
- weight : 차중
- acceleration : 가속능력
- year : 출시년도
- origin : 제조국 1(USA), 2(EU), 3(JPN)
- name : 모델명

> 우선 데이터 type을 알아보자

``` r
str(auto)
```

    ## 'data.frame':    397 obs. of  9 variables:
    ##  $ mpg         : num  18 15 18 16 17 15 14 14 14 15 ...
    ##  $ cylinders   : int  8 8 8 8 8 8 8 8 8 8 ...
    ##  $ displacement: num  307 350 318 304 302 429 454 440 455 390 ...
    ##  $ horsepower  : chr  "130" "165" "150" "150" ...
    ##  $ weight      : int  3504 3693 3436 3433 3449 4341 4354 4312 4425 3850 ...
    ##  $ acceleration: num  12 11.5 11 12 10.5 10 9 8.5 10 8.5 ...
    ##  $ year        : int  70 70 70 70 70 70 70 70 70 70 ...
    ##  $ origin      : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ name        : chr  "chevrolet chevelle malibu" "buick skylark 320" "plymouth satellite" "amc rebel sst" ...

> horsepower 변수가 character형으로 되어있어 integer로 변환하고, NA 값이
> 있는지 확인하도록 한다.

``` r
auto$horsepower <- as.integer(auto$horsepower) 
auto$year <- auto$year-70
table(is.na(auto$horsepower))
```

    ## 
    ## FALSE  TRUE 
    ##   392     5

> horsepower 변수에 결측값이 있는것을 확인할 수 있다. 결측값을 제거한다.

``` r
auto1 <- na.omit(auto)
table(is.na(auto1$horsepower))
```

    ## 
    ## FALSE 
    ##   392

``` r
mytable(auto1) #library('moonBook')
```

    ## 
    ##                Descriptive Statistics               
    ## ————————————————————————————————————————————————————— 
    ##                 Mean ± SD or %      N  Missing (%)
    ## ————————————————————————————————————————————————————— 
    ##    mpg                  23.4 ± 7.8  392  0  ( 0.0%)
    ##   cylinders                         392  0  ( 0.0%)
    ##     - 3                  4  (1.0%)                 
    ##     - 4               199  (50.8%)                 
    ##     - 5                  3  (0.8%)                 
    ##     - 6                83  (21.2%)                 
    ##     - 8               103  (26.3%)                 
    ##    displacement      194.4 ± 104.6  392  0  ( 0.0%)
    ##    horsepower         104.5 ± 38.5  392  0  ( 0.0%)
    ##    weight           2977.6 ± 849.4  392  0  ( 0.0%)
    ##    acceleration         15.5 ± 2.8  392  0  ( 0.0%)
    ##    year                  6.0 ± 3.7  392  0  ( 0.0%)
    ##   origin                            392  0  ( 0.0%)
    ##     - 1               245  (62.5%)                 
    ##     - 2                68  (17.3%)                 
    ##     - 3                79  (20.2%)                 
    ##   name          unique values  301  392  0  ( 0.0%)
    ## —————————————————————————————————————————————————————


> 연속형 독립변수들 간에 연관성을 확인하여 본다.

``` r
auto2 <- select(auto1, mpg,cylinders, displacement, horsepower, weight, acceleration, year)
cor(auto2) %>% data.frame() %>% 
  kable(caption = "Correlation",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Correlation
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
mpg
</th>
<th style="text-align:right;">
cylinders
</th>
<th style="text-align:right;">
displacement
</th>
<th style="text-align:right;">
horsepower
</th>
<th style="text-align:right;">
weight
</th>
<th style="text-align:right;">
acceleration
</th>
<th style="text-align:right;">
year
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
mpg
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
-0.7776175
</td>
<td style="text-align:right;">
-0.8051269
</td>
<td style="text-align:right;">
-0.7784268
</td>
<td style="text-align:right;">
-0.8322442
</td>
<td style="text-align:right;">
0.4233285
</td>
<td style="text-align:right;">
0.5805410
</td>
</tr>
<tr>
<td style="text-align:left;">
cylinders
</td>
<td style="text-align:right;">
-0.7776175
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.9508233
</td>
<td style="text-align:right;">
0.8429834
</td>
<td style="text-align:right;">
0.8975273
</td>
<td style="text-align:right;">
-0.5046834
</td>
<td style="text-align:right;">
-0.3456474
</td>
</tr>
<tr>
<td style="text-align:left;">
displacement
</td>
<td style="text-align:right;">
-0.8051269
</td>
<td style="text-align:right;">
0.9508233
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.8972570
</td>
<td style="text-align:right;">
0.9329944
</td>
<td style="text-align:right;">
-0.5438005
</td>
<td style="text-align:right;">
-0.3698552
</td>
</tr>
<tr>
<td style="text-align:left;">
horsepower
</td>
<td style="text-align:right;">
-0.7784268
</td>
<td style="text-align:right;">
0.8429834
</td>
<td style="text-align:right;">
0.8972570
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.8645377
</td>
<td style="text-align:right;">
-0.6891955
</td>
<td style="text-align:right;">
-0.4163615
</td>
</tr>
<tr>
<td style="text-align:left;">
weight
</td>
<td style="text-align:right;">
-0.8322442
</td>
<td style="text-align:right;">
0.8975273
</td>
<td style="text-align:right;">
0.9329944
</td>
<td style="text-align:right;">
0.8645377
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
-0.4168392
</td>
<td style="text-align:right;">
-0.3091199
</td>
</tr>
<tr>
<td style="text-align:left;">
acceleration
</td>
<td style="text-align:right;">
0.4233285
</td>
<td style="text-align:right;">
-0.5046834
</td>
<td style="text-align:right;">
-0.5438005
</td>
<td style="text-align:right;">
-0.6891955
</td>
<td style="text-align:right;">
-0.4168392
</td>
<td style="text-align:right;">
1.0000000
</td>
<td style="text-align:right;">
0.2903161
</td>
</tr>
<tr>
<td style="text-align:left;">
year
</td>
<td style="text-align:right;">
0.5805410
</td>
<td style="text-align:right;">
-0.3456474
</td>
<td style="text-align:right;">
-0.3698552
</td>
<td style="text-align:right;">
-0.4163615
</td>
<td style="text-align:right;">
-0.3091199
</td>
<td style="text-align:right;">
0.2903161
</td>
<td style="text-align:right;">
1.0000000
</td>
</tr>
</tbody>
</table>

> 상관계수를 구해본 결과

mpg는 cylinders, displacement, horsepower, weight 와 강한 음의 상관성을,

cylinders는 displacement, horsepower, weight와 강한 양의 상관성을,

displacement는 horsepower, weight과 강한 양의 상관성을,

horsepower는 weight과 강한 양의 상관성을 가지고 있음을 볼 수 있다.

acceleration, year 두 변수는 나머지 변수들과의 연관성이 작음을 볼 수
있다.

-   **Gamma glm**

> Gamma glm을 적합하기 위해 일단 먼저 모든 독립변수를 넣고 모델을
> 적합시키도록 한다.

``` r
autoModel1 <- glm(mpg ~ cylinders + displacement + horsepower + weight + acceleration + year + origin,family=Gamma(link=identity), data = auto1)

sum_autoModel1 <- summary(autoModel1)

data.frame(sum_autoModel1$coefficients)%>% 
  rename("coefficient"="Estimate","Std.Error"="Std..Error","t value"="t.value","p value"="Pr...t..") %>% 
  kable(caption = "Summary",booktabs = TRUE, valign = 't')%>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Summary
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
coefficient
</th>
<th style="text-align:right;">
Std.Error
</th>
<th style="text-align:right;">
t value
</th>
<th style="text-align:right;">
p value
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
(Intercept)
</td>
<td style="text-align:right;">
35.6804699
</td>
<td style="text-align:right;">
1.8479596
</td>
<td style="text-align:right;">
19.3080360
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
cylinders
</td>
<td style="text-align:right;">
-1.0578149
</td>
<td style="text-align:right;">
0.2394908
</td>
<td style="text-align:right;">
-4.4169342
</td>
<td style="text-align:right;">
0.0000130
</td>
</tr>
<tr>
<td style="text-align:left;">
displacement
</td>
<td style="text-align:right;">
0.0142433
</td>
<td style="text-align:right;">
0.0052893
</td>
<td style="text-align:right;">
2.6928477
</td>
<td style="text-align:right;">
0.0073948
</td>
</tr>
<tr>
<td style="text-align:left;">
horsepower
</td>
<td style="text-align:right;">
-0.0145053
</td>
<td style="text-align:right;">
0.0086449
</td>
<td style="text-align:right;">
-1.6779008
</td>
<td style="text-align:right;">
0.0941801
</td>
</tr>
<tr>
<td style="text-align:left;">
weight
</td>
<td style="text-align:right;">
-0.0043690
</td>
<td style="text-align:right;">
0.0004821
</td>
<td style="text-align:right;">
-9.0619670
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
acceleration
</td>
<td style="text-align:right;">
-0.0706537
</td>
<td style="text-align:right;">
0.0825588
</td>
<td style="text-align:right;">
-0.8557981
</td>
<td style="text-align:right;">
0.3926431
</td>
</tr>
<tr>
<td style="text-align:left;">
year
</td>
<td style="text-align:right;">
0.6199023
</td>
<td style="text-align:right;">
0.0475805
</td>
<td style="text-align:right;">
13.0284980
</td>
<td style="text-align:right;">
0.0000000
</td>
</tr>
<tr>
<td style="text-align:left;">
origin
</td>
<td style="text-align:right;">
1.6433755
</td>
<td style="text-align:right;">
0.3037828
</td>
<td style="text-align:right;">
5.4097051
</td>
<td style="text-align:right;">
0.0000001
</td>
</tr>
</tbody>
</table>

> 모든 변수를 넣었을 때 피팅 결과 acceleration 변수가 유의미하지 않은
> 결과를 보였다.

# Q-6

## Q-6 4.9

![](/study/img/[Categorical data analysis] Assignment 1/Q-6-4-9.jpg)
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-9.jpg)

## Q-6 4.13

![](/study/img/[Categorical data analysis] Assignment 1/Q-6-4-13.jpg)
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-13.jpg)

## Q-6 4.16

![](/study/img/[Categorical data analysis] Assignment 1/Q-6-4-16.jpg)
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-16.jpg)

## Q-6 4.22

![](/study/img/[Categorical data analysis] Assignment 1/Q-6-4-22.jpg)
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-22.jpg)

## Q-6 4.25
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-4-25.jpg)
![](/study/img/[Categorical data analysis] Assignment 1/Q-6-25.jpg)


