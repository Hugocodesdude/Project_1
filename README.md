# Project_1

# SHARK ATTACK! What's up with that? 

## Task: 
- We were given a dirty data set and asked to explore, clean, analysis and visualise it. Being our first end to end deliverable, the focus was on cleaning but also develop a hypothesis and present it. 

## Objective: 
- The aim of this "READ ME" is to take the reader through the project process.

## Hypothesis: 

- Shark attacks are increasing. We can better understand why that is when we consider factors such as habitat destruction & climate change in impacting sharks behavior in conjunction with a spike in human population growth.
- Second Hypothesis: Technological developments are allowing humans to better record and deter shark attacks.
- Third Hypothesis: Does began vegan mean that you are less likely to be attacked by a shark?


# Section 1: Data Exploration 

## Objective: 
- My aim here is tool get a better understanding of the provided dataset. For example, how many rows and columns there are. To gadge how dirty it is and also what data points intrigue me. 

## Process 

1. Import Tools & DataFrame

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
from matplotlib import rcParams
```
```python
#Data Import 

df = pd.read_csv ('./data/attacks.csv', encoding='unicode_escape')
```

2. First Glance¶
- View DataFrame
- Explore: Shape, Basic Statistics & Information


```python
#DataFrame Shape
df.shape
```
```python
#Basic Statistics
df.describe()

```
```python
#Display Data Types
df.info()
```

3. Identify Columns of Interest
- Display, list and visualise raw data
- Here is used various coding arguments to investigate the data subsets. 

```python
#Example Column Display: "Year"
df["Year"]
```

```python
#Value Count of Countries 
(df["Country"].value_counts() > 5)
```

```python
#List version of column "Countries" 
#We can see an outlier of "2229"

list(df["Country"].value_counts())
```
```python
#Raw Data Vis Countplot of Country ValueCount 
sns.countplot(data=df, x="Country")
```
```python
year_condition_1 = df["Year"] > 1900
df[year_condition_1]
```

- I followed this process of displaying the columns, creating value counts, putting them in lists then doing a raw data vis as another means of understanding how dirty this set is. 


## Exploration Conclusion: 

- country, Year, Area, Activity, Sex, Injury, Age & Species all present themselves as valuable data columns
- Hypothesis Ideas: It would be interesting to see if improving tech, habitat degredation & climate change are factors in rising shark attacks.
- I reached a point in the exploration in which I wasn't able to continue without cleaning.




 # Section 2: Clean & Analyse 

1. Import Tools & DataFrame

```python
#Tools Import

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
from matplotlib import rcParams
import re
pd.options.mode.chained_assignment = None
```

```python
#Data Import 

df = pd.read_csv ('./data/attacks.csv', encoding='unicode_escape')
```

2. Create new Data Frame to clean
- Delete irrelevant columns.
- Revise DataFrame & remove further columns if need be.
- View shape and sample data.

3. Begin to clean dataset

```python
#Drop irelavant columns 

df_clean = df_clean.drop(columns=["Unnamed: 23","Unnamed: 22","original order","Case Number.2","Case Number.1","href","href formula","pdf", "Investigator or Source", "Name"])

```

```python
#We can see that the DF has 12 less columns 

df_clean.shape
```


```python
#Revised DataFrame
df_clean
```

Interesting distinction between "Fatal (Y/N" & "Injury" in terms of fatalities

```python
df_clean["Fatal (Y/N)"].value_counts()
```

```python
pdf_clean["Injury"].value_counts()
```

```python
#Drop further irelavant columns 

df_clean = df_clean.drop(columns=["Case Number","Type","Area","Location", "Time","Injury"])
```


```python

df_clean['Date'].isna().sum()
```

```python
# Revised DF sample

df_clean.sample()
```
- This action showed how many NaN data points there were.

4. Incomplete data
- Count number of NaN
- Create new DF to clean
- Action Clean


```python
#Count of NaN under an entire DataFrame

df_clean.isnull().sum().sum()
```




```python
#Created a new DF
df_clean_nan = pd.DataFrame(df_clean)
```

```python
#Removing rows with "all" nans

df_clean_nan.dropna(axis=0, inplace=True, how="all")
```

5. Refine data columns 

- I then sought to apply the same cleaning method to the colums of interest. The following code series is an example. 


```python
year_counts = df_clean_nan["Year"].value_counts()
print(year_counts.to_string())
```

```python
df_clean_nan.drop(df_clean_nan[df_clean_nan.Year < 1900].index, inplace=True)
df_clean_nan = df_clean_nan[df_clean_nan['Year'].notna()]
```

```python
df_clean_nan["Year"] = df_clean_nan["Year"].astype("int")
```

Clean Titles
Remove spaces from "Sex " & "Species "

```python
df_clean_nan.rename(columns={"Sex ":"Sex"},inplace=True)
df_clean_nan.rename(columns={"Species ":"Species"},inplace=True)
```
```python
print(df_clean_nan.columns)
```
Clean "Fatal (Y/N)" Column
We are only interested in Y/N

```python
df_clean_nan["Fatal (Y/N)"].unique()
```
```python
pcheck = pd.DataFrame(df_clean_nan["Fatal (Y/N)"].value_counts())
check
```
```python
df_clean_nan["Fatal (Y/N)"] = df_clean_nan["Fatal (Y/N)"].str.extract("^(Y|N)")
df_clean_nan
```
Revising Countries
actioning string manipulation
Capitalise

```python
df_countries = df_clean_nan["Country"]
df_countries = df_clean_nan["Country"].str.replace("ENGLAND", "UK")
df_countries = df_clean_nan["Country"].str.replace("ENGLAND ", "UK")


df_clean_nan["Country"] = df_countries
```
```python
df_clean_nan["Country"] = df_clean_nan["Country"].str.replace("ENGLAND", "UK").replace("ST HELENA,British overseas territory", "ST HELENA").replace("UNITED KINGDOM","UK").replace("ST HELENA, British overseas territory","ST HELENA").replace("PALESTINIAN TERRITORIES","PALESTINIAN").replace("SCOTLAND","UK").replace("UNITED ARAB EMIRATES (UAE)","UAE").replace("AZORES","PORTUGAL").replace("PACIFIC OCEAN","OCEAN").replace("ATLANTIC OCEAN","OCEAN").replace("UNITED KINGDOM","UK").replace("SOUTH ATLANTIC OCEAN","OCEAN").replace("CARIBBEAN SEA","OCEAN").replace("NORTH PACIFIC OCEAN","OCEAN").replace("MID ATLANTIC OCEAN","OCEAN").replace("INDIAN OCEAN","OCEAN").replace("NORTH ATLANTIC OCEAN","OCEAN").replace(" TONGA","TONGA").replace("PERSIAN GULF","OCEAN").replace("Fiji","FIJI").replace("CENTRAL PACIFIC","OCEAN").replace("PACIFIC OCEAN","OCEAN").replace("SOUTHWEST PACIFIC OCEAN","OCEAN").replace("SOUTH PACIFIC OCEAN","OCEAN").replace("SUDAN?","SUDAN").replace("THE BALKANS","OCEAN").replace("IRAN / IRAQ","IRAN").replace("MID-PACIFC OCEAN","OCEAN").replace("ITALY / CROATIA","ITALY").replace(" PHILIPPINES","PHILIPPINES").replace("BAY OF BENGAL","OCEAN").replace("SOLOMON ISLANDS / VANUATU","SOLOMON ISLANDS").replace("NORTH ATLANTIC OCEAN","OCEAN").replace("ANDAMAN / NICOBAR ISLANDAS","ANDAMAN").replace("Seychelles","SEYCHELLES").replace("GULF OF ADEN","OCEAN").replace("EGYPT / ISRAEL","EGYPT").replace("NORTHERN ARABIAN SEA","OCEAN").replace("NORTH SEA","OCEAN").replace("RED SEA / INDIAN OCEAN","OCEAN").replace("RED SEA", "OCEAN").replace("BRITISH ISLES","UK").replace("SOUTH CHINA SEA", "OCEAN")

df_clean_nan
```
Cleaning using lambda 

```python
dftop20 = df_clean_nan["Species"].value_counts().loc[lambda x: x>20].index
dftop20
```

- By doing this I was able to seperate down the list into the top 20 species as a means to refine my data set. 

Review Activity

I created a function to group down the range of activities into categories 

```python
def group_activity(e):
    if type(e) == float:
        return None
    if "fish" in e:
        return "Fishing"
    
    elif "surf" in e:
        return "Surfing"
    
    elif "swim" in e:
        return "Swimming"
    
    elif "wading" in e or "wade" in e:
        return "Wading"
    
    elif "diving" in e or "dive" in e:
        return "Diving"
    
    elif "stand" in e:
        return "Standing"
    
    elif "board" in e:
        return "Boarding"
    
    elif "bath" in e:
        return "Bathing"
    
    elif "snorkel" in e:
        return "Snorkeling"
    
    else:
        return "Others"
    
new_df["Activity"] = new_df["Activity"].apply(group_activity)
```

```python
new_df["Activity"].value_counts()
```


```python
check_activity = pd.DataFrame(new_df["Activity"].value_counts())
check_activity.head(10)
```

Saved dataframe to new file. 


```python
new_df.to_csv("./data/new_df.csv", index=False)
```




 # Section 3: Data Visualisation & Conclusion

1. Import dataframe and tools

```python
#Tools Import

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
from matplotlib import rcParams
import re
pd.options.mode.chained_assignment = None
```

```python
new_df = pd.read_csv("./data/new_df.csv")
```
Clean DataFrame
25723 rows × 24 columns to 1710 rows × 8 columns


```python
print('hello world')

 <img src="images/graph_1.png" width="250" height="200">





 ```python
print('hello world')
```


 ```python
print('hello world')
```
# Primary Visualisation

- What can we ascertain from this? 

 <img src="images/graph_1.png" width="250" height="200">

By observing the density volume from 1900 onwards, we can begin to see a clear trend develop. Suggesting that shark attacks are indeed inceaseing.

From 1980 onwards the density rate begins to drastically increase.

Comparing 2000 - 2020 & 1940 - 1960. We can say that while more people are being bitten between 2000 - 2020, more people are surviving compared to recorded attacks between 1940 - 1960.

From 1990 onwards there has been an increase in attacks on humans 40 and above.


## Let's investigate attack rates further.
- Are attacks trully increasing?
- Does more attacks mean more deaths?

At first glance we can see that see that shark attacks are increasing.
The rate in attack growth begins to expand exponentially from 1980.
Interestingly while attack rates rise dramatically, we are not seeing fatalities increasing comparatively.
Questions:

Why is there such a stark increase in attacks from 1980? What factors could be contributing towards this?
How are more people surviving shark attacks?


## Shark Species Breakdown

- Are there certain species that are more dangerous than others? 

What this graph highlights is that Bull, Tiger & 'Great' White sharks are by far the most deadly.
White shark attack have a 100% fatality rate.
The increasing histogram density aligning with 'Year' suggests that attacks are increasing.
Beyond the 3 particularly agressive species, attacks don't necessarily equate to fatalities. Suggesting that technologies relating to prevention and health care are improving.

## Understanding Age Demographics  

- What insights can we can from visualising age ranges?

The stand out ages groups affected by shark attacks are 18-24, 25-31 year olds.
The median age is within the late 20's and mean age in the early 30's.
This indicate that these age groups are more active in terms of partaking in water sports and work.

## Are certain activities at risk? 

- What this histoplot clearly reveals is that fishing and surfing are the two most dangerous activities. 

# Are there certain countries worse affected than others?

- What insights can we gain from viewing a breakdown of countires affects?

The USA, Australia and South Africa are the clear break away outliers in terms of countries most affected by shark attacks.
This is in part due to all three countries having very large coast lines (US = 153652.12 km, Australia = 34000km, South Africa = 2,800km). Meaning more human activity in waters.
The three countries all share multiple bodies of water. A variation of gulfs, seas and oceans. All of which have their own aquatic ecosystems and support shark life.
Interestingly if we compare the population of the US (329.5 m) and Australia (25.69 m), we can see that per capita, Australians are much more at risk of shark attacks.

# Last but not least...

## Does being vegan mean you're safer?
- Taking a closer view at this graph one more we can see that 'fishing' represents 27.73% of activities affected by shark attacks. 
- Therefore, hypothetically we can assume that being vegan by default reduces you chance of being attacked by over a quarter. 


# Conclusion 

- The insights derived from analysing the data set clearly display that from the middle of the 20th Century, rates of shark attacks have increased. 
- This increase begin to dratically rise from 1980 onwards. 
- The conclusion I has drawn from this is that impacts habitat destruction and climate change mean that sharks are venturing into new territory as waters around the globe warm. 
  - Furthermore fish populations dwindle, sharks are searching further and further for food sources. 
- As the worlds population rises, naturally so does the likelyhood of more shark attacks as there are more people partaking in water sports, activities and working amoung the seas. Since 1980 the world's population has almost doubled, which is an astonishing feat. 
- Lastly I believe that while shark attack are more common for the reasons explained above, it is important to note the advancement of human technology in preventing attacks but also in terms of emergency assitance and treatment. Particularly in developed affected countries that have invested in systems such as Australia. 