---
layout: post
title:      "Tips on Visualization"
date:       2021-01-13 04:05:55 -0500
permalink:  tips_on_visualization
---


In every project that I have done, one of the biggest challenges I found was visualization. Finding the trend and information from the data is fun; however, it needs to be converted to visualization to communicate with the audience, especially the non-technical audience. Unless the visualization is successful, all your findings will be just pies in the sky.

The key focus of the visualization is to help the audience’s understanding of the topic. 

> > A picture is worth a thousand words.

Then how should we express our learning to the audience through visualization?

Here are a few tips on visualization using Pyplot.

We will start with importing necessary libraries.
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

## Tip 1. Figure Size

Determining your figure size is like choosing the right size of the canvas that you would like draw on. You need to consider and understand the ratio between variables. For instance when you have 20 variables in x-axis and 10 outputs in y-axis, your ratio of the figure should be in 2:1. 


```
plt.figure(figsize=[20,10], dpi=200) 
#dpi (dots per inch) is the resolution of the figure. 
#Normally 200~300 will be enough for visual presentation
```


![](https://i.pinimg.com/564x/38/78/6a/38786ae8ccf8f3be063ed4fecc4e26b9.jpg)

## Tip 2. Title

The title of the graph and charts should be the main message you want to deliver to the audience. Choose the wordings that can summarize your message. A title should include your subjects of the visual. To catch the audience's eyes, you should definitely use a bigger font size for the title. 


```
plt.title(“Title Summarizing the Visual”, fontdict={"fontsize":25}) 
#font size can be modified based on the size of the visual
```


## Tip 3. Axis Label

Labeling your axis tells your audience the subject of the visual. Axis labels are text that mark major divisions on a chart. Your axis can be a category axis labels that show category names or a value axis labels that show values. In case of value axis, showing the scale in parentheses will help the understanding of the audiences.


```
plt.xlabel('x-axis label (scale if needed)', fontdict={"fontsize":15}, labelpad=10)
plt.ylabel('y-axis label (scale if needed)', fontdict={"fontsize":15}, labelpad=10)
#labelpad is the space between the axis label (the text) and the axis. Default is at None but if 
#you like to relocate your axis label you can use scalar value to separate the axis and the label.
```


## Tip 4. Tick Markers

Utilize your tick markers to provide comfortable view and better understanding of the visual. Tick marks provide reference for points on a scale. Each tick mark represents a specified number of units on a continuous scale, or the value of a category on a categorical scale. Making a mark in scaled or proportion on visual makes it easier to see.

```
#Sample coding
plt.tick_params('x', direction="out", pad=10, length=6, width=2, colors='r')
plt.xticks(np.arange(0, 110, step=10))
```

## Tip 5. Use Annotation

Try not to make your visual into a some what philosophical art work. Why not use the annotation instead of the audience try to figure out what you are trying to say. By using the annotation, you can clearly point out your findings to the audience. Use arrow, labels, circles, active titles, details, or even explanations in your visuals. 

Here is the sample coding.

```
plt.annotate('The highest point of the graph', xy=(100, 200),  xycoords='data',
            xytext=(0.65, 0.85), textcoords='axes fraction',
            arrowprops=dict(facecolor='red', shrink=0.05, 
                            connectionstyle="angle3"),
            horizontalalignment='right', verticalalignment='top'
            )
```

## Wrap Up

We have covered 5 quick tips on visualization. According to the TECHNICAL UNIVERSITY OF DENMARK in their April 2019 article [Abundance of information narrows our collective attention span](https://www.eurekalert.org/pub_releases/2019-04/tuod-aoi041119.php), people's attention span is shortening due to abundance of information. As people are too busy obtaining too many informaion, visulai communication is coming more and more important. Visualization is one of the key tools for us, data scientists, to communicate to our audience and it can be very powerful when used right. So, let us go ahead keep pratice and have fun with visualization.

![](https://www.easel.ly/blog/wp-content/uploads/2017/06/Humans-Visual-Twitter-Size-1.jpg)


