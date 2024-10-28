# Python-Exploratory Data Analysis
### Project Description:
In this deliverable, you will perform an exploratory data analysis (EDA) on a dataset containing information about popular tracks on Most Streamed Spotify Songs 2023 (https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023Links to an external site.). This task aims to analyze, visualize, and interpret the data to extract meaningful insights.
### General Guidelines:
1. Begin by familiarizing yourself with the structure of the dataset. Check for missing values and data types, and perform an initial exploration to understand the different features available.
2. Provide summary statistics to give an overview of key metrics such as the number of streams, release dates, and musical attributes (e.g., BPM, danceability).
3. Use appropriate visualizations (e.g., bar charts, histograms, scatter plots) to uncover trends and patterns in the data. Ensure that your plots are well-labeled and easy to interpret.
4. Investigate correlations between different variables and provide insights based on your findings. Explore relationships between streams and other musical characteristics like tempo, energy, or playlists.
5. Based on your analysis, offer any insights or recommendations regarding the tracks, artists, or musical trends that could be useful for understanding what makes a track popular.

## Pre-Analysis Programming: 
### Initializing libraries and opening excel worksheet-
To start off, the first thing I did was import pandas and matplotlib in preparation for the following data analysis process. The initial line of code is as follows: <br/>

```
import pandas as pd
import matplotlib.pyplot as plt
```

After that, to open the .csv file containing the data I'll be analyzing, I used the .read_csv function in pandas to open the file. To make sure that the file has been read successfully, I tried printing out the data but noticed that the print function did not display all of the data. So, I googled online on how to get the entire worksheet displayed and found that I had to use the display function along with pandas' option_context function (refer to reference 1 in References). The code is implemented as follows:

```
a = pd.read_csv('spotify-2023.csv') 
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    display(a)
```

### Data categorization-

## Data-Analysis: 
## Post-Analysis Discussion: 
## References:
1. https://stackoverflow.com/questions/62207066/pandas-does-not-show-the-complete-csv-file
