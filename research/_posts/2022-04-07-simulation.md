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
\[Table 1\] 두 그룹의 표본크기, 분산 같은 경우: Normal distribution
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
0.0550
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0316
</td>
<td style="text-align:right;">
0.0414
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0481
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0557
</td>
<td style="text-align:right;">
0.0529
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0306
</td>
<td style="text-align:right;">
0.0397
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0553
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0545
</td>
<td style="text-align:right;">
0.0538
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0532
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0324
</td>
<td style="text-align:right;">
0.0444
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0507
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0510
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0332
</td>
<td style="text-align:right;">
0.0418
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0517
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0317
</td>
<td style="text-align:right;">
0.0455
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0464
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) t-test
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0484
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0308
</td>
<td style="text-align:right;">
0.0452
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0509
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-2-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 2\] 두 그룹의 표본크기, 분산 같은 경우: Uniform distribution
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
0.0542
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0513
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0300
</td>
<td style="text-align:right;">
0.0445
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0496
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) t-test
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0556
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0471
</td>
<td style="text-align:right;">
0.0517
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0347
</td>
<td style="text-align:right;">
0.0449
</td>
<td style="text-align:right;">
0.0533
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) t-test
</td>
<td style="text-align:right;">
0.0574
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0520
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0284
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0540
</td>
<td style="text-align:right;">
0.0482
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) t-test
</td>
<td style="text-align:right;">
0.0570
</td>
<td style="text-align:right;">
0.0539
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0557
</td>
<td style="text-align:right;">
0.0500
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0312
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0513
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) t-test
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0507
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0469
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) t-test
</td>
<td style="text-align:right;">
0.0548
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0531
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0283
</td>
<td style="text-align:right;">
0.0401
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-3-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 3\] 두 그룹의 표본크기, 분산 같은 경우: Chi-squared distribution
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
0.0299
</td>
<td style="text-align:right;">
0.0389
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0449
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0487
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0286
</td>
<td style="text-align:right;">
0.0412
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0533
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) t-test
</td>
<td style="text-align:right;">
0.0382
</td>
<td style="text-align:right;">
0.0445
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0489
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
0.0426
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0508
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) t-test
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0441
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0481
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0322
</td>
<td style="text-align:right;">
0.0396
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0514
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) t-test
</td>
<td style="text-align:right;">
0.0420
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0490
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0307
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0510
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) t-test
</td>
<td style="text-align:right;">
0.0425
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0426
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) t-test
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0460
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0324
</td>
<td style="text-align:right;">
0.0414
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0495
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
\[Table 4\] 두 그룹의 표본크기, 분산 같은 경우: Exponential distribution
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
0.0377
</td>
<td style="text-align:right;">
0.0433
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0543
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
0.0334
</td>
<td style="text-align:right;">
0.0452
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0536
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0412
</td>
<td style="text-align:right;">
0.0429
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0464
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0306
</td>
<td style="text-align:right;">
0.0423
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0515
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0372
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0461
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0518
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0443
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0481
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0406
</td>
<td style="text-align:right;">
0.0422
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0313
</td>
<td style="text-align:right;">
0.0420
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0517
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0381
</td>
<td style="text-align:right;">
0.0412
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0453
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0326
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0492
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0366
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0443
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0304
</td>
<td style="text-align:right;">
0.0441
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0509
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-5-1.png)

## N=10000,등분산,표본크기가 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 5\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Normal
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
0.0496
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0463
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0523
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0529
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0503
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
0.0494
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0502
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0474
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0391
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0518
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0507
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0424
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0536
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0420
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0507
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
0.0479
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0478
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0478
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-6-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 6\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Uniform
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
0.0479
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,1) U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0406
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0490
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) t-test
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2) U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0506
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) t-test
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0505
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3) U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0384
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0489
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4) U(0,4) t-test
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0456
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
0.0461
</td>
<td style="text-align:right;">
0.0454
</td>
<td style="text-align:right;">
0.0444
</td>
<td style="text-align:right;">
0.0541
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) t-test
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0537
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5) U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0437
</td>
<td style="text-align:right;">
0.0471
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0483
</td>
<td style="text-align:right;">
0.0498
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) t-test
</td>
<td style="text-align:right;">
0.0515
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0532
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6) U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0427
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0470
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-7-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 7\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Chi-squared
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
0.0393
</td>
<td style="text-align:right;">
0.0424
</td>
<td style="text-align:right;">
0.0418
</td>
<td style="text-align:right;">
0.0445
</td>
<td style="text-align:right;">
0.0480
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1) chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0394
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) t-test
</td>
<td style="text-align:right;">
0.0446
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0516
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2) chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0488
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) t-test
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0444
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0469
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3) chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0509
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
0.0468
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0473
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4) chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0404
</td>
<td style="text-align:right;">
0.0529
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0492
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) t-test
</td>
<td style="text-align:right;">
0.0437
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0506
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5) chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0410
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0470
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) t-test
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0472
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6) chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0406
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0486
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-8-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 8\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Exponential
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
0.0441
</td>
<td style="text-align:right;">
0.0453
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0483
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1) exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.0430
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0525
</td>
<td style="text-align:right;">
0.0485
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0501
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2) exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0395
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0505
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3) exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0448
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
exp(1/3) exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0434
</td>
<td style="text-align:right;">
0.0457
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0454
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0474
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0508
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4) exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0427
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0487
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0467
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5) exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0409
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0529
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6) exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0388
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0440
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0520
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-9-1.png)

## N=10000, 표본크기는 같고 분산이 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 9\] 두 그룹의 표본크기 같고, 분산 다른 경우: Normal distribution
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
0.0533
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0530
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1.5) N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0365
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0558
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) t-test
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0503
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2) N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0295
</td>
<td style="text-align:right;">
0.0457
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0510
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5) N(0,3) t-test
</td>
<td style="text-align:right;">
0.0463
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0495
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0510
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5) N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0343
</td>
<td style="text-align:right;">
0.0421
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,4) t-test
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0489
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3) N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0325
</td>
<td style="text-align:right;">
0.0411
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0504
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5) N(0,5) t-test
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0498
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0464
</td>
<td style="text-align:right;">
0.0532
</td>
<td style="text-align:right;">
0.0506
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5) N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0288
</td>
<td style="text-align:right;">
0.0438
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0475
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0482
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,6) t-test
</td>
<td style="text-align:right;">
0.0496
</td>
<td style="text-align:right;">
0.0555
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0468
</td>
<td style="text-align:right;">
0.0497
</td>
<td style="text-align:right;">
0.0506
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4) N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0296
</td>
<td style="text-align:right;">
0.0408
</td>
<td style="text-align:right;">
0.0476
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0497
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-10-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 10\] 두 그룹의 표본크기 같고, 분산 다른 경우: Uniform
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
0.0762
</td>
<td style="text-align:right;">
0.0627
</td>
<td style="text-align:right;">
0.0544
</td>
<td style="text-align:right;">
0.0512
</td>
<td style="text-align:right;">
0.0547
</td>
<td style="text-align:right;">
0.0474
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.46)-0.73 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.0491
</td>
<td style="text-align:right;">
0.0582
</td>
<td style="text-align:right;">
0.0669
</td>
<td style="text-align:right;">
0.0712
</td>
<td style="text-align:right;">
0.0704
</td>
<td style="text-align:right;">
0.0739
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)-0.42 U(0,2) t-test
</td>
<td style="text-align:right;">
0.0620
</td>
<td style="text-align:right;">
0.0537
</td>
<td style="text-align:right;">
0.0551
</td>
<td style="text-align:right;">
0.0508
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0531
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)-0.42 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.0358
</td>
<td style="text-align:right;">
0.0467
</td>
<td style="text-align:right;">
0.0553
</td>
<td style="text-align:right;">
0.0568
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0611
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)-0.08 U(0,3) t-test
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0524
</td>
<td style="text-align:right;">
0.0507
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0495
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)-0.08 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.0341
</td>
<td style="text-align:right;">
0.0436
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0532
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.25 U(0,4) t-test
</td>
<td style="text-align:right;">
0.0564
</td>
<td style="text-align:right;">
0.0521
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0522
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.25 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.0323
</td>
<td style="text-align:right;">
0.0457
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0531
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+0.62 U(0,5) t-test
</td>
<td style="text-align:right;">
0.0566
</td>
<td style="text-align:right;">
0.0540
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0503
</td>
<td style="text-align:right;">
0.0458
</td>
<td style="text-align:right;">
0.0519
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+0.62 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.0354
</td>
<td style="text-align:right;">
0.0469
</td>
<td style="text-align:right;">
0.0541
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0525
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1 U(0,6) t-test
</td>
<td style="text-align:right;">
0.0615
</td>
<td style="text-align:right;">
0.0529
</td>
<td style="text-align:right;">
0.0513
</td>
<td style="text-align:right;">
0.0528
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0493
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.0385
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0567
</td>
<td style="text-align:right;">
0.0584
</td>
<td style="text-align:right;">
0.0526
</td>
<td style="text-align:right;">
0.0555
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-11-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 11\] 두 그룹의 표본크기 같고, 분산 다른 경우: Chi-squared
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
chisq(1.5)-0.5 chisq(1) t-test
</td>
<td style="text-align:right;">
0.0423
</td>
<td style="text-align:right;">
0.0462
</td>
<td style="text-align:right;">
0.0479
</td>
<td style="text-align:right;">
0.0492
</td>
<td style="text-align:right;">
0.0480
</td>
<td style="text-align:right;">
0.0490
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1.5)-0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.0451
</td>
<td style="text-align:right;">
0.0719
</td>
<td style="text-align:right;">
0.1286
</td>
<td style="text-align:right;">
0.2136
</td>
<td style="text-align:right;">
0.2934
</td>
<td style="text-align:right;">
0.3617
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0 chisq(2) t-test
</td>
<td style="text-align:right;">
0.0384
</td>
<td style="text-align:right;">
0.0430
</td>
<td style="text-align:right;">
0.0487
</td>
<td style="text-align:right;">
0.0466
</td>
<td style="text-align:right;">
0.0518
</td>
<td style="text-align:right;">
0.0506
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
0.0434
</td>
<td style="text-align:right;">
0.0485
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.0402
</td>
<td style="text-align:right;">
0.0448
</td>
<td style="text-align:right;">
0.0464
</td>
<td style="text-align:right;">
0.0511
</td>
<td style="text-align:right;">
0.0489
</td>
<td style="text-align:right;">
0.0475
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.0303
</td>
<td style="text-align:right;">
0.0430
</td>
<td style="text-align:right;">
0.0564
</td>
<td style="text-align:right;">
0.0571
</td>
<td style="text-align:right;">
0.0597
</td>
<td style="text-align:right;">
0.0681
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1 chisq(4) t-test
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0459
</td>
<td style="text-align:right;">
0.0493
</td>
<td style="text-align:right;">
0.0509
</td>
<td style="text-align:right;">
0.0527
</td>
<td style="text-align:right;">
0.0507
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.0335
</td>
<td style="text-align:right;">
0.0424
</td>
<td style="text-align:right;">
0.0569
</td>
<td style="text-align:right;">
0.0593
</td>
<td style="text-align:right;">
0.0668
</td>
<td style="text-align:right;">
0.0728
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+1.5 chisq(5) t-test
</td>
<td style="text-align:right;">
0.0481
</td>
<td style="text-align:right;">
0.0506
</td>
<td style="text-align:right;">
0.0502
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0473
</td>
<td style="text-align:right;">
0.0496
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+1.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.0303
</td>
<td style="text-align:right;">
0.0456
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0602
</td>
<td style="text-align:right;">
0.0613
</td>
<td style="text-align:right;">
0.0759
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2 chisq(6) t-test
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0505
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0482
</td>
<td style="text-align:right;">
0.0510
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.0326
</td>
<td style="text-align:right;">
0.0472
</td>
<td style="text-align:right;">
0.0555
</td>
<td style="text-align:right;">
0.0652
</td>
<td style="text-align:right;">
0.0645
</td>
<td style="text-align:right;">
0.0668
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-12-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 12\] 두 그룹의 표본크기 같고, 분산 다른 경우: Exponential
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
exp(1/1.5)-0.5 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.0582
</td>
<td style="text-align:right;">
0.0561
</td>
<td style="text-align:right;">
0.0559
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0517
</td>
<td style="text-align:right;">
0.0476
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.0478
</td>
<td style="text-align:right;">
0.0738
</td>
<td style="text-align:right;">
0.1357
</td>
<td style="text-align:right;">
0.2326
</td>
<td style="text-align:right;">
0.3230
</td>
<td style="text-align:right;">
0.4193
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)-0 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.0388
</td>
<td style="text-align:right;">
0.0422
</td>
<td style="text-align:right;">
0.0444
</td>
<td style="text-align:right;">
0.0470
</td>
<td style="text-align:right;">
0.0542
</td>
<td style="text-align:right;">
0.0507
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)-0 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.0290
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0501
</td>
<td style="text-align:right;">
0.0490
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0472
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.0465
</td>
<td style="text-align:right;">
0.0435
</td>
<td style="text-align:right;">
0.0500
</td>
<td style="text-align:right;">
0.0484
</td>
<td style="text-align:right;">
0.0522
</td>
<td style="text-align:right;">
0.0530
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.0359
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0800
</td>
<td style="text-align:right;">
0.1074
</td>
<td style="text-align:right;">
0.1297
</td>
<td style="text-align:right;">
0.1570
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.0443
</td>
<td style="text-align:right;">
0.0477
</td>
<td style="text-align:right;">
0.0486
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0504
</td>
<td style="text-align:right;">
0.0537
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.0419
</td>
<td style="text-align:right;">
0.0582
</td>
<td style="text-align:right;">
0.1079
</td>
<td style="text-align:right;">
0.1547
</td>
<td style="text-align:right;">
0.2165
</td>
<td style="text-align:right;">
0.2708
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+1.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.0516
</td>
<td style="text-align:right;">
0.0514
</td>
<td style="text-align:right;">
0.0519
</td>
<td style="text-align:right;">
0.0494
</td>
<td style="text-align:right;">
0.0488
</td>
<td style="text-align:right;">
0.0528
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+1.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.0427
</td>
<td style="text-align:right;">
0.0722
</td>
<td style="text-align:right;">
0.1299
</td>
<td style="text-align:right;">
0.2034
</td>
<td style="text-align:right;">
0.2742
</td>
<td style="text-align:right;">
0.3560
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.0534
</td>
<td style="text-align:right;">
0.0564
</td>
<td style="text-align:right;">
0.0531
</td>
<td style="text-align:right;">
0.0535
</td>
<td style="text-align:right;">
0.0499
</td>
<td style="text-align:right;">
0.0491
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.0442
</td>
<td style="text-align:right;">
0.0785
</td>
<td style="text-align:right;">
0.1414
</td>
<td style="text-align:right;">
0.2278
</td>
<td style="text-align:right;">
0.3236
</td>
<td style="text-align:right;">
0.4090
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-13-1.png)

# Type II error

## N=10000,등분산,표본크기가 같은 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 13\] 두 그룹의 표본크기, 분산 같은 경우: Normal distribution
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
0.2073
</td>
<td style="text-align:right;">
0.0132
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
0.3185
</td>
<td style="text-align:right;">
0.0202
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
0.5039
</td>
<td style="text-align:right;">
0.1542
</td>
<td style="text-align:right;">
0.0017
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
0.6148
</td>
<td style="text-align:right;">
0.1936
</td>
<td style="text-align:right;">
0.0030
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
0.6328
</td>
<td style="text-align:right;">
0.3127
</td>
<td style="text-align:right;">
0.0228
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
0.7276
</td>
<td style="text-align:right;">
0.3659
</td>
<td style="text-align:right;">
0.0259
</td>
<td style="text-align:right;">
0.0005
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
0.7172
</td>
<td style="text-align:right;">
0.4388
</td>
<td style="text-align:right;">
0.0606
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
0.7879
</td>
<td style="text-align:right;">
0.4851
</td>
<td style="text-align:right;">
0.0778
</td>
<td style="text-align:right;">
0.0020
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
0.7586
</td>
<td style="text-align:right;">
0.5343
</td>
<td style="text-align:right;">
0.1289
</td>
<td style="text-align:right;">
0.0071
</td>
<td style="text-align:right;">
0.0003
</td>
<td style="text-align:right;">
1e-04
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+2 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.8293
</td>
<td style="text-align:right;">
0.5699
</td>
<td style="text-align:right;">
0.1500
</td>
<td style="text-align:right;">
0.0085
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
N(0,6)+2 N(0,6) t-test
</td>
<td style="text-align:right;">
0.7862
</td>
<td style="text-align:right;">
0.5883
</td>
<td style="text-align:right;">
0.1944
</td>
<td style="text-align:right;">
0.0189
</td>
<td style="text-align:right;">
0.0015
</td>
<td style="text-align:right;">
0e+00
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+2 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.8569
</td>
<td style="text-align:right;">
0.6412
</td>
<td style="text-align:right;">
0.2169
</td>
<td style="text-align:right;">
0.0221
</td>
<td style="text-align:right;">
0.0019
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
\[Table 14\] 두 그룹의 표본크기, 분산 같은 경우: Uniform distribution
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
0.3479
</td>
<td style="text-align:right;">
0.0333
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
0.5009
</td>
<td style="text-align:right;">
0.0898
</td>
<td style="text-align:right;">
0.0004
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
0.7929
</td>
<td style="text-align:right;">
0.5724
</td>
<td style="text-align:right;">
0.1505
</td>
<td style="text-align:right;">
0.0084
</td>
<td style="text-align:right;">
0.0004
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
0.8673
</td>
<td style="text-align:right;">
0.6346
</td>
<td style="text-align:right;">
0.2141
</td>
<td style="text-align:right;">
0.0214
</td>
<td style="text-align:right;">
0.0015
</td>
<td style="text-align:right;">
0.0002
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8871
</td>
<td style="text-align:right;">
0.7733
</td>
<td style="text-align:right;">
0.4975
</td>
<td style="text-align:right;">
0.1781
</td>
<td style="text-align:right;">
0.0611
</td>
<td style="text-align:right;">
0.0176
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9249
</td>
<td style="text-align:right;">
0.8131
</td>
<td style="text-align:right;">
0.5308
</td>
<td style="text-align:right;">
0.2428
</td>
<td style="text-align:right;">
0.0836
</td>
<td style="text-align:right;">
0.0303
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) t-test
</td>
<td style="text-align:right;">
0.9117
</td>
<td style="text-align:right;">
0.8523
</td>
<td style="text-align:right;">
0.6825
</td>
<td style="text-align:right;">
0.4317
</td>
<td style="text-align:right;">
0.2532
</td>
<td style="text-align:right;">
0.1409
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9484
</td>
<td style="text-align:right;">
0.8799
</td>
<td style="text-align:right;">
0.7068
</td>
<td style="text-align:right;">
0.4702
</td>
<td style="text-align:right;">
0.2885
</td>
<td style="text-align:right;">
0.1703
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9260
</td>
<td style="text-align:right;">
0.8894
</td>
<td style="text-align:right;">
0.7799
</td>
<td style="text-align:right;">
0.5979
</td>
<td style="text-align:right;">
0.4488
</td>
<td style="text-align:right;">
0.3164
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9540
</td>
<td style="text-align:right;">
0.9072
</td>
<td style="text-align:right;">
0.7970
</td>
<td style="text-align:right;">
0.6263
</td>
<td style="text-align:right;">
0.4688
</td>
<td style="text-align:right;">
0.3427
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9275
</td>
<td style="text-align:right;">
0.9097
</td>
<td style="text-align:right;">
0.8344
</td>
<td style="text-align:right;">
0.6963
</td>
<td style="text-align:right;">
0.5838
</td>
<td style="text-align:right;">
0.4774
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9606
</td>
<td style="text-align:right;">
0.9217
</td>
<td style="text-align:right;">
0.8465
</td>
<td style="text-align:right;">
0.7201
</td>
<td style="text-align:right;">
0.6139
</td>
<td style="text-align:right;">
0.4970
</td>
</tr>
</tbody>
</table>
![](/research/img/simlulation/unnamed-chunk-15-1.png)
<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 15\] 두 그룹의 표본크기, 분산 같은 경우: Chi-squared
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
0.8932
</td>
<td style="text-align:right;">
0.8446
</td>
<td style="text-align:right;">
0.7194
</td>
<td style="text-align:right;">
0.5492
</td>
<td style="text-align:right;">
0.4030
</td>
<td style="text-align:right;">
0.2929
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.8680
</td>
<td style="text-align:right;">
0.6789
</td>
<td style="text-align:right;">
0.2989
</td>
<td style="text-align:right;">
0.0643
</td>
<td style="text-align:right;">
0.0105
</td>
<td style="text-align:right;">
0.0012
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9345
</td>
<td style="text-align:right;">
0.9162
</td>
<td style="text-align:right;">
0.8508
</td>
<td style="text-align:right;">
0.7566
</td>
<td style="text-align:right;">
0.6524
</td>
<td style="text-align:right;">
0.5621
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.9437
</td>
<td style="text-align:right;">
0.8811
</td>
<td style="text-align:right;">
0.7330
</td>
<td style="text-align:right;">
0.5207
</td>
<td style="text-align:right;">
0.3485
</td>
<td style="text-align:right;">
0.2265
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9454
</td>
<td style="text-align:right;">
0.9271
</td>
<td style="text-align:right;">
0.8843
</td>
<td style="text-align:right;">
0.8217
</td>
<td style="text-align:right;">
0.7648
</td>
<td style="text-align:right;">
0.6943
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9566
</td>
<td style="text-align:right;">
0.9246
</td>
<td style="text-align:right;">
0.8450
</td>
<td style="text-align:right;">
0.7297
</td>
<td style="text-align:right;">
0.6297
</td>
<td style="text-align:right;">
0.5288
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9517
</td>
<td style="text-align:right;">
0.9282
</td>
<td style="text-align:right;">
0.9052
</td>
<td style="text-align:right;">
0.8574
</td>
<td style="text-align:right;">
0.8124
</td>
<td style="text-align:right;">
0.7586
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9629
</td>
<td style="text-align:right;">
0.9339
</td>
<td style="text-align:right;">
0.8910
</td>
<td style="text-align:right;">
0.8058
</td>
<td style="text-align:right;">
0.7453
</td>
<td style="text-align:right;">
0.6735
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
0.9355
</td>
<td style="text-align:right;">
0.9172
</td>
<td style="text-align:right;">
0.8707
</td>
<td style="text-align:right;">
0.8334
</td>
<td style="text-align:right;">
0.8007
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9631
</td>
<td style="text-align:right;">
0.9398
</td>
<td style="text-align:right;">
0.9035
</td>
<td style="text-align:right;">
0.8570
</td>
<td style="text-align:right;">
0.8010
</td>
<td style="text-align:right;">
0.7459
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9481
</td>
<td style="text-align:right;">
0.9412
</td>
<td style="text-align:right;">
0.9176
</td>
<td style="text-align:right;">
0.8909
</td>
<td style="text-align:right;">
0.8569
</td>
<td style="text-align:right;">
0.8268
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9649
</td>
<td style="text-align:right;">
0.9444
</td>
<td style="text-align:right;">
0.9191
</td>
<td style="text-align:right;">
0.8781
</td>
<td style="text-align:right;">
0.8326
</td>
<td style="text-align:right;">
0.7981
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-16-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 16\] 두 그룹의 표본크기, 분산 같은 경우: Exponential
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
0.8634
</td>
<td style="text-align:right;">
0.7804
</td>
<td style="text-align:right;">
0.5572
</td>
<td style="text-align:right;">
0.2963
</td>
<td style="text-align:right;">
0.1422
</td>
<td style="text-align:right;">
0.0644
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.8781
</td>
<td style="text-align:right;">
0.6931
</td>
<td style="text-align:right;">
0.3148
</td>
<td style="text-align:right;">
0.0612
</td>
<td style="text-align:right;">
0.0102
</td>
<td style="text-align:right;">
0.0015
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9364
</td>
<td style="text-align:right;">
0.9133
</td>
<td style="text-align:right;">
0.8456
</td>
<td style="text-align:right;">
0.7465
</td>
<td style="text-align:right;">
0.6615
</td>
<td style="text-align:right;">
0.5702
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.9429
</td>
<td style="text-align:right;">
0.8799
</td>
<td style="text-align:right;">
0.7336
</td>
<td style="text-align:right;">
0.5166
</td>
<td style="text-align:right;">
0.3416
</td>
<td style="text-align:right;">
0.2249
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9488
</td>
<td style="text-align:right;">
0.9364
</td>
<td style="text-align:right;">
0.9046
</td>
<td style="text-align:right;">
0.8657
</td>
<td style="text-align:right;">
0.8201
</td>
<td style="text-align:right;">
0.7760
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9538
</td>
<td style="text-align:right;">
0.9231
</td>
<td style="text-align:right;">
0.8467
</td>
<td style="text-align:right;">
0.7379
</td>
<td style="text-align:right;">
0.6362
</td>
<td style="text-align:right;">
0.5299
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9525
</td>
<td style="text-align:right;">
0.9472
</td>
<td style="text-align:right;">
0.9258
</td>
<td style="text-align:right;">
0.9039
</td>
<td style="text-align:right;">
0.8797
</td>
<td style="text-align:right;">
0.8510
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9614
</td>
<td style="text-align:right;">
0.9379
</td>
<td style="text-align:right;">
0.8899
</td>
<td style="text-align:right;">
0.8256
</td>
<td style="text-align:right;">
0.7639
</td>
<td style="text-align:right;">
0.7026
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9592
</td>
<td style="text-align:right;">
0.9484
</td>
<td style="text-align:right;">
0.9303
</td>
<td style="text-align:right;">
0.9192
</td>
<td style="text-align:right;">
0.9086
</td>
<td style="text-align:right;">
0.8839
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9651
</td>
<td style="text-align:right;">
0.9473
</td>
<td style="text-align:right;">
0.9119
</td>
<td style="text-align:right;">
0.8778
</td>
<td style="text-align:right;">
0.8303
</td>
<td style="text-align:right;">
0.7906
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9597
</td>
<td style="text-align:right;">
0.9561
</td>
<td style="text-align:right;">
0.9419
</td>
<td style="text-align:right;">
0.9296
</td>
<td style="text-align:right;">
0.9193
</td>
<td style="text-align:right;">
0.9080
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9640
</td>
<td style="text-align:right;">
0.9474
</td>
<td style="text-align:right;">
0.9292
</td>
<td style="text-align:right;">
0.8938
</td>
<td style="text-align:right;">
0.8673
</td>
<td style="text-align:right;">
0.8349
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-17-1.png)

## N=10000,등분산,표본크기가 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 17\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Normal
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
0.8504
</td>
<td style="text-align:right;">
0.7361
</td>
<td style="text-align:right;">
0.6148
</td>
<td style="text-align:right;">
0.4215
</td>
<td style="text-align:right;">
0.2770
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1)+0.5 N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.8757
</td>
<td style="text-align:right;">
0.7421
</td>
<td style="text-align:right;">
0.6484
</td>
<td style="text-align:right;">
0.4440
</td>
<td style="text-align:right;">
0.3079
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) t-test
</td>
<td style="text-align:right;">
0.9024
</td>
<td style="text-align:right;">
0.8375
</td>
<td style="text-align:right;">
0.7884
</td>
<td style="text-align:right;">
0.6718
</td>
<td style="text-align:right;">
0.5645
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.9144
</td>
<td style="text-align:right;">
0.8532
</td>
<td style="text-align:right;">
0.7975
</td>
<td style="text-align:right;">
0.6804
</td>
<td style="text-align:right;">
0.5827
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,3) t-test
</td>
<td style="text-align:right;">
0.9229
</td>
<td style="text-align:right;">
0.8779
</td>
<td style="text-align:right;">
0.8407
</td>
<td style="text-align:right;">
0.7579
</td>
<td style="text-align:right;">
0.6873
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9330
</td>
<td style="text-align:right;">
0.8836
</td>
<td style="text-align:right;">
0.8487
</td>
<td style="text-align:right;">
0.7757
</td>
<td style="text-align:right;">
0.6939
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,4) t-test
</td>
<td style="text-align:right;">
0.9292
</td>
<td style="text-align:right;">
0.8964
</td>
<td style="text-align:right;">
0.8682
</td>
<td style="text-align:right;">
0.8136
</td>
<td style="text-align:right;">
0.7583
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9377
</td>
<td style="text-align:right;">
0.9040
</td>
<td style="text-align:right;">
0.8750
</td>
<td style="text-align:right;">
0.8122
</td>
<td style="text-align:right;">
0.7565
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+0.5 N(0,5) t-test
</td>
<td style="text-align:right;">
0.9293
</td>
<td style="text-align:right;">
0.9115
</td>
<td style="text-align:right;">
0.8877
</td>
<td style="text-align:right;">
0.8419
</td>
<td style="text-align:right;">
0.7968
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,5)+0.5 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9443
</td>
<td style="text-align:right;">
0.9115
</td>
<td style="text-align:right;">
0.8886
</td>
<td style="text-align:right;">
0.8464
</td>
<td style="text-align:right;">
0.8058
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+0.5 N(0,6) t-test
</td>
<td style="text-align:right;">
0.9314
</td>
<td style="text-align:right;">
0.9165
</td>
<td style="text-align:right;">
0.8941
</td>
<td style="text-align:right;">
0.8593
</td>
<td style="text-align:right;">
0.8155
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,6)+0.5 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9473
</td>
<td style="text-align:right;">
0.9169
</td>
<td style="text-align:right;">
0.8989
</td>
<td style="text-align:right;">
0.8658
</td>
<td style="text-align:right;">
0.8243
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-18-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 18\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Uniform
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
0.1029
</td>
<td style="text-align:right;">
0.0031
</td>
<td style="text-align:right;">
0.0001
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
0.1965
</td>
<td style="text-align:right;">
0.0114
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
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) t-test
</td>
<td style="text-align:right;">
0.6648
</td>
<td style="text-align:right;">
0.3661
</td>
<td style="text-align:right;">
0.1900
</td>
<td style="text-align:right;">
0.0394
</td>
<td style="text-align:right;">
0.0065
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2)+0.5 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.7178
</td>
<td style="text-align:right;">
0.4426
</td>
<td style="text-align:right;">
0.2495
</td>
<td style="text-align:right;">
0.0705
</td>
<td style="text-align:right;">
0.0164
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8236
</td>
<td style="text-align:right;">
0.6692
</td>
<td style="text-align:right;">
0.5253
</td>
<td style="text-align:right;">
0.3072
</td>
<td style="text-align:right;">
0.1577
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3)+0.5 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.8597
</td>
<td style="text-align:right;">
0.7068
</td>
<td style="text-align:right;">
0.5734
</td>
<td style="text-align:right;">
0.3501
</td>
<td style="text-align:right;">
0.2025
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) t-test
</td>
<td style="text-align:right;">
0.8821
</td>
<td style="text-align:right;">
0.7976
</td>
<td style="text-align:right;">
0.7097
</td>
<td style="text-align:right;">
0.5436
</td>
<td style="text-align:right;">
0.4167
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4)+0.5 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.8936
</td>
<td style="text-align:right;">
0.8139
</td>
<td style="text-align:right;">
0.7353
</td>
<td style="text-align:right;">
0.5743
</td>
<td style="text-align:right;">
0.4553
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9061
</td>
<td style="text-align:right;">
0.8548
</td>
<td style="text-align:right;">
0.8016
</td>
<td style="text-align:right;">
0.6951
</td>
<td style="text-align:right;">
0.5837
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,5)+0.5 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9217
</td>
<td style="text-align:right;">
0.8609
</td>
<td style="text-align:right;">
0.8134
</td>
<td style="text-align:right;">
0.7112
</td>
<td style="text-align:right;">
0.6076
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9185
</td>
<td style="text-align:right;">
0.8827
</td>
<td style="text-align:right;">
0.8465
</td>
<td style="text-align:right;">
0.7603
</td>
<td style="text-align:right;">
0.6981
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,6)+0.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9322
</td>
<td style="text-align:right;">
0.8892
</td>
<td style="text-align:right;">
0.8489
</td>
<td style="text-align:right;">
0.7847
</td>
<td style="text-align:right;">
0.6994
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-19-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 19\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Chi-squared
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
0.8874
</td>
<td style="text-align:right;">
0.8044
</td>
<td style="text-align:right;">
0.7442
</td>
<td style="text-align:right;">
0.6211
</td>
<td style="text-align:right;">
0.5245
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1)+0.5 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.7101
</td>
<td style="text-align:right;">
0.5175
</td>
<td style="text-align:right;">
0.3662
</td>
<td style="text-align:right;">
0.1710
</td>
<td style="text-align:right;">
0.0774
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9318
</td>
<td style="text-align:right;">
0.8968
</td>
<td style="text-align:right;">
0.8641
</td>
<td style="text-align:right;">
0.8070
</td>
<td style="text-align:right;">
0.7356
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)+0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.8813
</td>
<td style="text-align:right;">
0.8011
</td>
<td style="text-align:right;">
0.7395
</td>
<td style="text-align:right;">
0.6114
</td>
<td style="text-align:right;">
0.4982
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9411
</td>
<td style="text-align:right;">
0.9137
</td>
<td style="text-align:right;">
0.8909
</td>
<td style="text-align:right;">
0.8555
</td>
<td style="text-align:right;">
0.8147
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+0.5 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9223
</td>
<td style="text-align:right;">
0.8732
</td>
<td style="text-align:right;">
0.8469
</td>
<td style="text-align:right;">
0.7729
</td>
<td style="text-align:right;">
0.7136
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9461
</td>
<td style="text-align:right;">
0.9294
</td>
<td style="text-align:right;">
0.9139
</td>
<td style="text-align:right;">
0.8831
</td>
<td style="text-align:right;">
0.8535
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+0.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9314
</td>
<td style="text-align:right;">
0.9004
</td>
<td style="text-align:right;">
0.8805
</td>
<td style="text-align:right;">
0.8382
</td>
<td style="text-align:right;">
0.7958
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) t-test
</td>
<td style="text-align:right;">
0.9500
</td>
<td style="text-align:right;">
0.9322
</td>
<td style="text-align:right;">
0.9173
</td>
<td style="text-align:right;">
0.8983
</td>
<td style="text-align:right;">
0.8739
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(5)+0.5 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9429
</td>
<td style="text-align:right;">
0.9164
</td>
<td style="text-align:right;">
0.9028
</td>
<td style="text-align:right;">
0.8672
</td>
<td style="text-align:right;">
0.8362
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9488
</td>
<td style="text-align:right;">
0.9319
</td>
<td style="text-align:right;">
0.9233
</td>
<td style="text-align:right;">
0.9071
</td>
<td style="text-align:right;">
0.8913
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(6)+0.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9454
</td>
<td style="text-align:right;">
0.9232
</td>
<td style="text-align:right;">
0.9188
</td>
<td style="text-align:right;">
0.8873
</td>
<td style="text-align:right;">
0.8629
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-20-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 20\] 두 그룹의 표본크기는 다르고, 분산 같은 경우: Exponential
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
0.8237
</td>
<td style="text-align:right;">
0.6934
</td>
<td style="text-align:right;">
0.5784
</td>
<td style="text-align:right;">
0.4026
</td>
<td style="text-align:right;">
0.2638
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1)+0.5 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.7364
</td>
<td style="text-align:right;">
0.5187
</td>
<td style="text-align:right;">
0.3729
</td>
<td style="text-align:right;">
0.1778
</td>
<td style="text-align:right;">
0.0788
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9382
</td>
<td style="text-align:right;">
0.8908
</td>
<td style="text-align:right;">
0.8620
</td>
<td style="text-align:right;">
0.7939
</td>
<td style="text-align:right;">
0.7452
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.8858
</td>
<td style="text-align:right;">
0.7953
</td>
<td style="text-align:right;">
0.7275
</td>
<td style="text-align:right;">
0.6077
</td>
<td style="text-align:right;">
0.4988
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9485
</td>
<td style="text-align:right;">
0.9327
</td>
<td style="text-align:right;">
0.9147
</td>
<td style="text-align:right;">
0.8920
</td>
<td style="text-align:right;">
0.8607
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+0.5 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9207
</td>
<td style="text-align:right;">
0.8688
</td>
<td style="text-align:right;">
0.8454
</td>
<td style="text-align:right;">
0.7751
</td>
<td style="text-align:right;">
0.7099
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9575
</td>
<td style="text-align:right;">
0.9432
</td>
<td style="text-align:right;">
0.9316
</td>
<td style="text-align:right;">
0.9146
</td>
<td style="text-align:right;">
0.9015
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+0.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9333
</td>
<td style="text-align:right;">
0.9006
</td>
<td style="text-align:right;">
0.8842
</td>
<td style="text-align:right;">
0.8446
</td>
<td style="text-align:right;">
0.8092
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9578
</td>
<td style="text-align:right;">
0.9489
</td>
<td style="text-align:right;">
0.9390
</td>
<td style="text-align:right;">
0.9324
</td>
<td style="text-align:right;">
0.9231
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/5)+0.5 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9377
</td>
<td style="text-align:right;">
0.9164
</td>
<td style="text-align:right;">
0.9005
</td>
<td style="text-align:right;">
0.8776
</td>
<td style="text-align:right;">
0.8563
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9579
</td>
<td style="text-align:right;">
0.9478
</td>
<td style="text-align:right;">
0.9461
</td>
<td style="text-align:right;">
0.9425
</td>
<td style="text-align:right;">
0.9293
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/6)+0.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9478
</td>
<td style="text-align:right;">
0.9241
</td>
<td style="text-align:right;">
0.9182
</td>
<td style="text-align:right;">
0.9024
</td>
<td style="text-align:right;">
0.8830
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-21-1.png)

## N=10000, 표본크기는 같고 분산이 다른 경우

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 21\] 두 그룹의 표본크기 같고, 분산 다른 경우: Normal
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
0.9031
</td>
<td style="text-align:right;">
0.8430
</td>
<td style="text-align:right;">
0.6623
</td>
<td style="text-align:right;">
0.4024
</td>
<td style="text-align:right;">
0.2262
</td>
<td style="text-align:right;">
0.1148
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,1.5)+0.5 N(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.9358
</td>
<td style="text-align:right;">
0.8669
</td>
<td style="text-align:right;">
0.6725
</td>
<td style="text-align:right;">
0.4245
</td>
<td style="text-align:right;">
0.2418
</td>
<td style="text-align:right;">
0.1392
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) t-test
</td>
<td style="text-align:right;">
0.9227
</td>
<td style="text-align:right;">
0.8816
</td>
<td style="text-align:right;">
0.7617
</td>
<td style="text-align:right;">
0.5767
</td>
<td style="text-align:right;">
0.4136
</td>
<td style="text-align:right;">
0.2988
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2)+0.5 N(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.9466
</td>
<td style="text-align:right;">
0.8940
</td>
<td style="text-align:right;">
0.7765
</td>
<td style="text-align:right;">
0.6028
</td>
<td style="text-align:right;">
0.4368
</td>
<td style="text-align:right;">
0.3174
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5)+0.5 N(0,3) t-test
</td>
<td style="text-align:right;">
0.9271
</td>
<td style="text-align:right;">
0.9034
</td>
<td style="text-align:right;">
0.8219
</td>
<td style="text-align:right;">
0.6841
</td>
<td style="text-align:right;">
0.5403
</td>
<td style="text-align:right;">
0.4385
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,2.5)+0.5 N(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9522
</td>
<td style="text-align:right;">
0.9133
</td>
<td style="text-align:right;">
0.8251
</td>
<td style="text-align:right;">
0.6975
</td>
<td style="text-align:right;">
0.5625
</td>
<td style="text-align:right;">
0.4484
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,4) t-test
</td>
<td style="text-align:right;">
0.9302
</td>
<td style="text-align:right;">
0.9136
</td>
<td style="text-align:right;">
0.8463
</td>
<td style="text-align:right;">
0.7354
</td>
<td style="text-align:right;">
0.6317
</td>
<td style="text-align:right;">
0.5379
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3)+0.5 N(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9591
</td>
<td style="text-align:right;">
0.9239
</td>
<td style="text-align:right;">
0.8492
</td>
<td style="text-align:right;">
0.7485
</td>
<td style="text-align:right;">
0.6516
</td>
<td style="text-align:right;">
0.5495
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5)+0.5 N(0,5) t-test
</td>
<td style="text-align:right;">
0.9375
</td>
<td style="text-align:right;">
0.9191
</td>
<td style="text-align:right;">
0.8617
</td>
<td style="text-align:right;">
0.7748
</td>
<td style="text-align:right;">
0.6860
</td>
<td style="text-align:right;">
0.6015
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,3.5)+0.5 N(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9595
</td>
<td style="text-align:right;">
0.9286
</td>
<td style="text-align:right;">
0.8738
</td>
<td style="text-align:right;">
0.7809
</td>
<td style="text-align:right;">
0.6970
</td>
<td style="text-align:right;">
0.6173
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,6) t-test
</td>
<td style="text-align:right;">
0.9376
</td>
<td style="text-align:right;">
0.9207
</td>
<td style="text-align:right;">
0.8781
</td>
<td style="text-align:right;">
0.8048
</td>
<td style="text-align:right;">
0.7300
</td>
<td style="text-align:right;">
0.6515
</td>
</tr>
<tr>
<td style="text-align:left;">
N(0,4)+0.5 N(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9593
</td>
<td style="text-align:right;">
0.9331
</td>
<td style="text-align:right;">
0.8879
</td>
<td style="text-align:right;">
0.8122
</td>
<td style="text-align:right;">
0.7375
</td>
<td style="text-align:right;">
0.6653
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-22-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 22\] 두 그룹의 표본크기 같고, 분산 다른 경우: Uniform
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
0.7666
</td>
<td style="text-align:right;">
0.5289
</td>
<td style="text-align:right;">
0.1121
</td>
<td style="text-align:right;">
0.0036
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
U(0,2.46)-0.23 U(0,1) Wilcoxon
</td>
<td style="text-align:right;">
0.8532
</td>
<td style="text-align:right;">
0.6665
</td>
<td style="text-align:right;">
0.2856
</td>
<td style="text-align:right;">
0.0602
</td>
<td style="text-align:right;">
0.0117
</td>
<td style="text-align:right;">
0.0012
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)+0.08 U(0,2) t-test
</td>
<td style="text-align:right;">
0.8409
</td>
<td style="text-align:right;">
0.7000
</td>
<td style="text-align:right;">
0.3146
</td>
<td style="text-align:right;">
0.0577
</td>
<td style="text-align:right;">
0.0109
</td>
<td style="text-align:right;">
0.0011
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,2.84)+0.08 U(0,2) Wilcoxon
</td>
<td style="text-align:right;">
0.8996
</td>
<td style="text-align:right;">
0.7538
</td>
<td style="text-align:right;">
0.4097
</td>
<td style="text-align:right;">
0.1227
</td>
<td style="text-align:right;">
0.0360
</td>
<td style="text-align:right;">
0.0079
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)+0.42 U(0,3) t-test
</td>
<td style="text-align:right;">
0.8858
</td>
<td style="text-align:right;">
0.7867
</td>
<td style="text-align:right;">
0.5144
</td>
<td style="text-align:right;">
0.2081
</td>
<td style="text-align:right;">
0.0687
</td>
<td style="text-align:right;">
0.0230
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.16)+0.42 U(0,3) Wilcoxon
</td>
<td style="text-align:right;">
0.9295
</td>
<td style="text-align:right;">
0.8106
</td>
<td style="text-align:right;">
0.5481
</td>
<td style="text-align:right;">
0.2581
</td>
<td style="text-align:right;">
0.1020
</td>
<td style="text-align:right;">
0.0354
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.75 U(0,4) t-test
</td>
<td style="text-align:right;">
0.9048
</td>
<td style="text-align:right;">
0.8398
</td>
<td style="text-align:right;">
0.6532
</td>
<td style="text-align:right;">
0.3756
</td>
<td style="text-align:right;">
0.1975
</td>
<td style="text-align:right;">
0.1029
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.5)+0.75 U(0,4) Wilcoxon
</td>
<td style="text-align:right;">
0.9413
</td>
<td style="text-align:right;">
0.8649
</td>
<td style="text-align:right;">
0.6755
</td>
<td style="text-align:right;">
0.4168
</td>
<td style="text-align:right;">
0.2557
</td>
<td style="text-align:right;">
0.1347
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+1.12 U(0,5) t-test
</td>
<td style="text-align:right;">
0.9150
</td>
<td style="text-align:right;">
0.8703
</td>
<td style="text-align:right;">
0.7218
</td>
<td style="text-align:right;">
0.5137
</td>
<td style="text-align:right;">
0.3350
</td>
<td style="text-align:right;">
0.2124
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,3.76)+1.12 U(0,5) Wilcoxon
</td>
<td style="text-align:right;">
0.9472
</td>
<td style="text-align:right;">
0.8954
</td>
<td style="text-align:right;">
0.7683
</td>
<td style="text-align:right;">
0.5919
</td>
<td style="text-align:right;">
0.4381
</td>
<td style="text-align:right;">
0.3129
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1.5 U(0,6) t-test
</td>
<td style="text-align:right;">
0.9159
</td>
<td style="text-align:right;">
0.8950
</td>
<td style="text-align:right;">
0.7849
</td>
<td style="text-align:right;">
0.6207
</td>
<td style="text-align:right;">
0.4541
</td>
<td style="text-align:right;">
0.3316
</td>
</tr>
<tr>
<td style="text-align:left;">
U(0,4.0)+1.5 U(0,6) Wilcoxon
</td>
<td style="text-align:right;">
0.9488
</td>
<td style="text-align:right;">
0.9162
</td>
<td style="text-align:right;">
0.8209
</td>
<td style="text-align:right;">
0.6987
</td>
<td style="text-align:right;">
0.5743
</td>
<td style="text-align:right;">
0.4663
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-23-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 23\] 두 그룹의 표본크기 같고, 분산 다른 경우: Chi-squared
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
chisq(1.5)-0 chisq(1) t-test
</td>
<td style="text-align:right;">
0.9418
</td>
<td style="text-align:right;">
0.9019
</td>
<td style="text-align:right;">
0.7932
</td>
<td style="text-align:right;">
0.6323
</td>
<td style="text-align:right;">
0.5044
</td>
<td style="text-align:right;">
0.3849
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(1.5)-0 chisq(1) Wilcoxon
</td>
<td style="text-align:right;">
0.9365
</td>
<td style="text-align:right;">
0.8670
</td>
<td style="text-align:right;">
0.6905
</td>
<td style="text-align:right;">
0.4446
</td>
<td style="text-align:right;">
0.2651
</td>
<td style="text-align:right;">
0.1533
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0.5 chisq(2) t-test
</td>
<td style="text-align:right;">
0.9384
</td>
<td style="text-align:right;">
0.9143
</td>
<td style="text-align:right;">
0.8461
</td>
<td style="text-align:right;">
0.7479
</td>
<td style="text-align:right;">
0.6608
</td>
<td style="text-align:right;">
0.5676
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2)-0.5 chisq(2) Wilcoxon
</td>
<td style="text-align:right;">
0.9456
</td>
<td style="text-align:right;">
0.8863
</td>
<td style="text-align:right;">
0.7353
</td>
<td style="text-align:right;">
0.5148
</td>
<td style="text-align:right;">
0.3453
</td>
<td style="text-align:right;">
0.2234
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+1 chisq(3) t-test
</td>
<td style="text-align:right;">
0.9381
</td>
<td style="text-align:right;">
0.9167
</td>
<td style="text-align:right;">
0.8753
</td>
<td style="text-align:right;">
0.8062
</td>
<td style="text-align:right;">
0.7305
</td>
<td style="text-align:right;">
0.6627
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(2.5)+1 chisq(3) Wilcoxon
</td>
<td style="text-align:right;">
0.9537
</td>
<td style="text-align:right;">
0.9056
</td>
<td style="text-align:right;">
0.7916
</td>
<td style="text-align:right;">
0.6307
</td>
<td style="text-align:right;">
0.4861
</td>
<td style="text-align:right;">
0.3660
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1.5 chisq(4) t-test
</td>
<td style="text-align:right;">
0.9431
</td>
<td style="text-align:right;">
0.9235
</td>
<td style="text-align:right;">
0.8882
</td>
<td style="text-align:right;">
0.8362
</td>
<td style="text-align:right;">
0.7851
</td>
<td style="text-align:right;">
0.7257
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3)+1.5 chisq(4) Wilcoxon
</td>
<td style="text-align:right;">
0.9558
</td>
<td style="text-align:right;">
0.9135
</td>
<td style="text-align:right;">
0.8253
</td>
<td style="text-align:right;">
0.6987
</td>
<td style="text-align:right;">
0.5901
</td>
<td style="text-align:right;">
0.4733
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+2 chisq(5) t-test
</td>
<td style="text-align:right;">
0.9421
</td>
<td style="text-align:right;">
0.9278
</td>
<td style="text-align:right;">
0.9024
</td>
<td style="text-align:right;">
0.8530
</td>
<td style="text-align:right;">
0.8070
</td>
<td style="text-align:right;">
0.7636
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(3.5)+2 chisq(5) Wilcoxon
</td>
<td style="text-align:right;">
0.9550
</td>
<td style="text-align:right;">
0.9277
</td>
<td style="text-align:right;">
0.8534
</td>
<td style="text-align:right;">
0.7428
</td>
<td style="text-align:right;">
0.6443
</td>
<td style="text-align:right;">
0.5559
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2.5 chisq(6) t-test
</td>
<td style="text-align:right;">
0.9415
</td>
<td style="text-align:right;">
0.9342
</td>
<td style="text-align:right;">
0.9078
</td>
<td style="text-align:right;">
0.8723
</td>
<td style="text-align:right;">
0.8336
</td>
<td style="text-align:right;">
0.8022
</td>
</tr>
<tr>
<td style="text-align:left;">
chisq(4)+2.5 chisq(6) Wilcoxon
</td>
<td style="text-align:right;">
0.9591
</td>
<td style="text-align:right;">
0.9295
</td>
<td style="text-align:right;">
0.8701
</td>
<td style="text-align:right;">
0.7841
</td>
<td style="text-align:right;">
0.6930
</td>
<td style="text-align:right;">
0.6106
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-24-1.png)

<table class=" lightable-classic-2" style="font-size: 15px; font-family: Cambria; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">
\[Table 24\] 두 그룹의 표본크기 같고, 분산 다른 경우: Exponential
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
exp(1/1.5)-0 exp(1/1) t-test
</td>
<td style="text-align:right;">
0.9372
</td>
<td style="text-align:right;">
0.8910
</td>
<td style="text-align:right;">
0.7377
</td>
<td style="text-align:right;">
0.4926
</td>
<td style="text-align:right;">
0.3197
</td>
<td style="text-align:right;">
0.1930
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/1.5)-0 exp(1/1) Wilcoxon
</td>
<td style="text-align:right;">
0.9484
</td>
<td style="text-align:right;">
0.8986
</td>
<td style="text-align:right;">
0.7673
</td>
<td style="text-align:right;">
0.5968
</td>
<td style="text-align:right;">
0.4369
</td>
<td style="text-align:right;">
0.3041
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) t-test
</td>
<td style="text-align:right;">
0.9408
</td>
<td style="text-align:right;">
0.9139
</td>
<td style="text-align:right;">
0.8468
</td>
<td style="text-align:right;">
0.7545
</td>
<td style="text-align:right;">
0.6558
</td>
<td style="text-align:right;">
0.5623
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2)+0.5 exp(1/2) Wilcoxon
</td>
<td style="text-align:right;">
0.9424
</td>
<td style="text-align:right;">
0.8861
</td>
<td style="text-align:right;">
0.7211
</td>
<td style="text-align:right;">
0.5113
</td>
<td style="text-align:right;">
0.3419
</td>
<td style="text-align:right;">
0.2190
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+1 exp(1/3) t-test
</td>
<td style="text-align:right;">
0.9359
</td>
<td style="text-align:right;">
0.9157
</td>
<td style="text-align:right;">
0.8922
</td>
<td style="text-align:right;">
0.8404
</td>
<td style="text-align:right;">
0.7863
</td>
<td style="text-align:right;">
0.7378
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/2.5)+1 exp(1/3) Wilcoxon
</td>
<td style="text-align:right;">
0.9438
</td>
<td style="text-align:right;">
0.8753
</td>
<td style="text-align:right;">
0.7227
</td>
<td style="text-align:right;">
0.5286
</td>
<td style="text-align:right;">
0.3601
</td>
<td style="text-align:right;">
0.2303
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1.5 exp(1/4) t-test
</td>
<td style="text-align:right;">
0.9297
</td>
<td style="text-align:right;">
0.9175
</td>
<td style="text-align:right;">
0.9020
</td>
<td style="text-align:right;">
0.8696
</td>
<td style="text-align:right;">
0.8448
</td>
<td style="text-align:right;">
0.8080
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3)+1.5 exp(1/4) Wilcoxon
</td>
<td style="text-align:right;">
0.9398
</td>
<td style="text-align:right;">
0.8805
</td>
<td style="text-align:right;">
0.7372
</td>
<td style="text-align:right;">
0.5446
</td>
<td style="text-align:right;">
0.3681
</td>
<td style="text-align:right;">
0.2501
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+2 exp(1/5) t-test
</td>
<td style="text-align:right;">
0.9309
</td>
<td style="text-align:right;">
0.9168
</td>
<td style="text-align:right;">
0.9083
</td>
<td style="text-align:right;">
0.8903
</td>
<td style="text-align:right;">
0.8715
</td>
<td style="text-align:right;">
0.8573
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/3.5)+2 exp(1/5) Wilcoxon
</td>
<td style="text-align:right;">
0.9436
</td>
<td style="text-align:right;">
0.8813
</td>
<td style="text-align:right;">
0.7455
</td>
<td style="text-align:right;">
0.5472
</td>
<td style="text-align:right;">
0.3819
</td>
<td style="text-align:right;">
0.2555
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2.5 exp(1/6) t-test
</td>
<td style="text-align:right;">
0.9277
</td>
<td style="text-align:right;">
0.9230
</td>
<td style="text-align:right;">
0.9156
</td>
<td style="text-align:right;">
0.9024
</td>
<td style="text-align:right;">
0.8948
</td>
<td style="text-align:right;">
0.8758
</td>
</tr>
<tr>
<td style="text-align:left;">
exp(1/4)+2.5 exp(1/6) Wilcoxon
</td>
<td style="text-align:right;">
0.9391
</td>
<td style="text-align:right;">
0.8855
</td>
<td style="text-align:right;">
0.7481
</td>
<td style="text-align:right;">
0.5600
</td>
<td style="text-align:right;">
0.3963
</td>
<td style="text-align:right;">
0.2779
</td>
</tr>
</tbody>
</table>

![](/research/img/simlulation/unnamed-chunk-25-1.png)
