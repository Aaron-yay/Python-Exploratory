# Python-Exploratory Data Analysis
### Project Description:
In this deliverable, you will perform an exploratory data analysis (EDA) on a dataset containing information about popular tracks on Most Streamed Spotify Songs 2023 (https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023Links to an external site.). This task aims to analyze, visualize, and interpret the data to extract meaningful insights.
### General Guidelines:
1. Begin by familiarizing yourself with the structure of the dataset. Check for missing values and data types, and perform an initial exploration to understand the different features available.
2. Provide summary statistics to give an overview of key metrics such as the number of streams, release dates, and musical attributes (e.g., BPM, danceability).
3. Use appropriate visualizations (e.g., bar charts, histograms, scatter plots) to uncover trends and patterns in the data. Ensure that your plots are well-labeled and easy to interpret.
4. Investigate correlations between different variables and provide insights based on your findings. Explore relationships between streams and other musical characteristics like tempo, energy, or playlists.
5. Based on your analysis, offer any insights or recommendations regarding the tracks, artists, or musical trends that could be useful for understanding what makes a track popular.

## Note: 
So that the ReadMe isn't extremely cluttered (since it gets a bit challenging organizing my thoughts and the readme at the same time) I've mostly put here my code, reasoning for the code, observations, and opinions from what I've gathered from programming in Jupyter Notebook. The graphs I have decided to leave it in the Jupyter Notebook so for the best possible experience, I recommend using 2 tabs to read my observations and opinions while viewing the graphs simultaneously. :D

## Pre-Analysis Programming: 
#### Initializing libraries and opening excel worksheet-
To start off, the first thing I did was import pandas, seaborn and matplotlib in preparation for the following data analysis process. The initial line of code is as follows: <br/>

```
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
import seaborn as sns
```
#### Loading the .csv file-
After that, to open the .csv file containing the data I'll be analyzing, I used the .read_csv function in pandas to open the file. To make sure that the file has been read successfully, I tried printing out the data but noticed that the print function did not display all of the data. So, I googled online on how to get the entire worksheet displayed and found that I had to use the display function along with pandas' option_context function (refer to reference 1 in References). The code is implemented as follows:

```
a = pd.read_csv('spotify-2023.csv') 
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    display(a)
```
#### Bar Graph Function-
Next, to have an easier time with plotting data from data frames, I setup functions for some graphs. At first, while writing the code in Jupyter Notebook, I tried graphing the data
manually but noticed how much I needed to type in order to get one graph to work. Knowing that it'd get very tedious very quickly, I decided to somewhat automate the process for
graphing to atleast have a much more efficient workflow and allow me to focus more on data analysis than typing. <br/>

For the first function, I set it up for bar graphs since they're the most widely used in my Jupyter Notebook:

```
def barg(ey,bee,si,di,ee):
    plt.figure(figsize=(15, 6))
    sns.barplot(data=ey, x=bee, y=si)
    plt.title(di, fontsize=30, fontweight='bold')
    plt.ylabel(ee)
    plt.xlabel('')
    plt.grid(True)
    return plt.show()
```
#### Violin Plot Function- 
Next, is a violin plot which I used quite a bit when dealing with data spread out across the board instead of being fixed. I decided to only focus on bar graphs at first but then
found out that the violin plot gives a much better visualization for some instances which can be viewed later in this ReadMe file. The function is as follows:

```
def bayolin(ei,bi,de):
    plt.figure(figsize=(10, 20))
    sns.violinplot(data=a, x=ei, y='key', hue="mode")
    plt.title(bi, fontsize=30, fontweight='bold')
    plt.ylabel('Key')
    plt.xlabel(de)
    plt.grid(True)
```
#### Largest Value Finder Function- 
Up next is a function that helps identify the largest value within a given column using the .nlargest pandas function. I also looked up online and found out that there are other ways to get the largest value, but I decided to stick with this one since I found its name the most simple and self-explanatory. The extra lines of code are for finding it in the given data frame so I can easily view all the associated attributes they have.

```
def larg(x):
    playlist = a[x].nlargest(1).index
    top = a.loc[playlist]
    with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
        return display(top)
```
#### Column-Value Replacing Function-
Lastly, this function replaces the values of a column with the values of another. In this case, it replaces the values of the playlists in dataframe 'ey' with their associated artist name just so I have an easier time finding the most frequently artists.
```
def char(x):
    ey[x] = ey.apply(lambda row: row['artist(s)_name'] if pd.notna(row['artist(s)_name']) and row[x] not in [0, 'NaN'] else row[x], axis=1)

char('in_spotify_charts')
char('in_apple_charts')
char('in_deezer_charts')
char('in_shazam_charts')
```
 Also, the reason why the function and the function calls are in 1 cell is because this function was coded at a much later time, when I was nearly finished with the Jupyter Notebook, but I decided to put it together with the other functions for better organization.

## Structural Analysis:
For this part, it will mostly be about the structure of the given data frame named 'spotify-2023.csv'. Here, I'll mostly discuss the shape, data types, and the missing data present in the
given data frame. </br> </br>

#### Data Frame Shape- 
The shape of the data frame can be obtained by using the .shape pandas function. After using the .shape function, I was surprised to see a whopping 953 rows and 24 columns. This basically meant that I would have to use pandas functions to pick out the observations that I think are relevant for the topic of interest.
```
print(a.shape)
```
#### Data Type per Column- 
Next I used the .info function to find the data type housed within each individual column.
```
a.info()
```
Before manipulation the column data types are as follows:
1. track_name: object </br>
2. artist(s)_name: object </br>
3. artist_count: integer </br>
4. released_year: integer
5. released_month: integer
6. released_day: integer
7. in_spotify_playlists: integer
8. in_spotify_charts: integer
9. streams: object
10. in_apple_playlists: integer
11. in_apple_charts: integer
12. in_deezer_playlists: object
13. in_deezer_charts: integer
14. in_shazam_charts: object
15. bpm: integer
16. key: object
17. mode: object
18. danceability_%: integer
19. valence_%: integer
20. energy_%: integer
21. acousticness_%: integer
22. instrumentalness_%: integer
23. liveness_%: integer
24. speechiness_%: integer </br> </br>
Totalling to about 17-integers and 7-objects </br> </br>

#### (Extra) Data Type Conversion- 
Next, before talking about the missing values I converted the numerical object data types into float as some functions just don't work when being used on strings. 
```
a['streams']= pd.to_numeric(a['streams'],errors='coerce')
a['in_deezer_playlists'] = a['in_deezer_playlists'].str.replace(',', '', regex=False)
a['in_deezer_playlists']= pd.to_numeric(a['in_deezer_playlists'],errors='coerce')
a['in_shazam_charts'] = a['in_shazam_charts'].str.replace(',', '', regex=False)
a['in_shazam_charts']= pd.to_numeric(a['in_shazam_charts'],errors='coerce')
pd.set_option('display.float_format', '{:.2f}'.format)
```
I discovered while coding for some of the functions such as the .nlargest and .mean don't work with object data types and lead to an error message. So a bit of googling online and asking chatgpt I ended up with the following lines of code. It's a bit messy since I wasn't too familiar with how to clean the following lines of code but I do know how they work. </br> </br>
The ones with the .to_numeric function change the data type while the str.replace removes the commas in the numbers since the .to_numeric function returns an error when encountering non-numerical characters. Lastly, the pd.set_option allows me to view the resulting float data types in a non-scientific notation form. </br></br>

#### Observations Lacking Values (NaN)- 
Next, to find the observations lacking data (i.e.) the ones with NaN, I used the following functions. 
```
rows_with_nan = a[a.isna().any(axis=1)]
result = a.loc[rows_with_nan.index]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(result)
```
The .isna function and the .any(axis=1) allows me to find the rows containing NaN data types and by applying it to the .loc function, I'm able to find it within the data frame. </br></br>

#### Number of Rows Missing Values- 
By using the .shape function again, but this time on the data frame containing the missing data, I'm able to find how many of them are present.
```
print(result.shape)
```
The result is that there are 137 observations lacking missing values. And from what I've observed, the missing values are usually under the 'key' column and 'instrumentalness' column.

## Data-Analysis: 
From here on out, I'll be talking about the various functions I used to observe the various information present in the data frame. </br></br>

#### Mean, Median, Mode-
First up, the mean or means. Using the .mean function, I'm able to get the average of each respective column.
```
print(a.mean(numeric_only=True))
```
The results are as follows: 
1. artist_count = 1.56
2. released_year = 2018.24
3. released_month = 6.03
4. released_day = 13.93
5. in_spotify_playlists = 5200.12
6. in_spotify_charts = 12.01
7. streams = 514137424.94
8. in_apple_playlists = 67.81
9. in_apple_charts = 51.91
10. in_deezer_charts = 2.67
11. in_shazam_charts = 60.00
12. bpm = 122.54
13. danceability_% = 66.97
14. valence_% = 51.43
15. energy_% = 64.28
16. acousticness_% = 27.06
17. instrumentalness_% = 1.58
18. liveness_% = 18.21
19. speechiness_% = 10.13 </br></br>

From here, I noticed a pretty high bpm and danceability average. Which I think could have a potential correlation with each other. </br></br> 

Next, I used the .median function to find the 'middle' value from each column.
```
print(a.median(numeric_only=True))
```
The results are as follows:
1. artist_count = 1
2. released_year = 2022
3. released_month = 6
4. released day = 13
5. in_spotify_playlists = 2224
6. in_spotify_charts = 3
7. streams = 290530915
8. in_apple_playlists = 34
9. in_apple_charts = 38
10. in_deezer_playlists = 44
11. in_deezer_charts = 0
12. in_shazam_charts = 2
13. bpm = 121
14. danceability_% = 69
15. valence_% = 51
16. energy_% = 66
17. acousticness_% = 18
18. instrumentalness_% = 0
19. liveness_% = 12
20. speechiness_% = 6 </br></br>

From the results, I noticed deezer charts having 0 median. Median of 0, being the middle value when a set of numbers are plot out in order, could probably mean that Deezer has incredibly high standards or is simply not too well known and wide-reaching to have a lot of tracks ranked. </br></br>

Lastly, for the mode, I used the .mode function.
```
print(a.mode(numeric_only=True))
```
And got the following results:
1. artist_count = 1
2. released_year = 2022
3. released_month = 1
4. released_day = 1
5. in_spotify_playlists = 86, 356, 685, 811, 892, 896, 1112, 1150, 1473, 3006
6. in_spotify_charts = 0
7. streams = 156338624, 395591396, 723894473, 1223481149
8. in_apple_playlists = 0
9. in_apple_charts = 0
10. in_deezer_playlists = 0
11. in_deezer_charts = 0
12. in_shazam_charts = 0
13. bpm = 120
14. danceability_% = 70
15. valence_% = 24
16. energy_% = 74
17. acousticness_% = 0
18. instrumentalness_% = 0
19. liveness_% = 11
20. speechiness_% = 4 </br></br>

From the results, I'm able to infer that those with 0 modes either have alot of missing values or having very few track appearances.

#### Artist Count Distribution Per Year-
To find the artist count released per year, I separated the 2 columns from the main data frame.
```
c = a.loc[:,['artist_count','released_year']]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(c)
```
This allows me to put it into a more visual perspective using the .histplot function from matplot library and seaborn.
```
plt.subplots(figsize=(10, 10))
sns.histplot(data= c, x='released_year', hue='artist_count', multiple= 'stack', bins=30, edgecolor= '.3', alpha= 0.75, kde=True)
plt.title('Year-Artist Relationship')
plt.xlabel('Year')
plt.ylabel('Frequency of Specific Artist Count')
plt.show()
```
I decided to name the graph 'year-artist' instead of 'year-artist count' since it looks much better for me that way. </br></br>

Aside from that, I've observed that there are a few outliers between the years before 1940 and 2000. Which I did not expect to see since the ones I assumed to see are from the years between 2000 to 2020. Also another surprising observation is that the artist count increases all the way to 8 and it is concentrated at a time near 2020. So from that, I think it means collaboration between artists has become an extremely frequent occurence in the modern times.

#### Top 5 and Bottom 5- 
To get the top 5 and bottom 5, I used the nlargest and nsmallest function. 
```
streamed = a['streams'].nlargest(5).index 
topfive = a.loc[streamed]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(topfive)
streame = a['streams'].nsmallest(5).index 
botfive = a.loc[streame]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(botfive)
```
I did not use the earlier defined function here since I set it up for a different purpose. Although I'm sure I could've made it much more universal if I had more experience so for now, this will have to do. </br></br>

Here, I noticed that Blinding Lights is incredibly famous, having the most amount of streams and that the track "Que Valvas" is not so famous, barely having over 2700 streams. Which I can agree with since Blinding Lights is a song I have seen almost everywhere but it is the first time I have ever heard of Que Valvas.

#### 5 Most Frequently Appearing Artists:
To find the most frequently appearing artist, I used the following .value_counts and .head functions.
```
top_5 = a['artist(s)_name'].value_counts().head(5).index
fivetop = a.loc[a['artist(s)_name'].isin(top_5)]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(fivetop)
```
You may have noticed by now that my codes are very repetitive. I tried finding some ways to make it cleaner, hence the functions at the start, but I don't have much know-how and confidence in applying functions to all of them so I decided to code them straight up for consistency. </br></br>

The results here surprised me a bit. After seeing 'The Weeknd' being the top 1 in streams, I expected them to be the most frequent but it was actually Taylor Swift. So my take on this is that, even though The Weeknd was able to produce one of the most famous songs in the world, Taylor Swift beats them by consistency.
</br></br>
To get the summary, I simply printed out the condition.
```
print(top_5)
```
From what I've seen, the top 5 are: Taylor Swift, The Weeknd, Bad Bunny, SZA, and Harry Styles. Bad Bunny surprised me since its an artist I've almost never encountered nor heard of. 

#### Tracks Released Per Year-
To find the number of tracks released in a year, I took the frequency of the appearance of a single year and turned it into its own data frame. 
```
trackperyear = a['released_year'].value_counts() 
datrack = trackperyear.reset_index()
datrack.columns = ['Year', 'Count']
display(datrack)
```
From the data frame, I don't really have much to say. I just noticed that alot of the songs are from the years 2020 to 2022. </br></br>

To graph it, I used a line graph which did not have its own function since I only used it for this situation.
```
plt.figure(figsize=(10, 6))
sns.lineplot(data=datrack, x='Year', y='Count', marker='o') 
plt.title('Tracks per Year Graph') 
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.grid(True)
plt.show()
```
From here, I've observed that the peak at 2020 is incredibly high. I think we can even consider it the 'golden age' of pop songs.

#### Tracks-Per-Month- 
Similar to finding the number of tracks per year, the same approach is taken for finding the tracks per month. Finding their frequency and then getting turning it into its own data frame.
```
trackpermonth = a['released_month'].value_counts() 
trackframe = trackpermonth.reset_index()
trackframe.columns = ['Month', 'Count']
trackframe['Month'] = pd.to_datetime(trackframe['Month'], format='%m').dt.month_name()
display(trackframe)
```
So here again, we see the use of .value_counts and .columns. I think that there is definitely a way to turn this into a function but I'm not yet able to find it online and I don't want to overly rely on chatgpt. </br></br>

From the resulting data frame, I was surprised that the tracks are mostly released during January (start of the year) and May (mid-ish year). It genuinely intrigued me when I first saw that since I initially expected the results to be more balanced or scattered. </br></br>

For better visualization, I decided to plot it down using a bar graph. 
```
plt.figure(figsize=(15, 5)) ### Set large fig size 
sns.barplot(data=trackframe, x='Month', y='Count') ### Set a Bar plot to compare frequency 
plt.title('Tracks in a Month')
plt.xlabel('Month')
plt.ylabel('Count')
plt.grid(True) ### Grid to show comparison easier
plt.show()
```

The graph just shows how dominant January and May are with releases compared to the rest of the months. It's definitely a surprise to me but there isn't much for me to talk about from this aside from the fact that I should start frequently listening to the radio during January and May since there is a good chance a lot of new songs come out.

#### Attribute Comparison of Top 5 and Bottom 5- 
If you were wondering what the top 5 and bottom 5 data frames just now were to be used for, here it is. I decided to put the data frame initialization earlier is because it feels more fitting to be introduced earlier, so I apologize if it comes off as weird. </br></br>

In any case, I used the .loc function to obtain the top 5 and bottom 5's various attributes separated from the main data frame and turned into their own data frame.
```
wok = topfive.loc[:,['track_name', 'streams', 'danceability_%', 'valence_%', 'energy_%' , 'acousticness_%', 'instrumentalness_%', 'liveness_%','speechiness_%']]
kow = botfive.loc[:,['track_name', 'streams', 'danceability_%', 'valence_%', 'energy_%' , 'acousticness_%', 'instrumentalness_%', 'liveness_%','speechiness_%']]
display(wok)
display(kow)
```
Looking at the data frames, I don't have much to say since it isn't very easy to read in its data frame form, so I decided to graph it using bar graphs. And when turning it into bar graphs, I used the function defined earlier as follows:
```
barg(wok,'track_name','danceability_%','Danceability of Top 5','Danceability')
barg(wok,'track_name','valence_%','Valence of Top 5','Valence')
barg(wok,'track_name','energy_%','Energy of Top 5','Energy')
barg(wok,'track_name','acousticness_%','Acousticness of Top 5','Acousticness')
barg(wok,'track_name','instrumentalness_%','Instrumentalness of Top 5','Instrumentalness')
barg(wok,'track_name','liveness_%','Liveness of Top 5','Liveness')
barg(wok,'track_name','speechiness_%','Speechiness of Top 5','Speechiness')
```
With "Blinding Lights" as a form of reference, I can definitely say that 'energy' is definitely a defining factor. Although I still would not consider it the make it or break it since following "Blinding Lights" is "Shape of You" which has an extremely high valence and danceability percentage. </br></br>

After graphing the top 5, I also did the same for the bottom 5 just in case.
```
barg(kow,'track_name','danceability_%','Danceability of Bottom 5','Danceability')
barg(kow,'track_name','valence_%','Valence of Bottom 5','Valence')
barg(kow,'track_name','energy_%','Energy of Bottom 5','Energy')
barg(kow,'track_name','acousticness_%','Acousticness of Bottom 5','Acousticness')
barg(kow,'track_name','instrumentalness_%','Instrumentalness of Bottom 5','Instrumentalness')
barg(kow,'track_name','liveness_%','Liveness of Bottom 5','Liveness')
barg(kow,'track_name','speechiness_%','Speechiness of Bottom 5','Speechiness')
```
Surprisingly, the song "QUEMA" has high energy, valence, and danceability but is part of the lowest 5. </br></br>

So from here my conclusion is that, a popular song is definitely energetic, has good valence, and has high danceability but they do not define whether or not a song makes it into the top 10 categories. But honestly, whether a song is top 10 or not isn't the most important thing, and I am very curious with the song "QUEMA" right now (as of the time of writing).

#### Comparison between Danceabiltity and Energy- 
With my conclusion just now, I can somewhat expect that there is definitely a correlation with the two. So to make sure, I displayed their graphs again as follows: 
```
barg(wok,'track_name','danceability_%','Danceability of Top 5','Danceability')
barg(wok,'track_name','energy_%','Energy of Top 5','Energy')
barg(kow,'track_name','danceability_%','Danceability of Bottom 5','Danceability')
barg(kow,'track_name','energy_%','Energy of Bottom 5','Energy')
```
From here I've observed that an extremely high danceability is paired with a lower energy and vice versa. Which may imply that in order to maximize danceability, there needs to be a balanced 'energy' and that maximizing energy does not automatically mean that it is has a high danceability percent. From my point of view, that makes sense since rap songs which are usually extremely energetic are not always used for dances but songs with bouncy tunes but not necessarily energetic can be quite fun to dance to.

#### Comparison between Valence and Acousticness-
For these 2 attributes I took a similar approach, and hence, a similar coding process.
```
barg(wok,'track_name','valence_%','Valence of Top 5','Valence')
barg(wok,'track_name','acousticness_%','Acousticness of Top 5','Acousticness')
barg(kow,'track_name','valence_%','Valence of Bottom 5','Valence')
barg(kow,'track_name','acousticness_%','Acousticness of Bottom 5','Acousticness')
```
Here, in the top 5 the relationship is similar to energy and danceability. High valence doesn't lead to high acousticness but it may imply that. However it is quite hard to confirm since there are some missing values. 

#### Finding the Most Popular Platform Playlists- 
Similar to taking the top 5 and bottom 5's music attributes, this time I took the top 5 and bottom 5's playlists and charts and separated them into their own data frame.
```
plat = topfive.loc[:,['track_name', 'streams','in_spotify_playlists','in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 'in_deezer_playlists' , 'in_deezer_charts', 'in_shazam_charts']]
form = botfive.loc[:,['track_name', 'streams','in_spotify_playlists','in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 'in_deezer_playlists' , 'in_deezer_charts', 'in_shazam_charts']]
with pd.option_context('display.max_rows', None, 'display.max_columns', None):  
    display(plat)
    display(form)
```
Again, there is not much I can say about the data yet, so the next step I took is to get the mean of the charts and playlists.
```
AvePlat = plat.mean(numeric_only=True)
df = pd.DataFrame(AvePlat, columns=['A']).reset_index()
df.columns =['Platform','Average']
df = df.drop(index=0)
display(df)

AveForm= form.mean(numeric_only=True)
fd = pd.DataFrame(AveForm, columns=['A']).reset_index()
fd.columns =['Platform','Average']
fd = fd.drop(index=0)
display(fd)
```
From here we can now see that spotify playlists is the absolute winner in popularity. Beating the rest of the platforms by almost an exponential degree.
</br></br>
Next is the chart mean. The way I 've interpreted this is that the lower the mean, the higher the popularity as that would mean they rank high in the charts. From here we can see that, due to spotify's popularity, they have a more spread out mean. However, in Deezer, they rank much higher most likely due to being a lesser known platform. </br></br>
Two strange outliers for the top 5 here are apple and shazam. As apple has an extremely distributed mean indicating that the ranking is all over the place and shazam has too little data to work with. Looking back at the data frame, it can be seen that there is a few too many NaNs present there. </br></br>

Surprisingly the bottom 5 gives the most consistent results. Reinforcing that spotify is the most popular pattern, spotify playlist once again is the highest value mean. Another surprising observation is how the bottom 5 all rank higher in charts. So from here we can definitely conclude that streams and charts definitely don't have a directly positive correlation with one another.</br></br>

To better illustrate, I graphed them as bar graphs as follows:
```
barg(df,'Platform','Average','Platform of Top 5','Average')
ab = df.drop(index=1)
barg(ab,'Platform','Average','Platform of Top 5[w/o Spotify Playlist]','Average')
```
and, 
```
barg(fd,'Platform','Average','Platform of Bottom 5','Average')
ba = fd.drop(index=1)
barg(ba,'Platform','Average','Platform of Bottom 5[w/o Spotify Playlist]','Average')
```
### Advanced Analysis:
From here on, the following 2 attempts are merely just attempts to answer the question. I struggled the most here and took the most time despite it only being 2 questions. </br></br>

#### Finding All the Tracks in Major Modes-
First, I tried getting the tracks to see if I can spot any patterns via the following code.
```
majmode = a.loc[(a['mode']=='Major')]
with pd.option_context('display.max_rows', None, 'display.max_columns', None): 
    display(majmode)
```
Based on the data frame, there is not much I can find nor say about it. So the next thing I did was plot alot of violin plots to based on their various attributes as follows: 
```
bayolin('bpm', 'BPM Based on Key and Mode', 'BPM Value')
bayolin('danceability_%','Danceability Based on Key and Mode', 'Danceability Value')
bayolin('valence_%','Valence Based on Key and Mode', 'Valence Value')
bayolin('energy_%','Energy Based on Key and Mode', 'Energy Value')
bayolin('acousticness_%','Acousticness Based on Key and Mode', 'Acousticness Value')
bayolin('instrumentalness_%','Instrumentalness Based on Key and Mode', 'Instrumentalness Value')
bayolin('liveness_%','Liveness Based on Key and Mode', 'Liveness Value')
bayolin('speechiness_%','Speechiness Based on Key and Mode', 'Speechiness Value')
```
From here I noticed a couple of things:
1. Both major and minor modes have a very balanced distribution in bpm
2. Danceability and energy are more distributed for major keys but has a frequently higher value for minor keys.
3. Valence of keys between major and minor share and interesting relationship. With the major keys having lower values and minor keys having higher values

#### Finding the Most Popular Songs Based On Platform (Playlists)-
Here, I tried finding more information on the relation between the various attributes by obtaining them as follows.
```
larg('in_spotify_playlists')
larg('in_apple_playlists')
larg('in_deezer_playlists')
```
The results here I got are as follows:
1. Most Popular in Spotify = Get Lucky
2. Most Popular in Apple = Blinding Lights
3. Most Popular in Deezer = Smells Like Teen Spirit

After that, I concatenated the results as follows.
```
spot = a.loc[a['track_name']=='Get Lucky - Radio Edit']
app = a.loc[a['track_name']=='Blinding Lights']
deez = a.loc[a['track_name']=='Smells Like Teen Spirit - Remastered 2021']
playlistt = pd.concat([spot, app, deez], ignore_index=True)
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    display(playlistt)
```

## Summary of Observations: 
## References:
1. https://stackoverflow.com/questions/62207066/pandas-does-not-show-the-complete-csv-file
2. https://note.nkmk.me/en/python-pandas-len-shape-size/
3. https://www.statology.org/pandas-mean-median-mode/
4. https://seaborn.pydata.org/examples/histogram_stacked.html
5. https://scales.arabpsychology.com/stats/how-to-change-font-size-in-seaborn-plots-with-examples/#google_vignette
