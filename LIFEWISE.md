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

![Formula](input/CodeCogsEqn-1.svg)

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

## Analysis

By examining the 'Complaint Type' field in our data, we can start to see trends emerge. Perhaps we find that noise complaints spike during the summer months, or that a specific borough has a high incidence of pothole reports. This analysis can provide insights that help shape city policy and resource allocation.



## Wrapping Up

The data from 3-1-1 service requests tells a story that goes beyond mere numbers. It's a narrative about the city's responsiveness, its challenges, and its ongoing journey to improve the lives of its residents. As we continue to analyze this data, we can help create a more responsive and efficient city.

---

Thanks for reading. Stay tuned for more data-driven stories!

[Edit this page](https://github.com/onefact/blog.datathinking.org/edit/main/pages/understanding-3-1-1-service-requests.md)