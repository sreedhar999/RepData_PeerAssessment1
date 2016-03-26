

## Overview of the data set

This assignment makes use of data from a personal activity monitoring
device. This device collects data at 5 minute intervals throughout the
day. The data consists of two months of data from one anonymous
individual collected during the months of October and November, 2012
and includes the number of steps taken in 5 minute intervals each day.

Each interval represents a 5-minute time within the day.  For example,
the interval 1730 represents the data collected between 17:30 to
17:35.

There are missing data in the `steps` column, which we deal with in
this project by imputing the missing values based on the average
values for those intervals for all records that have data.

## Loading and preprocessing the data


The data are contained in a CSV file called `activity.csv`.  The first
step in our process is to assure that CSV file is available.  The data
are included in a compressed zip file in this project.




## What is the mean total number of steps taken per day?
This is calculated by first determining the total number of steps for each day, then giving the mean



<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(activity.csv,</span> <span class="hl kwc">header</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">colClasses</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;numeric&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;Date&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;numeric&quot;</span><span class="hl std">))</span>
</pre></div>
<div class="error"><pre class="knitr r">## Error in read.table(file = file, header = header, sep = sep, quote = quote, : object 'activity.csv' not found
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">gort</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">na.omit</span><span class="hl std">(activity)</span>
<span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">steps</span><span class="hl std">=gort</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">date</span><span class="hl std">=gort</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl std">dt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.table</span><span class="hl std">(df)</span>
<span class="hl std">dt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">dt[,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps),</span> <span class="hl kwc">sd</span><span class="hl std">=</span><span class="hl kwd">sd</span><span class="hl std">(steps),</span> <span class="hl kwc">tot</span><span class="hl std">=</span><span class="hl kwd">sum</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=date]</span>
</pre></div>
</div></div>

Here is a histogram of the daily total number of steps (showing frequency)

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">hist</span><span class="hl std">(dt</span><span class="hl opt">$</span><span class="hl std">tot,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Distribution of total steps per day&quot;</span><span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;# daily steps&quot;</span>  <span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" class="plot" /></div>
</div></div>

The mean of the total number of steps taken each day is **<code class="knitr inline">10,766.19</code>**

The median is **<code class="knitr inline">10,765.00</code>**

## What is the average daily activity pattern?
<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">steps</span><span class="hl std">=gort</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">interval</span><span class="hl std">=gort</span><span class="hl opt">$</span><span class="hl std">interval)</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.table</span><span class="hl std">(df)</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">intervaldt[,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=interval]</span>
<span class="hl std">intmax</span> <span class="hl kwb">&lt;-</span> <span class="hl std">intervaldt[intervaldt</span><span class="hl opt">$</span><span class="hl std">mean</span> <span class="hl opt">==</span> <span class="hl kwd">max</span><span class="hl std">(intervaldt</span><span class="hl opt">$</span><span class="hl std">mean),]</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">interval,</span> <span class="hl kwc">y</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">mean,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Average steps per 5-minute interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;avg # steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" class="plot" /></div>
</div></div>

The maximum number of average steps across all days takes place in
time interval **<code class="knitr inline">835</code>**, which has a value of
**<code class="knitr inline">206.17</code>**



## Imputing missing values

There are quite a number of measurement periods with missing data for
the number of steps (coded as `NA`).  Because this missing data could
skew or bias calculations, we will try to impute the missing values.

The method used here is a simple average of time intervals.  We will
replace each missing value with the average number of steps for that
interval

<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">## First, add timestamp to convert date and interval into POSIXct</span>
<span class="hl std">activity</span><span class="hl opt">$</span><span class="hl std">timestamp</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.POSIXct</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">date,</span> <span class="hl kwd">formatC</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">interval</span><span class="hl opt">/</span><span class="hl num">100</span><span class="hl std">,</span> <span class="hl kwc">decimal.mark</span><span class="hl std">=</span><span class="hl str">&quot;:&quot;</span><span class="hl std">,</span> <span class="hl kwc">digits</span><span class="hl std">=</span><span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">format</span><span class="hl std">=</span><span class="hl str">&quot;f&quot;</span><span class="hl std">)),</span> <span class="hl kwc">tz</span><span class="hl std">=</span><span class="hl str">&quot;GMT&quot;</span><span class="hl std">)</span>
<span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.frame</span><span class="hl std">(</span><span class="hl kwc">steps</span><span class="hl std">=activity</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">interval</span><span class="hl std">=activity</span><span class="hl opt">$</span><span class="hl std">interval)</span>
<span class="hl std">activitydt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.table</span><span class="hl std">(activity)</span>
<span class="hl com">## calculate mean for time intervals that have no missing data</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">data.table</span><span class="hl std">(df)</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">intervaldt[,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)),</span> <span class="hl kwc">by</span><span class="hl std">=interval]</span>
<span class="hl std">intmax</span> <span class="hl kwb">&lt;-</span> <span class="hl std">intervaldt[intervaldt</span><span class="hl opt">$</span><span class="hl std">mean</span> <span class="hl opt">==</span> <span class="hl kwd">max</span><span class="hl std">(intervaldt</span><span class="hl opt">$</span><span class="hl std">mean),]</span>
<span class="hl com">## now merge the average values with the missing interval values</span>
<span class="hl std">imputed</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">merge</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=activitydt,</span> <span class="hl kwc">y</span><span class="hl std">=intervaldt,</span> <span class="hl kwc">by</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">)</span>
<span class="hl com">## replace all missing values with the mean number of steps for that interval</span>
<span class="hl kwa">for</span> <span class="hl std">(i</span> <span class="hl kwa">in</span> <span class="hl num">1</span><span class="hl opt">:</span><span class="hl kwd">nrow</span><span class="hl std">(imputed)) imputed[i]</span><span class="hl opt">$</span><span class="hl std">steps</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ifelse</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(imputed[i]</span><span class="hl opt">$</span><span class="hl std">steps), imputed[i]</span><span class="hl opt">$</span><span class="hl std">mean, imputed[i]</span><span class="hl opt">$</span><span class="hl std">steps)</span>
</pre></div>
</div></div>

Here is a histogram of the daily total number of steps with imputed missing values

<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">dailyall</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputed[,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">tot</span><span class="hl std">=</span><span class="hl kwd">sum</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=date]</span>
<span class="hl kwd">hist</span><span class="hl std">(dailyall</span><span class="hl opt">$</span><span class="hl std">tot,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Distribution of total steps per day with imputed values&quot;</span><span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;# steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl std">meanall</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(dailyall</span><span class="hl opt">$</span><span class="hl std">tot)</span>
<span class="hl std">medianall</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">(dailyall</span><span class="hl opt">$</span><span class="hl std">tot)</span>
</pre></div>
</div></div>

The mean of the total number of steps taken each day is **<code class="knitr inline">10,766.19</code>**

The median is **<code class="knitr inline">10,766.19</code>**

With the imputed values added in, the median and mean now match.

## Are there differences in activity patterns between weekdays and weekends?
Let's look at possible differences in activity patterns between weekdays and weekends

<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">imputed</span><span class="hl opt">$</span><span class="hl std">weekday</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ifelse</span><span class="hl std">(</span> <span class="hl kwd">wday</span><span class="hl std">(imputed</span><span class="hl opt">$</span><span class="hl std">timestamp)</span> <span class="hl opt">%in%</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">7</span><span class="hl std">),</span> <span class="hl str">&quot;weekend&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;weekday&quot;</span><span class="hl std">)</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputed[imputed</span><span class="hl opt">$</span><span class="hl std">weekday</span><span class="hl opt">==</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=interval]</span>
<span class="hl kwd">par</span><span class="hl std">(</span><span class="hl kwc">mfrow</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">2</span><span class="hl std">,</span><span class="hl num">1</span><span class="hl std">))</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputed[imputed</span><span class="hl opt">$</span><span class="hl std">weekday</span><span class="hl opt">==</span><span class="hl str">&quot;weekday&quot;</span><span class="hl std">,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=interval]</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">interval,</span>
     <span class="hl kwc">y</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">mean,</span>
     <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Weekday&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;# steps&quot;</span><span class="hl std">)</span>
<span class="hl std">intervaldt</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputed[imputed</span><span class="hl opt">$</span><span class="hl std">weekday</span><span class="hl opt">==</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">,</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">mean</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span><span class="hl std">=interval]</span>
<span class="hl kwd">plot</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">interval,</span>
     <span class="hl kwc">y</span><span class="hl std">=intervaldt</span><span class="hl opt">$</span><span class="hl std">mean,</span>
     <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Weekend&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;# steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" class="plot" /></div>
</div></div>

