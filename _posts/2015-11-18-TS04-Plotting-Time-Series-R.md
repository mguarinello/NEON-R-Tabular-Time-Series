---
layout: post
title: "Lesson 04: Plotting Time Series"
date:   2015-10-21
authors: [Megan A. Jones, Marisa Guarinello, Courtney Soderberg]
contributors: [Leah A. Wasser, Michael Patterson]
dateCreated: 2015-10-22
lastModified: 2015-11-19
tags: [module-1]
packagesLibraries: [lubridate, ggplot2, scales, gridExtra, dplyr]
category: 
description: "This lesson will teach individuals how to conduct basic data manipulation and create plots of time series data using ggplot2."
code1:
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: /R/Plotting-Time-Series
comments: false
---

{% include _toc.html %}


##About
This activity will walk you through the fundamentals of data manipulation and 
basic plotting.

**R Skill Level:** Intermediate - you've got the basics of `R` down.


<div id="objectives" markdown="1">

### Goals / Objectives
After completing this lesson, you will:
 * Be able to create basic time series plots in `R`
 * Know several ways to manipulate data in `R`

###Things You'll Need To Complete This Lesson
Please be sure you have the most current version of `R` and, preferably,
RStudio to write your code.

####R Libraries to Install
<li><strong>lubridate:</strong> <code> install.packages("lubridate")</code></li>
<li><strong>ggplot2:</strong> <code> install.packages("ggplot2")</code></li>
<li><strong>scales:</strong> <code> install.packages("scales")</code></li>
<li><strong>gridExtra:</strong> <code> install.packages("gridExtra")</code></li>
<li><strong>dplyr:</strong> <code> install.packages("dplyr")</code></li>

  <a href="http://neondataskills.org/R/Packages-In-R/" target="_blank">
More on Packages in R - Adapted from Software Carpentry.</a>

####Data to Download
Make sure you have downloaded the AtmosData folder from
http://figshare.com/articles/NEON_Spatio_Temporal_Teaching_Dataset/1580068


The data used in this tutorial were collected at Harvard Forest which is
a the National Ecological Observatory Network field site <a href="http://www.neoninc.org/science-design/field-sites/harvard-forest" target="_blank">
More about the NEON Harvard Forest field site</a>. These data are proxy data for what will be
available for 30 years from the NEON flux tower [from the NEON data portal](http://data.neoninc.org/ "NEON data").


####Setting the Working Directory
The code in this lesson assumes that you have set your working directory to the
location of the unzipped file of data downloaded above.  If you would like a
refresher on setting the working directory, please view the <a href="XXX" target="_blank">Setting A Working Directory In R</a> lesson prior to beginning the lesson.

####Recommended Pre-Lesson Reading

</div>

Working with micro-meterology data from Harvard Forest, we are going to
learn skills that will enable us to visualize our data by:

1) plotting 15-minute air temperature data across years,
2) manipulating the date to group and summarize by year, daily average, and 
julian date, and 
3) comparing plots of 15-minute and daily average air temperature. 

You will need the daily and 15-minute micro-meteorology data for 2009-2011 from
the Harvard Forest. If you do not have these data sets, as R objects created in
<a href="XXX" target="_blank">Tabular Time Series Lesson 03: Subset Data and No Data Values</a>,
please load them from the .csv files in the downloaded data.  


    #Remember it is good coding technique to add additional libraries to the top of
      #your script 
    library (ggplot2)  #for creating graphs
    library (scales)   #to access breaks/formatting functions
    
    #set working directory to ensure R can find the file we wish to import
    #setwd("working-dir-path-here")
    
    #15-min Harvard Forest met data, 2009-2011
    harMet15.09.11<- read.csv(file="Met_HARV_15min_2009_2011.csv",
                              stringsAsFactors = FALSE)
    #convert datetime to POSIXct
    harMet15.09.11$datetime<-as.POSIXct(harMet15.09.11$datetime,
                        format = "%Y-%m-%dT%H:%M",
                        tz = "America/New_York")
    #daily HARV met data, 2009-2011
    harMetDaily.09.11 <- read.csv(file="AtmosData/HARV/hf001-06-daily-m.csv",
                         stringsAsFactors = FALSE)
    
    #covert date to Date type
    
    harMetDaily.09.11$date<-as.Date(harMetDaily.09.11$date)

##Plotting Time Series Data
Visualization of data is important as it allow us to start to get a sense of 
general trends, as well as see possible outliers or non-sensical values. 

###Intro to ggplot2 

Using the package `ggplot2` we could use the simple `qplot()` function to make a
quick plot of the air temperature (`airt`) across all three years using the
15-min data. 


    qplot (x=datetime, y=airt,
          data= harMet15.09.11,
          main= "Air temperature at the Harvard Forest 2009-2011",
          xlab= "Date", ylab= "Temperature (°C)")

    ## Error in seq.int(0, to0 - from, by): 'to' cannot be NA, NaN or infinite

We get a warning message reminding us there there are two missing values in the 
air temperature data and a plot showing the pattern of air temperature across
the three years.

While `qplot` is a simple way to quickly plot data it is limited in it's ability
to be modified and customized into professional looking plots.  The `ggplot()` 
function within `ggplot2` package allows the user to have much more control
over the appearance of the plot.  

Let's plot the same data again using `ggplot()`.  While `ggplot()` is based on 
using much of the information we just used with `qplot` however the syntax is 
different: 

* the data source is now the first element,
* `aes` (aesthetics) is where the field names for the x, y (and 
other) axes are entered,  
* `geom_XXX`, specifically `geom_point`, is used to define the type of
demarkation you want to represent the data.  We want a scatterplot so we are
using geom_point. The default is small round filled circles, 
* within the parameters for `geom_point` we will specify that all na values 
shoud be removed `na.rm=TRUE`.  This will prevent the warning messages from 
showing,
* `ggtitle` is now how we name the plot,
* and `xlab` and `ylab` are still how we label the axes.  

Remember `help(ggplot2)` will list the many other parameters that can be 
defined.  

    #plot Air Temperature Data across 2009-2011 using 15-minute data
    AirTemp15a <- ggplot(harMet15.09.11, aes(datetime, airt)) +
               geom_point(na.rm=TRUE) +    #na.rm=TRUE prevents a warning stating
                                          # that 2 NA values were removed.
               ggtitle("15 min Air Temperature At Harvard Forest") +
               xlab("Date") + ylab("Air Temperature (C)")
    AirTemp15a

    ## Error in seq.int(0, to0 - from, by): 'to' cannot be NA, NaN or infinite

Notice we created an object (AirTemp15a) that is our plot.  We then have to
write in the object name to get the plot to appear.  Creating plots as an 
`R object` has many advantages including being able to simply add to a plot as we
will do in just a few lines and to be able to recall a plot later on in code as
we do at the end of this lesson. 

The dates on the x-axis in this last plot are unreadable and not particularly
well formatted. We can reformat them so they are in the Month/Day/Year format by 
simply adding onto the `AirTemp15a` plot that we just created. 

    #format x axis with dates
    AirTemp15b<-AirTemp15a + scale_x_datetime(labels=date_format ("%m/%d/%y") )
    AirTemp15b

    ## Error in seq.int(0, to0 - from, by): 'to' cannot be NA, NaN or infinite

The dates are now legible and in a format that is more common. 

Bonus: If an x variable is not a datetime class (e.g., not POSIX),
`scale_x_...()` exist to help format x-axes. {.notice2}

The ability to customize nearly every aspect of the plot is one of the reasons
to use `ggplot2`. We can make the labels and title look better as well by
specifying aspects of `theme()`.  


    #format x axis with dates
    AirTemp15<-AirTemp15b +
      theme(plot.title = element_text(lineheight=.8, face="bold",size = 20)) +
      theme(text = element_text(size=20)) 
    AirTemp15

    ## Error in seq.int(0, to0 - from, by): 'to' cannot be NA, NaN or infinite


##Challenge 2: Plotting daily precipitaiton data
Using the daily precipitation data you imported earlier, create a plot with the
x-axis in a European Format (Day/Month/Year).  For ease in future activities 
name the plot 'PrecipDaily".


    PrecipDaily <- ggplot(harMetDaily.09.11, aes(date, prec)) +
               geom_point() +
               ggtitle("Daily Precipitation at Harvard Forest") +
               theme(plot.title = element_text(lineheight=.8, face="bold",
                     size = 20)) +
               theme(text = element_text(size=20)) +
               xlab("Date") + ylab("Precipitation (mm)") +
              scale_x_datetime(labels=date_format ("%d/%m/%y"))
    
    PrecipDaily

    ## Error: Invalid input: time_trans works with objects of class POSIXct only

We will return to precipitation data in Challenge 4 and discuss this plot. 

##Manipulating Data using `dplyr`
Looking back at the 15 minute air temperature plot, the data are interesting and
we can, unsurprisingly, see a clear seasonal trend. Yet the air temperature data
at 15 minute intervals is at a finer time-step than we want. Instead, we  want to 
look at how daily average temperature changes over time. To do this we first 
need to learn a bit about how to manipulate data in R. We'll use the `dplyr` 
package. 


    library (dplyr)   #aid with manipulating data #remember it is good coing practice to load all packages at the beginning of your script

The `dplyr` package is designed to simplify more complicated data manipulations
in dataframes.  Beyond what we are focusing on today, it is also useful
for manipulating data stored in an external database. This is especially useful
to know about if you will be working with very large datasets or relational 
databases in the future. 

If you are interested in learning more about `dplyr` after this lesson consider
following up with `dplyr` lessons from 

1) [NEON Data Skills] (http://neondataskills.org/R/GREPL-Filter-Piping-in-DPLYR-Using-R/) or 

2) [Data Carpentry] (http://www.datacarpentry.org/R-ecology/04-dplyr.html)

and reading the CRAN `dplyr` package [description] (https://cran.r-project.org/web/packages/dplyr/dplyr.pdf).

For the purposes of our data in the Harvard Forest we want to be able to split
our larger dataset into groups (e.g., by year), then manipulate each of the
smaller groups (e.g., take the average) before bringing them back together as a
whole (e.g., to plot the data). This is called using the "split-apply-combine"
technique, for which `dplyr` is particularly useful. 

### Grouping by a variable (or two)
Getting back to our basic question of understanding annual phenology patterns, we 
would like to look at the daily average temperature throughout the year.  We 
plotted the 15 minute data across the three years earlier in this lesson, 
however, now we'd like to look at the average temperature on a specific day for
all three years. To do this we can use the `group-by()` function. 

Before getting into our data, let's count up the number of observations per
Julian date using the `group-by()` function to determine how we split up our
data. Remember we created a column named "julian" for our Julian Day data.


    harMet15.09.11 %>%          #Within the harMet15.09.11 data
      group_by(julian) %>%      #group the data by the julian day
      tally()                   #and tally how many observations per julian day

    ## Error in eval(expr, envir, enclos): unknown column 'julian'

The `%>%` at the end of the lines are 'pipes', pipes are an important tool in
`dplyr` that allow us to skip creating and naming intermediate steps. Pipes,
like the name implies, allow us to take the output of one function and send
(pipe) it directly to the next function. We don't have to save the intermediate
steps between functions. The above code essentially says: go into the
harMet15.09.11 dataframe, find the julian dates, group them, and then count
(tally) how many values for each of the grouped days.  

Bonus: Older `dplyr` coded pipes as %.% and you may still see that format in some
older Stack Overflow answers and other tutuorials. 

As the output shows we have 288 values for each day.  Is that what we expect? 


    3*24*4  # 3 years * 24 hours/day * 4 15-min data points/hour

    ## [1] 288

Yep!  Looks good. 

Now that we've learned about pipes, let's use them to calculate what we are more
interested in, calculating daily average values by julian day. We can use the
`summarize()` function for this. `summarize()` will collapse each group (e.g.,
julian day) and output the group value for whatever function (e.g., mean) we 
specify. 

We can use the pipes to get the mean air temperature for each julian day. Since
we know we have a few missing values we can add `na.rm=TRUE` to force R to ignore
any NA values when making the calculations. 


    harMet15.09.11 %>%
      group_by(julian) %>%
      summarize(mean_airt = mean(airt, na.rm = TRUE))  

    ## Error in eval(expr, envir, enclos): unknown column 'julian'

Julian days repeat 1-365 (or 366) each year, therefore what we have here is that
the mean temperature for all 3 January 1st in 2009, 2010, and 2011 was -3.7C. 
Sometimes this is how we want to summarize our data, however, we may also want 
to summarize our air temperature data for each day in each of the three years.  
To do that we need to group our data by two different values at once, year and 
julian day. 

### Extracting year data from a date & time variable
Currently our date and time information is in one column (datetime) but to group
by year, we're first going to need to create a year-only variable. 

For this we'll again use the `lubridate` package:


    harMet15.09.11$year <- year(as.Date(harMet15.09.11$datetime, "%y-%b-%d",
          tz = "America/New_York"))

or since our date data is already a POSIX date/time variable, we can be more efficient: 


    harMet15.09.11$year <- year(harMet15.09.11$datetime)

Using `names()` we can see that we now have a variable named year. Here we are specifying the last column we added (32).

    #check to make sure it worked
    names(harMet15.09.11 [32])

    ## [1] "year"

Now we have our two variables: julian and year. To get the mean air temperature
for each day for each year we can use the `dplyr` pipes to group by year and
julian date.


    harMet15.09.11 %>%
      group_by(year, julian) %>%
      summarize(mean_airt = mean(airt, na.rm = TRUE))

    ## Error in eval(expr, envir, enclos): unknown column 'julian'

Given just the header in the output we can see the difference between the
3 year average temperature for 1 January (-3.7C) and the average temperature for
1 January 2009 (-15.1C).

###Combining functions to increase efficiency
To create more efficient code, could we create the year variable within our
`dplyr` function call? 

Yes, we can use the `mutate()` function of `dplyr` and include our `lubridate`
function within the `mutate()` call. `mutate()` is used to add new data to a
dataframe. These are often new data that are created from a calculation or
manipulation of existing data. If you are familiar with the `tranform()` core R
function the usage is similar:  `mutate()` allows us to create and immediately use a
variable (year2), which is something that the core `R` function `transform()`
will not do.


    harMet15.09.11 %>%
      mutate(year2 = year(datetime)) %>%
      group_by(year2, julian) %>%
      summarize(mean_airt = mean(airt, na.rm = TRUE))

    ## Error in eval(expr, envir, enclos): unknown column 'julian'

For illustration purposes, we named the new variable we were creating with
`mutate()` year2 so we could distinguish it from the year already created by
`year()`. Notice that after using this code, we don't see year2 as a column
in our harMet15.09.11 dataframe.  

    names(harMet15.09.11)

    ##  [1] "X"        "datetime" "jd"       "airt"     "f.airt"   "rh"      
    ##  [7] "f.rh"     "dewp"     "f.dewp"   "prec"     "f.prec"   "slrr"    
    ## [13] "f.slrr"   "parr"     "f.parr"   "netr"     "f.netr"   "bar"     
    ## [19] "f.bar"    "wspd"     "f.wspd"   "wres"     "f.wres"   "wdir"    
    ## [25] "f.wdir"   "wdev"     "f.wdev"   "gspd"     "f.gspd"   "s10t"    
    ## [31] "f.s10t"   "year"

We want to save this information so that we can plot the daily air temperature
as well. So, we save the output of our dplyr function as a new data frame. 

What additional variables do we want for our plots? 
In order to have nicely formatted x-axis we need to retain the datetime
information. But we only want one datetime entry per air temperature measurement.
We can get this by asking R to output the first datetime entry for each day for
each year using the `summarize()` function.


    temp.daily.09.11 <- harMet15.09.11 %>%
      mutate(year3 = year(datetime)) %>%
      group_by(year3, julian) %>%
      summarize(mean_airt = mean(airt, na.rm = TRUE), datetime = first(datetime))

    ## Error in eval(expr, envir, enclos): unknown column 'julian'

    str(temp.daily.09.11)

    ## Error in str(temp.daily.09.11): object 'temp.daily.09.11' not found

Now we have a dataframe with only values for Year, Julian Day, Mean Air Temp,
and a Date. 

##Challenge 3: Applying `dplyr` Skills
Calculate and save a dataframe of the average air temperate for each month in
each year.  For ease with future activities name your new dataframe
"temp.monthly.09.11"


    temp.monthly.09.11 <- harMet15.09.11 %>%
      mutate(month = month(datetime), year= year(datetime)) %>%
      group_by(month, year) %>%
      summarize(mean_airt = mean(airt, na.rm = TRUE), datetime = first(datetime))
    
    str(temp.monthly.09.11)

    ## Classes 'grouped_df', 'tbl_df', 'tbl' and 'data.frame':	1 obs. of  4 variables:
    ##  $ month    : num NA
    ##  $ year     : num NA
    ##  $ mean_airt: num 8.47
    ##  $ datetime : POSIXct, format: NA
    ##  - attr(*, "vars")=List of 1
    ##   ..$ : symbol month
    ##  - attr(*, "drop")= logi TRUE


## Plotting daily & monthly temperature data
Now that we have daily and monthly air temperature data (temp.daily.09.11 &
temp.monthly.09.11 dataframes), let's plot the average daily temperature.  

    AirTempDaily <- ggplot(temp.daily.09.11, aes(datetime, mean_airt)) +
               geom_point() +
               ggtitle("Average Daily Air Temperature At Harvard Forest") +
               theme(plot.title = element_text(lineheight=.8, face="bold",
                      size = 20)) +
               theme(text = element_text(size=20)) +
               xlab("Date") + ylab("Air Temperature (C)")+
               scale_x_datetime(labels=date_format ("%d%b%y"))

    ## Error in ggplot(temp.daily.09.11, aes(datetime, mean_airt)): object 'temp.daily.09.11' not found

    AirTempDaily 

    ## Error in eval(expr, envir, enclos): object 'AirTempDaily' not found

Notice in the code for the x scale (`scale_x_datetime()`), we changed the date format from %m to %b which gives the abreviated month.

Now that we've plotted daily temperature together, plot the monthly on your own
before looking at the next set of code. If you get stuck, suggested code is
below.  For ease of future code, name your plot "AirTempMonthly".


    AirTempMonthly <- ggplot(temp.monthly.09.11, aes(datetime, mean_airt)) +
               geom_point() +
               ggtitle("Average Monthly Air Temperature At Harvard Forest") +
               theme(plot.title = element_text(lineheight=.8, face="bold",
                    size = 20)) +
               theme(text = element_text(size=20)) +
               xlab("Date") + ylab("Air Temperature (C)") +
              scale_x_datetime(labels=date_format ("%b%y"))
    AirTempMonthly

    ## Error in seq.int(0, to0 - from, by): 'to' cannot be NA, NaN or infinite

Notice in the code for the x scale (`scale_x_datetime()`), we further changed
the date format by removing the day since we are graphing monthly averages. 

Lets compare the two air tempurature figures we have created. Unfortunately
`ggplot` doesn't recognize `par(mfrow=())` to show side by side figures.
Instead we can use another package, `gridExtra`, to do this. 


    require(gridExtra)
    grid.arrange(AirTemp15, AirTempDaily, AirTempMonthly, ncol=1)

    ## Error in arrangeGrob(...): object 'AirTempDaily' not found

On which plot is it easier to see annual patterns of air temperature?  Can you
think of when you might use one plot or another?  Remember if the plots are too
small you can use the zoom feature in R Studio to pop the out into a seperate
window.  

##Challenge 4: Combining `dplyr` and `ggplot` Skills
Plot the precipiation by month in all years available. 

Using the Harvard Meterorological 15-min datafile, plot the average monthly 
precipitation using all years (not just 09-11).  Does it make more sense to
calculate the total precipitation for each month or the average percipitation
for each month?


    prec.monthly <- harMet15 %>%
      mutate(month = month(datetime), year= year(datetime)) %>%
      group_by(month, year) %>%
      summarize(total_prec = sum(prec, na.rm = TRUE), datetime=first(datetime))

    ## Error in eval(expr, envir, enclos): object 'harMet15' not found

    str(prec.monthly)

    ## Error in str(prec.monthly): object 'prec.monthly' not found

    PrecipMonthly <- ggplot(prec.monthly,aes(month, total_prec)) +
      geom_point(na.rm=TRUE) +
      ggtitle("Monthly Precipitation in Harvard Forest (2005-2011") +
      theme(plot.title = element_text(lineheight=.8, face="bold",size = 20)) +
      theme(text = element_text(size=20)) +
      xlab("Month") + ylab("Precipitation (mm)") +
      scale_x_discrete(labels=month)  

    ## Error in ggplot(prec.monthly, aes(month, total_prec)): object 'prec.monthly' not found

    #month is no longer datetime, but a discrete number. Change from scale_x_datetime()
      #to scale_x_discrete()
    
    PrecipMonthly

    ## Error in eval(expr, envir, enclos): object 'PrecipMonthly' not found

    #If we want written out month labels
    PrecipMonthly + scale_x_discrete("month", labels = c("1" = "Jan","2" = "Feb",
      "3" = "Mar","4" = "Apr","5" = "May","6" = "Jun","7" = "Jul","8" = "Aug","9" = "Sep","10" = "Oct","11" = "Nov","12" = "Dec") )

    ## Error in eval(expr, envir, enclos): object 'PrecipMonthly' not found

Is there an obvious annual trend in percipitation at Harvard Forest?  

Compare how you plotted daily (Challenge 2: DailyPrecip) and monthly
precipitation (Challenge 4: PrecipMontly). When would one format be better than 
another? 


    grid.arrange(PrecipDaily, PrecipMonthly, ncol=1)

    ## Error in arrangeGrob(...): object 'PrecipMonthly' not found

In the next lesson we will learn to expand our plotting abilities to plot two
variables side-by-side and to incorporate values from spacial data sets (NDVI)
into our phenology related plots. 
