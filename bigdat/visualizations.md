---
layout: default
title: Visualizations
parent: BIOS823
nav_order: 3
---


# Visualizing Malaria 

Malaria is a public and global health problem that has placed heavy burden on individual health, community wellbeing, and economic growth. Malaria's effects are ever-present, as an approximate [409,000 individuals died of malaria in 2019.](https://www.cdc.gov/malaria/malaria_worldwide/impact.html)  

We use open source datasets from the [MalariaAtlas package](https://github.com/rfordatascience/tidytuesday/tree/master/data/2018/2018-11-13) to visualize Malaria's impact on the globe. Three visualizations are constructed. This blog-post will include the images of each of the visualizations and walk the reader through ideas and inference drawn from the graphs. Note that two of the visualizations are interactive. To maximize impact of the visualizations and allow for interactivity, click here.   
I use the [plotly](https://plotly.com) library to make the two of the visualization and interactive plots. I chose plotly for its ease of use in python, flexibility, dynamic features, interactive capabilities, and mapping abilities.   
The final plot is made with [seaborn](https://seaborn.pydata.org/). I selected seaborn since it allows for faceting and coloring of longitudinal line plots with relative ease.  
All computing is done in Python 3.7.8.  

## Mapping Malaria  
I first visualized longitudinal Malaria death rates (Maralia Deaths per 100,000 people) across the globe. The heat map shows which regions of the world are most affected by Malarial deaths by year. Below I show a static version of the plot.   

![Map Plot One](deaths_map.PNG)

The interactive/dynamic version of the plot can be found [here.](https://nbviewer.jupyter.org/github/delashu/pysolve_notebooks/blob/d49054890cfb5e1c40b134accdc204dc54edc779/visualizations/viz.ipynb) Use the play button to watch the map change with year through 1990 - 2015.  
The plot shows a substantial concentration of high malarial death rates in Africa, particularly in West Africa. 

This plot motivated a more specific look of Malaria in Africa. I subsetted the data to African countries to obtain a more granular view of the problem.  


## Pinpointing Malaria in Africa  
African countries were isolated from the malarial deaths dataset. I created a barplot where each bar represents each country's death rates. The plot is ordered from least deaths per 100k on the very left to most deaths per 100k on the very right.  Below is a static version of the plot. The interactive/dynamic version of the plot can be found [here.](https://nbviewer.jupyter.org/github/delashu/pysolve_notebooks/blob/d49054890cfb5e1c40b134accdc204dc54edc779/visualizations/viz.ipynb)  


![Deaths Plot One](deaths_by_country_one.PNG)

When the play button is clicked, as the years progress, the bars shift to represent the countries with the lowest and highest malarial death rates per 100k from 1990 - 2016. During the early 90's Uganda, Sierrra Leone, Burundi, and Burkina Faso were affected the worst by malarial death rates. Burkina Faso and Sierra Leone continued to struggle through the 2000's and into 2015. Overall malarial death rates decreased from 1990 - 2015 as seen by the drop in bar heights.  

Now that the visualizations show which countries in Africa are most affected, I was curious which groupings of individuals were most affected by malarial deaths.  
The death rate dataset stratified by age was one source that would help with this investigation.  


## Who is Most Affected?  
I graphed longitudinal death rates in Africa faceted by age groupings. The age groupings provided in the dataset are the following: Under 5, 5-14, 15-49, 50-69, 70 or older. While graphing, I noticed three countries that stood out from the rest of the African countries: *Uganda, Nigeria, and the Democratic Republic of Congo*. These countries are colored separately. 

 
![Age Plot One](age_plt_one.png)


During the first pass of this graph, I used the " 'sharey' : False" parameter in seaborn. This gave all plots different y axes. In these plots, it was clear that  Uganda, Nigeria, and the Democratic Republic of Congo were affected most in nearly all age groupings. I then tweaked this parameter to be " 'sharey' : True", meaning all the plots shared the same y axis maximum and minimum.  


![Age Plot Two](age_plt_two.png)



This change yields a different story to the data. While we know that Uganda, Nigeria, and the Democratic Republic of Congo were affected most, by making the y axis shared, it is clear that the Under 5 Age Grouping is affected most by Malarial Deaths across all countries in Africa. 