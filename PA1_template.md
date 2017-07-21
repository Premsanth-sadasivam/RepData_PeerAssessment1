---
title: "Reproducible-Research"
author: "prem"
date: "July 21, 2017"
output: html_document
---

# 1.Loading and preprocessing the data.
The below mentioed code is used to load the data in to an object.
<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">act</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;C:/Users/premsanth/Desktop/Data science coursera/R_programming/repdata_data_activity/activity.csv&quot;</span><span class="hl std">)</span>
<span class="hl kwd">library</span><span class="hl std">(lubridate)</span>
<span class="hl std">act</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ymd</span><span class="hl std">(act</span><span class="hl opt">$</span><span class="hl std">date)</span> <span class="hl com">##This line transform the data type of the date field.</span>
</pre></div>
</div></div>

# 2.What is mean total number of steps taken per day?

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>
<span class="hl std">Total_steps_perday</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tapply</span><span class="hl std">(</span><span class="hl kwc">X</span><span class="hl std">=act</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">INDEX</span> <span class="hl std">= act</span><span class="hl opt">$</span><span class="hl std">date,</span><span class="hl kwc">FUN</span><span class="hl std">=sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl std">Total_steps_perday_mat</span><span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">Date</span> <span class="hl std">=</span> <span class="hl kwd">rownames</span><span class="hl std">(Total_steps_perday),Total_steps_perday,</span><span class="hl kwc">row.names</span><span class="hl std">=</span><span class="hl kwa">NULL</span><span class="hl std">)</span> <span class="hl com"># To convert the array in to dataframe.</span>
</pre></div>
</div></div>

### The sample of total number of steps taken per day is (only head of the table is printed below)

<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">head</span><span class="hl std">(Total_steps_perday_mat))</span>
</pre></div>
<div class="output"><pre class="knitr r">##         Date Total_steps_perday
## 1 2012-10-01                  0
## 2 2012-10-02                126
## 3 2012-10-03              11352
## 4 2012-10-04              12116
## 5 2012-10-05              13294
## 6 2012-10-06              15420
</pre></div>
</div></div>

### A histogram of the total number of steps taken each day

<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">hist</span><span class="hl std">(Total_steps_perday)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" class="plot" /></div>
</div></div>

### The mean and median of the total number of steps taken per day.
<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">(</span><span class="hl kwd">summary</span><span class="hl std">(Total_steps_perday))[</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Mean&quot;</span><span class="hl std">,</span><span class="hl str">&quot;Median&quot;</span><span class="hl std">)]</span>
</pre></div>
<div class="output"><pre class="knitr r">##     Mean   Median 
##  9354.23 10395.00
</pre></div>
</div></div>

# 3.The average daily activity pattern.
### Time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days.
<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl kwd">library</span><span class="hl std">(reshape2)</span>
<span class="hl std">Average_steps_Interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tapply</span><span class="hl std">(</span><span class="hl kwc">X</span><span class="hl std">=act</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">INDEX</span> <span class="hl std">= act</span><span class="hl opt">$</span><span class="hl std">interval,</span><span class="hl kwc">FUN</span><span class="hl std">=mean,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">ggplot</span><span class="hl std">(</span><span class="hl kwd">melt</span><span class="hl std">(Average_steps_Interval),</span><span class="hl kwd">aes</span><span class="hl std">(Var1,value))</span><span class="hl opt">+</span><span class="hl kwd">geom_line</span><span class="hl std">()</span><span class="hl opt">+</span><span class="hl kwd">geom_point</span><span class="hl std">()</span><span class="hl opt">+</span><span class="hl kwd">scale_x_discrete</span><span class="hl std">(</span><span class="hl kwc">limits</span><span class="hl std">=</span><span class="hl kwd">seq</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span><span class="hl num">2355</span><span class="hl std">,</span><span class="hl kwc">by</span><span class="hl std">=</span><span class="hl num">150</span><span class="hl std">))</span><span class="hl opt">+</span><span class="hl kwd">labs</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=</span> <span class="hl str">&quot;5 minitues interval&quot;</span><span class="hl std">,</span><span class="hl kwc">y</span><span class="hl std">=</span><span class="hl str">&quot;number of steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" class="plot" /></div>
</div></div>


#### 5-minute interval contains the maximum number of steps

The 5-minute interval contains the maximum number of steps is <code class="knitr inline">835</code>

# 4.Imputing missing values.
Total number of missing values in the dataset is `length(which(!complete.cases(act)))`
### Filling in all of the missing values in the dataset.
The code for Filling in all of the missing values in the dataset.
<div class="chunk" id="unnamed-chunk-7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">imput_act</span> <span class="hl kwb">&lt;-</span> <span class="hl std">act</span>
<span class="hl std">imput_act[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(imput_act</span><span class="hl opt">$</span><span class="hl std">steps)),</span><span class="hl str">&quot;steps&quot;</span><span class="hl std">]</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.vector</span><span class="hl std">(Average_steps_Interval[(</span><span class="hl kwd">as.character</span><span class="hl std">(imput_act[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(imput_act</span><span class="hl opt">$</span><span class="hl std">steps)),</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">]))])</span>
</pre></div>
</div></div>

The head of new dataset that is equal to the original dataset but with the missing data filled in is
<div class="chunk" id="unnamed-chunk-8"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">head</span><span class="hl std">(imput_act)</span>
</pre></div>
<div class="output"><pre class="knitr r">##       steps       date interval
## 1 1.7169811 2012-10-01        0
## 2 0.3396226 2012-10-01        5
## 3 0.1320755 2012-10-01       10
## 4 0.1509434 2012-10-01       15
## 5 0.0754717 2012-10-01       20
## 6 2.0943396 2012-10-01       25
</pre></div>
</div></div>

## For the new dataset.

### total daily number of steps
<div class="chunk" id="unnamed-chunk-9"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">Total_steps_perday_new</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tapply</span><span class="hl std">(</span><span class="hl kwc">X</span><span class="hl std">=imput_act</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">INDEX</span> <span class="hl std">= imput_act</span><span class="hl opt">$</span><span class="hl std">date,</span><span class="hl kwc">FUN</span><span class="hl std">=sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
</pre></div>
</div></div>

### Mean and median

<div class="chunk" id="unnamed-chunk-10"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">(</span><span class="hl kwd">summary</span><span class="hl std">(Total_steps_perday_new))[</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Mean&quot;</span><span class="hl std">,</span><span class="hl str">&quot;Median&quot;</span><span class="hl std">)]</span>
</pre></div>
<div class="output"><pre class="knitr r">##     Mean   Median 
## 10766.19 10766.19
</pre></div>
</div></div>

### histogram

<div class="chunk" id="unnamed-chunk-11"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">hist</span><span class="hl std">(Total_steps_perday_new)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" class="plot" /></div>
</div></div>

# Activity patterns between weekdays and weekends.
### The code for creating a new factor variable in the dataset.

<div class="chunk" id="unnamed-chunk-12"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">imput_act</span><span class="hl opt">$</span><span class="hl std">days</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">factor</span><span class="hl std">(</span><span class="hl kwd">wday</span><span class="hl std">(imput_act</span><span class="hl opt">$</span><span class="hl std">date))</span>
<span class="hl kwd">levels</span><span class="hl std">(imput_act</span><span class="hl opt">$</span><span class="hl std">days)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">week_day</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;2&quot;</span><span class="hl std">,</span><span class="hl str">&quot;3&quot;</span><span class="hl std">,</span><span class="hl str">&quot;4&quot;</span><span class="hl std">,</span><span class="hl str">&quot;5&quot;</span><span class="hl std">,</span><span class="hl str">&quot;6&quot;</span><span class="hl std">),</span> <span class="hl kwc">week_end</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;1&quot;</span><span class="hl std">,</span><span class="hl str">&quot;7&quot;</span><span class="hl std">))</span>
<span class="hl std">Average_steps_Interval_weekday</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tapply</span><span class="hl std">(</span><span class="hl kwc">X</span><span class="hl std">=act</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">INDEX</span> <span class="hl std">=</span> <span class="hl kwd">list</span><span class="hl std">(imput_act</span><span class="hl opt">$</span><span class="hl std">days,imput_act</span><span class="hl opt">$</span><span class="hl std">interval),</span><span class="hl kwc">FUN</span><span class="hl std">=mean,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
</pre></div>
</div></div>

## plot containing a time series plot for weekday and week end.

<div class="chunk" id="unnamed-chunk-13"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">ggplot</span><span class="hl std">(</span><span class="hl kwd">melt</span><span class="hl std">(Average_steps_Interval_weekday),</span><span class="hl kwd">aes</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=Var2,</span><span class="hl kwc">y</span><span class="hl std">=value))</span><span class="hl opt">+</span><span class="hl kwd">facet_grid</span><span class="hl std">(Var1</span><span class="hl opt">~</span><span class="hl std">.)</span><span class="hl opt">+</span><span class="hl kwd">geom_line</span><span class="hl std">()</span><span class="hl opt">+</span><span class="hl kwd">theme_light</span><span class="hl std">()</span><span class="hl opt">+</span><span class="hl kwd">labs</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=</span> <span class="hl str">&quot;5 minitues interval&quot;</span><span class="hl std">,</span><span class="hl kwc">y</span><span class="hl std">=</span><span class="hl str">&quot;number of steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" class="plot" /></div>
</div></div>

# End of the document.



