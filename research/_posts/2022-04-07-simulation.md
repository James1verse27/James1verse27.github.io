---
layout: post
title:  "[Simulation] Statistical Errors"
category : research
tag: [R, Simulation, markdown]
toc: true
---
* this unordered seed list will be replaced by the toc
{:toc}

- 뒤로가기를 누르시면 목차로 되돌아옵니다. 😉

> 연구 목적

모의실험의 목적은 표본크기와 분산 등의 변화에 따라 비모수적 검정과, 모수적 검정시 범하는 1종오류, 2종오류의 차이를 보고자 하는 것이다.

> 연구 방법

R을 통해 Normal distribution, Uniform distribution, Chi-squared distribution, Exponential distribution를 따르는 난수를 발생시켜 주어진 상황에 따라 표본을 뽑아 총 10,000번 모수적 검정인 t-test, 비모수적 검정인 Wilcoxon rank sum test를 시행하여 각 상황에 따른 차이들을 1종오류와 2종오류에서 확인하여 볼 것이다.

따라서 각 조건별로 Type I error와 Type II error를 구해서 테이블로 제시하고, 그래프로 변화하는 양상을 살펴볼 수 있도록 그래프로 제시할 것이다.

Type I error: 귀무가설이 참임에도 불구하고 귀무가설을 기각하는 것

Type II error: 귀무가설이 참이 아닌데 귀무가설을 받아들이는 것

즉, Type I error는 두 집단의 모평균이 같을 때, Type II error는 두 집단의 모평균이 다를 때의 상황을 조건으로 한다.

주어진 상황은 다음과 같다.

- Type I error
    - 등분산, 표본크기가 같은 경우
    - 등분산, 표본크기가 다른 경우
    - 분산이 다르고, 표본크기는 같은 경우

- Type II error
    - 등분산, 표본크기가 같은 경우
    - 등분산, 표본크기가 다른 경우
    - 분산이 다르고, 표본크기는 같은 경우

``` r
library(kableExtra)
library(exactRankTests)
library(plyr)
alpha.p <- 0.05
n.simulations <- 10000
```

# Type I error

## N=10000,등분산,표본크기가 같은 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 1\] Type I error, 등분산,표본크기가 같은 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) t-test
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0539
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0492
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0333
</td>
<td style="text-align:right;">
0.0398
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0457
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0503
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0447
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0472
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0325
</td>
<td style="text-align:right;">
0.0448
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0464
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0461
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0331
</td>
<td style="text-align:right;">
0.0414
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0509
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0480
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0322
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0484
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0538
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0494
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0316
</td>
<td style="text-align:right;">
0.0447
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) t-test
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0549
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0562
</td>
<td style="text-align:right;">
0.0523
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0331
</td>
<td style="text-align:right;">
0.0429
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-2-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 2\] Type I error, 등분산,표본크기가 같은 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) t-test
</td>
<td style="text-align:right;">
0.0563
</td>
<td style="text-align:right;">
0.0539
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0540
</td>
<td style="text-align:right;">
0.0494
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0311
</td>
<td style="text-align:right;">
0.0454
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) t-test
</td>
<td style="text-align:right;">
0.0561
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0511
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0285
</td>
<td style="text-align:right;">
0.0405
</td>
<td style="text-align:right;">
0.0525
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0510
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) t-test
</td>
<td style="text-align:right;">
0.0555
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0530
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0526
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0347
</td>
<td style="text-align:right;">
0.0440
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0481
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) t-test
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0530
</td>
<td style="text-align:right;">
0.0520
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0294
</td>
<td style="text-align:right;">
0.0436
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) t-test
</td>
<td style="text-align:right;">
0.0554
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0489
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0319
</td>
<td style="text-align:right;">
0.0388
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0509
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) t-test
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0567
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0463
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0291
</td>
<td style="text-align:right;">
0.0443
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0488
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-3-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 3\] Type I error, 등분산,표본크기가 같은 경우: Chi-squared
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) t-test
</td>
<td style="text-align:right;">
0.0303
</td>
<td style="text-align:right;">
0.0403
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0445
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0468
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0438
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0517
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) t-test
</td>
<td style="text-align:right;">
0.0399
</td>
<td style="text-align:right;">
0.0418
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0448
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0485
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.0306
</td>
<td style="text-align:right;">
0.0428
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) t-test
</td>
<td style="text-align:right;">
0.0408
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0447
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0527
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0310
</td>
<td style="text-align:right;">
0.0396
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0493
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) t-test
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0446
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0484
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0312
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0471
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0515
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) t-test
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0460
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0525
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0315
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) t-test
</td>
<td style="text-align:right;">
0.0448
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0568
</td>
<td style="text-align:right;">
0.0511
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0303
</td>
<td style="text-align:right;">
0.0404
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0490
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-4-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 4\] Type I error, 등분산,표본크기가 같은 경우: Exponential
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1) exp(1/1) t-test
</td>
<td style="text-align:right;">
0.0370
</td>
<td style="text-align:right;">
0.0438
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0486
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1) exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.0293
</td>
<td style="text-align:right;">
0.0417
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0525
</td>
<td style="text-align:right;">
0.0493
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0395
</td>
<td style="text-align:right;">
0.0441
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0470
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0412
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0412
</td>
<td style="text-align:right;">
0.0443
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0321
</td>
<td style="text-align:right;">
0.0439
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0519
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0397
</td>
<td style="text-align:right;">
0.0427
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0482
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0293
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0521
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0369
</td>
<td style="text-align:right;">
0.0452
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0328
</td>
<td style="text-align:right;">
0.0455
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0524
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0377
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0287
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0473
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-5-1.png)

## N=10000,등분산,표본크기가 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 5\] Type I error, 등분산,표본크기가 다른 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) t-test
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0535
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0433
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0515
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0494
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0392
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0489
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0525
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0471
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0450
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0499
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0538
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0496
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0371
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0508
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0487
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0433
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) t-test
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0494
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0429
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0450
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-6-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 6\] Type I error, 등분산,표본크기가 다른 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) t-test
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0488
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0403
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0536
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) t-test
</td>
<td style="text-align:right;">
0.0544
</td>
<td style="text-align:right;">
0.0566
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0431
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0508
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) t-test
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0533
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0471
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0399
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0477
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) t-test
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0441
</td>
<td style="text-align:right;">
0.0528
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0409
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) t-test
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0487
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0470
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) t-test
</td>
<td style="text-align:right;">
0.0561
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0471
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0390
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0518
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-7-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 7\] Type I error, 등분산,표본크기가 다른 경우: Chi-squared
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) t-test
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0440
</td>
<td style="text-align:right;">
0.0460
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0508
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0486
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) t-test
</td>
<td style="text-align:right;">
0.0404
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0457
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0445
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.0392
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0517
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) t-test
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0414
</td>
<td style="text-align:right;">
0.0557
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0485
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) t-test
</td>
<td style="text-align:right;">
0.0430
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0492
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0428
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0503
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) t-test
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0461
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0416
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0482
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) t-test
</td>
<td style="text-align:right;">
0.0448
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0543
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0476
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-8-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 8\] Type I error, 등분산,표본크기가 다른 경우: Exponential
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1) exp(1/1) t-test
</td>
<td style="text-align:right;">
0.0416
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0447
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0432
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1) exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0533
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0499
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0411
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0462
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0479
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0433
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0545
</td>
<td style="text-align:right;">
0.0509
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0446
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0484
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0388
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0520
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0415
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0498
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0406
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0515
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-9-1.png)

## N=10000, 분산이 다르고, 표본크기는 같은 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 9\] Type I error, 분산이 다르고, 표본크기는 같은 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1.5) N(0,1) t-test
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1.5) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0322
</td>
<td style="text-align:right;">
0.0417
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0450
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0498
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0319
</td>
<td style="text-align:right;">
0.0437
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0521
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0469
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0300
</td>
<td style="text-align:right;">
0.0436
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0460
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0545
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0315
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0485
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0548
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0478
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0296
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0472
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,6) t-test
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0462
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0339
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0571
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0503
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-10-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 10\] Type I error, 분산이 다르고, 표본크기는 같은 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,2.46)-0.73 U(0,1) t-test
</td>
<td style="text-align:right;">
0.0766
</td>
<td style="text-align:right;">
0.0602
</td>
<td style="text-align:right;">
0.0537
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0522
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.46)-0.73 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0611
</td>
<td style="text-align:right;">
0.0696
</td>
<td style="text-align:right;">
0.0681
</td>
<td style="text-align:right;">
0.0701
</td>
<td style="text-align:right;">
0.0730
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)-0.42 U(0,2) t-test
</td>
<td style="text-align:right;">
0.0585
</td>
<td style="text-align:right;">
0.0525
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0512
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)-0.42 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0328
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0544
</td>
<td style="text-align:right;">
0.0535
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)-0.08 U(0,3) t-test
</td>
<td style="text-align:right;">
0.0579
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0544
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)-0.08 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0298
</td>
<td style="text-align:right;">
0.0446
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0466
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.25 U(0,4) t-test
</td>
<td style="text-align:right;">
0.0536
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0482
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.25 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0326
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+0.62 U(0,5) t-test
</td>
<td style="text-align:right;">
0.0550
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0525
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+0.62 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0341
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0544
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1 U(0,6) t-test
</td>
<td style="text-align:right;">
0.0613
</td>
<td style="text-align:right;">
0.0567
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0395
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0569
</td>
<td style="text-align:right;">
0.0538
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-11-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 11\] Type I error, 분산이 다르고, 표본크기는 같은 경우:
Chi-squared distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1.5)-0.5 chisq(1) t-test
</td>
<td style="text-align:right;">
0.0423
</td>
<td style="text-align:right;">
0.0455
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0493
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1.5)-0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0447
</td>
<td style="text-align:right;">
0.0713
</td>
<td style="text-align:right;">
0.1353
</td>
<td style="text-align:right;">
0.2049
</td>
<td style="text-align:right;">
0.2868
</td>
<td style="text-align:right;">
0.3530
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0 chisq(2) t-test
</td>
<td style="text-align:right;">
0.0374
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0519
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.0320
</td>
<td style="text-align:right;">
0.0435
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0466
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.0417
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0531
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0319
</td>
<td style="text-align:right;">
0.0452
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0601
</td>
<td style="text-align:right;">
0.0651
</td>
<td style="text-align:right;">
0.0642
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1 chisq(4) t-test
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0536
</td>
<td style="text-align:right;">
0.0518
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0325
</td>
<td style="text-align:right;">
0.0422
</td>
<td style="text-align:right;">
0.0558
</td>
<td style="text-align:right;">
0.0590
</td>
<td style="text-align:right;">
0.0658
</td>
<td style="text-align:right;">
0.0705
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+1.5 chisq(5) t-test
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0449
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0461
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+1.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0318
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0601
</td>
<td style="text-align:right;">
0.0628
</td>
<td style="text-align:right;">
0.0658
</td>
<td style="text-align:right;">
0.0747
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2 chisq(6) t-test
</td>
<td style="text-align:right;">
0.0440
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0489
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0327
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0562
</td>
<td style="text-align:right;">
0.0596
</td>
<td style="text-align:right;">
0.0675
</td>
<td style="text-align:right;">
0.0711
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-12-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 12\] Type I error, 분산이 다르고, 표본크기는 같은 경우:
Exponential distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0.5 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.0560
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0561
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0525
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.0460
</td>
<td style="text-align:right;">
0.0808
</td>
<td style="text-align:right;">
0.1411
</td>
<td style="text-align:right;">
0.2372
</td>
<td style="text-align:right;">
0.3157
</td>
<td style="text-align:right;">
0.4020
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)-0 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0446
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0453
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)-0 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0318
</td>
<td style="text-align:right;">
0.0444
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0423
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0481
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0357
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0782
</td>
<td style="text-align:right;">
0.1075
</td>
<td style="text-align:right;">
0.1250
</td>
<td style="text-align:right;">
0.1588
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0519
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0406
</td>
<td style="text-align:right;">
0.0623
</td>
<td style="text-align:right;">
0.1011
</td>
<td style="text-align:right;">
0.1607
</td>
<td style="text-align:right;">
0.2247
</td>
<td style="text-align:right;">
0.2654
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+1.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0499
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+1.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0790
</td>
<td style="text-align:right;">
0.1274
</td>
<td style="text-align:right;">
0.1948
</td>
<td style="text-align:right;">
0.2842
</td>
<td style="text-align:right;">
0.3566
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0543
</td>
<td style="text-align:right;">
0.0538
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0551
</td>
<td style="text-align:right;">
0.0529
</td>
<td style="text-align:right;">
0.0478
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0803
</td>
<td style="text-align:right;">
0.1413
</td>
<td style="text-align:right;">
0.2288
</td>
<td style="text-align:right;">
0.3243
</td>
<td style="text-align:right;">
0.4027
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-13-1.png)

# Type II error

## N=10000,등분산,표본크기가 같은 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 13\] Type II error, 등분산,표본크기가 같은 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1)+2 N(0,1) t-test
</td>
<td style="text-align:right;">
0.2015
</td>
<td style="text-align:right;">
0.0106
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1)+2 N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.3302
</td>
<td style="text-align:right;">
0.0184
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+2 N(0,2) t-test
</td>
<td style="text-align:right;">
0.4979
</td>
<td style="text-align:right;">
0.1562
</td>
<td style="text-align:right;">
0.0012
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+2 N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.6124
</td>
<td style="text-align:right;">
0.1946
</td>
<td style="text-align:right;">
0.0022
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+2 N(0,3) t-test
</td>
<td style="text-align:right;">
0.6311
</td>
<td style="text-align:right;">
0.3075
</td>
<td style="text-align:right;">
0.0207
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+2 N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.7363
</td>
<td style="text-align:right;">
0.3640
</td>
<td style="text-align:right;">
0.0232
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+2 N(0,4) t-test
</td>
<td style="text-align:right;">
0.7117
</td>
<td style="text-align:right;">
0.4324
</td>
<td style="text-align:right;">
0.0674
</td>
<td style="text-align:right;">
0.0006
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+2 N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.7942
</td>
<td style="text-align:right;">
0.4862
</td>
<td style="text-align:right;">
0.0790
</td>
<td style="text-align:right;">
0.0022
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+2 N(0,5) t-test
</td>
<td style="text-align:right;">
0.7675
</td>
<td style="text-align:right;">
0.5337
</td>
<td style="text-align:right;">
0.1320
</td>
<td style="text-align:right;">
0.0059
</td>
<td style="text-align:right;">
0.0002
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+2 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.8319
</td>
<td style="text-align:right;">
0.5752
</td>
<td style="text-align:right;">
0.1478
</td>
<td style="text-align:right;">
0.0099
</td>
<td style="text-align:right;">
0.0007
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+2 N(0,6) t-test
</td>
<td style="text-align:right;">
0.7932
</td>
<td style="text-align:right;">
0.5935
</td>
<td style="text-align:right;">
0.1912
</td>
<td style="text-align:right;">
0.0203
</td>
<td style="text-align:right;">
0.0011
</td>
<td style="text-align:right;">
1e-04
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+2 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.8518
</td>
<td style="text-align:right;">
0.6353
</td>
<td style="text-align:right;">
0.2185
</td>
<td style="text-align:right;">
0.0253
</td>
<td style="text-align:right;">
0.0022
</td>
<td style="text-align:right;">
1e-04
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-14-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 14\] Type II error, 등분산,표본크기가 같은 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,1)+0.5 U(0,1) t-test
</td>
<td style="text-align:right;">
0.3608
</td>
<td style="text-align:right;">
0.0337
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1)+0.5 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.4950
</td>
<td style="text-align:right;">
0.0861
</td>
<td style="text-align:right;">
0.0003
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) t-test
</td>
<td style="text-align:right;">
0.8003
</td>
<td style="text-align:right;">
0.5697
</td>
<td style="text-align:right;">
0.1489
</td>
<td style="text-align:right;">
0.0100
</td>
<td style="text-align:right;">
0.0002
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.8698
</td>
<td style="text-align:right;">
0.6298
</td>
<td style="text-align:right;">
0.2128
</td>
<td style="text-align:right;">
0.0232
</td>
<td style="text-align:right;">
0.0015
</td>
<td style="text-align:right;">
0.0001
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8769
</td>
<td style="text-align:right;">
0.7790
</td>
<td style="text-align:right;">
0.4942
</td>
<td style="text-align:right;">
0.1854
</td>
<td style="text-align:right;">
0.0605
</td>
<td style="text-align:right;">
0.0175
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9243
</td>
<td style="text-align:right;">
0.8113
</td>
<td style="text-align:right;">
0.5301
</td>
<td style="text-align:right;">
0.2365
</td>
<td style="text-align:right;">
0.0851
</td>
<td style="text-align:right;">
0.0336
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) t-test
</td>
<td style="text-align:right;">
0.9116
</td>
<td style="text-align:right;">
0.8586
</td>
<td style="text-align:right;">
0.6865
</td>
<td style="text-align:right;">
0.4351
</td>
<td style="text-align:right;">
0.2514
</td>
<td style="text-align:right;">
0.1337
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9402
</td>
<td style="text-align:right;">
0.8790
</td>
<td style="text-align:right;">
0.7038
</td>
<td style="text-align:right;">
0.4739
</td>
<td style="text-align:right;">
0.2840
</td>
<td style="text-align:right;">
0.1782
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9258
</td>
<td style="text-align:right;">
0.8896
</td>
<td style="text-align:right;">
0.7850
</td>
<td style="text-align:right;">
0.5961
</td>
<td style="text-align:right;">
0.4447
</td>
<td style="text-align:right;">
0.3243
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9511
</td>
<td style="text-align:right;">
0.9050
</td>
<td style="text-align:right;">
0.7981
</td>
<td style="text-align:right;">
0.6280
</td>
<td style="text-align:right;">
0.4619
</td>
<td style="text-align:right;">
0.3566
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9326
</td>
<td style="text-align:right;">
0.9132
</td>
<td style="text-align:right;">
0.8319
</td>
<td style="text-align:right;">
0.7012
</td>
<td style="text-align:right;">
0.5866
</td>
<td style="text-align:right;">
0.4772
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9576
</td>
<td style="text-align:right;">
0.9220
</td>
<td style="text-align:right;">
0.8399
</td>
<td style="text-align:right;">
0.7185
</td>
<td style="text-align:right;">
0.6009
</td>
<td style="text-align:right;">
0.4969
</td>
</tr>
</tbody>
</table>
![](/research/img/simlulation/unnamed-chunk-15-1.png)
<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 15\] Type II error, 등분산,표본크기가 같은 경우: Chi-squared
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) t-test
</td>
<td style="text-align:right;">
0.8868
</td>
<td style="text-align:right;">
0.8364
</td>
<td style="text-align:right;">
0.7319
</td>
<td style="text-align:right;">
0.5456
</td>
<td style="text-align:right;">
0.4094
</td>
<td style="text-align:right;">
0.2921
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.8646
</td>
<td style="text-align:right;">
0.6730
</td>
<td style="text-align:right;">
0.3034
</td>
<td style="text-align:right;">
0.0587
</td>
<td style="text-align:right;">
0.0089
</td>
<td style="text-align:right;">
0.0014
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9380
</td>
<td style="text-align:right;">
0.9113
</td>
<td style="text-align:right;">
0.8486
</td>
<td style="text-align:right;">
0.7532
</td>
<td style="text-align:right;">
0.6582
</td>
<td style="text-align:right;">
0.5668
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.9384
</td>
<td style="text-align:right;">
0.8819
</td>
<td style="text-align:right;">
0.7402
</td>
<td style="text-align:right;">
0.5223
</td>
<td style="text-align:right;">
0.3436
</td>
<td style="text-align:right;">
0.2203
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9441
</td>
<td style="text-align:right;">
0.9277
</td>
<td style="text-align:right;">
0.8911
</td>
<td style="text-align:right;">
0.8188
</td>
<td style="text-align:right;">
0.7542
</td>
<td style="text-align:right;">
0.6946
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9558
</td>
<td style="text-align:right;">
0.9238
</td>
<td style="text-align:right;">
0.8460
</td>
<td style="text-align:right;">
0.7337
</td>
<td style="text-align:right;">
0.6281
</td>
<td style="text-align:right;">
0.5287
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9458
</td>
<td style="text-align:right;">
0.9308
</td>
<td style="text-align:right;">
0.9085
</td>
<td style="text-align:right;">
0.8590
</td>
<td style="text-align:right;">
0.8033
</td>
<td style="text-align:right;">
0.7619
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9602
</td>
<td style="text-align:right;">
0.9379
</td>
<td style="text-align:right;">
0.8803
</td>
<td style="text-align:right;">
0.8055
</td>
<td style="text-align:right;">
0.7402
</td>
<td style="text-align:right;">
0.6787
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) t-test
</td>
<td style="text-align:right;">
0.9461
</td>
<td style="text-align:right;">
0.9334
</td>
<td style="text-align:right;">
0.9079
</td>
<td style="text-align:right;">
0.8742
</td>
<td style="text-align:right;">
0.8436
</td>
<td style="text-align:right;">
0.7979
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9660
</td>
<td style="text-align:right;">
0.9447
</td>
<td style="text-align:right;">
0.9051
</td>
<td style="text-align:right;">
0.8568
</td>
<td style="text-align:right;">
0.8019
</td>
<td style="text-align:right;">
0.7467
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9506
</td>
<td style="text-align:right;">
0.9377
</td>
<td style="text-align:right;">
0.9211
</td>
<td style="text-align:right;">
0.8911
</td>
<td style="text-align:right;">
0.8522
</td>
<td style="text-align:right;">
0.8236
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9618
</td>
<td style="text-align:right;">
0.9405
</td>
<td style="text-align:right;">
0.9131
</td>
<td style="text-align:right;">
0.8748
</td>
<td style="text-align:right;">
0.8381
</td>
<td style="text-align:right;">
0.7908
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-16-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 16\] Type II error, 등분산,표본크기가 같은 경우: Exponential
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.8642
</td>
<td style="text-align:right;">
0.7770
</td>
<td style="text-align:right;">
0.5512
</td>
<td style="text-align:right;">
0.2927
</td>
<td style="text-align:right;">
0.1401
</td>
<td style="text-align:right;">
0.0638
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.8743
</td>
<td style="text-align:right;">
0.6954
</td>
<td style="text-align:right;">
0.3058
</td>
<td style="text-align:right;">
0.0665
</td>
<td style="text-align:right;">
0.0117
</td>
<td style="text-align:right;">
0.0021
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9409
</td>
<td style="text-align:right;">
0.9096
</td>
<td style="text-align:right;">
0.8456
</td>
<td style="text-align:right;">
0.7446
</td>
<td style="text-align:right;">
0.6516
</td>
<td style="text-align:right;">
0.5624
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.9437
</td>
<td style="text-align:right;">
0.8855
</td>
<td style="text-align:right;">
0.7312
</td>
<td style="text-align:right;">
0.5162
</td>
<td style="text-align:right;">
0.3484
</td>
<td style="text-align:right;">
0.2123
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9447
</td>
<td style="text-align:right;">
0.9345
</td>
<td style="text-align:right;">
0.9088
</td>
<td style="text-align:right;">
0.8606
</td>
<td style="text-align:right;">
0.8135
</td>
<td style="text-align:right;">
0.7823
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9548
</td>
<td style="text-align:right;">
0.9178
</td>
<td style="text-align:right;">
0.8461
</td>
<td style="text-align:right;">
0.7418
</td>
<td style="text-align:right;">
0.6284
</td>
<td style="text-align:right;">
0.5342
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9544
</td>
<td style="text-align:right;">
0.9465
</td>
<td style="text-align:right;">
0.9310
</td>
<td style="text-align:right;">
0.9046
</td>
<td style="text-align:right;">
0.8758
</td>
<td style="text-align:right;">
0.8500
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9665
</td>
<td style="text-align:right;">
0.9368
</td>
<td style="text-align:right;">
0.8870
</td>
<td style="text-align:right;">
0.8317
</td>
<td style="text-align:right;">
0.7584
</td>
<td style="text-align:right;">
0.7010
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9577
</td>
<td style="text-align:right;">
0.9489
</td>
<td style="text-align:right;">
0.9361
</td>
<td style="text-align:right;">
0.9176
</td>
<td style="text-align:right;">
0.9008
</td>
<td style="text-align:right;">
0.8893
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9641
</td>
<td style="text-align:right;">
0.9407
</td>
<td style="text-align:right;">
0.9108
</td>
<td style="text-align:right;">
0.8701
</td>
<td style="text-align:right;">
0.8320
</td>
<td style="text-align:right;">
0.7911
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9589
</td>
<td style="text-align:right;">
0.9518
</td>
<td style="text-align:right;">
0.9417
</td>
<td style="text-align:right;">
0.9289
</td>
<td style="text-align:right;">
0.9177
</td>
<td style="text-align:right;">
0.9092
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9674
</td>
<td style="text-align:right;">
0.9442
</td>
<td style="text-align:right;">
0.9240
</td>
<td style="text-align:right;">
0.8984
</td>
<td style="text-align:right;">
0.8683
</td>
<td style="text-align:right;">
0.8323
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-17-1.png)

## N=10000,등분산,표본크기가 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 17\] Type II error, 등분산,표본크기가 다른 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1)+0.5 N(0,1) t-test
</td>
<td style="text-align:right;">
0.8508
</td>
<td style="text-align:right;">
0.7361
</td>
<td style="text-align:right;">
0.6235
</td>
<td style="text-align:right;">
0.4363
</td>
<td style="text-align:right;">
0.2799
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1)+0.5 N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.8768
</td>
<td style="text-align:right;">
0.7524
</td>
<td style="text-align:right;">
0.6395
</td>
<td style="text-align:right;">
0.4578
</td>
<td style="text-align:right;">
0.3028
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) t-test
</td>
<td style="text-align:right;">
0.8970
</td>
<td style="text-align:right;">
0.8400
</td>
<td style="text-align:right;">
0.7811
</td>
<td style="text-align:right;">
0.6691
</td>
<td style="text-align:right;">
0.5641
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.9177
</td>
<td style="text-align:right;">
0.8478
</td>
<td style="text-align:right;">
0.8001
</td>
<td style="text-align:right;">
0.6884
</td>
<td style="text-align:right;">
0.5782
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,3) t-test
</td>
<td style="text-align:right;">
0.9151
</td>
<td style="text-align:right;">
0.8794
</td>
<td style="text-align:right;">
0.8458
</td>
<td style="text-align:right;">
0.7650
</td>
<td style="text-align:right;">
0.6798
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9350
</td>
<td style="text-align:right;">
0.8829
</td>
<td style="text-align:right;">
0.8465
</td>
<td style="text-align:right;">
0.7708
</td>
<td style="text-align:right;">
0.7005
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,4) t-test
</td>
<td style="text-align:right;">
0.9282
</td>
<td style="text-align:right;">
0.8975
</td>
<td style="text-align:right;">
0.8688
</td>
<td style="text-align:right;">
0.8131
</td>
<td style="text-align:right;">
0.7520
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9388
</td>
<td style="text-align:right;">
0.9045
</td>
<td style="text-align:right;">
0.8796
</td>
<td style="text-align:right;">
0.8197
</td>
<td style="text-align:right;">
0.7654
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+0.5 N(0,5) t-test
</td>
<td style="text-align:right;">
0.9287
</td>
<td style="text-align:right;">
0.9083
</td>
<td style="text-align:right;">
0.8870
</td>
<td style="text-align:right;">
0.8394
</td>
<td style="text-align:right;">
0.8012
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+0.5 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9375
</td>
<td style="text-align:right;">
0.9131
</td>
<td style="text-align:right;">
0.8940
</td>
<td style="text-align:right;">
0.8525
</td>
<td style="text-align:right;">
0.8024
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+0.5 N(0,6) t-test
</td>
<td style="text-align:right;">
0.9368
</td>
<td style="text-align:right;">
0.9123
</td>
<td style="text-align:right;">
0.8990
</td>
<td style="text-align:right;">
0.8585
</td>
<td style="text-align:right;">
0.8202
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+0.5 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9406
</td>
<td style="text-align:right;">
0.9136
</td>
<td style="text-align:right;">
0.9034
</td>
<td style="text-align:right;">
0.8669
</td>
<td style="text-align:right;">
0.8293
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-18-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 18\] Type II error, 등분산,표본크기가 다른 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,1)+0.5 U(0,1) t-test
</td>
<td style="text-align:right;">
0.1122
</td>
<td style="text-align:right;">
0.0020
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1)+0.5 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.2095
</td>
<td style="text-align:right;">
0.0094
</td>
<td style="text-align:right;">
0.0010
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) t-test
</td>
<td style="text-align:right;">
0.6526
</td>
<td style="text-align:right;">
0.3670
</td>
<td style="text-align:right;">
0.1797
</td>
<td style="text-align:right;">
0.0380
</td>
<td style="text-align:right;">
0.0056
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.7314
</td>
<td style="text-align:right;">
0.4381
</td>
<td style="text-align:right;">
0.2480
</td>
<td style="text-align:right;">
0.0718
</td>
<td style="text-align:right;">
0.0180
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8226
</td>
<td style="text-align:right;">
0.6688
</td>
<td style="text-align:right;">
0.5283
</td>
<td style="text-align:right;">
0.3026
</td>
<td style="text-align:right;">
0.1582
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.8564
</td>
<td style="text-align:right;">
0.7044
</td>
<td style="text-align:right;">
0.5690
</td>
<td style="text-align:right;">
0.3531
</td>
<td style="text-align:right;">
0.2135
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) t-test
</td>
<td style="text-align:right;">
0.8849
</td>
<td style="text-align:right;">
0.7916
</td>
<td style="text-align:right;">
0.7074
</td>
<td style="text-align:right;">
0.5475
</td>
<td style="text-align:right;">
0.4125
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9007
</td>
<td style="text-align:right;">
0.8057
</td>
<td style="text-align:right;">
0.7343
</td>
<td style="text-align:right;">
0.5848
</td>
<td style="text-align:right;">
0.4520
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9064
</td>
<td style="text-align:right;">
0.8480
</td>
<td style="text-align:right;">
0.7971
</td>
<td style="text-align:right;">
0.6925
</td>
<td style="text-align:right;">
0.5740
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9214
</td>
<td style="text-align:right;">
0.8586
</td>
<td style="text-align:right;">
0.8140
</td>
<td style="text-align:right;">
0.7080
</td>
<td style="text-align:right;">
0.6104
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9197
</td>
<td style="text-align:right;">
0.8826
</td>
<td style="text-align:right;">
0.8410
</td>
<td style="text-align:right;">
0.7639
</td>
<td style="text-align:right;">
0.6926
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9374
</td>
<td style="text-align:right;">
0.8904
</td>
<td style="text-align:right;">
0.8504
</td>
<td style="text-align:right;">
0.7793
</td>
<td style="text-align:right;">
0.7126
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-19-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 19\] Type II error, 등분산,표본크기가 다른 경우: Chi-squared
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) t-test
</td>
<td style="text-align:right;">
0.8867
</td>
<td style="text-align:right;">
0.8043
</td>
<td style="text-align:right;">
0.7338
</td>
<td style="text-align:right;">
0.6223
</td>
<td style="text-align:right;">
0.5186
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.7085
</td>
<td style="text-align:right;">
0.5072
</td>
<td style="text-align:right;">
0.3777
</td>
<td style="text-align:right;">
0.1780
</td>
<td style="text-align:right;">
0.0760
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9359
</td>
<td style="text-align:right;">
0.8958
</td>
<td style="text-align:right;">
0.8626
</td>
<td style="text-align:right;">
0.7995
</td>
<td style="text-align:right;">
0.7381
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.8880
</td>
<td style="text-align:right;">
0.8064
</td>
<td style="text-align:right;">
0.7295
</td>
<td style="text-align:right;">
0.6166
</td>
<td style="text-align:right;">
0.4921
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9384
</td>
<td style="text-align:right;">
0.9193
</td>
<td style="text-align:right;">
0.8975
</td>
<td style="text-align:right;">
0.8576
</td>
<td style="text-align:right;">
0.8175
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9209
</td>
<td style="text-align:right;">
0.8830
</td>
<td style="text-align:right;">
0.8452
</td>
<td style="text-align:right;">
0.7724
</td>
<td style="text-align:right;">
0.7100
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9467
</td>
<td style="text-align:right;">
0.9291
</td>
<td style="text-align:right;">
0.9101
</td>
<td style="text-align:right;">
0.8820
</td>
<td style="text-align:right;">
0.8493
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9340
</td>
<td style="text-align:right;">
0.9032
</td>
<td style="text-align:right;">
0.8839
</td>
<td style="text-align:right;">
0.8403
</td>
<td style="text-align:right;">
0.7907
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) t-test
</td>
<td style="text-align:right;">
0.9453
</td>
<td style="text-align:right;">
0.9339
</td>
<td style="text-align:right;">
0.9200
</td>
<td style="text-align:right;">
0.8980
</td>
<td style="text-align:right;">
0.8744
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9407
</td>
<td style="text-align:right;">
0.9138
</td>
<td style="text-align:right;">
0.8979
</td>
<td style="text-align:right;">
0.8711
</td>
<td style="text-align:right;">
0.8419
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9438
</td>
<td style="text-align:right;">
0.9325
</td>
<td style="text-align:right;">
0.9226
</td>
<td style="text-align:right;">
0.9008
</td>
<td style="text-align:right;">
0.8895
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9461
</td>
<td style="text-align:right;">
0.9250
</td>
<td style="text-align:right;">
0.9089
</td>
<td style="text-align:right;">
0.8915
</td>
<td style="text-align:right;">
0.8662
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-20-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 20\] Type II error, 등분산,표본크기가 다른 경우: Exponential
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n15, n5
</th>
<th style="text-align:right;">
n30, n10
</th>
<th style="text-align:right;">
n45, n15
</th>
<th style="text-align:right;">
n75, n25
</th>
<th style="text-align:right;">
n105, n35
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.8286
</td>
<td style="text-align:right;">
0.6931
</td>
<td style="text-align:right;">
0.5763
</td>
<td style="text-align:right;">
0.3977
</td>
<td style="text-align:right;">
0.2686
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.7374
</td>
<td style="text-align:right;">
0.5206
</td>
<td style="text-align:right;">
0.3608
</td>
<td style="text-align:right;">
0.1758
</td>
<td style="text-align:right;">
0.0774
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9362
</td>
<td style="text-align:right;">
0.8916
</td>
<td style="text-align:right;">
0.8636
</td>
<td style="text-align:right;">
0.8055
</td>
<td style="text-align:right;">
0.7425
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.8838
</td>
<td style="text-align:right;">
0.7945
</td>
<td style="text-align:right;">
0.7440
</td>
<td style="text-align:right;">
0.6050
</td>
<td style="text-align:right;">
0.4936
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9497
</td>
<td style="text-align:right;">
0.9320
</td>
<td style="text-align:right;">
0.9152
</td>
<td style="text-align:right;">
0.8870
</td>
<td style="text-align:right;">
0.8624
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9189
</td>
<td style="text-align:right;">
0.8678
</td>
<td style="text-align:right;">
0.8470
</td>
<td style="text-align:right;">
0.7744
</td>
<td style="text-align:right;">
0.7108
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9547
</td>
<td style="text-align:right;">
0.9427
</td>
<td style="text-align:right;">
0.9331
</td>
<td style="text-align:right;">
0.9190
</td>
<td style="text-align:right;">
0.8999
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9344
</td>
<td style="text-align:right;">
0.8980
</td>
<td style="text-align:right;">
0.8801
</td>
<td style="text-align:right;">
0.8450
</td>
<td style="text-align:right;">
0.8093
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9548
</td>
<td style="text-align:right;">
0.9508
</td>
<td style="text-align:right;">
0.9440
</td>
<td style="text-align:right;">
0.9302
</td>
<td style="text-align:right;">
0.9156
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9369
</td>
<td style="text-align:right;">
0.9197
</td>
<td style="text-align:right;">
0.9050
</td>
<td style="text-align:right;">
0.8777
</td>
<td style="text-align:right;">
0.8498
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9611
</td>
<td style="text-align:right;">
0.9495
</td>
<td style="text-align:right;">
0.9471
</td>
<td style="text-align:right;">
0.9400
</td>
<td style="text-align:right;">
0.9302
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9431
</td>
<td style="text-align:right;">
0.9251
</td>
<td style="text-align:right;">
0.9183
</td>
<td style="text-align:right;">
0.9030
</td>
<td style="text-align:right;">
0.8831
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-21-1.png)

## N=10000, 분산이 다르고, 표본크기는 같은 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 21\] Type II error, 표본크기는 같고 분산이 다른 경우: Normal
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
N(0,1.5)+0.5 N(0,1) t-test
</td>
<td style="text-align:right;">
0.9006
</td>
<td style="text-align:right;">
0.8423
</td>
<td style="text-align:right;">
0.6577
</td>
<td style="text-align:right;">
0.3989
</td>
<td style="text-align:right;">
0.2282
</td>
<td style="text-align:right;">
0.1231
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1.5)+0.5 N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.9306
</td>
<td style="text-align:right;">
0.8605
</td>
<td style="text-align:right;">
0.6763
</td>
<td style="text-align:right;">
0.4205
</td>
<td style="text-align:right;">
0.2453
</td>
<td style="text-align:right;">
0.1402
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) t-test
</td>
<td style="text-align:right;">
0.9169
</td>
<td style="text-align:right;">
0.8804
</td>
<td style="text-align:right;">
0.7786
</td>
<td style="text-align:right;">
0.5890
</td>
<td style="text-align:right;">
0.4248
</td>
<td style="text-align:right;">
0.3022
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.9459
</td>
<td style="text-align:right;">
0.8984
</td>
<td style="text-align:right;">
0.7763
</td>
<td style="text-align:right;">
0.5923
</td>
<td style="text-align:right;">
0.4439
</td>
<td style="text-align:right;">
0.3170
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5)+0.5 N(0,3) t-test
</td>
<td style="text-align:right;">
0.9297
</td>
<td style="text-align:right;">
0.9021
</td>
<td style="text-align:right;">
0.8217
</td>
<td style="text-align:right;">
0.6812
</td>
<td style="text-align:right;">
0.5467
</td>
<td style="text-align:right;">
0.4337
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5)+0.5 N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9539
</td>
<td style="text-align:right;">
0.9174
</td>
<td style="text-align:right;">
0.8262
</td>
<td style="text-align:right;">
0.6959
</td>
<td style="text-align:right;">
0.5685
</td>
<td style="text-align:right;">
0.4605
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,4) t-test
</td>
<td style="text-align:right;">
0.9375
</td>
<td style="text-align:right;">
0.9092
</td>
<td style="text-align:right;">
0.8448
</td>
<td style="text-align:right;">
0.7347
</td>
<td style="text-align:right;">
0.6387
</td>
<td style="text-align:right;">
0.5322
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9583
</td>
<td style="text-align:right;">
0.9234
</td>
<td style="text-align:right;">
0.8540
</td>
<td style="text-align:right;">
0.7437
</td>
<td style="text-align:right;">
0.6372
</td>
<td style="text-align:right;">
0.5507
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5)+0.5 N(0,5) t-test
</td>
<td style="text-align:right;">
0.9341
</td>
<td style="text-align:right;">
0.9244
</td>
<td style="text-align:right;">
0.8644
</td>
<td style="text-align:right;">
0.7795
</td>
<td style="text-align:right;">
0.6832
</td>
<td style="text-align:right;">
0.5923
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5)+0.5 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9571
</td>
<td style="text-align:right;">
0.9283
</td>
<td style="text-align:right;">
0.8693
</td>
<td style="text-align:right;">
0.7807
</td>
<td style="text-align:right;">
0.7052
</td>
<td style="text-align:right;">
0.6068
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,6) t-test
</td>
<td style="text-align:right;">
0.9366
</td>
<td style="text-align:right;">
0.9203
</td>
<td style="text-align:right;">
0.8804
</td>
<td style="text-align:right;">
0.7956
</td>
<td style="text-align:right;">
0.7258
</td>
<td style="text-align:right;">
0.6452
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9579
</td>
<td style="text-align:right;">
0.9354
</td>
<td style="text-align:right;">
0.8838
</td>
<td style="text-align:right;">
0.8076
</td>
<td style="text-align:right;">
0.7348
</td>
<td style="text-align:right;">
0.6598
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-22-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 22\] Type II error, 표본크기는 같고 분산이 다른 경우: Uniform
distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
U(0,2.46)-0.23 U(0,1) t-test
</td>
<td style="text-align:right;">
0.7758
</td>
<td style="text-align:right;">
0.5262
</td>
<td style="text-align:right;">
0.1076
</td>
<td style="text-align:right;">
0.0040
</td>
<td style="text-align:right;">
0.0000
</td>
<td style="text-align:right;">
0.0000
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.46)-0.23 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.8516
</td>
<td style="text-align:right;">
0.6667
</td>
<td style="text-align:right;">
0.3032
</td>
<td style="text-align:right;">
0.0580
</td>
<td style="text-align:right;">
0.0100
</td>
<td style="text-align:right;">
0.0015
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)+0.08 U(0,2) t-test
</td>
<td style="text-align:right;">
0.8456
</td>
<td style="text-align:right;">
0.6930
</td>
<td style="text-align:right;">
0.3240
</td>
<td style="text-align:right;">
0.0588
</td>
<td style="text-align:right;">
0.0086
</td>
<td style="text-align:right;">
0.0008
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)+0.08 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.9042
</td>
<td style="text-align:right;">
0.7583
</td>
<td style="text-align:right;">
0.4181
</td>
<td style="text-align:right;">
0.1270
</td>
<td style="text-align:right;">
0.0315
</td>
<td style="text-align:right;">
0.0075
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)+0.42 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8842
</td>
<td style="text-align:right;">
0.7901
</td>
<td style="text-align:right;">
0.5092
</td>
<td style="text-align:right;">
0.2053
</td>
<td style="text-align:right;">
0.0696
</td>
<td style="text-align:right;">
0.0222
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)+0.42 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9158
</td>
<td style="text-align:right;">
0.8218
</td>
<td style="text-align:right;">
0.5444
</td>
<td style="text-align:right;">
0.2534
</td>
<td style="text-align:right;">
0.1080
</td>
<td style="text-align:right;">
0.0398
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.75 U(0,4) t-test
</td>
<td style="text-align:right;">
0.9099
</td>
<td style="text-align:right;">
0.8477
</td>
<td style="text-align:right;">
0.6414
</td>
<td style="text-align:right;">
0.3771
</td>
<td style="text-align:right;">
0.2031
</td>
<td style="text-align:right;">
0.1026
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.75 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9383
</td>
<td style="text-align:right;">
0.8652
</td>
<td style="text-align:right;">
0.6801
</td>
<td style="text-align:right;">
0.4230
</td>
<td style="text-align:right;">
0.2455
</td>
<td style="text-align:right;">
0.1414
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+1.12 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9145
</td>
<td style="text-align:right;">
0.8743
</td>
<td style="text-align:right;">
0.7321
</td>
<td style="text-align:right;">
0.5191
</td>
<td style="text-align:right;">
0.3401
</td>
<td style="text-align:right;">
0.2143
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+1.12 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9447
</td>
<td style="text-align:right;">
0.9020
</td>
<td style="text-align:right;">
0.7658
</td>
<td style="text-align:right;">
0.5935
</td>
<td style="text-align:right;">
0.4313
</td>
<td style="text-align:right;">
0.3026
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9189
</td>
<td style="text-align:right;">
0.8931
</td>
<td style="text-align:right;">
0.7835
</td>
<td style="text-align:right;">
0.6128
</td>
<td style="text-align:right;">
0.4633
</td>
<td style="text-align:right;">
0.3264
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9483
</td>
<td style="text-align:right;">
0.9110
</td>
<td style="text-align:right;">
0.8200
</td>
<td style="text-align:right;">
0.7049
</td>
<td style="text-align:right;">
0.5740
</td>
<td style="text-align:right;">
0.4602
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-23-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 23\] Type II error, 표본크기는 같고 분산이 다른 경우:
Chi-squared distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
chisq(1.5)-0 chisq(1) t-test
</td>
<td style="text-align:right;">
0.9403
</td>
<td style="text-align:right;">
0.8981
</td>
<td style="text-align:right;">
0.7898
</td>
<td style="text-align:right;">
0.6339
</td>
<td style="text-align:right;">
0.4917
</td>
<td style="text-align:right;">
0.3848
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1.5)-0 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.9358
</td>
<td style="text-align:right;">
0.8701
</td>
<td style="text-align:right;">
0.6940
</td>
<td style="text-align:right;">
0.4458
</td>
<td style="text-align:right;">
0.2590
</td>
<td style="text-align:right;">
0.1463
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9343
</td>
<td style="text-align:right;">
0.9064
</td>
<td style="text-align:right;">
0.8468
</td>
<td style="text-align:right;">
0.7507
</td>
<td style="text-align:right;">
0.6608
</td>
<td style="text-align:right;">
0.5693
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.9415
</td>
<td style="text-align:right;">
0.8811
</td>
<td style="text-align:right;">
0.7332
</td>
<td style="text-align:right;">
0.5158
</td>
<td style="text-align:right;">
0.3376
</td>
<td style="text-align:right;">
0.2273
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+1 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9405
</td>
<td style="text-align:right;">
0.9219
</td>
<td style="text-align:right;">
0.8689
</td>
<td style="text-align:right;">
0.8106
</td>
<td style="text-align:right;">
0.7396
</td>
<td style="text-align:right;">
0.6641
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+1 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9507
</td>
<td style="text-align:right;">
0.9043
</td>
<td style="text-align:right;">
0.8024
</td>
<td style="text-align:right;">
0.6279
</td>
<td style="text-align:right;">
0.4908
</td>
<td style="text-align:right;">
0.3644
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9435
</td>
<td style="text-align:right;">
0.9326
</td>
<td style="text-align:right;">
0.8930
</td>
<td style="text-align:right;">
0.8401
</td>
<td style="text-align:right;">
0.7816
</td>
<td style="text-align:right;">
0.7298
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9545
</td>
<td style="text-align:right;">
0.9114
</td>
<td style="text-align:right;">
0.8323
</td>
<td style="text-align:right;">
0.6976
</td>
<td style="text-align:right;">
0.5786
</td>
<td style="text-align:right;">
0.4838
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+2 chisq(5) t-test
</td>
<td style="text-align:right;">
0.9393
</td>
<td style="text-align:right;">
0.9285
</td>
<td style="text-align:right;">
0.9007
</td>
<td style="text-align:right;">
0.8572
</td>
<td style="text-align:right;">
0.8162
</td>
<td style="text-align:right;">
0.7677
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+2 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9605
</td>
<td style="text-align:right;">
0.9215
</td>
<td style="text-align:right;">
0.8507
</td>
<td style="text-align:right;">
0.7498
</td>
<td style="text-align:right;">
0.6492
</td>
<td style="text-align:right;">
0.5506
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9420
</td>
<td style="text-align:right;">
0.9319
</td>
<td style="text-align:right;">
0.9108
</td>
<td style="text-align:right;">
0.8688
</td>
<td style="text-align:right;">
0.8355
</td>
<td style="text-align:right;">
0.8000
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9574
</td>
<td style="text-align:right;">
0.9260
</td>
<td style="text-align:right;">
0.8688
</td>
<td style="text-align:right;">
0.7792
</td>
<td style="text-align:right;">
0.7024
</td>
<td style="text-align:right;">
0.6164
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-24-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 24\] Type II error, 표본크기는 같고 분산이 다른 경우:
Exponential distribution
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
n5
</th>
<th style="text-align:right;">
n10
</th>
<th style="text-align:right;">
n25
</th>
<th style="text-align:right;">
n50
</th>
<th style="text-align:right;">
n75
</th>
<th style="text-align:right;">
n100
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.9382
</td>
<td style="text-align:right;">
0.8862
</td>
<td style="text-align:right;">
0.7413
</td>
<td style="text-align:right;">
0.4908
</td>
<td style="text-align:right;">
0.3148
</td>
<td style="text-align:right;">
0.1991
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.9461
</td>
<td style="text-align:right;">
0.8961
</td>
<td style="text-align:right;">
0.7749
</td>
<td style="text-align:right;">
0.5913
</td>
<td style="text-align:right;">
0.4327
</td>
<td style="text-align:right;">
0.3118
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9339
</td>
<td style="text-align:right;">
0.9088
</td>
<td style="text-align:right;">
0.8505
</td>
<td style="text-align:right;">
0.7568
</td>
<td style="text-align:right;">
0.6554
</td>
<td style="text-align:right;">
0.5772
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.9438
</td>
<td style="text-align:right;">
0.8818
</td>
<td style="text-align:right;">
0.7209
</td>
<td style="text-align:right;">
0.5087
</td>
<td style="text-align:right;">
0.3453
</td>
<td style="text-align:right;">
0.2245
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+1 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9310
</td>
<td style="text-align:right;">
0.9163
</td>
<td style="text-align:right;">
0.8828
</td>
<td style="text-align:right;">
0.8350
</td>
<td style="text-align:right;">
0.7930
</td>
<td style="text-align:right;">
0.7405
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+1 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9409
</td>
<td style="text-align:right;">
0.8806
</td>
<td style="text-align:right;">
0.7384
</td>
<td style="text-align:right;">
0.5198
</td>
<td style="text-align:right;">
0.3570
</td>
<td style="text-align:right;">
0.2376
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9278
</td>
<td style="text-align:right;">
0.9184
</td>
<td style="text-align:right;">
0.9011
</td>
<td style="text-align:right;">
0.8709
</td>
<td style="text-align:right;">
0.8429
</td>
<td style="text-align:right;">
0.8146
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9378
</td>
<td style="text-align:right;">
0.8802
</td>
<td style="text-align:right;">
0.7364
</td>
<td style="text-align:right;">
0.5396
</td>
<td style="text-align:right;">
0.3711
</td>
<td style="text-align:right;">
0.2493
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+2 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9320
</td>
<td style="text-align:right;">
0.9232
</td>
<td style="text-align:right;">
0.9102
</td>
<td style="text-align:right;">
0.8946
</td>
<td style="text-align:right;">
0.8826
</td>
<td style="text-align:right;">
0.8571
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+2 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9353
</td>
<td style="text-align:right;">
0.8781
</td>
<td style="text-align:right;">
0.7465
</td>
<td style="text-align:right;">
0.5492
</td>
<td style="text-align:right;">
0.3952
</td>
<td style="text-align:right;">
0.2633
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9310
</td>
<td style="text-align:right;">
0.9247
</td>
<td style="text-align:right;">
0.9172
</td>
<td style="text-align:right;">
0.9106
</td>
<td style="text-align:right;">
0.8850
</td>
<td style="text-align:right;">
0.8769
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9375
</td>
<td style="text-align:right;">
0.8867
</td>
<td style="text-align:right;">
0.7500
</td>
<td style="text-align:right;">
0.5567
</td>
<td style="text-align:right;">
0.4059
</td>
<td style="text-align:right;">
0.2726
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-25-1.png)
