# San Francisco Housing Data Visualization
## Introduction
This application is designed to find properties in San Francisco market that represent viable investment opportunities. This single Jupyter Notebook utilizes Pandas, PyViz and hvPlot functions to generate interactive data visualiztions, including data aggregation, and geospatial analysis (using geoviews).

Added live application link: https://github.com/davidlp94/06_DLP_San_Francisco_Data_Visualization/blob/main/san_francisco_housing.ipynb

---

## Technologies

The following technology/software was used for this application:


-Python 3.7.11 (Programming language)

    Imported Python Libraries/Packages:
    
    -import pandas as pd
    
    -import hvplot.pandas
    
    -from pathlib import Path
    
-JupyterLab

-Git version 2.34.11.windows.1

-Windows 10 Operating System

---

## Installation Guide

To install this application on your machine, download (or clone using http link) all files contained in this repository davidlp94/06_DLP_San_Francisco_Data Vaisualization.

Next, open GitBash (terminal for MacOS), change your directory to the 06_DLP_San_Francisco_Data_Visualization, open JupyterLab in a dev environment and open/run the san_francisco_housing.ipynb file.

---

## Usage

This san_franscisco_housing.ipynb application contains the following files/directories:
```
.ipynb_checkpoints/ - This directory contains checkpoint/auto-save files.
san_francisco_housing.ipynb - This file contains the main source code for this application and can be ran in JupyterLab.
Resources/ - A directory containing .csv files for San Francisco housing data.
README.md - A README file that contains an overview of this application.
license.txt - MIT License
```

---

## Main Source Code

## Part 1: Calculate and Plot Housing Units per Year

Our very first step in this application involvs importing the necessary modules/libraries, reading the .csv data into our notebok and convertng it into a DataFrame. Then using the hvplot and groupby functions, we can plot a bar chart that displays the overall # of housing units per year.

```
import pandas as pd
import hvplot.pandas
from pathlib import Path

sfo_data_df = pd.read_csv(Path('Resources/sfo_neighborhoods_census_data.csv'))

housing_units_by_year = sfo_data_df.groupby('year').mean()

housing_units_by_year['housing_units'].hvplot.bar(frame_width=700, frame_height=400, ylim=[370000, 385000], \
xlabel='Year', ylabel='# of Housing Units', title='San Francisco Housing Units Per Year').opts(yformatter='%.0f')
```
![image](https://user-images.githubusercontent.com/96163075/153290736-63923b64-c4b6-4db2-abba-67d29661227b.png)

Based on the above plot, the overall trend shows that the number of housing units per year in the San Francisco area is steadily increasing each year.

---

## Part 2: Calculate and Plot the Average Sale Prices per Square Foot

This next part of this application involves the use of .groupby().mean() functions where the generated DataFrame is grouped by yearly data then the results are averaged.
```
sfo_data_sorted_df = sfo_data_df.groupby('year').mean()

prices_square_foot_by_year = sfo_data_sorted_df.loc[:, ['sale_price_sqr_foot', 'gross_rent']]

prices_square_foot_by_year.hvplot(frame_width=700, frame_height=400, xlabel='Year', ylabel='Cost in $', \
title='Sale Price per Sq. Foot and Gross Rent per Year in San Francisco').opts(legend_position='top_left')
```
![image](https://user-images.githubusercontent.com/96163075/153292428-94c4c41f-c381-4bf6-886e-df8e739f290b.png)

Based on the above DataFrames and plot generated, the San Francisco area experienced a drop in average sale price per square foot in the year 2011. However, the price of rent did not decrease. In fact, the average cost of rent ncreased in the year 2011.

---
## Part 3: Compare the Average Sale Prices by Neighborhood

The next part of this application involves calculating and plotting the average sale price per sq. foot for each San Francisco neighboorhood.
```
prices_by_year_by_neighborhood = sfo_data_df.groupby(['year', 'neighborhood']).mean()

prices_by_year_by_neighborhood = prices_by_year_by_neighborhood.loc[:, ['sale_price_sqr_foot', 'gross_rent']]

prices_by_year_by_neighborhood.hvplot(groupby='neighborhood', xlabel='year', ylabel='Cost in $', frame_width=600, \
frame_height=400, title='Sale Price per Sq. Foot and Gross Rent per Year for Each Neighboorhood')\
.opts(legend_position='top_left')
```
![image](https://user-images.githubusercontent.com/96163075/153293029-10e04e7c-e528-49ef-b46f-f832712f6476.png)

The interactive hvplot that was generated also contains a widget where you can select and see the plot for each neighborhood. Reviewing this plot, the average sale price per sq. foot in Anza Vista in 2016 is less than the price listed ofr in 2012.

---
## Part 4: Build an Interactive Neighborhood Map
The final part of this application utilizes hvplot and geoviews to plot on a map view and/or satelittle view of a geographical area. For this part, we need to read in some .csv data that contains the longitude and latitude of San Francisco neighborhoods, concatentate with the housing data and plotted geographically using hvplot and geoviews.
```
neighborhood_locations_df = pd.read_csv(Path('Resources/neighborhoods_coordinates.csv'), index_col='Neighborhood')

all_neighborhood_info_df = sfo_data_df.groupby('neighborhood').mean()

all_neighborhoods_df = pd.concat(
    [neighborhood_locations_df, all_neighborhood_info_df], 
    axis="columns",
    sort=False
)
all_neighborhoods_df = all_neighborhoods_df.reset_index().dropna()
all_neighborhoods_df = all_neighborhoods_df.rename(columns={"index": "Neighborhood"})

all_neighborhoods_df.hvplot.points(
    'Lon',
    'Lat',
    geo=True,
    size='sale_price_sqr_foot',
    color='gross_rent',
    tiles='OSM',
    frame_width=700,
    frame_height=500,
)
```
![image](https://user-images.githubusercontent.com/96163075/153302356-1c6afb21-8be7-4805-8620-3d72fdd77468.png)

Based on the interactive plot above, the neighborhood that has the highest gross rent is Westwood Park and Union Square District has the highest sale price per square foot. The majority of San Francisco neighborhoods rental prices have been steadily increasing year-over-year. Most nieghborhoods average sale price per square foot is steadily increasing as well but not as drastic as rent prices. However, Union Square District average sale price per square foot is drastically increasing. Based on the entire analysis, I would suggest to invest in Westwood Park becvause it has one of the highest gross rent with the lowestt average sale price per sq. foot cost.

---

## Contributors

David Lee Ping

email: davidleeping@gmail.com

Phone: 570.269.5973

LinkedIn: https://www.linkedin.com/in/david-lee-ping/

---

## License

MIT License

Copyright (c) [2022] [David Lee Ping]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
































