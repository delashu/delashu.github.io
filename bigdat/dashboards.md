---
layout: default
title: Dashboards
parent: BIOS823
nav_order: 4
---

# Dashboards in Python  

The National Science Foundation collects and houses various information on doctoral degree recipients. This data includes demographic variables, education history, and post-graduate plans. The repository of the data can be found [here](https://ncses.nsf.gov/pubs/nsf19301/data).  

The aim of this project is to extract, aggregate, visualize, and analyze this data in a dashboard. Python has various extensions and libraries that facilitate the creation of dashboards. This project uses [streamlit](https://streamlit.io/).  

**Note for developers**:  
The following dashboard should be run on your own machine using an IDE such as [VSCode](https://code.visualstudio.com/). I highly recommend and encourage the use of a [virtual environment](https://code.visualstudio.com/docs/python/environments) to set-up and build your dashboard.  

### Dashboard Features Part I   
The NSF has collected the number of doctoral degrees awarded from 1987 - 2017. The dashboard visualizes this longitudinal trend with a line plot.  
The user can interact with the first click-down menu item called, "Specific Area of Study" to select a specific major of interest. 

This portion of the dashboard has three main parts:  
1. Plot and corresponding regression line  
2. Raw data 
3. Summary Statistics  
  
The plot includes the number of doctoral recipients and the best fit OLS regression line.  
The table on the right provides the raw data used to create the plot.  
The table at the bottom outputs summary statistics of the plot such as the mean, median, max, min, range, OLS regression intercept, and OLS regression slope.   

The dashboard provides a blurb on the data and its capabilities:  
```
Select a Specific Area of study to analyze using the first click-down menu on the left. 
Longitudinal plots, tables, and summaries of number of doctorates awarded from 1987 - 2017 are shown.
 Blue points and lines on the longitudinal plot are the true number of doctorates awarded from 1987 to 2017. 
 The red line is the OLS best fit line.
```


The first gif shows the user selecting, "Mathematics and Statistics".  
Once selected, all elements of the dashboard change accordingly. The default is set to, "All fields".      
![Example Dash one](dash_one.gif)   


The elements of the dashboard can be expanded. The gif below shows the user getting a better look at the line plot and raw data.   

![Example Dash two](dash_two.gif)  


### Dashboard Features Part II   
The second portion of the dashboard provides visualizations and analytics of post-graduate plans and landing locations of PhD graduates in 2017.  
Two barplots and one table are shown. The left barplot visualizes the post-graduate plans for PhD graduates. Each bar represents the number of graduates who plan to continue study ("PostGrad Study"), have found a job ("Employment"), are looking for the next opportunity ("Seeking"), and "Other".  
The right barplot shows the landing locations for PhD graduates for those who plan to remain in the US. Note that this barplot represents a percentage of the PhD graduates who will reside in "New England", "Middle Atlantic", "East North Central", "West North Central", "South Atlantic", "East South Central", "West South Central", "Mountain", or "Pacific/Insular" regions.  

The bottom table shows the percentage breakdown of Primary Activity of Employment among those graduates who chose found employment.  
Primary Activity can be categorized as "R&D", "Teaching", "Management or Administration", "Professional services", or "Other". 


The dashboard provides a blurb on the post-grad plan data and its capabilities:  
```
Select a General Area of study to analyze using the second click-down menu on the left.
 Two barplots are rendered based on your input that shows post-graduate plans and 
 landing locations for the year of 2017.
```

The user can select a "General Area of Study" using the second drop-down menu to visualize post-grad plans by a general area of study.  
The gif below shows the user selecting, "Mathematics and computer science".  
Once selected, all elements of the dashboard change accordingly.   

![Example Dash three](dash_three.gif)  

The elements of the dashboard are all interactive. The gif below shows the user scrolling through the bars and expanding the table.   
![Example Dash three](dash_four.gif)  