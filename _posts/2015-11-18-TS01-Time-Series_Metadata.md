---
layout: post
title: "Lesson 01: Load and Understand Time Series Data"
date:   2015-10-24
authors: [Megan A. Jones, Marisa Guarinello, Courtney Soderberg]
contributors: [Leah A. Wasser]
dateCreated:  2015-10-22
lastModified: 2015-11-19
category: 
tags: [module-1]
description: "This lesson will teach students how to import tabular data from a 
CSV file into R. Students will examine the structure of the dataset and get 
information from a related metadata text file."
code1: 
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: /R/Load-Understand-Data-in-R
comments: false
---

{% include _toc.html %}


##About
This lesson will teach students how to import tabular data from a CSV file into 
R. Students will examine the structure of the dataset and get information from a 
related metadata text file.

**R Skill Level:** Intermediate - you've got the basics of `R` down and 
understand the general structure of tabular data.

<div id="objectives" markdown="1">

###Goals / Objectives
After completing this lesson, you will know how to:

* Open a .csv file in R and understand why we are using that format
* Examine data structures and types
* Use metadata to get more information about input data

###Things You'll Need To Complete This Lesson
Please be sure you have the most current version of R and preferably
R studio to write your code.

####R Libraries to Install
No additional libraries are needed for this lesson

####Recommended Pre-Lesson Reading


####Data to Download
Make sure you have downloaded the AtmosData folder from
http://figshare.com/articles/NEON_Spatio_Temporal_Teaching_Dataset/1580068

The data used in this tutorial were collected at Harvard Forest which is
a the National Ecological Observatory Network field site <a href="http://www.neoninc.org/science-design/field-sites/harvard-forest" target="_blank">
More about the NEON Harvard Forest field site</a>. These data are proxy data for what will be available for 30 years from the NEON flux tower [from the NEON data portal](http://data.neoninc.org/ "NEON data").

####Setting the Working Directory
The code in this lesson assumes that you have set your working directory to the
location of the unzipped file of data downloaded above.  If you would like a 
refresher on setting the working directory, please view the <a href="XXX" target="_blank">Setting A Working Directory In R</a> lesson prior to beginning the lesson. 

</div>

#Understand Your Data 
There are many different contexts for time series data in ecology.  We will be 
exploring the relationships between air temperature, precipiation,  
photosyntheically active radiation (PAR) and plant phenology,especially the 
greening up and browning down temporal patterns 

When initiating a new script and workflow it is always a good idea to 

1) load all necessary packages,
2) set the working directory,
3) load the data and
4) figure out how `R` should interpret that data and how `R` actually interprets
it.  

Let's go through these steps with our data. Load any packages prior to importing
and working with the data. If we need another package later, we will return to
this section to add it so that at a glance we can see all the packages in the
script.  Including a short note in the code allows us to denote each library's 
purpose in the script. This will help later when we write new scripts for other 
projects.

Once the libraries are loaded we need to set the working directory to the
location where our data is stored. 

We are now ready to import the data.  This data is a .csv or comma-seperated
value file.  This file format is an excellent way to store tabular data in a
plain text format that can be read by many different programs including R.


    # Load packages required for entire script
    # library(nameOfLibrary)  #purpose of library
    
    #set working directory to ensure R can find the file we wish to import
    #setwd("working-dir-path-here")
    
    # Load csv file of 15 min meterological data from Harvard Forest
    #Factors=FALSE so strings, a series of letters/ words/ numerals, remain characters
    harMet_15min <- read.csv(file="AtmosData/HARV/hf001-10-15min-m.csv",
          stringsAsFactors = FALSE)

# Data Structure & Metadata
Now it is time to look at our data, metadata and information about how `R` 
*interprets* the data.
There are three options for how we can look at our data, in the console using 
head(), by opening the object using View() or clicking on the object in the 
Environment pane of RStudio. Once we open the data, we will look at the 
structure of the data as interpreted by R.


    #to see first few lines of data file
    head(harMet_15Min)

    ##              datetime jd airt f.airt rh f.rh dewp f.dewp prec f.prec slrr
    ## 1 2005-01-01 00:15:00  1  5.1        84       2.5           0           0
    ## 2 2005-01-01 00:30:00  1  5.0        84       2.5           0           0
    ## 3 2005-01-01 00:45:00  1  4.9        85       2.6           0           0
    ## 4 2005-01-01 01:00:00  1  4.7        86       2.6           0           0
    ## 5 2005-01-01 01:15:00  1  4.5        87       2.6           0           0
    ## 6 2005-01-01 01:30:00  1  4.6        87       2.7           0           0
    ##   f.slrr parr f.parr netr f.netr  bar f.bar wspd f.wspd wres f.wres wdir
    ## 1           0         -58        1017        2.6         2.4         205
    ## 2           0         -59        1017        2.3         2.1         213
    ## 3           0         -59        1017        2.1         1.8         217
    ## 4           0         -58        1017        1.8         1.6         226
    ## 5           0         -58        1017        1.4         1.2         224
    ## 6           0         -58        1017        1.6         1.4         214
    ##   f.wdir wdev f.wdev gspd f.gspd s10t f.s10t julian
    ## 1          26         7.2         0.7             1
    ## 2          25         5.9         0.7             1
    ## 3          27         5.8         0.7             1
    ## 4          26         5.1         0.7             1
    ## 5          29         4.6         0.7             1
    ## 6          30         4.4         0.7             1

    #to see it in spreadsheet form and scroll
    #View(harMet15)
    
    #Check how R is interpreting the data. What is the structure (str) of the data? 
    #Is it what we expect to see?  
    str(harMet_15Min)

    ## 'data.frame':	376800 obs. of  31 variables:
    ##  $ datetime: POSIXct, format: "2005-01-01 00:15:00" "2005-01-01 00:30:00" ...
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
    ##  $ julian  : num  1 1 1 1 1 1 1 1 1 1 ...

These commands let us see the data and get some information about it.

Notice that data types that are not numbers or integers are classified as 
characters(chr), we specificed for `R` to recognize these as characters instead 
as factors using the`stringsAsFactors=FALSE` command. 


##Challenge: Pertinent Meta Data
There is information other than how R interprets our data that we need to know.
Can you think of any examples? 


#Open Associated Metadata Files
To get some of this information, or clues to this information, we will open the
metadata text file associated with this 15-min meterology data that is in the 
AtmosData folder. Navigate to this file and open it in a text editor.

![Harvard Forest 15 min Atmospheric Data Screenshot] (/Users/mjones01/Documents/Git_Repositories/NEON-R-Tabular-Time-Series/images)

Remember we are currently interested in plant phenology so we'll be looking at
air temperature, precipitation and PAR. Look for these variables and identify
what information is contained for each in this metadata. 

As we find pertinent information about our variables we will add it to the 
script so that when revisiting the script in the future we don't have to look at
the metadata aain.  These notes are also helpful if sharing the script with 
others, so we make notes that are intuitive and intelligible not only to 
ourselves but also to colleagues and collaborators.


    #Metadata Notes from hf001_10-15-m_Metadata.txt
    # column names for variables we are going to use: datetime, airt, prec, parr 
    # units for quantitative variables: celsius, millimeters, molePerMeterSquared
    # airt and parr are averages of measurements taken every 1 sec; precip is total of each 15 min period 
    #for quantitative variables missing values are given as NA

# Metadata Sleuthing
Questions: Is there information provided here that doesn't match what you see in
the data? Is there information you want to know that is not provided in this 
metadata file? Do you have ideas how you might find that information?



We notice that no information is given for `datetime` besides that it is a date 
and time. It would be nice to know if it was recorded in UTC/GMT or local time.
We also noticed that there is a link to the source website at the top of the 
text file. Let's check it out to see if we can get the answer to our question.


The website gives more information that we have in our metadata file. And it 
gives us important information about datetime -- all values are in Eastern 
Standard Time.

Again, it is a good practice to make relevant notes from the metadata in your R 
script so that you can easily refer to it as you actually work with the data.


    # website for more information: http://harvardforest.fas.harvard.edu:8080/exist/apps/datasets/showData.html?id=hf001
    # date-times are in Eastern Standard Time
    # preview tab give plots of all variables

#Challenge: Other Metadata
Do you see anything else on the website that might be related to metadata? 





Answer: EML file. This is the official standards-based metadata file using the 
Ecological Metadata Language (EML). Download this to your data folder. EML seems
like an odd choice but seeing this is an LTER site it makes sense because LTER
uses EML extensively in their projects.


    # EML file in the data folder now - all metadata information in there.

Now we have important information that will help us as we continue to use these
data for time series analysis.
