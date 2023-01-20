``` r
getwd()
```

    ## [1] "C:/git_blog/study/data/ch9"

``` r
library(readxl)
library(dplyr)
library(kableExtra)
library(PairedData)
```

**엑셀파일불러오기**

``` r
#모든 시트를 하나의 리스트로 불러오는 함수
read_excel_allsheets <- function(file, tibble = FALSE) {
  sheets <- readxl::excel_sheets(file)
  x <- lapply(sheets, function(X) readxl::read_excel(file, sheet = X))
  if(!tibble) x <- lapply(x, as.data.frame)
  names(x) <- sheets
  x
}
```

## 9장

**9장 연습문제 불러오기**

``` r
#data_chap09에 연습문제 9장 모든 문제 저장
data_chap09 <- read_excel_allsheets("data_chap09.xls")

#연습문제 각각 데이터 생성
for (x in 1:length(data_chap09)){
  assign(paste0('ex9_',1:length(data_chap09))[x],data_chap09[x])
  }

#연습문제 데이터 형식을 리스트에서 데이터프레임으로 변환
for (x in 1:length(data_chap09)){
  assign(paste0('ex9_',1:length(data_chap09))[x],data.frame(data_chap09[x]))
  }
```

## EXAMPLE 9.1

![](C:/git_blog/study/img/ch9/9-1.png)

``` r
#데이터셋
ex9_1%>%
  kbl(caption = "Dataset",escape=F) %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
exam9_1.deer
</th>
<th style="text-align:right;">
exam9_1.hindleg
</th>
<th style="text-align:right;">
exam9_1.foreleg
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
142
</td>
<td style="text-align:right;">
138
</td>
</tr>
<tr>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
140
</td>
<td style="text-align:right;">
136
</td>
</tr>
<tr>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
144
</td>
<td style="text-align:right;">
147
</td>
</tr>
<tr>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
144
</td>
<td style="text-align:right;">
139
</td>
</tr>
<tr>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
142
</td>
<td style="text-align:right;">
143
</td>
</tr>
<tr>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
146
</td>
<td style="text-align:right;">
141
</td>
</tr>
<tr>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
149
</td>
<td style="text-align:right;">
143
</td>
</tr>
<tr>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
150
</td>
<td style="text-align:right;">
145
</td>
</tr>
<tr>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
142
</td>
<td style="text-align:right;">
136
</td>
</tr>
<tr>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
148
</td>
<td style="text-align:right;">
146
</td>
</tr>
</tbody>
</table>

-   해당 데이터는 사슴의 앞 다리와 뒷 다리에 대한 데이터로써 서로 짝을
    이룬 대응표본 데이터이다.<br/>
-   사슴 모집단의 앞 다리 길이와 뒷 다리 길이의 차이가 있는지 검정하고자
    한다.

$$
\begin{aligned}
H_0 &: \mu_d = 0 \\\\
H_1 &: \mu_d \not=\\ 0
\end{aligned}
$$

-   서로 짝을 이룬 데이터이므로 Paired Sample t-test를 시행할 수 있으며,
    앞 다리와 뒷 다리의 차이를 모수로 두어 One Sample t-test를 시행할
    수도 있다.<br/>
-   Paired Sample t-test의 경우 정규성을 만족해야하는 가정이 있으므로
    정규성 검정을 시행한다.

> 표본의 크기가 작으므로 Shapiro-Wilk test를 사용할 것이다.

``` r
ex9_1$diff <- ex9_1$exam9_1.hindleg-ex9_1$exam9_1.foreleg
shapiro.test(ex9_1$diff)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ex9_1$diff
    ## W = 0.81366, p-value = 0.02123

-   Paired Sample의 경우 정규성 가정시 두 표본에 대한 차이 값으로 정규성
    검정을 시행하여야 한다.<br/>
-   정규성 검정의 결과 p-value = 0.02603으로써 정규성 가정을 만족하지
    못하였으므로 비모수 검정을 시행하여야 하지만 비모수 검정은 EXAMPLE
    9.4에서 다시 다루도록 하겠다.<br/>
-   이 예제에서는 Paired Sample t-test를 시행하여 보겠다.

``` r
t.test(ex9_1$exam9_1.hindleg,ex9_1$exam9_1.foreleg,mu=0,alt='two.sided', paired=T) # t.test(ex9_1$diff,mu=0,alt='two.sided') 같은 결과를 준다.
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  ex9_1$exam9_1.hindleg and ex9_1$exam9_1.foreleg
    ## t = 3.4138, df = 9, p-value = 0.007703
    ## alternative hypothesis: true mean difference is not equal to 0
    ## 95 percent confidence interval:
    ##  1.113248 5.486752
    ## sample estimates:
    ## mean difference 
    ##             3.3

-   검정통계량 값이 3.4138 문제의 값인 3.402와 다르게 나온 이유는 두
    표본에 대한 표준편차가 반올림을 해서 계산이 되었기 때문이다. <br/>
-   이 예제에서는 두 표본에 대한 표준편차 0.9666667를 소수점
    셋째자리에서 반올림하였다. <br/>
-   p-value = 0.007703이므로 유의수준 5%하에 귀무가설을 기각할 수 있다.
    <br/>
-   즉, 사슴 모집단의 앞 다리와 뒷 다리의 길이의 차이가 있다고 할 수
    있다.

## EXAMPLE 9.2

![](C:/git_blog/study/img/ch9/9-2.png)

``` r
#데이터셋
ex9_2%>%
  kbl(caption = "Dataset",escape=F) %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed", "responsive"))
```

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
Dataset
</caption>
<thead>
<tr>
<th style="text-align:right;">
exam9_2.plot
</th>
<th style="text-align:right;">
exam9_2.new
</th>
<th style="text-align:right;">
exam9_2.old
</th>
<th style="text-align:right;">
exam9_2.d
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2250
</td>
<td style="text-align:right;">
1920
</td>
<td style="text-align:right;">
330
</td>
</tr>
<tr>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2410
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
390
</td>
</tr>
<tr>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2260
</td>
<td style="text-align:right;">
2060
</td>
<td style="text-align:right;">
200
</td>
</tr>
<tr>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2200
</td>
<td style="text-align:right;">
1960
</td>
<td style="text-align:right;">
240
</td>
</tr>
<tr>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2360
</td>
<td style="text-align:right;">
1960
</td>
<td style="text-align:right;">
400
</td>
</tr>
<tr>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2320
</td>
<td style="text-align:right;">
2140
</td>
<td style="text-align:right;">
180
</td>
</tr>
<tr>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
2240
</td>
<td style="text-align:right;">
1980
</td>
<td style="text-align:right;">
260
</td>
</tr>
<tr>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2300
</td>
<td style="text-align:right;">
1940
</td>
<td style="text-align:right;">
360
</td>
</tr>
<tr>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
2090
</td>
<td style="text-align:right;">
1790
</td>
<td style="text-align:right;">
300
</td>
</tr>
</tbody>
</table>

-   해당 데이터는 기존 비료와 새로운 비료에 따른 수확량을 기록한
    데이터이다. <br/>
-   새로운 비료와 기존 비료의 곡물 수확량 차이 모평균에 대한 검정을
    수행할 것이다.

$$
\begin{aligned}
H_0 &: \mu_d \leq25 kg/ha \\\\
H_1 &: \mu_d\>250kg/ha
\end{aligned}
$$

-   Paired Sample t-test의 경우 정규성을 만족해야하는 가정이 있으므로
    정규성 검정을 시행한다.

> 표본의 크기가 작으므로 Shapiro-Wilk test를 사용할 것이다.

``` r
shapiro.test(ex9_2$exam9_2.d)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ex9_2$exam9_2.d
    ## W = 0.94359, p-value = 0.6202

-   p-value가 0.6202 이므로 유의수준 5%하에 두 집단 모두 정규성을 따르지
    않는다고 할 충분한 증거가 없다.<br/>
-   따라서 정규성 가정을 만족한다 보고 Paired Sample t-test를
    시행하겠다.

``` r
t.test(ex9_2$exam9_2.new, ex9_2$exam9_2.old, mu=250, alt='greater', paired=T) # t.test(ex9_2$exam9_2.d, mu=250, alt="greater")  같은 결과를 준다.
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  ex9_2$exam9_2.new and ex9_2$exam9_2.old
    ## t = 1.6948, df = 8, p-value = 0.06428
    ## alternative hypothesis: true mean difference is greater than 250
    ## 95 percent confidence interval:
    ##  245.571     Inf
    ## sample estimates:
    ## mean difference 
    ##        295.5556

## EXAMPLE 9.3

![](C:/git_blog/study/img/ch9/9-3.png)

-   이 예제는 Example 9.1 데이터를 사용하여 모분산이 같은지에 대한
    검정을 해보도록 할 것이다.

$$
\begin{aligned}
H_0 &: \sigma_1^2=\sigma_2^2 \\\\
H_1 &: \sigma_1^2\neq\sigma_2^2
\end{aligned}
$$

-   두개의 paired-samples에 대해 분산이 같은지 여부를 검증하기 위해
    Pitman 검정을 진행했다.

> 직접 함수를 작성해서 구하여 보자.

``` r
v1=round(var(ex9_1$exam9_1.hindleg),2)
v2=round(var(ex9_1$exam9_1.foreleg),2)
r=cor(ex9_1$exam9_1.hindleg,ex9_1$exam9_1.foreleg)
F=round(v1/v2,4)
n=length(ex9_1$exam9_1.hindleg)
t=round(((F-1)*sqrt(n-2))/(2*sqrt(F*(1-r^2))),3)
t_0.05=round(qt(0.025, df=8, lower.tail=F),3)
cat("\t F=",F," t=",t," t_0.05=",t_0.05, "\n\t Therefore cannot reject null hypothesis")
```

    ##   F= 0.7111  t= -0.656  t_0.05= 2.306 
    ##   Therefore cannot reject null hypothesis

-   t 값은 -0.656으로 자유도가 8인 t_0.05값 2.306보다 그 절대값이 작기
    때문에 사슴 앞 다리의 길이와 사슴 뒷 다리의 길이의 모분산이 같다는
    귀무가설을 기각할 수 없다.

> 다음은 PairedData 패키지의 pitman.morgan.test.default() 함수 사용

``` r
library(PairedData)
pitman.morgan.test.default(ex9_1$exam9_1.hindleg,ex9_1$exam9_1.foreleg)
```

    ## 
    ##  Paired Pitman-Morgan test
    ## 
    ## data:  ex9_1$exam9_1.hindleg and ex9_1$exam9_1.foreleg
    ## t = -0.65591, df = 8, p-value = 0.5303
    ## alternative hypothesis: true ratio of variances is not equal to 1
    ## 95 percent confidence interval:
    ##  0.227042 2.226964
    ## sample estimates:
    ## variance of x variance of y 
    ##      11.56667      16.26667

-   p-value가 0.5303으로 유의수준 5%하에 귀무가설을 기각할 수 없다.

> Fisher의 방법으로 분산이 동일한지 검정

``` r
var.test(ex9_1$exam9_1.hindleg,ex9_1$exam9_1.foreleg,ratio=1,alternative = "two.sided",paired=T)
```

    ## 
    ##  F test to compare two variances
    ## 
    ## data:  ex9_1$exam9_1.hindleg and ex9_1$exam9_1.foreleg
    ## F = 0.71107, num df = 9, denom df = 9, p-value = 0.6197
    ## alternative hypothesis: true ratio of variances is not equal to 1
    ## 95 percent confidence interval:
    ##  0.1766186 2.8627458
    ## sample estimates:
    ## ratio of variances 
    ##          0.7110656

-   p-value가 0.6197로 유의수준 5%하에 귀무가설을 기각할 수 없다.<br/>
-   따라서 유의수준 5%하에 사슴 앞 다리 길이와 뒷 다리 길이의 모분산이
    다르다고 말할만한 충분한 근거가 없다.

## EXAMPLE 9.4

![](C:/git_blog/study/img/ch9/9-4.png)

-   이 예제에서는 Example 9.1 데이터가 정규성 가정을 만족하지
    못하였으므로 비모수 검정을 시행하여보겠다.

$$
H_o:\\ Deer\\ hindleg\\ length\\ is\\ the\\ same\\ as\\ foreleg\\ length.\\\\ 
H_A: eer\\ hind\\ leg\\ length\\ is\\ not\\ the\\ same\\ as\\ forele\\ length.
$$

``` r
ex9_1$diff=ex9_1$exam9_1.hindleg-ex9_1$exam9_1.foreleg
ex9_1$d.rank= rank(abs(ex9_1$diff))
ex9_1$sign_rank= ifelse(ex9_1$diff < 0, ex9_1$d.rank *(-1), ex9_1$d.rank) 
T.plus= sum(subset(ex9_1, sign_rank>=0)$sign_rank)
T.minus= abs(sum(subset(ex9_1, sign_rank< 0)$sign_rank)) 
```

``` r
T.plus;T.minus
```

    ## [1] 51

    ## [1] 4

-   signed rank의 양수 합은 51, 음수 합은 4이다.<br/>
-   대응 표본에 대하여 비모수 검정법인 Wilcoxon signed rank test
    수행하였다.

``` r
wilcox.test(ex9_1$exam9_1.hindleg, ex9_1$exam9_1.foreleg, paired=T)
```

    ## 
    ##  Wilcoxon signed rank test with continuity correction
    ## 
    ## data:  ex9_1$exam9_1.hindleg and ex9_1$exam9_1.foreleg
    ## V = 51, p-value = 0.01859
    ## alternative hypothesis: true location shift is not equal to 0

-   p유의수준 5%에서 기각역은 8이다. <br/>
-   4 \< 8이므로 귀무가설을 기각하고 대립가설을 채택한다. <br/>
-   즉. 유의수준 5% 하에서 사슴 앞다리 길이의 모평균과 뒷다리 길이의
    모평균은 다르다고 볼 수 있는 충분한 근거가 있다.
