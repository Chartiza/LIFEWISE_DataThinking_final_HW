
# LIFEWISE 🏡
## ✅ Life Index for Well-being and Expense Sustainability

- Authors: Kateryna Pantiukh
- Co-author: ChatGPT 3.5

[Data]() | [Analysis](/Analysis) | [Conclusion](/)


# Project Overview

**LIFEWISE** _(Life Index for Well-being and Expense Sustainability)_ project utilizes regression analysis to identify optimal places to live, considering the balance between well-being and expenses. It offers actionable insights for individuals seeking high-quality living within their budget, while providing decision-making support for policymakers, urban planners, and real estate developers. The project aims to empower individuals and communities to make informed choices that enhance their overall quality of life and long-term sustainability. 

# Importance

🇺🇦 The topic of LIFEWISE holds deep personal significance to me, as it is rooted in my own journey of seeking a new home in Europe. Having been forced to move because of war, I found myself faced with the daunting task of deciding where to rebuild my life. With limited knowledge about European countries and their living conditions, I yearned for a data-driven approach to guide my decision-making process. It was this pivotal moment that sparked my interest in exploring the balance between quality of life and cost of living. Through the LIFEWISE project, I aim to empower individuals like myself, who seek a fresh start in unfamiliar territories, by providing actionable insights that enable informed choices.

# Input Data

The data regarding the cost of living featured in the LIFEWISE project is sourced from [NUMBEO](https://www.numbeo.com/cost-of-living/rankings_by_country.jsp). 

**Numbeo** is the world’s largest cost of living database. Numbeo is also a crowd-sourced global database of quality of life data: housing indicators, perceived crime rates, healthcare quality, transport quality, and other statistics. By utilizing Numbeo's data, LIFEWISE ensures the accuracy and relevance of the cost-related information.

The **input files** used in the LIFEWISE project are stored in the following location: input/data.xlsx. These files serve as the foundation of the analysis, containing the relevant data on cost of living, quality of life, counry and region.

**Quality of Life Index** _(higher is better)_ is an estimation of overall quality of life by using an empirical formula which takes into account 
- purchasing power index (higher is better), 
- pollution index (lower is better), 
- house price to income ratio (lower is better), 
- cost of living index (lower is better), 
- safety index (higher is better), 
- health care index (higher is better), 
- traffic commute time index (lower is better) and 
- climate index (higher is better).

![Formula](input/CodeCogsEqn-2.svg)

_where is_
**QLI** - Quality of Life Index;
**pPIR** - purchasingPowerInclRentIndex;
**hPIR** - housePriceToIncomeRatio;
**LivI** - costOfLivingIndex;
**SafI** - safetyIndex;
**HltI** - healthIndex;
**TrfI** - trafficTimeIndex;
**PolI** - pollutionIndex;
**ClmI** - climateIndex.

These indices are relative to New York City (NYC). Which means that for New York City, each index should be 100(%). If a city has a Cost of Living Index of 120, it means Numbeo has estimated it is 20% more expensive than New York (excluding rent).

## Part 1. Analysis of the correlation between the quality of life and the cost of living

The primary objective of this research is to investigate the correlation between the cost of living and the quality of life in various countries. The research aims to address two fundamental questions: 

1) Does a positive correlation exist between the cost of living and the quality of life?
2) Are there countries that offer a higher quality of life than expected based on their cost of living? 

To answer this questions we first build an interactive plot with vizualisation relationship between this indexes (Figure 1).

[Fig1](/results/Cost_vs_quality.html)

## Script. Part 1.
```python
# import packages
import pandas as pd
import plotly.express as px
import numpy as np
from sklearn import datasets, linear_model
import matplotlib.pyplot as plt
from scipy.stats import linregress
import scipy.stats as stats

# read data
rank = pd.read_csv('input/rank.csv', sep='\t')
reg = pd.read_csv('input/regions.csv', sep='\t')
mrg = pd.merge(rank, reg, on='Country')
# read custom correct data
mrg = pd.read_excel('results/merged_m.xlsx')

# define colors for regions
region_colors = {'Oceania': '#f4a259',
                 'Western_Asia': '#F44336',
                 'Northern_Europe': '#9ed0ab',
                 'Northern_America': '#4b6d8f',
                 'South-eastern_Asia': '#E57373',
                 'Australia': '#aa79d1',
                 'Central_America': '#6d8dc3',
                 'Southern_Asia': '#f9d6b8',
                 'Eastern_Asia': '#EF9A9A',
                 'Southern_Europe': '#bde3c5',
                 'Eastern_Europe': '#d0f0dc',
                 'South_America': '#6d8dc3',
                 'Northern_Africa': '#f9d9a0',
                 'Eastern_Africa': '#f7c36d',
                 'Western_Europe': '#7fcd91',
                 'Southern_Africa': '#e5981d',
                 'Western_Africa': '#edb880',
                 'Central_Asia': '#FFCDD2'}

# Create interactive scatterplot using Plotly Express
custom_order = ['Western_Europe', 'Northern_Europe', 'Eastern_Europe', 'Southern_Europe',
                'Western_Asia', 'Central_Asia', 'Eastern_Asia', 'Southern_Asia',
                'Northern_Africa', 'Western_Africa', 'Eastern_Africa', 'Southern_Africa',
                'South_America', 'Central_America', 'South-eastern_Asia', 'Australia',
                'Northern_America']

fig = px.scatter(mrg, x='Quality_of_Life_Index', y='Cost_of_Living_Index', color='Region',
                 color_discrete_map=region_colors, symbol='Region', hover_name='Country',
                 width=800, height=600, category_orders={'Region': ['Australia', 'Central_America', 'Northern_America', 'South_America',
                                             'Eastern_Africa', 'Northern_Africa', 'Southern_Africa', 'Western_Africa',
                                             'Eastern_Asia', 'Southern_Asia', 'South-eastern_Asia', 'Western_Asia','Central_Asia',
                                             'Eastern_Europe', 'Northern_Europe', 'Southern_Europe', 'Western_Europe']})

# Modify the legend and layout
fig.update_traces(marker=dict(size=10, line=dict(width=0.5, color='Gray')),
                  selector=dict(mode='markers'))
fig.update_layout(legend=dict(title='', orientation='v', yanchor='top', y=0.99, xanchor='left', x=1.02),
                  font=dict(family='Arial', size=14),
                  margin=dict(l=40, r=40, t=20, b=20), 
                  xaxis_title='Quality of Life Index', yaxis_title='Cost of Living Index',
                  width=900, height=600, template='plotly_white')

import plotly.io as pio
pio.write_html(fig, file='results/Cost_vs_quality.html', auto_open=True)

# Add to data information about Cost to quality ratio and quantiles
mrg['Cost_vs_quality_ratio'] = mrg['Quality_of_Life_Index']/mrg['Cost_of_Living_Index']
mrg['quantiles'] = pd.qcut(mrg['Cost_vs_quality_ratio'], q=4, labels=['bad_high','bad_low','good_low', 'good_high'])
mrg.to_excel('results/LQI_quantiles.xlsx', index=False)

# Visualize quantiles information
quantiles_colors = {'good_high': '#759116',
                 'good_low': '#056517',
                 'bad_low': '#f57a9b',
                 'bad_high': '#de1a24'}

fig = px.scatter(mrg, x='Quality_of_Life_Index', y='Cost_of_Living_Index', color='quantiles',
                 symbol='quantiles', hover_name='Country', color_discrete_map=quantiles_colors,
                 category_orders={'quantiles': ['good_high', 'good_low','bad_low','bad_high']},
                 width=800, height=600)

# Modify the legend and layout
fig.update_traces(marker=dict(size=10, line=dict(width=0.5, color='Gray')),
                  selector=dict(mode='markers'))
fig.update_layout(legend=dict(title='', orientation='v', yanchor='top', y=0.99, xanchor='left', x=1.02),
                  font=dict(family='Arial', size=14),
                  margin=dict(l=40, r=40, t=20, b=20), 
                  xaxis_title='Quality of Life Index', yaxis_title='Cost of Living Index',
                  width=900, height=600, template='plotly_white')

pio.write_html(fig, file='results/Quantiles.html', auto_open=True)

# Choose top countries with hight life quality index and good Cost to quality ratio 
mrg1 = mrg[(mrg['Quality_of_Life_Index'] >= 160) & 
           ((mrg['quantiles'] == 'good_high') | (mrg['quantiles'] == 'good_low'))]

mrg1.to_excel('results/top_counties.xlsx')
```

## Wrapping Up

The data from 3-1-1 service requests tells a story that goes beyond mere numbers. It's a narrative about the city's responsiveness, its challenges, and its ongoing journey to improve the lives of its residents. As we continue to analyze this data, we can help create a more responsive and efficient city.

---

Thanks for reading. Stay tuned for more data-driven stories!

[Edit this page](https://github.com/onefact/blog.datathinking.org/edit/main/pages/understanding-3-1-1-service-requests.md)