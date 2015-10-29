---
layout: post
title: "Lesson 00 Intro to Tabular Time Series "
date:   2015-10-20
authors: "Marisa Guarinello, Megan Jones, Courtney Soderberg"
dateCreated:  2015-10-22
lastModified: 2015-10-29
tags: [module-1]
description: "This learning module explains the fundamental principles, 
functions, and metadata that you need to work with tabular (.csv) temporal data
in R."
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network
  (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: 
---
<section id="table-of-contents" class="toc">
  <header>
    <h3>Contents</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

##About
This learning module will walk you through the fundamental principles of working 
with .csv temporal data in R including how to open, clean, explore, and plot 
time series data. 

**R Skill Level:** Intermediate - you've got the basics of `R` down.

<div id="objectives">

<h3>Goals / Objectives</h3>
After completing this learning module, you will know how to:
<ol>
<li>Open a csv file in R and why we are using this format.</li>
<li>Examine data structures and types.</li>
<li>Prepare data for analysis including cleaning, converting/transfroming, and
calculating basic summary statistics for your data.</li>
<li>Create a basic time-series plot. </li>
<li>Exploring basic trends in your data. </li>
<li>Map NDVI data with timeseries data. </li>
</ol>

<h3>Things You'll Need To Complete This Lesson</h3>

<h3>R Libraries to Install:</h3>
<ul>
<li><strong>lubridate:</strong> <code> install.packages("lubridate")</code></li>
<li><strong>ggplot:</strong> <code> install.packages("ggplot2")</code></li>
<li><strong>scales:</strong> <code> install.packages("scales")</code></li>

</ul>
<h4>Tools To Install</h4>

Please be sure you have the most current version of R and, preferably,
RStudio to write your code.

<h4>Data to Download</h4>

Download the Spatio-temporal data folder:
<ul>
<li><a href="http://XXX" class="btn btn-success"> XXX</a></li>
<li><a href="{{ site.baseurl }}/XXX" class="btn btn-success"> XXX</a></li>
</ul>
<p>If you are working through the workshop lessons in sequence you will not need
to download the following files. If you are doing this module on its own, please
download the XXX folder with needed data files including an NDVI file that was
created in XXX module. </p>

<p>The data used to create the .csv files in this dataset collected at Harvard 
Forest.  The entire dataset can be accessed from their website (http://harvardforest.fas.harvard.edu).</p>  

<h4>Recommended Pre-Module Reading</h4>
<p> None </p>

</div>


# Additional inforamation if working through module as an individual, not as
# part of a workshop. 
This .csv time-series learning module was build as part of the two day Spatio-
Temporal Data Workshop [LINK]XXX. Therefore some background information and 
basic coding skills have been covered extensively in previous modules and are
presented here in a more limited fashion. You should be able to complete this 
module as long as you are comfortable with basic 'R' skills and have a general
understanding of spatial data.   

This workshop is built around Jessica, an ecology graduate student interested in
exploring some of her field sites to understand how phenology of vegetation 
(greening in the spring summer and senescence in the winter, varies across 
multiple sites and through time). Her potential study sites have very different 
vegetation communities including Harvard Forest (Massachusetts) and San Joaquin Experimental Range (California). The data includes spatial and temporal raster 
data ([Link] Module XXX), vector data ([Link] Module XXX), and various 
atmospheric parameters that are collected over several years (this module) that 
she would like to be able explore prior to her initial site visits and writing 
her dissertation proposal. 

# Time series data
Time series data can come in many different forms.  At the fundemental level a 
time series is a series of data points collected at successive time intervals. 
Lots of data collected can be time series data: heights of ocean tides, the 
number of toes that different equine species had over the last 60 mya, or daily 
temperature measures.  This module will specifically look at atmospheric data 
(temperature, precipitation, and Photosynthetically Active Radiation (PAR) ) and
normalized difference vegetative index (NDVI). 
 
# PAR
PAR is a measure how much light within the solar radiation spectral range 
(wave band) from 400 to 700 nanometers occurs at the time when the data is 
collected.  This wavelength is important for phenology studies as that spectral
range is what photosynthetic organisms are able to use in the process of 
photosynthesis.

# NDVI
NDVI is an index from remote sensing imaging that measures the live vegetation 
in the target area. 
 

You are now ready for Lesson 1 in the Tabular Time Series Module
