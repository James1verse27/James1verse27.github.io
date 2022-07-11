---
layout: post
title:  "[의학통계학방법론] test"
subtitle : "첫 과제"
category : Study
tags: R
date: 2022-07-11
last_modified_at: 2022-07-11
---
* this unordered seed list will be replaced by the toc
{:toc}

<p style="font-size:40px">의학통계방법론 1조 5주차 과제</p>
<p style="font-size:20px">김재혁, 정지현</p>
2022-04-07

**뒤로가기를 누르시면 목차로 되돌아옵니다.**

# SAS 프로그램 결과 <a class="anchor" id="sas--------"></a>


```python
import saspy #SAS출력 코드
sas=saspy.SASsession()
```

    Please enter the name of the SAS Config you wish to run. Available Configs are: ['default', 'ssh', 'iomlinux', 'iomwin', 'winlocal', 'winiomlinux', 'winiomwin', 'httpsviya', 'httpviya', 'iomcom'] winlocal
    SAS Connection established. Subprocess id is 14000
    
    

winlocal 입력

## 10장

### EXAMPLE 10.1


```python
import os
from PIL import Image, ImageFont, ImageDraw
from IPython.display import display
img_PIL = Image.open("10-1.png")
img_PIL.show()
display(img_PIL)
```


    
![png](/study/img//study/img/output_7_0.png)
    



```python
%%SAS

libname ex 'C:\Biostat';
run;

proc import out=ex.ex10_1
datafile='C:\Biostat\data_chap10.xls' REPLACE;
SHEET='exam10_1$';
run;

/*정규성 검정*/
ods graphics off;ods exclude all;ods noresults;
proc univariate data=ex.ex10_1 normal plot;
class diet;
var weight;
ods output TestsForNormality = TestsForNormality;
run;
ods graphics on;ods exclude none;ods results;
proc sort data=TestsForNormality;
by descending Test;
run;
title " ex10_1 : 정규성 가정";
proc print data=TestsForNormality label;run;
title;

/*등분산성 검정*/
ods graphics off;ods exclude all;ods noresults;
proc glm data=ex.ex10_1;
class diet;
model weight=diet/p;
means diet / HOVTEST=BARTLETT;
ods output Bartlett = Bartlett;
run;

proc glm data=ex.ex10_1 plots=(DIAGNOSTICS RESIDUALS);
class diet;
model weight = diet;
means diet / HOVTEST=LEVENE;
ods output HOVFTest = HOVFTest;
run;
ods graphics on;ods exclude none;ods results;

title "ex10_1 : 등분산 가정 (Levene's Test for Homogeneity of weight Variance)";
proc print data=HOVFTest label;run;
title "ex10_1 : 등분산 가정 (Bartlett's Test for Homogeneity of weight Variance)";
proc print data=Bartlett label;run;
title;

/*독립성 검정*/
ods graphics off;ods exclude all;ods noresults;
proc glm data=ex.ex10_1;
	class diet;
	model weight=diet/p;
	output out=out_ds r=resid_var;
run;

data out_ds; set out_ds;
	time=_n_;
ods graphics on;ods exclude none;ods results;

proc gplot data=out_ds;
	plot resid_var * time;
run;

/*anova*/
ods graphics off;ods exclude all;ods noresults;
PROC ANOVA data=ex.ex10_1;
	class diet;
	model weight=diet;
	means diet/ TUKEY hovtest=levene(type=abs);
    ods output OverallANOVA = OverallANOVA;
    ods output CLDiffs=CLDiffs;
run;
ods graphics on;ods exclude none;ods results;
proc print data=OverallANOVA; run;
proc print data=CLDiffs; run;
```

    Please enter the name of the SAS Config you wish to run. Available Configs are: ['default', 'ssh', 'iomlinux', 'iomwin', 'winlocal', 'winiomlinux', 'winiomwin', 'httpsviya', 'httpviya', 'iomcom'] winlocal
    SAS Connection established. Subprocess id is 26244
    
    





<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta charset="utf-8"/>
<meta content="SAS 9.4" name="generator"/>
<title>SAS 출력</title>
<style>
/*<![CDATA[*/
.body.c > table, .body.c > pre, .body.c div > table,
.body.c div > pre, .body.c > table, .body.c > pre,
.body.j > table, .body.j > pre, .body.j div > table,
.body.j div > pre, .body.j > table, .body.j > pre,
.body.c p.note, .body.c p.warning, .body.c p.error, .body.c p.fatal,
.body.j p.note, .body.j p.warning, .body.j p.error, .body.j p.fatal,
.body.c > table.layoutcontainer, .body.j > table.layoutcontainer { margin-left: auto; margin-right: auto }
.layoutregion.l table, .layoutregion.l pre, .layoutregion.l p.note,
.layoutregion.l p.warning, .layoutregion.l p.error, .layoutregion.l p.fatal { margin-left: 0 }
.layoutregion.c table, .layoutregion.c pre, .layoutregion.c p.note,
.layoutregion.c p.warning, .layoutregion.c p.error, .layoutregion.c p.fatal { margin-left: auto; margin-right: auto }
.layoutregion.r table, .layoutregion.r pre, .layoutregion.r p.note,
.layoutregion.r table, .layoutregion.r pre, .layoutregion.r p.note,
.layoutregion.r p.warning, .layoutregion.r p.error, .layoutregion.r p.fatal { margin-right: 0 }
article, aside, details, figcaption, figure, footer, header, hgroup, nav, section { display: block }
html{ font-size: 100% }
.body { margin: 1em; font-size: 13px; line-height: 1.231 }
sup { position: relative; vertical-align: baseline; bottom: 0.25em; font-size: 0.8em }
sub { position: relative; vertical-align: baseline; top: 0.25em; font-size: 0.8em }
ul, ol { margin: 1em 0; padding: 0 0 0 40px }
dd { margin: 0 0 0 40px }
nav ul, nav ol { list-style: none; list-style-image: none; margin: 0; padding: 0 }
img { border: 0; vertical-align: middle }
svg:not(:root) { overflow: hidden }
figure { margin: 0 }
table { border-collapse: collapse; border-spacing: 0 }
.layoutcontainer { border-collapse: separate; border-spacing: 0 }
p { margin-top: 0; text-align: left }
h1.heading1 { text-align: left }
h2.heading2 { text-align: left }
h3.heading3 { text-align: left }
h4.heading4 { text-align: left }
h5.heading5 { text-align: left }
h6.heading6 { text-align: left }
span { text-align: left }
table { margin-bottom: 1em }
td, th { text-align: left; padding: 3px 6px; vertical-align: top }
td[class$="fixed"], th[class$="fixed"] { white-space: pre }
section, article { padding-top: 1px; padding-bottom: 8px }
hr.pagebreak { height: 0px; border: 0; border-bottom: 1px solid #c0c0c0; margin: 1em 0 }
.stacked-value { text-align: left; display: block }
.stacked-cell > .stacked-value, td.data > td.data, th.data > td.data, th.data > th.data, td.data > th.data, th.header > th.header { border: 0 }
.stacked-cell > div.data { border-width: 0 }
.systitleandfootercontainer { white-space: nowrap; margin-bottom: 1em }
.systitleandfootercontainer > p { margin: 0 }
.systitleandfootercontainer > p > span { display: inline-block; width: 100%; white-space: normal }
.batch { display: table }
.toc { display: none }
.proc_note_group, .proc_title_group { margin-bottom: 1em }
p.proctitle { margin: 0 }
p.note, p.warning, p.error, p.fatal { display: table }
.notebanner, .warnbanner, .errorbanner, .fatalbanner,
.notecontent, .warncontent, .errorcontent, .fatalcontent { display: table-cell; padding: 0.5em }
.notebanner, .warnbanner, .errorbanner, .fatalbanner { padding-right: 0 }
.body > div > ol li { text-align: left }
.beforecaption > h4 { margin-top: 0; margin-bottom: 0 }
.c { text-align: center }
.r { text-align: right }
.l { text-align: left }
.j { text-align: justify }
.d { text-align: right }
.b { vertical-align: bottom }
.m { vertical-align: middle }
.t { vertical-align: top }
.accessiblecaption {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
a:active { color: #800080 }
.aftercaption {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
    padding-top: 4pt;
}
.batch > colgroup {
    border-left: 1px solid #c1c1c1;
    border-right: 1px solid #c1c1c1;
}
.batch > tbody, .batch > thead, .batch > tfoot {
    border-top: 1px solid #c1c1c1;
    border-bottom: 1px solid #c1c1c1;
}
.batch { border: hidden; }
.batch {
    background-color: #fafbfe;
    border: 1px solid #c1c1c1;
    border-collapse: separate;
    border-spacing: 1px;
    color: #000000;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    padding: 7px;
    }
.beforecaption {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.body {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    margin-left: 8px;
    margin-right: 8px;
}
.bodydate {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    text-align: right;
    vertical-align: top;
    width: 100%;
}
.bycontentfolder {
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: none;
    margin-left: 6pt;
}
.byline {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.bylinecontainer > col, .bylinecontainer > colgroup > col, .bylinecontainer > colgroup, .bylinecontainer > tr, .bylinecontainer > * > tr, .bylinecontainer > thead, .bylinecontainer > tbody, .bylinecontainer > tfoot { border: none; }
.bylinecontainer {
    background-color: #fafbfe;
    border: none;
    border-spacing: 1px;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.caption {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.cell, .container {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.contentfolder, .contentitem {
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: none;
    margin-left: 6pt;
}
.contentproclabel, .contentprocname {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.contents {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: decimal;
    margin-left: 8px;
    margin-right: 8px;
}
.contentsdate {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.contenttitle {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: bold;
}
.continued {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
    width: 100%;
}
.data, .dataemphasis {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.dataemphasisfixed {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.dataempty {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.datafixed {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.datastrong {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.datastrongfixed {
    background-color: #ffffff;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.date {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.document {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.errorbanner {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.errorcontent {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.errorcontentfixed {
    background-color: #fafbfe;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.extendedpage {
    background-color: #fafbfe;
    border-style: solid;
    border-width: 1pt;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
    text-align: center;
}
.fatalbanner {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.fatalcontent {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.fatalcontentfixed {
    background-color: #fafbfe;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.folderaction {
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: none;
    margin-left: 6pt;
}
.footer {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.footeremphasis {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.footeremphasisfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.footerempty {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.footerfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.footerstrong {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.footerstrongfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.frame {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.graph > colgroup {
    border-left: 1px solid #c1c1c1;
    border-right: 1px solid #c1c1c1;
}
.graph > tbody, .graph > thead, .graph > tfoot {
    border-top: 1px solid #c1c1c1;
    border-bottom: 1px solid #c1c1c1;
}
.graph { border: hidden; }
.graph {
    background-color: #fafbfe;
    border: 1px solid #c1c1c1;
    border-collapse: separate;
    border-spacing: 1px;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    }
.header {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.headeremphasis {
    background-color: #d8dbd3;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.headeremphasisfixed {
    background-color: #d8dbd3;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.headerempty {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.headerfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.headersandfooters {
    background-color: #edf2f9;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.headerstrong {
    background-color: #d8dbd3;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.headerstrongfixed {
    background-color: #d8dbd3;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #000000;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.heading1, .heading2, .heading3, .heading4, .heading5, .heading6 { font-family: Gulim, Helvetica, sans-serif }
.index {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.indexaction, .indexitem {
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: none;
    margin-left: 6pt;
}
.indexprocname {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.indextitle {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: bold;
}
.layoutcontainer, .layoutregion {
    border-width: 0;
    border-spacing: 30px;
}
.linecontent {
    background-color: #fafbfe;
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
a:link { color: #0000ff }
.list {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: disc;
}
.list10 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: square;
}
.list2 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: circle;
}
.list3, .list4, .list5, .list6, .list7, .list8, .list9 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: square;
}
.listitem {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: disc;
}
.listitem10 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: square;
}
.listitem2 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: circle;
}
.listitem3, .listitem4, .listitem5, .listitem6, .listitem7, .listitem8, .listitem9 {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: square;
}
.note {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.notebanner {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.notecontent {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.notecontentfixed {
    background-color: #fafbfe;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.output > colgroup {
    border-left: 1px solid #c1c1c1;
    border-right: 1px solid #c1c1c1;
}
.output > tbody, .output > thead, .output > tfoot {
    border-top: 1px solid #c1c1c1;
    border-bottom: 1px solid #c1c1c1;
}
.output { border: hidden; }
.output {
    background-color: #fafbfe;
    border: 1px solid #c1c1c1;
    border-collapse: separate;
    border-spacing: 1px;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    }
.pageno {
    background-color: #fafbfe;
    border-spacing: 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
    text-align: right;
    vertical-align: top;
}
.pages {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: decimal;
    margin-left: 8px;
    margin-right: 8px;
}
.pagesdate {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.pagesitem {
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    list-style-type: none;
    margin-left: 6pt;
}
.pagesproclabel, .pagesprocname {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.pagestitle {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: bold;
}
.paragraph {
    background-color: #fafbfe;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.parskip > col, .parskip > colgroup > col, .parskip > colgroup, .parskip > tr, .parskip > * > tr, .parskip > thead, .parskip > tbody, .parskip > tfoot { border: none; }
.parskip {
    border: none;
    border-spacing: 0;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
    }
.prepage {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    text-align: left;
}
.proctitle {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.proctitlefixed {
    background-color: #fafbfe;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowfooter {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowfooteremphasis {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.rowfooteremphasisfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.rowfooterempty {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowfooterfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.rowfooterstrong {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowfooterstrongfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowheader {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowheaderemphasis {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.rowheaderemphasisfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: italic;
    font-weight: normal;
}
.rowheaderempty {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowheaderfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.rowheaderstrong {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.rowheaderstrongfixed {
    background-color: #edf2f9;
    border-color: #b0b7bb;
    border-style: solid;
    border-width: 0 1px 1px 0;
    color: #112277;
    font-family: BatangChe, Courier, monospace, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.systemfooter, .systemfooter10, .systemfooter2, .systemfooter3, .systemfooter4, .systemfooter5, .systemfooter6, .systemfooter7, .systemfooter8, .systemfooter9 {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.systemtitle, .systemtitle10, .systemtitle2, .systemtitle3, .systemtitle4, .systemtitle5, .systemtitle6, .systemtitle7, .systemtitle8, .systemtitle9 {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size: small;
    font-style: normal;
    font-weight: bold;
}
.systitleandfootercontainer > col, .systitleandfootercontainer > colgroup > col, .systitleandfootercontainer > colgroup, .systitleandfootercontainer > tr, .systitleandfootercontainer > * > tr, .systitleandfootercontainer > thead, .systitleandfootercontainer > tbody, .systitleandfootercontainer > tfoot { border: none; }
.systitleandfootercontainer {
    background-color: #fafbfe;
    border: none;
    border-spacing: 1px;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.table > col, .table > colgroup > col {
    border-left: 1px solid #c1c1c1;
    border-right: 0 solid #c1c1c1;
}
.table > tr, .table > * > tr {
    border-top: 1px solid #c1c1c1;
    border-bottom: 0 solid #c1c1c1;
}
.table { border: hidden; }
.table {
    border-color: #c1c1c1;
    border-style: solid;
    border-width: 1px 0 0 1px;
    border-collapse: collapse;
    border-spacing: 0;
    }
.titleandnotecontainer > col, .titleandnotecontainer > colgroup > col, .titleandnotecontainer > colgroup, .titleandnotecontainer > tr, .titleandnotecontainer > * > tr, .titleandnotecontainer > thead, .titleandnotecontainer > tbody, .titleandnotecontainer > tfoot { border: none; }
.titleandnotecontainer {
    background-color: #fafbfe;
    border: none;
    border-spacing: 1px;
    color: #000000;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
    width: 100%;
}
.titlesandfooters {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.usertext {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
a:visited { color: #800080 }
.warnbanner {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: bold;
}
.warncontent {
    background-color: #fafbfe;
    color: #112277;
    font-family: Gulim, Helvetica, Helv, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
.warncontentfixed {
    background-color: #fafbfe;
    color: #112277;
    font-family: BatangChe, Courier, sans-serif;
    font-size:  normal;
    font-style: normal;
    font-weight: normal;
}
/*]]>*/
</style>
</head>
<body class="l body">
<div style="padding-bottom: 8px; padding-top: 1px">
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<div id="IDX" class="systitleandfootercontainer" style="border-spacing: 1px">
<p><span class="c systemtitle">ex10_1 : 정규성 가정</span> </p>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<table class="table" style="border-spacing: 0" aria-label="데이터셋 WORK.TESTSFORNORMALITY">
<caption aria-label="데이터셋 WORK.TESTSFORNORMALITY"></caption>
<colgroup><col/></colgroup><colgroup><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead>
<tr>
<th class="r header" scope="col">OBS</th>
<th class="header" scope="col">VarName</th>
<th class="header" scope="col">diet</th>
<th class="header" scope="col">적합도 검정</th>
<th class="header" scope="col">적합도 통계량에 대한 레이블</th>
<th class="r header" scope="col">적합도 통계량 값</th>
<th class="header" scope="col">p-값 레이블</th>
<th class="header" scope="col">p-값의 부호</th>
<th class="r header" scope="col">p-값</th>
</tr>
</thead>
<tbody>
<tr>
<th class="r rowheader" scope="row">1</th>
<td class="data">weight</td>
<td class="data">1</td>
<td class="data">Shapiro-Wilk</td>
<td class="data">W</td>
<td class="r data">0.933155</td>
<td class="data">Pr &lt; W</td>
<td class="data"> &#160;</td>
<td class="r data">0.6180</td>
</tr>
<tr>
<th class="r rowheader" scope="row">2</th>
<td class="data">weight</td>
<td class="data">2</td>
<td class="data">Shapiro-Wilk</td>
<td class="data">W</td>
<td class="r data">0.94389</td>
<td class="data">Pr &lt; W</td>
<td class="data"> &#160;</td>
<td class="r data">0.6936</td>
</tr>
<tr>
<th class="r rowheader" scope="row">3</th>
<td class="data">weight</td>
<td class="data">3</td>
<td class="data">Shapiro-Wilk</td>
<td class="data">W</td>
<td class="r data">0.952018</td>
<td class="data">Pr &lt; W</td>
<td class="data"> &#160;</td>
<td class="r data">0.7287</td>
</tr>
<tr>
<th class="r rowheader" scope="row">4</th>
<td class="data">weight</td>
<td class="data">4</td>
<td class="data">Shapiro-Wilk</td>
<td class="data">W</td>
<td class="r data">0.990268</td>
<td class="data">Pr &lt; W</td>
<td class="data"> &#160;</td>
<td class="r data">0.9806</td>
</tr>
<tr>
<th class="r rowheader" scope="row">5</th>
<td class="data">weight</td>
<td class="data">1</td>
<td class="data">Kolmogorov-Smirnov</td>
<td class="data">D</td>
<td class="r data">0.208622</td>
<td class="data">Pr &gt; D</td>
<td class="data">&gt;</td>
<td class="r data">0.1500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">6</th>
<td class="data">weight</td>
<td class="data">2</td>
<td class="data">Kolmogorov-Smirnov</td>
<td class="data">D</td>
<td class="r data">0.2016</td>
<td class="data">Pr &gt; D</td>
<td class="data">&gt;</td>
<td class="r data">0.1500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">7</th>
<td class="data">weight</td>
<td class="data">3</td>
<td class="data">Kolmogorov-Smirnov</td>
<td class="data">D</td>
<td class="r data">0.206041</td>
<td class="data">Pr &gt; D</td>
<td class="data">&gt;</td>
<td class="r data">0.1500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">8</th>
<td class="data">weight</td>
<td class="data">4</td>
<td class="data">Kolmogorov-Smirnov</td>
<td class="data">D</td>
<td class="r data">0.145566</td>
<td class="data">Pr &gt; D</td>
<td class="data">&gt;</td>
<td class="r data">0.1500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">9</th>
<td class="data">weight</td>
<td class="data">1</td>
<td class="data">Cramer-von Mises</td>
<td class="data">W-Sq</td>
<td class="r data">0.035311</td>
<td class="data">Pr &gt; W-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">10</th>
<td class="data">weight</td>
<td class="data">2</td>
<td class="data">Cramer-von Mises</td>
<td class="data">W-Sq</td>
<td class="r data">0.033613</td>
<td class="data">Pr &gt; W-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">11</th>
<td class="data">weight</td>
<td class="data">3</td>
<td class="data">Cramer-von Mises</td>
<td class="data">W-Sq</td>
<td class="r data">0.034212</td>
<td class="data">Pr &gt; W-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">12</th>
<td class="data">weight</td>
<td class="data">4</td>
<td class="data">Cramer-von Mises</td>
<td class="data">W-Sq</td>
<td class="r data">0.020098</td>
<td class="data">Pr &gt; W-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">13</th>
<td class="data">weight</td>
<td class="data">1</td>
<td class="data">Anderson-Darling</td>
<td class="data">A-Sq</td>
<td class="r data">0.235734</td>
<td class="data">Pr &gt; A-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">14</th>
<td class="data">weight</td>
<td class="data">2</td>
<td class="data">Anderson-Darling</td>
<td class="data">A-Sq</td>
<td class="r data">0.220699</td>
<td class="data">Pr &gt; A-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">15</th>
<td class="data">weight</td>
<td class="data">3</td>
<td class="data">Anderson-Darling</td>
<td class="data">A-Sq</td>
<td class="r data">0.215876</td>
<td class="data">Pr &gt; A-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
<tr>
<th class="r rowheader" scope="row">16</th>
<td class="data">weight</td>
<td class="data">4</td>
<td class="data">Anderson-Darling</td>
<td class="data">A-Sq</td>
<td class="r data">0.151062</td>
<td class="data">Pr &gt; A-Sq</td>
<td class="data">&gt;</td>
<td class="r data">0.2500</td>
</tr>
</tbody>
</table>
</div>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<hr class="pagebreak"/>
<div id="IDX1" class="systitleandfootercontainer" style="border-spacing: 1px">
<p><span class="c systemtitle">ex10_1 : 등분산 가정 (Levene&apos;s Test for Homogeneity of weight Variance)</span> </p>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<table class="table" style="border-spacing: 0" aria-label="데이터셋 WORK.HOVFTEST">
<caption aria-label="데이터셋 WORK.HOVFTEST"></caption>
<colgroup><col/></colgroup><colgroup><col/><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead>
<tr>
<th class="r header" scope="col">OBS</th>
<th class="header" scope="col">Effect</th>
<th class="header" scope="col">Dependent</th>
<th class="header" scope="col">Method</th>
<th class="header" scope="col">Source</th>
<th class="r header" scope="col">DF</th>
<th class="r header" scope="col">Sum of Squares</th>
<th class="r header" scope="col">Mean Square</th>
<th class="r header" scope="col">F Value</th>
<th class="r header" scope="col">Pr &gt; F</th>
</tr>
</thead>
<tbody>
<tr>
<th class="r rowheader" scope="row">1</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">LV</td>
<td class="data">diet</td>
<td class="r data">3</td>
<td class="r data">56.6171</td>
<td class="r data">18.8724</td>
<td class="r data">0.54</td>
<td class="r data">0.6643</td>
</tr>
<tr>
<th class="r rowheader" scope="row">2</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">LV</td>
<td class="data">Error</td>
<td class="r data">15</td>
<td class="r data">527.6</td>
<td class="r data">35.1708</td>
<td class="r data">_</td>
<td class="r data">_</td>
</tr>
</tbody>
</table>
</div>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<hr class="pagebreak"/>
<div id="IDX2" class="systitleandfootercontainer" style="border-spacing: 1px">
<p><span class="c systemtitle">ex10_1 : 등분산 가정 (Bartlett&apos;s Test for Homogeneity of weight Variance)</span> </p>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<table class="table" style="border-spacing: 0" aria-label="데이터셋 WORK.BARTLETT">
<caption aria-label="데이터셋 WORK.BARTLETT"></caption>
<colgroup><col/></colgroup><colgroup><col/><col/><col/><col/><col/><col/></colgroup>
<thead>
<tr>
<th class="r header" scope="col">OBS</th>
<th class="header" scope="col">Effect</th>
<th class="header" scope="col">Dependent</th>
<th class="header" scope="col">Source</th>
<th class="r header" scope="col">DF</th>
<th class="r header" scope="col">Chi-Square</th>
<th class="r header" scope="col">Pr &gt; ChiSq</th>
</tr>
</thead>
<tbody>
<tr>
<th class="r rowheader" scope="row">1</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">diet</td>
<td class="r data">3</td>
<td class="r data">0.4752</td>
<td class="r data">0.9243</td>
</tr>
</tbody>
</table>
</div>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<hr class="pagebreak"/>
<div id="IDX3" style="padding-bottom: 8px; padding-top: 1px">
<div class="c">
<script type="text/ecmascript">
var SVG_IDX3_SVGRoot = null;
var SVG_IDX3_TrueCoords = null;
var SVG_IDX3_singleTip = null;
var SVG_IDX3_singleBox = null;
var SVG_IDX3_singleText = null;
var SVG_IDX3_tiptspan = null;
var SVG_IDX3_attrScale = 1;
var SVG_IDX3_rightEdge = null;
var SVG_IDX3_SVGHeight = null;
var SVG_IDX3_SVGWidth = null;
function SVG_IDX3_Init(anchor) {
   SVG_IDX3_SVGRoot = document.getElementById(anchor);
   SVG_IDX3_TrueCoords = SVG_IDX3_SVGRoot.createSVGPoint();
   SVG_IDX3_singleTip = document.getElementById(anchor + '_singleTip1');
   SVG_IDX3_singleBox = document.getElementById(anchor + '_singleBox1');
   SVG_IDX3_singleText = document.getElementById(anchor + '_singleText1');
   SVG_IDX3_tiptspan = document.getElementById(anchor + '_tiptspan1');
   SVG_IDX3_tiptspan.firstChild.nodeValue = null;
   SVG_IDX3_SVGRoot.addEventListener('mousemove', SVG_IDX3_ShowTooltip, false);
   SVG_IDX3_SVGRoot.addEventListener('mouseout', SVG_IDX3_HideTooltip, false);
   SVG_IDX3_SVGHeight = SVG_IDX3_SVGRoot.getAttributeNS(null, "height");
   SVG_IDX3_SVGWidth = SVG_IDX3_SVGRoot.getAttributeNS(null, "width");
   if (!isNaN(parseFloat(SVG_IDX3_SVGHeight)) && !isNaN(parseFloat(SVG_IDX3_SVGWidth)))
      SVG_IDX3_GetHW();
}
function SVG_IDX3_GetHW()
{
    if (SVG_IDX3_SVGWidth != null && SVG_IDX3_SVGWidth != "") {
       if (SVG_IDX3_SVGWidth.match(/%/)) {
          var factor = parseFloat(SVG_IDX3_SVGWidth.replace(/%/,""))/100;
          SVG_IDX3_SVGWidth = window.innerWidth * factor;
       }
       else if (SVG_IDX3_SVGWidth.match(/in/))
          SVG_IDX3_SVGWidth = parseFloat(SVG_IDX3_SVGWidth.replace(/in/,""))*96;
       else if (SVG_IDX3_SVGWidth.match(/cm/))
          SVG_IDX3_SVGWidth = parseFloat(SVG_IDX3_SVGWidth.replace(/in/,""))*96/2.54;
       else
           SVG_IDX3_SVGWidth = parseFloat(SVG_IDX3_SVGWidth);
    }
    if (SVG_IDX3_SVGHeight != null && SVG_IDX3_SVGHeight != "") {
       if (SVG_IDX3_SVGHeight.match(/%/)) {
          var factor = parseFloat(SVG_IDX3_SVGHeight.replace(/%/,""))/100;
          SVG_IDX3_SVGHeight = window.innerHeight * factor;
       }
       else if (SVG_IDX3_SVGHeight.match(/in/))
          SVG_IDX3_SVGHeight = parseFloat(SVG_IDX3_SVGHeight.replace(/in/,""))*96;
       else if (SVG_IDX3_SVGHeight.match(/cm/))
          SVG_IDX3_SVGHeight = parseFloat(SVG_IDX3_SVGHeight.replace(/in/,""))*96/2.54;
       else
           SVG_IDX3_SVGHeight = parseFloat(SVG_IDX3_SVGHeight);
    }
}
function SVG_IDX3_GetScaleFactors(evt)
{
   var zoomFactor = 1;
   windowWidth  = (top.innerWidth==undefined)  ? window.innerWidth  : top.innerWidth;
   windowHeight = (top.innerHeight==undefined) ? window.innerHeight : top.innerHeight;
   SVG_IDX3_rightEdge = (SVG_IDX3_SVGWidth) ? SVG_IDX3_SVGWidth - window.pageXOffset : windowWidth;
   if (parseFloat(SVG_IDX3_SVGWidth) > 0 || parseFloat(SVG_IDX3_SVGHeight) > 0)
      var tipScale = 1.5;
   else {
      var tipXScale  = (screen.width/windowWidth) / SVG_IDX3_SVGRoot.currentScale;
      var tipYScale  = (screen.height/windowHeight) / SVG_IDX3_SVGRoot.currentScale;
      var tipScale   = (tipXScale + tipYScale)/2.0;
   }
   SVG_IDX3_attrScale = tipScale;
   if (navigator.appName == 'Adobe SVG Viewer') {
      if (screen.deviceXDPI != null && screen.logicalXDPI != null)
         zoomFactor = screen.deviceXDPI/screen.logicalXDPI;
      SVG_IDX3_attrScale = zoomFactor*tipScale;
   }
   else if (navigator.userAgent != null && navigator.userAgent != '') {
      var ua = navigator.userAgent.toLowerCase();
      if (ua.indexOf("msie") != -1 || ua.indexOf("trident") != -1) {
         zoomFactor = screen.deviceXDPI/screen.logicalXDPI;
         SVG_IDX3_attrScale = tipScale/zoomFactor;
         SVG_IDX3_rightEdge /= zoomFactor;
      }
      else if (ua.indexOf("ipad") != -1) {
         zoomFactor = SVG_IDX3_SVGRoot.clientWidth / window.innerWidth;
         SVG_IDX3_attrScale = 1.5 / zoomFactor;
         SVG_IDX3_rightEdge /= zoomFactor;
      }
      else if (ua.indexOf("opera") != -1)
         SVG_IDX3_attrScale = tipScale;
      else if (ua.indexOf("firefox") != -1)
         SVG_IDX3_attrScale = tipScale;
      else {
         if (SVG_IDX3_SVGWidth > 0 || SVG_IDX3_SVGHeight > 0)
            zoomFactor = 1;
         else
            zoomFactor = parseInt(document.defaultView.getComputedStyle(SVG_IDX3_SVGRoot, null).width,10)/
                                  SVG_IDX3_SVGRoot.clientWidth;
         SVG_IDX3_attrScale = tipScale/zoomFactor;
         SVG_IDX3_rightEdge *= zoomFactor;
      }
   }
}
function SVG_IDX3_GetTrueCoords(evt)
{
   var trans = SVG_IDX3_SVGRoot.currentTranslate;
   var scale = SVG_IDX3_SVGRoot.currentScale;
   SVG_IDX3_GetScaleFactors();
   var p1    = SVG_IDX3_SVGRoot.createSVGPoint();
   var p2;
      var m = SVG_IDX3_SVGRoot.getScreenCTM();
      if (navigator.userAgent.toLowerCase().indexOf("ipad") != -1) {
         p1.x = evt.clientX + window.pageXOffset;
         p1.y = evt.clientY + window.pageYOffset;
      }
      else {
      p1.x = evt.clientX;
      p1.y = evt.clientY;
      }
      p2 = p1.matrixTransform(m.inverse());
   SVG_IDX3_TrueCoords.x = Math.round(p2.x*100) / 100;
   SVG_IDX3_TrueCoords.y = Math.round(p2.y*100) / 100;
}
function SVG_IDX3_HideTooltip( evt )
{
   SVG_IDX3_tiptspan.firstChild.nodeValue = null;
   SVG_IDX3_singleTip.setAttributeNS(null, 'visibility', 'hidden');
}
function SVG_IDX3_ShowTooltip( evt )
{
   SVG_IDX3_GetTrueCoords( evt );
   var targetElement = evt.target;
   var tspanCount = targetElement.getElementsByTagName('desc').length;
   if (tspanCount == 1) {
      var targetTspan = targetElement.getElementsByTagName('desc').item(0);
      if ( targetTspan) {
         if (targetTspan.firstChild != null)
            SVG_IDX3_tiptspan.firstChild.nodeValue = targetTspan.firstChild.nodeValue; }
   }
   if ( '' != SVG_IDX3_tiptspan.firstChild.nodeValue )
   {
      var outline = SVG_IDX3_singleText.getBBox();
      SVG_IDX3_singleBox.setAttributeNS(null, 'transform', 'scale(' + SVG_IDX3_attrScale + ',' + SVG_IDX3_attrScale + ')' );
      SVG_IDX3_singleText.setAttributeNS(null, 'transform', 'scale(' + SVG_IDX3_attrScale + ',' + SVG_IDX3_attrScale + ')' );
      SVG_IDX3_singleTip.setAttributeNS(null, 'visibility', 'visible');
      if (evt.clientX + ((outline.width + 10)/1) * SVG_IDX3_attrScale > SVG_IDX3_rightEdge) {
         var xPos = SVG_IDX3_TrueCoords.x - parseInt((outline.width + 10)*SVG_IDX3_attrScale);
         if (xPos < 0)
            xPos = 0;
      }
      else
         var xPos = SVG_IDX3_TrueCoords.x;
      if (SVG_IDX3_TrueCoords.y < 20) {
         var yPos = SVG_IDX3_TrueCoords.y + ((outline.height + 20)/1)*SVG_IDX3_attrScale;
         if (yPos > windowHeight)
            yPos = SVG_IDX3_TrueCoords.y - parseInt((outline.height + 10)*SVG_IDX3_attrScale);
      }
      else
         var yPos = SVG_IDX3_TrueCoords.y - parseInt((outline.height + 10)*SVG_IDX3_attrScale);
      if (Number(outline.width == 0))
         SVG_IDX3_singleBox.setAttributeNS(null, 'width', outline.width);
      else
         SVG_IDX3_singleBox.setAttributeNS(null, 'width', outline.width + 10);
      SVG_IDX3_singleBox.setAttributeNS(null, 'height', outline.height + 10);
      SVG_IDX3_singleTip.setAttributeNS(null, 'transform', 'translate(' + xPos + ',' + yPos + ')');
   }
}
</script>
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" baseProfile="full" version="1.1" id="SVG_IDX3" onload='SVG_IDX3_Init("SVG_IDX3")' viewBox="-1 -1 801 601" height="600" width="800">
<title>svgtitle</title>
<desc id="desc_SVG_IDX3">resid_var * time 도표</desc>
<defs>
<clipPath id="SVG_IDX3_clipPage1">
<rect x="-1" y="-1" width="801" height="601"></rect>
</clipPath>
</defs>
<g id="SVG_IDX3_Page1" clip-path="url(#SVG_IDX3_clipPage1)">
<rect x="0" y="0" width="800" height="600" style="fill: #FFFFFF; fill-opacity: 0; stroke: #FFFFFF; stroke-width: 1; stroke-opacity: 0; 
stroke-linejoin: round; "></rect>
<rect x="0" y="0" width="799" height="599" style="fill: #FFFFFF; stroke: #FFFFFF; stroke-width: 1; stroke-linejoin: round; "></rect>
<path d="M65 537L63 537M64 530L65 530M64 524L65 524M64 517L65 517M64 
511L65 511M64 505L65 505M64 498L65 498M64 492L65 492M64 485L65 
485M64 479L65 479M65 472L63 472M64 466L65 466M64 460L65 460M64 
453L65 453M64 447L65 447M64 440L65 440M64 434L65 434M64 427L65 
427M64 421L65 421M64 415L65 415M65 408L63 408M64 402L65 402M64 
395L65 395M64 389L65 389M64 382L65 382M64 376L65 376M64 369L65 
369M64 363L65 363M64 357L65 357M64 350L65 350M65 344L63 344M64 
337L65 337M64 331L65 331M64 324L65 324M64 318L65 318M64 312L65 
312M64 305L65 305M64 299L65 299M64 292L65 292M64 286L65 286M65 
279L63 279M64 273L65 273M64 266L65 266M64 260L65 260M64 254L65 
254M64 247L65 247M64 241L65 241M64 234L65 234M64 228L65 228M64 
221L65 221M65 215L63 215M64 209L65 209M64 202L65 202M64 196L65 
196M64 189L65 189M64 183L65 183M64 176L65 176M64 170L65 170M64 
164L65 164M64 157L65 157M65 151L63 151M64 144L65 144M64 138L65 
138M64 131L65 131M64 125L65 125M64 118L65 118M64 112L65 112M64 
106L65 106M64 99L65 99M64 93L65 93M65 86L63 86M64 80L65 
80M64 73L65 73M64 67L65 67M64 61L65 61M64 54L65 54M64 48L
65 48M64 41L65 41M64 35L65 35M64 28L65 28M65 22L63 22" style="stroke: #989EA1; stroke-width: 1; fill: none; stroke-linejoin: round; "></path>
<text x="60" y="12" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 13px; text-anchor: end; white-space: pre; ">&#x000072;&#x000065;&#x000073;&#x000069;&#x000064;&#x00005F;&#x000076;&#x000061;&#x000072;</text>
<text x="60" y="541" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x00002D;&#x000034;</text>
<text x="60" y="476" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x00002D;&#x000033;</text>
<text x="60" y="412" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x00002D;&#x000032;</text>
<text x="60" y="348" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x00002D;&#x000031;</text>
<text x="60" y="283" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x000030;</text>
<text x="60" y="219" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x000031;</text>
<text x="60" y="155" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x000032;</text>
<text x="60" y="90" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x000033;</text>
<text x="60" y="26" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: end; white-space: pre; ">&#x000034;</text>
<path d="M68 543L68 548M77 546L77 543M87 546L87 543M96 546L96 543M106 
543L106 548M115 546L115 543M125 546L125 543M134 546L134 543M144 543L
144 548M153 546L153 543M163 546L163 543M172 546L172 543M182 543L182 
548M191 546L191 543M200 546L200 543M210 546L210 543M219 543L219 548M
229 546L229 543M238 546L238 543M248 546L248 543M257 543L257 548M267 
546L267 543M276 546L276 543M286 546L286 543M295 543L295 548M305 546L
305 543M314 546L314 543M324 546L324 543M333 543L333 548M342 546L342 
543M352 546L352 543M361 546L361 543M371 543L371 548M380 546L380 543M
390 546L390 543M399 546L399 543M409 543L409 548M418 546L418 543M428 
546L428 543M437 546L437 543M447 543L447 548M456 546L456 543M466 546L
466 543M475 546L475 543M485 543L485 548M494 546L494 543M503 546L503 
543M513 546L513 543M522 543L522 548M532 546L532 543M541 546L541 543M
551 546L551 543M560 543L560 548M570 546L570 543M579 546L579 543M589 
546L589 543M598 543L598 548M608 546L608 543M617 546L617 543M627 546L
627 543M636 543L636 548M645 546L645 543M655 546L655 543M664 546L664 
543M674 543L674 548M683 546L683 543M693 546L693 543M702 546L702 543M
712 543L712 548M721 546L721 543M731 546L731 543M740 546L740 543M750 
543L750 548M759 546L759 543M769 546L769 543M778 546L778 543M788 543L
788 548" style="stroke: #989EA1; stroke-width: 1; fill: none; stroke-linejoin: round; "></path>
<text x="428" y="590" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 13px; text-anchor: middle; white-space: pre; ">&#x000074;&#x000069;&#x00006D;&#x000065;</text>
<text x="68" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;</text>
<text x="106" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000032;</text>
<text x="144" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000033;</text>
<text x="182" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000034;</text>
<text x="219" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000035;</text>
<text x="257" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000036;</text>
<text x="295" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000037;</text>
<text x="333" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000038;</text>
<text x="371" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000039;</text>
<text x="409" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000030;</text>
<text x="447" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000031;</text>
<text x="485" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000032;</text>
<text x="522" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000033;</text>
<text x="560" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000034;</text>
<text x="598" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000035;</text>
<text x="636" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000036;</text>
<text x="674" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000037;</text>
<text x="712" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000038;</text>
<text x="750" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000031;&#x000039;</text>
<text x="788" y="564" style="fill: #000000; font-family: 'SVG_IDX3_Gulim Font3', 'Gulim', sans-serif; font-size: 12px; text-anchor: middle; white-space: pre; ">&#x000032;&#x000030;</text>
<path d="M71 525L65 525M68 522L68 528M109 126L103 126M106 123L106 129M147 
255L141 255M144 252L144 258M185 23L179 23M182 20L182 26M222 467L216 
467M219 464L219 470M260 447L254 447M257 444L257 450M298 511L292 511M
295 508L295 514M336 41L330 41M333 38L333 44M374 151L368 151M371 148L
371 154M412 247L406 247M409 244L409 250M450 521L444 521M447 518L447 
524M488 38L482 38M485 35L485 41M525 160L519 160M522 157L522 163M563 
398L557 398M560 395L560 401M639 366L633 366M636 363L636 369M677 218L
671 218M674 215L674 221M715 288L709 288M712 285L712 291M753 57L747 
57M750 54L750 60M791 469L785 469M788 466L788 472" style="stroke: #445694; stroke-width: 1; fill: none; stroke-linejoin: round; "></path>
<path d="M65 542L65 16L790 16L790 542L65 542" style="stroke: #989EA1; stroke-width: 1; fill: none; stroke-linejoin: round; "></path>
<rect x="0" y="0" width="800" height="600" fill="#FFFFFF" fill-opacity="0" aria-hidden="true">
<desc>resid_var * time 도표</desc>
</rect>
</g>
<g id="SVG_IDX3_singleTip1" visibility="hidden" pointer-events="none">
   <rect id="SVG_IDX3_singleBox1" x="0" y="0" width="88" height="20" rx="2" ry="2" fill="white" stroke="black" stroke-width="1" opacity="0.8"></rect>
   <text id="SVG_IDX3_singleText1" x="5" y="10" font-family="'Gulim', sans-serif" font-size="8"><tspan id="SVG_IDX3_tiptspan1" x="5" class="tiptext"><![CDATA[ ]]></tspan></text>
</g>
<defs>
<style type="text/css"><![CDATA[
    tspan.tiptext { white-space: pre }
]]></style>
</defs>
<defs>
<style type="text/css"><![CDATA[
  @font-face {
      font-family: "SVG_IDX3_Gulim Font3";
      src: url("#SVG_IDX3_Font3") format("svg")
      }
]]></style>
<font id="SVG_IDX3_Font3" horiz-adv-x="3072" >
   <font-face      font-family="SVG_IDX3_Gulim Font3"
    units-per-em="2048" ascent="879" descent="-145" >
      <font-face-src>
         <font-face-name name="SVG_IDX3_Gulim Font3"></font-face-name>
      </font-face-src>
   </font-face>
<missing-glyph><path d="M0 0v4608h2304v-4608h-2304" fill="none"></path>
</missing-glyph>
<glyph unicode="&#40;" horiz-adv-x="765" 
d="M511 1648Q357 1428 284 1204 Q213 986 213 734 Q213 464 280 242 Q357 -6 511 
-184 Q533 -210 561 -212 Q587 -214 607 -196 Q627 -178 629 -152 Q631 -122 611 -98 Q
465 82 401 286 Q341 480 341 734 Q341 978 407 1178 Q469 1366 613 1578 Q629 
1604 623 1628 Q619 1650 597 1664 Q577 1676 553 1674 Q527 1672 511 1648Z" ></glyph>
<glyph unicode="&#47;" horiz-adv-x="850" 
d="M664 1600L104 -108Q90 -156 136 -166 Q182 -178 200 -128 L752 1576Q766 1622 720 
1634 Q676 1644 664 1600Z" ></glyph>
<glyph unicode="&#104;" horiz-adv-x="1147" 
d="M170 1390L170 64Q170 34 189 16 Q208 0 234 0 Q259 2 278 16 Q298 34 
298 64 L298 746Q370 840 450 888 Q538 938 639 938 Q740 938 798 880 Q853 
829 853 750 L853 60Q853 32 871 14 Q890 0 916 0 Q942 0 961 14 Q981 
34 981 62 L981 770Q981 892 895 974 Q800 1066 642 1066 Q528 1066 436 1016 Q
363 976 298 900 L298 1389Q298 1418 278 1435 Q259 1450 234 1450 Q208 1450 189 
1435 Q170 1418 170 1390Z" ></glyph>
<glyph unicode="&#65;" horiz-adv-x="1317" 
d="M380 597L658 1357L664 1357L940 597L380 597ZM546 1364L52 83Q38 52 50 
30 Q62 7 86 0 Q110 -8 132 3 Q158 16 170 44 L340 469L982 469L1150 
42Q1162 14 1186 2 Q1210 -8 1234 0 Q1258 7 1268 28 Q1280 52 1270 81 L
774 1363Q758 1407 726 1430 Q696 1450 662 1450 Q624 1450 594 1430 Q562 1407 546 
1364Z" ></glyph>
<glyph unicode="&#103;" horiz-adv-x="1232" 
d="M938 1008L938 902Q877 994 794 1040 Q708 1066 596 1066 Q370 1066 240 898 Q128 
748 128 532 Q128 308 240 164 Q368 0 596 0 Q716 0 814 54 Q897 102 938 
172 L938 54Q938 -38 854 -100 Q762 -170 592 -170 Q456 -170 374 -131 Q298 -95 
262 -26 Q248 -4 224 0 Q202 6 180 -8 Q156 -20 150 -42 Q140 -67 156 -94 Q
210 -196 322 -247 Q430 -298 590 -298 Q818 -298 948 -191 Q1066 -98 1066 41 L1066 
1009Q1066 1036 1046 1052 Q1028 1066 1001 1066 Q975 1066 956 1052 Q938 1036 938 
1008ZM596 938Q761 938 856 810 Q938 696 938 533 Q938 360 856 250 Q763 128 
596 128 Q427 128 336 250 Q256 360 256 533 Q256 696 336 810 Q431 938 596 
938Z" ></glyph>
<glyph unicode="&#114;" horiz-adv-x="680" 
d="M170 1012L170 42Q170 0 189 0 Q208 -42 234 -42 Q259 -42 278 0 Q298 
0 298 42 L298 776Q334 850 402 891 Q478 938 576 938 Q626 938 624 1004 Q
622 1066 578 1066 Q482 1066 406 1034 Q348 998 298 928 L298 1010Q298 1038 278 
1054 Q259 1066 234 1066 Q208 1066 189 1053 Q170 1037 170 1012Z" ></glyph>
<glyph unicode="&#101;" horiz-adv-x="1147" 
d="M878 308Q831 225 770 184 Q692 128 574 128 Q424 128 336 239 Q259 332 252 
469 L1024 469L1024 500Q1024 732 910 892 Q784 1066 574 1066 Q364 1066 240 902 Q
128 750 128 531 Q128 314 240 164 Q364 0 574 0 Q723 0 833 70 Q941 136 
998 262 Q1010 289 998 312 Q988 332 965 339 Q939 349 916 341 Q890 332 878 
308ZM256 597Q268 728 340 822 Q429 938 575 938 Q720 938 811 820 Q884 724 
896 597 Z" ></glyph>
<glyph unicode="&#115;" horiz-adv-x="1062" 
d="M500 1066Q336 1066 235 999 Q128 928 128 804 Q128 688 217 620 Q298 554 478 
512 Q676 469 750 418 Q810 376 810 304 Q810 226 742 179 Q669 128 542 128 Q
389 128 320 174 Q256 216 256 298 Q253 320 230 332 Q208 343 178 341 Q146 
341 128 327 Q126 310 128 286 Q134 146 238 73 Q344 0 540 0 Q736 0 841 
90 Q938 172 938 304 Q938 436 855 511 Q768 592 556 640 Q381 674 316 713 Q
256 751 256 815 Q256 870 324 906 Q392 938 494 938 Q628 938 696 903 Q760 
870 768 808 Q770 788 790 776 Q810 766 834 768 Q861 771 878 785 Q898 802 
896 826 Q887 930 810 993 Q705 1066 500 1066Z" ></glyph>
<glyph unicode="&#105;" horiz-adv-x="510" 
d="M213 1012L213 61Q213 33 231 14 Q250 0 276 0 Q302 0 321 16 Q341 33 
341 61 L341 1012Q341 1038 321 1053 Q302 1066 276 1066 Q250 1066 231 1052 Q213 
1038 213 1012ZM276 1450Q224 1450 196 1418 Q170 1387 170 1342 Q170 1298 196 1270 Q
224 1237 276 1237 Q326 1237 356 1270 Q384 1300 384 1342 Q384 1387 356 1418 Q326 
1450 276 1450Z" ></glyph>
<glyph unicode="&#100;" horiz-adv-x="1232" 
d="M1066 1387Q1066 1418 1046 1435 Q1027 1450 1002 1450 Q976 1450 957 1437 Q938 1420 
938 1391 L938 884Q877 978 792 1022 Q708 1066 596 1066 Q368 1066 240 902 Q128 
756 128 533 Q128 312 240 166 Q368 0 596 0 Q714 0 812 32 Q895 80 938 
150 L938 64Q938 36 957 18 Q976 2 1000 0 Q1027 0 1044 13 Q1064 30 1066 
61 ZM597 938Q763 938 856 813 Q938 702 938 532 Q938 364 856 253 Q763 128 
597 128 Q429 128 336 253 Q256 364 256 532 Q256 702 336 813 Q429 938 597 
938Z" ></glyph>
<glyph unicode="&#95;" horiz-adv-x="1020" 
d="M1024 -170L1024 -42L0 -42L0 -170L1024 -170Z" ></glyph>
<glyph unicode="&#118;" horiz-adv-x="1020" 
d="M412 42Q430 -42 488 -42 Q544 -42 566 42 L938 1000Q946 1022 930 1042 Q918 
1059 892 1066 Q865 1074 843 1066 Q818 1058 810 1032 L490 124L170 1032Q160 1058 
136 1066 Q114 1074 87 1066 Q62 1059 48 1042 Q34 1022 42 1000 Z" ></glyph>
<glyph unicode="&#97;" horiz-adv-x="1147" 
d="M853 271Q806 206 718 169 Q619 128 480 128 Q357 128 298 169 Q256 208 256 
286 Q256 367 315 408 Q376 453 524 469 Q672 489 756 515 Q826 537 853 561 ZM
576 1066Q404 1066 301 999 Q195 928 170 787 Q164 760 183 743 Q200 727 225 
725 Q252 723 271 735 Q294 746 298 767 Q308 848 369 890 Q438 938 574 938 Q
716 938 786 892 Q853 847 853 760 Q853 683 794 654 Q732 624 520 597 Q280 
570 189 474 Q128 407 128 286 Q128 151 212 77 Q298 0 480 0 Q614 0 720 
24 Q796 58 858 116 Q858 54 894 28 Q938 0 1053 0 Q1081 0 1097 20 Q1111 
38 1111 64 Q1109 91 1095 110 Q1079 128 1055 128 Q1019 128 999 146 Q981 163 
981 195 L981 786Q981 934 863 1005 Q759 1066 576 1066Z" ></glyph>
<glyph unicode="&#45;" horiz-adv-x="1275" 
d="M1094 810L182 810Q132 810 134 748 Q134 682 182 682 L1094 682Q1144 682 1144 
748 Q1144 810 1094 810Z" ></glyph>
<glyph unicode="&#52;" horiz-adv-x="1190" 
d="M768 1281L768 469L213 469L768 1281ZM744 1398L108 480Q75 432 85 387 Q97 
341 150 341 L768 341L768 54Q768 28 786 14 Q805 0 832 0 Q856 0 875 
14 Q896 30 896 54 L896 341L1058 341Q1082 341 1096 362 Q1109 380 1109 406 Q
1109 432 1097 450 Q1084 469 1063 469 L896 469L896 1386Q896 1446 838 1450 Q781 
1452 744 1398Z" ></glyph>
<glyph unicode="&#51;" horiz-adv-x="1190" 
d="M595 1450Q394 1450 276 1337 Q170 1235 170 1085 Q170 1057 190 1038 Q206 1024 
234 1024 Q260 1024 277 1038 Q298 1054 298 1080 Q298 1180 371 1248 Q454 1322 597 
1322 Q736 1322 819 1250 Q896 1182 896 1084 Q896 990 818 924 Q736 853 592 853 L
536 853Q480 853 480 790 Q480 725 536 725 L606 725Q768 725 864 635 Q938 
552 938 427 Q938 308 853 223 Q757 128 596 128 Q435 128 339 220 Q256 303 
256 419 L256 458Q256 485 235 499 Q216 512 192 512 Q165 512 146 498 Q128 
483 128 455 L128 415Q128 247 244 130 Q374 0 597 0 Q814 0 946 130 Q1066 
248 1066 414 Q1066 554 983 659 Q919 738 832 774 Q896 800 938 868 Q1024 958 
1024 1079 Q1024 1230 916 1336 Q796 1450 595 1450Z" ></glyph>
<glyph unicode="&#50;" horiz-adv-x="1190" 
d="M596 1450Q394 1450 275 1338 Q170 1238 170 1092 L170 1045Q170 1015 188 997 Q
207 981 233 981 Q260 981 278 996 Q298 1011 298 1036 L298 1083Q298 1182 373 
1248 Q455 1322 596 1322 Q737 1322 821 1244 Q896 1175 896 1071 Q896 944 806 856 Q
737 790 538 682 Q330 551 233 397 Q126 228 128 0 L998 0Q1024 0 1045 
20 Q1066 38 1066 64 Q1066 91 1045 110 Q1024 128 998 128 L256 128Q270 270 
338 360 Q416 467 628 597 L668 623Q864 738 926 805 Q1024 909 1024 1076 Q1024 
1229 917 1333 Q796 1450 596 1450Z" ></glyph>
<glyph unicode="&#49;" horiz-adv-x="1190" 
d="M346 1152L554 1152L554 74Q554 40 573 19 Q592 2 618 0 Q643 0 662 
18 Q682 37 682 69 L682 1403Q682 1452 632 1450 Q581 1449 579 1418 Q573 1342 
507 1308 Q450 1280 341 1280 Q296 1280 298 1216 Q300 1152 346 1152Z" ></glyph>
<glyph unicode="&#48;" horiz-adv-x="1190" 
d="M597 1450Q374 1450 244 1226 Q128 1023 128 725 Q128 429 244 226 Q374 0 597 
0 Q818 0 948 226 Q1066 429 1066 725 Q1066 1023 948 1226 Q818 1450 597 1450ZM
597 1322Q758 1322 853 1137 Q938 970 938 723 Q938 480 853 313 Q758 128 597 
128 Q432 128 339 313 Q256 480 256 723 Q256 970 339 1137 Q432 1322 597 1322Z" ></glyph>
<glyph unicode="&#116;" horiz-adv-x="637" 
d="M213 1347L213 1066L123 1066Q85 1066 85 1004 Q85 938 123 938 L213 938L213 
182Q213 109 266 58 Q324 0 425 0 L528 0Q560 0 580 20 Q597 38 597 
64 Q597 91 578 110 Q558 128 526 128 L442 128Q392 128 364 155 Q341 177 
341 210 L341 938L503 938Q554 938 554 1004 Q554 1066 503 1066 L341 1066L341 
1347Q341 1378 321 1394 Q302 1408 276 1408 Q250 1408 231 1394 Q213 1378 213 1347Z" ></glyph>
<glyph unicode="&#109;" horiz-adv-x="1742" 
d="M170 1000L170 58Q170 32 189 15 Q208 0 234 0 Q259 0 278 14 Q298 30 
298 56 L298 764Q363 858 436 898 Q508 938 612 938 Q706 938 761 887 Q810 
840 810 762 L810 56Q810 30 829 14 Q848 0 874 0 Q899 0 918 14 Q938 
30 938 56 L938 747Q996 849 1066 893 Q1137 938 1250 938 Q1348 938 1401 887 Q
1450 840 1450 762 L1450 56Q1450 30 1469 14 Q1488 0 1514 0 Q1539 0 1558 
14 Q1578 32 1578 58 L1578 800Q1578 902 1496 978 Q1402 1066 1246 1066 Q1132 1066 
1040 1014 Q956 968 904 886 Q868 962 810 1006 Q730 1066 608 1066 Q512 1066 420 
1028 Q346 986 298 922 L298 996Q298 1030 278 1048 Q259 1066 234 1066 Q208 1066 
189 1049 Q170 1032 170 1000Z" ></glyph>
<glyph unicode="&#53;" horiz-adv-x="1190" 
d="M915 1450L275 1450L170 759Q158 697 207 675 Q256 655 297 713 Q338 775 404 
811 Q484 853 580 853 Q748 853 848 742 Q938 640 938 490 Q938 343 844 239 Q
742 128 575 128 Q440 128 356 176 Q276 223 256 300 Q248 326 222 336 Q200 
347 175 341 Q148 336 134 315 Q120 294 128 267 Q156 146 277 72 Q398 
0 566 0 Q822 0 953 152 Q1066 282 1066 490 Q1066 692 948 829 Q820 981 600 
981 Q507 981 428 953 Q354 927 298 873 L375 1322L914 1322Q944 1322 965 1343 Q
981 1361 981 1386 Q981 1414 963 1432 Q945 1450 915 1450Z" ></glyph>
<glyph unicode="&#54;" horiz-adv-x="1190" 
d="M256 477Q256 621 354 716 Q454 810 607 810 Q774 810 862 710 Q938 621 938 
479 Q938 320 844 224 Q752 128 601 128 Q448 128 352 224 Q256 320 256 477ZM
642 1450Q378 1450 244 1196 Q128 974 128 602 Q128 256 279 107 Q390 0 584 
0 Q812 0 937 127 Q1066 256 1066 493 Q1066 711 919 836 Q796 938 641 938 Q
494 938 389 880 Q304 832 256 752 Q270 996 346 1136 Q448 1322 640 1322 Q758 
1322 826 1270 Q884 1226 900 1148 Q912 1099 972 1109 Q1034 1120 1028 1170 Q1008 
1313 894 1386 Q794 1450 642 1450Z" ></glyph>
<glyph unicode="&#55;" horiz-adv-x="1190" 
d="M184 1322L896 1322Q682 1072 512 672 Q366 326 332 79 Q326 45 344 22 Q362 
2 390 0 Q418 -2 440 15 Q464 36 470 73 Q522 478 728 874 Q882 1175 1047 
1329 Q1086 1370 1066 1410 Q1046 1450 991 1450 L184 1450Q128 1450 128 1388 Q128 
1322 184 1322Z" ></glyph>
<glyph unicode="&#56;" horiz-adv-x="1190" 
d="M1024 1092Q1024 1238 915 1339 Q794 1450 596 1450 Q398 1450 277 1338 Q170 1234 
170 1087 Q170 964 238 878 Q290 811 362 782 Q250 725 188 634 Q85 538 85 
415 Q85 248 213 130 Q350 0 576 0 Q804 0 941 132 Q1066 252 1066 422 Q
1066 546 998 648 Q932 744 826 791 Q900 819 954 885 Q1024 972 1024 1092ZM575 
725Q742 725 844 632 Q938 548 938 426 Q938 304 844 220 Q742 128 575 128 Q
405 128 305 220 Q213 304 213 426 Q213 548 305 632 Q405 725 575 725ZM595 
1322Q734 1322 818 1250 Q896 1184 896 1088 Q896 992 818 926 Q734 853 595 853 Q
456 853 374 926 Q298 992 298 1088 Q298 1184 374 1250 Q456 1322 595 1322Z" ></glyph>
<glyph unicode="&#57;" horiz-adv-x="1190" 
d="M938 964Q938 826 838 734 Q738 640 587 640 Q418 640 330 741 Q256 826 256 
962 Q256 1128 348 1226 Q438 1322 593 1322 Q758 1322 852 1216 Q938 1118 938 964ZM
554 0Q832 0 950 196 Q1066 389 1066 841 Q1066 1180 913 1322 Q802 1450 612 
1450 Q382 1450 257 1324 Q128 1192 128 946 Q128 720 274 604 Q391 512 554 512 Q
697 512 804 572 Q888 620 938 696 Q924 396 847 272 Q758 128 557 128 Q438 
128 370 181 Q312 225 298 300 Q284 351 224 341 Q162 330 170 279 Q186 138 
300 66 Q400 0 554 0Z" ></glyph>
</font>
</defs>
</svg>
</div>
</div>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<hr class="pagebreak"/>
<div id="IDX4" style="padding-bottom: 8px; padding-top: 1px">
<table class="table" style="border-spacing: 0" aria-label="데이터셋 WORK.OVERALLANOVA">
<caption aria-label="데이터셋 WORK.OVERALLANOVA"></caption>
<colgroup><col/></colgroup><colgroup><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead>
<tr>
<th class="r header" scope="col">OBS</th>
<th class="header" scope="col">Dependent</th>
<th class="header" scope="col">Source</th>
<th class="r header" scope="col">DF</th>
<th class="r header" scope="col">SS</th>
<th class="r header" scope="col">MS</th>
<th class="r header" scope="col">FValue</th>
<th class="r header" scope="col">ProbF</th>
</tr>
</thead>
<tbody>
<tr>
<th class="r rowheader" scope="row">1</th>
<td class="data">weight</td>
<td class="data">Model</td>
<td class="r data">3</td>
<td class="r data">338.9373684</td>
<td class="r data">112.9791228</td>
<td class="r data">12.04</td>
<td class="r data">0.0003</td>
</tr>
<tr>
<th class="r rowheader" scope="row">2</th>
<td class="data">weight</td>
<td class="data">Error</td>
<td class="r data">15</td>
<td class="r data">140.7500000</td>
<td class="r data">9.3833333</td>
<td class="r data">_</td>
<td class="r data">_</td>
</tr>
<tr>
<th class="r rowheader" scope="row">3</th>
<td class="data">weight</td>
<td class="data">Corrected Total</td>
<td class="r data">18</td>
<td class="r data">479.6873684</td>
<td class="r data">_</td>
<td class="r data">_</td>
<td class="r data">_</td>
</tr>
</tbody>
</table>
</div>
</div>
<div style="padding-bottom: 8px; padding-top: 1px">
<hr class="pagebreak"/>
<div id="IDX5" style="padding-bottom: 8px; padding-top: 1px">
<table class="table" style="border-spacing: 0" aria-label="데이터셋 WORK.CLDIFFS">
<caption aria-label="데이터셋 WORK.CLDIFFS"></caption>
<colgroup><col/></colgroup><colgroup><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead>
<tr>
<th class="r header" scope="col">OBS</th>
<th class="header" scope="col">Effect</th>
<th class="header" scope="col">Dependent</th>
<th class="header" scope="col">Method</th>
<th class="header" scope="col">Comparison</th>
<th class="r header" scope="col">LowerCL</th>
<th class="r header" scope="col">Difference</th>
<th class="r header" scope="col">UpperCL</th>
<th class="r header" scope="col">Significance</th>
</tr>
</thead>
<tbody>
<tr>
<th class="r rowheader" scope="row">1</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">3 - 2</td>
<td class="r data" style="white-space: nowrap">-3.872</td>
<td class="r data">2.050</td>
<td class="r data">7.972</td>
<td class="r data">0</td>
</tr>
<tr>
<th class="r rowheader" scope="row">2</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">3 - 1</td>
<td class="r data">2.808</td>
<td class="r data">8.730</td>
<td class="r data">14.652</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">3</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">3 - 4</td>
<td class="r data">4.188</td>
<td class="r data">10.110</td>
<td class="r data">16.032</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">4</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">2 - 3</td>
<td class="r data" style="white-space: nowrap">-7.972</td>
<td class="r data" style="white-space: nowrap">-2.050</td>
<td class="r data">3.872</td>
<td class="r data">0</td>
</tr>
<tr>
<th class="r rowheader" scope="row">5</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">2 - 1</td>
<td class="r data">1.096</td>
<td class="r data">6.680</td>
<td class="r data">12.264</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">6</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">2 - 4</td>
<td class="r data">2.476</td>
<td class="r data">8.060</td>
<td class="r data">13.644</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">7</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">1 - 3</td>
<td class="r data" style="white-space: nowrap">-14.652</td>
<td class="r data" style="white-space: nowrap">-8.730</td>
<td class="r data" style="white-space: nowrap">-2.808</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">8</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">1 - 2</td>
<td class="r data" style="white-space: nowrap">-12.264</td>
<td class="r data" style="white-space: nowrap">-6.680</td>
<td class="r data" style="white-space: nowrap">-1.096</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">9</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">1 - 4</td>
<td class="r data" style="white-space: nowrap">-4.204</td>
<td class="r data">1.380</td>
<td class="r data">6.964</td>
<td class="r data">0</td>
</tr>
<tr>
<th class="r rowheader" scope="row">10</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">4 - 3</td>
<td class="r data" style="white-space: nowrap">-16.032</td>
<td class="r data" style="white-space: nowrap">-10.110</td>
<td class="r data" style="white-space: nowrap">-4.188</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">11</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">4 - 2</td>
<td class="r data" style="white-space: nowrap">-13.644</td>
<td class="r data" style="white-space: nowrap">-8.060</td>
<td class="r data" style="white-space: nowrap">-2.476</td>
<td class="r data">1</td>
</tr>
<tr>
<th class="r rowheader" scope="row">12</th>
<td class="data">diet</td>
<td class="data">weight</td>
<td class="data">Tukey</td>
<td class="data">4 - 1</td>
<td class="r data" style="white-space: nowrap">-6.964</td>
<td class="r data" style="white-space: nowrap">-1.380</td>
<td class="r data">4.204</td>
<td class="r data">0</td>
</tr>
</tbody>
</table>
</div>
</div>
</body>
</html>




**[10.1] A single-Factor Analysis of Variance (Model1)**

**데이터: 19마리 돼지를 4개 그룹으로 무작위로 나누어 서로 다른 다이어트 처리 후 중량(kg) 측정**

$$
\begin{aligned}
H_0&:\ \mu_1=\mu_2=\mu_3=\mu_4.\\
H_A&:\ The\ mean\ weights\ of\ pigs\ on\ the\ four\ diets\ are\ not\ all\ equal.\\
\alpha&=0.05\\
\end{aligned}
$$

**단일 요인 분산 분석 ANOVA는 완전히 랜덤화 된 실험 설계 방법이다.**

**"completely randomized design"이라고도 한다.**

**일반적으로 데이터에서 그룹의 통계적 비교는 각 그룹의 데이터 수가 동일한 경우(균형적인 상황)에 가장 적합하다.**

**균형적일 때 검정의 검정력이 높아진다.**

**하지만 10.1의 데이터의 경우 4개의 그룹 각각에 5마리의 실험동물이 있지만, Feed 3에 속한 한 마리의 동물의 몸무게는 어떤 적절한 이유로 분석에 사용되지 않았다. 아니면 병이 났거나 임신한 것으로 밝혀졌을 수도 있다.** 

**분산 분석에서는 실험 요인을 제외한 모든 면에서 가능한 유사해야 한다.**

**(즉, 동물은 동일한 품종, 성별, 연령이어야 하고, 같은 온도로 유지되어야 한다.)**

**Shapiro-Wilk test로 네 그룹의 정규성을 평가하였다.**

**네 그룹 모두 정규성 검정 결과 p-value가 0.05보다 매우 크므로 네 그룹 모두 모집단이 정규성을 따르지 않는다는 대립가설을 채택할 충분한 근거가 없다.**

**따라서 정규성을 만족한다는 사실을 바탕으로 그룹별 등분산성 검정을 진행한다.**

**levene test와 Bartlett's Test를 통해 네 그룹의 등분산성 검정을 진행하였다.**

**levene test의 유의확률은 p-value = 0.6643으로 유의수준 0.05보다 매우 크므로 등분산성이 아니라는 대립가설을 채택할 근거가 없다.**

**Bartlett's Test의 유의확률은 p-value = 0.9243으로 유의수준 0.05보다 매우 크므로 등분산성이 아니라는 대립가설을 채택할 근거가 없다.**

**proc glm 프로시저와 gplot 프로시저를 통해 잔차들의 산포도를 그렸으며, 독립성임을 확인할 수 있다.**

**Shapiro-Wilk test와 levene test으로 정규성과 등분산성을 확인하였으므로 ANOVA 검정을 진행한다.**

**diet 네그룹간 평균의 차이가 있는지 검정했을 때, p=0.0003로 귀무가설 기각하므로, 네 그룹간 평균의 차이가 있다.**

**사후검정(Tukey's HSD test)을 통해 어떠한 그룹의 평균이 차이가 있는지 살펴보았으며, 1-3, 3-4, 2-1, 2-4간 차이가 있음을 알 수 있다.**

**[11.10] Tukey-Type multiple comparision for differences among Four variances(i.e., k=4)**

$H_0$ : **네 그룹의 분산이 차이가 없다.**

$H_A$ : **not H0**


**네 개 그룹의 분산이 차이가 있는지 사후검정 해본 결과, 그룹2와 그룹 3만 유의한 차이가 있었다.**
