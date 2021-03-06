---
layout: post
title: "Lesson 02: Convert Date-Time Fields from Characters to a Date-Time Class (POSIX) in R"
date: 2015-10-23
authors: [Megan A. Jones, Marisa Guarinello, Courtney Soderberg]
contributors: [Leah A. Wasser]
dateCreated: 2015-10-22
lastModified: 2015-11-19
packagesLibraries: [lubridate]
tags: module-1
description: "This lesson will teach individuals how to prepare tabular data for
further analysis in R, addressing missing values and date-time formats. Students
will also learn how to convert characters to a date-time class and to convert
date-time to Julian day."
code1:
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: /R/Convert-to-Date-Time-Class-POSIX-R
comments: false
---

{% include _toc.html %}

##About
This lesson will teach students how to prepare tabular data for further analysis
in R by addressing missing values and date-time formats. Students will also
learn how to convert characters to a date-time class and to convert date-time to
Julian day.

**R Skill Level:** Intermediate - you've got the basics of `R` down and 
understand the general structure of tabular data.

<div id="objectives" markdown="1">

### Goals / Objectives
After completing this activity, you will:
 * Know how to clean data
 * Be able to convert between date & time formats
 * Examine data structures and types

###Things You'll Need To Complete This Lesson
Please be sure you have the most current version of `R` and preferably
R studio to write your code.

####R Libraries to Install
<li><strong>lubridate:</strong> <code> install.packages("lubridate")</code></li>

####Data to Download

<a href="http://files.figshare.com/2437700/AtmosData.zip" class="btn btn-success">
Download Atmospheric Data</a>

The data used in this tutorial were collected at Harvard Forest which is
a the National Ecological Observatory Network field site <a href="http://www.neoninc.org/science-design/field-sites/harvard-forest" target="_blank">
More about the NEON Harvard Forest field site</a>. These data are proxy data for what will be
available for 30 years from the NEON flux tower [from the NEON data portal](http://data.neoninc.org/ "NEON data").

####Setting the Working Directory
The code in this lesson assumes that you have set your working directory to the
location of the unzipped file of data downloaded above.  If you would like a
refresher on setting the working directory, please view the <a href="XXX" target="_blank">Setting A Working Directory In R</a> lesson prior to beginning the lesson.

</div>

In [{{site.baseurl}} / LINKHERE](Lesson 00 - NEED TO ADD LINK) we imported a
`.csv` time series dataset. We learned how to cleanly plot these data by 
converting the date field to an `R` `Date` data type. In this lesson we will 
explore how to work with data field that contains both a date AND a time stamp.

In this lesson, we will use functions from both base `R` and the `lubridate` 
library learn how to work with date-time data types.

We will use the `hf001-10-15min-m.csv` file that contains micrometereology
data including our variables of interest: temperature, precipitation and PAR, 
for Harvard Forest, aggregated at 15 minute intervals.

We will begin by loading the data and viewing its structure in `R`.


    # Load packages required for entire script
    library(lubridate)  #work with dates
    
    #set working directory to ensure R can find the file we wish to import
    #setwd("working-dir-path-here")
    
    #Load csv file of 15 min meterological data from Harvard Forest
    #Factors=FALSE so strings, series of letters/ words/ numerals, remain characters
    harMet_15Min <- read.csv(file="AtmosData/HARV/hf001-10-15min-m.csv",
                         stringsAsFactors = FALSE)
    
    #what type of R object is our imported data?
    class(harMet_15Min)

    ## [1] "data.frame"

    #what data classes are within our R object? 
    #What is our date-time field called?
    str(harMet_15Min)

    ## 'data.frame':	376800 obs. of  30 variables:
    ##  $ datetime: chr  "2005-01-01T00:15" "2005-01-01T00:30" "2005-01-01T00:45" "2005-01-01T01:00" ...
    ##  $ jd      : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ airt    : num  5.1 5 4.9 4.7 4.5 4.6 4.6 4.7 4.6 4.6 ...
    ##  $ f.airt  : chr  "" "" "" "" ...
    ##  $ rh      : int  84 84 85 86 87 87 87 88 88 88 ...
    ##  $ f.rh    : chr  "" "" "" "" ...
    ##  $ dewp    : num  2.5 2.5 2.6 2.6 2.6 2.7 2.7 2.8 2.8 2.8 ...
    ##  $ f.dewp  : chr  "" "" "" "" ...
    ##  $ prec    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ f.prec  : chr  "" "" "" "" ...
    ##  $ slrr    : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ f.slrr  : chr  "" "" "" "" ...
    ##  $ parr    : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ f.parr  : chr  "" "" "" "" ...
    ##  $ netr    : int  -58 -59 -59 -58 -58 -58 -57 -57 -59 -62 ...
    ##  $ f.netr  : chr  "" "" "" "" ...
    ##  $ bar     : int  1017 1017 1017 1017 1017 1017 1017 1017 1017 1016 ...
    ##  $ f.bar   : chr  "" "" "" "" ...
    ##  $ wspd    : num  2.6 2.3 2.1 1.8 1.4 1.6 1.5 1.5 1.6 1.7 ...
    ##  $ f.wspd  : chr  "" "" "" "" ...
    ##  $ wres    : num  2.4 2.1 1.8 1.6 1.2 1.4 1.3 1.4 1.4 1.6 ...
    ##  $ f.wres  : chr  "" "" "" "" ...
    ##  $ wdir    : int  205 213 217 226 224 214 214 213 217 214 ...
    ##  $ f.wdir  : chr  "" "" "" "" ...
    ##  $ wdev    : int  26 25 27 26 29 30 30 27 27 25 ...
    ##  $ f.wdev  : chr  "" "" "" "" ...
    ##  $ gspd    : num  7.2 5.9 5.8 5.1 4.6 4.4 5 4.2 4.2 4.6 ...
    ##  $ f.gspd  : chr  "" "" "" "" ...
    ##  $ s10t    : num  0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 ...
    ##  $ f.s10t  : chr  "" "" "" "" ...

##Dealing with date and time

Have a look at the `datetime` field in the `harmet_15Min object`. What data type
is it in? 


    #view field data class
    class(harMet_15Min$datetime)

    ## [1] "character"

    #view sample data
    head(harMet_15Min$datetime)

    ## [1] "2005-01-01T00:15" "2005-01-01T00:30" "2005-01-01T00:45"
    ## [4] "2005-01-01T01:00" "2005-01-01T01:15" "2005-01-01T01:30"

Our data are in a character data type. We need to convert them to date-time data
type. What happens when we use the `as.Date` method on our `datetime` field?


    #convert field to date format
    har_dateOnly <- as.Date(harMet_15Min$datetime)
    
    #view data
    head(har_dateOnly)

The `as.Date` method worked great for a field with just dates, but it doesn't 
store any time information. Let's take a minute to explore the various date/time
formats in `R`.

## Exploring Date and Time Classes

###R - Date Class - as.Date
We can use the`as.Date` method to convert a date field in string/character 
format to an `R` `Date` class. It will ignore all values after the date string.


    #Convert character data to date (no time) 
    myDate <- as.Date("2015-10-19 10:15")   
    str(myDate)

    ##  Date[1:1], format: "2015-10-19"

    head(myDate)

    ## [1] "2015-10-19"

    #whhat happens if the date has text at the end?
    myDate2 <- as.Date("2015-10-19Hello")   
    str(myDate2)

    ##  Date[1:1], format: "2015-10-19"

    head(myDate2)

    ## [1] "2015-10-19"

###R - Date-Time - The POSIX classes
Given that we do have data and time that are both important for our time series 
we need to use a format that includes time and date.  Base `R` offers two 
closely related classes for date and time: `POSIXct` and `POSIXlt`. Let's 
explore both of these before choosing which one to use with our data.


    #Convert character data to date and time.
    timeDate <- as.POSIXct("2015-10-19 10:15")   
    str(timeDate)

    ##  POSIXct[1:1], format: "2015-10-19 10:15:00"

    head(timeDate)

    ## [1] "2015-10-19 10:15:00 MDT"

The `as.POSIXct` method converts a date-time string into a `POSIXct` or 
date-time class. 

If we look at the data, we see the date and time with a time zone designation. 


    #view date - notice the time zone - MDT (mountain daylight time)
    timeDate

    ## [1] "2015-10-19 10:15:00 MDT"

POSIXct stores date and time as the number of seconds since 1 January 1970
(negative numbers are used for dates before 1970). As each date and time is 
just a single number this format is easy to use in dataframes and to convert
from one format to another. 


    unclass(timeDate)

    ## [1] 1445271300
    ## attr(,"tzone")
    ## [1] ""

Here we see the number of seconds from 1 January 1970 to 9 October 2015 and
see that it has a time zone attribute stored with it. 

While easy for data storage and manipulations, humans aren't so good at working
with seconds from 1970 to figure out a date. Plus, we often want to pull out
some portion of the data (e.g., months).  POSIXlt stores date and time
information in a format that we are used to seeing (e.g., 
second, min, hour, day of month, month, year, numeric day of year, etc). 


    #Convert character data to POSIXlt date and time
    timeDatelt<- as.POSIXlt("2015-10-1910:15")  
    str(timeDatelt)

    ##  POSIXlt[1:1], format: "2015-10-19 10:15:00"

    head(timeDatelt)

    ## [1] "2015-10-19 10:15:00 MDT"

    unclass(timeDatelt)

    ## $sec
    ## [1] 0
    ## 
    ## $min
    ## [1] 15
    ## 
    ## $hour
    ## [1] 10
    ## 
    ## $mday
    ## [1] 19
    ## 
    ## $mon
    ## [1] 9
    ## 
    ## $year
    ## [1] 115
    ## 
    ## $wday
    ## [1] 1
    ## 
    ## $yday
    ## [1] 291
    ## 
    ## $isdst
    ## [1] 1
    ## 
    ## $zone
    ## [1] "MDT"
    ## 
    ## $gmtoff
    ## [1] NA

When we converted the string to `POSIXlt`, it still appears to be the same as 
`POSIXct`. However, `unclass()` shows us that the data are stored differently. 
The `POSIXlt` format stores the hour, minute, second, and month day year as 
separate components.

Yet, just like `POSIXct` being based on seconds since 1 Jan. 1970, `POSIXlt` has
a few quirks.  First, the months use zero based indexing which starts counting
with zero instead of 1 so October is stored as the 9th month (`$mon = 9`) and 
the years are base on years since 1900 so 2015 is stored as 115 (`$year = 115`)
Information like this can always be found in the `R` documentation by using `?`
or specifically `?POSIXlt`.  

Having dates classified as seperate components makes `POSIXlt` a bit more
difficult to use in data.frames and to manipulate, which is why we won't use
POSIXlt for this lesson.  

Instead we can specified which format (year-month-day hour:minute) we wanted to 
see the date and time when using POSIXct.  This way we know what the date is and
the program can still work with the date/time in a single format.

###R- Data-Time - Other Classes
There are a many other options in `R` for working with date-time formats, 
including packages `readr` and `zoo`. The `chron` package allows the use of time
without a data attached.  

##Convert data.frame Field to a Date-time Class

### Date-Time Formats
We can tell R how our data are formated (or, in other contexts, how we want to
view date and time) using a `format=` string.  However, we have to know what
format our dates are currently in. 


    #view one date time entry
    harMet_15Min$datetime[1]

    ## [1] "2005-01-01T00:15"

Our data is in Year-Month-Day "T" Hour:Minute:Second, so we can use the 
following designations for the parts of the date-time field:

* %Y - year
* %m - month
* %d - day
* %H:%M - hours:minutes

A list of options for the date-time formatting are given in the help for the
`strptime` function (`help(strptime)`).   The "T" inserted into the middle of 
datetime isn't a standard character for date and time, however we can tell `R`
where the character is and it will still be able to figure out the date and time
around it.  


    #convert single instance of date/time in format year-month-day hour:min:sec
    as.POSIXct(harMet_15Min$datetime[1],format="%Y-%m-%dT%H:%M")

    ## [1] "2005-01-01 00:15:00 MST"

    ##The format of date-time MUST match the specified format or the field will not
    # convert
    as.POSIXct(harMet_15Min$datetime[1],format="%m-%d-%YT%H:%M")

    ## [1] NA

Using the syntax we've learned, we can reformat the entire datetime format into 
an `R POSIX` data type.

##Check the Time Zone
Whenever using a time field it is essential to know what time zone the
data were collected in and what time zone they are stored with `R` in.  Some 
data will be collected and stored in thier local time zone but storage in UTC or
GMT is also a common default. 


    #checking time zone of data
    tz(harMet_15Min)

    ## [1] "UTC"

`R` currently interprets that the time data are record in Coordinated Universal 
Time (UTC) . Now that we've found this out we need to make sure it corresponds 
with what we know is true for the data.  In the <a href="XXX" target="_blank"> Understand The Data</a>
lesson we viewed the metadata associated with this data and found out that the 
data were collected in eastern time.  We need to designate the correct time zone
when convert the field to POSIX. There are individual designations for different
time zones and time zone variants (e.g., is daylight savings time observed). All
of the codes can be found <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank"> here</a>. 

The code for the eastern time zone the is the closest match to the Harvard
Forest is `America/New_York`. To try this out let's, convert the first entry to 
POSIX specifying the format and the correct time zone. 


    #assign time zone to just the first entry
    as.POSIXct(harMet_15Min$datetime[1],
                format = "%Y-%m-%dT%H:%M",
                tz = "America/New_York")

    ## [1] "2005-01-01 00:15:00 EST"

## Convert to Date-time Format
Now, using the syntax that we learned above, we can reformat the entire datetime
field in the data.frame to an `POSIXct` class.


    #convert to date-time class
    harMet_15Min$datetime <- as.POSIXct(harMet_15Min$datetime,
                                    format = "%Y-%m-%dT%H:%M",
                                    tz = "America/New_York")
    
    #view format of the newly defined datetime data.frame column 
    str(harMet_15Min$datetime)

    ##  POSIXct[1:376800], format: "2005-01-01 00:15:00" "2005-01-01 00:30:00" ...

Now that our datetime field is properly identified as a `POSIXct` date-time
class we can continue on and look at the patterns of precipitation,
temperature, and PAR through time. 
