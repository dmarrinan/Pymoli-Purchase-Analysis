# Pymoli-Purchase-Analysis

## Purpose
This jupyter notebook analyzes trends in purchasing data included in the json files purchase_data.json and purchase_data2.json. <br/>
Each object in the json file contains the following information about a purchase: <br/>
Purchaser's Screen Name [SN] <br/>
Purchaser's [Age] <br/>
Purchaser's [Gender] <br/>
Purchased [Item ID] <br/>
Purchased [Item Name] <br/>
Purchase [Price] <br/>

It finds some general summary stats about all of the purchases, breaks down the gender demographics of the purchasers, analyzes the purchasing habits of different genders,breaks down the age demographics of the purchasers,finds the players who spent the most purchasing items, finds the most popular items purchased, and finds the items that were the most profitable.

## Requirements
This notebook requires python to be installed. Python 3.6.2 was used during development. It also utilizes the json library included with python to read the data files. Pandas version 0.21.0 and jupyter version 1.0.0 were also used to develop this code. The necessary libraries to run this program can be installed with the file requirements.txt using the following command in the terminal:
`$ pip install requirements.txt -r`

##Running the Code
To run the notebook enter the following into the command line: 
`$ jupyter notebook`
Open up the notebook purchasing_analysis.ipynb in your browser and then click the 'Kernal' and the 'Restart and run all option'

## Methods
This notebook relies heavily on the summary statstical functions built into pandas such as sum, max, describe, and nunique.
It also utilizes pandas indexing,sorting and grouping functions such as loc, iloc, sort_values, and groupby. 

## Results
## Observations in Data for File 1

1) The average item's purchase price was highest for other/nongender users. The average item's purchase price for men was greater than that for women.

2) The largest portion of users were between the ages of 20-24. Over 40% of users fell in this age range.

3) The most profitable items were bought fewer times for a greater cost than the most popular items.


```python
#import relevant libraries
import pandas as pd
import json
```


```python
#set names of files to read in 
file_name = "purchase_data.json"
```


```python
raw_df = pd.read_json(file_name, orient="records")
raw_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count


```python
#calculate number of players and print results
num_users_df = pd.DataFrame()
num_users = raw_df["SN"].nunique()
num_users_df["Total Players"] = num_users
num_users
```




    573



## Purchasing Analysis (Total)


```python
#set up data frame to store purchasing analysis
purchasing_analysis = pd.DataFrame()
```


```python
#find number of unique items
num_unique_items = raw_df["Item Name"].nunique()
purchasing_analysis["Number of Unique Items"] = [num_unique_items]
```


```python
#find average price of items
average_price = raw_df["Price"].mean()
purchasing_analysis["Average Price"] = [average_price] 
```


```python
#find the total number of purchases
num_purchases = len(raw_df.index)
purchasing_analysis["Number of Purchases"] = [num_purchases]
```


```python
#find the total revenue
total_revenue = raw_df["Price"].sum()
purchasing_analysis["Total Revenue"] = [total_revenue] 
```


```python
#display results
purchasing_analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>2.931192</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
#create gender demographics dataframe
gender_demographics = pd.DataFrame(columns = ["Percentage of Players","Total Count"],index = ["Male","Female","Other/Non-Disclosed"])
```


```python
#filter by gender to get df full of male users
male_demographics = pd.DataFrame()
male_df = raw_df.loc[raw_df["Gender"]=="Male"]
```


```python
#filter by gender to get df full of female users
female_df = raw_df.loc[raw_df["Gender"]=="Female"]
```


```python
#get other/Non-Disclosed users
other_gender_filter = (raw_df["Gender"]!="Male")&(raw_df["Gender"]!="Female")
other_gender_df = raw_df.loc[other_gender_filter]
```


```python
#calculate number of males
num_males = male_df["SN"].nunique()
gender_demographics.loc["Male"]["Total Count"] = num_males
num_males
```




    465




```python
#calculate number of females
num_females = female_df["SN"].nunique()
gender_demographics.loc["Female"]["Total Count"] = num_females
```


```python
#calculate number of users that are of other/non-disclosed gender
num_other_gender = other_gender_df["SN"].nunique()
gender_demographics.loc["Other/Non-Disclosed"]["Total Count"] = num_other_gender
```


```python
#calculate percentage of users that are male
percent_male = num_males/num_users*100
gender_demographics.loc["Male"]["Percentage of Players"] = percent_male
percent_male
```




    81.15183246073299




```python
#calculate percentage of users that are female
percent_female = num_females/num_users*100
gender_demographics.loc["Female"]["Percentage of Players"] = percent_female
```


```python
#calculate percntage of users that are of other/non-disclosed gender
percent_other_gender = num_other_gender/num_users*100
gender_demographics.loc["Other/Non-Disclosed"]["Percentage of Players"] = percent_other_gender
```


```python
#display gender demographics
gender_demographics
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.1518</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.452</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.39616</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
#create purchasing analysis gender dataframe
purchase_gender_index = ["Male","Female","Other/Non-Disclosed"]
purchase_gender_columns = ["Purchase Count","Average Purchase Price","Total Purchase Value", "Normalized Totals"]
purchasing_analysis_gender = pd.DataFrame(index=purchase_gender_index,columns = purchase_gender_columns)
purchasing_analysis_gender.index
```




    Index(['Male', 'Female', 'Other/Non-Disclosed'], dtype='object')




```python
#calculate number of purchases by male users
num_purchases_male = len(male_df.index)
purchasing_analysis_gender.loc["Male"]["Purchase Count"] = num_purchases_male
```


```python
#calculate number of purchases by female users
num_purchases_female = len(female_df.index)
purchasing_analysis_gender.loc["Female"]["Purchase Count"] =num_purchases_female
```


```python
#calculate number of purchases by users of other/non-disclosed gender
num_purchases_other_gender = len(other_gender_df.index)
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Purchase Count"] =num_purchases_other_gender
```


```python
#find average purchase price for items by male users
average_price_male = male_df["Price"].mean()
purchasing_analysis_gender.loc["Male"]["Average Purchase Price"] = average_price_male
```


```python
#find average purchase price for items by female users
average_price_female = female_df["Price"].mean()
purchasing_analysis_gender.loc["Female"]["Average Purchase Price"] = average_price_female
```


```python
#find average purchase price for items by users with other/nonspecified genders
average_price_other_gender = other_gender_df["Price"].mean()
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Average Purchase Price"] = average_price_other_gender
```


```python
#find total purchase value for male users
total_value_male = male_df["Price"].sum()
purchasing_analysis_gender.loc["Male"]["Total Purchase Value"] = total_value_male
```


```python
#find total purchase value for female users
total_value_female = female_df["Price"].sum()
purchasing_analysis_gender.loc["Female"]["Total Purchase Value"] = total_value_female
```


```python
#find total purchase value for male users
total_value_other_gender = other_gender_df["Price"].sum()
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Total Purchase Value"] = total_value_other_gender
```


```python
#find percentage of total purchase value by male users
purchasing_analysis_gender.loc["Male"]["Normalized Totals"] = total_value_male/num_males
```


```python
#find percentage of total purchase value by female users
purchasing_analysis_gender.loc["Female"]["Normalized Totals"] = total_value_female/num_females
```


```python
#find percentage of total purchase value by male users
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Normalized Totals"] = total_value_other_gender/num_other_gender
```


```python
#display purchasing analysis by gender
purchasing_analysis_gender
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.95052</td>
      <td>1867.68</td>
      <td>4.01652</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.81551</td>
      <td>382.91</td>
      <td>3.8291</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>11</td>
      <td>3.24909</td>
      <td>35.74</td>
      <td>4.4675</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
#find info on age distribution
raw_df["Age"].describe()
```




    count    780.000000
    mean      22.729487
    std        6.930604
    min        7.000000
    25%       19.000000
    50%       22.000000
    75%       25.000000
    max       45.000000
    Name: Age, dtype: float64




```python
#set age bins
if raw_df["Age"].max() == 40:
    bins = [0, 10, 15, 20, 25,30,35,40]
    bin_index = ["<11","11-15","16-20","21-25","26-30","31-35","36-40"]
else:
    bins = [0, 10, 15, 20, 25,30,35,40,raw_df["Age"].max()]
    bin_index = ["<11","11-15","16-20","21-25","26-30","31-35","36-40","41+"]
```


```python
#bin by age
df_age_bins = pd.cut(raw_df["Age"],bins)
```


```python
#add age bin column to data frame
age_df = raw_df
age_df["Age Bin"] = df_age_bins
```


```python
#group by age bin
groupby_age = age_df.groupby(by="Age Bin")
```


```python
num_users_age_bin = []
age_demo_columns = ["Percentage of Players","Total Count"]
age_demographics = pd.DataFrame(columns = age_demo_columns,index=bin_index)

for age_bin,group in groupby_age:
    #num_per_age = group["SN"].value_counts()
    num_users_age_bin.append(group["SN"].nunique())

age_demographics.loc[:,"Total Count"] = num_users_age_bin
age_demographics["Percentage of Players"] = age_demographics["Total Count"]/num_users*100
age_demographics
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;11</th>
      <td>3.839442</td>
      <td>22</td>
    </tr>
    <tr>
      <th>11-15</th>
      <td>9.424084</td>
      <td>54</td>
    </tr>
    <tr>
      <th>16-20</th>
      <td>24.258290</td>
      <td>139</td>
    </tr>
    <tr>
      <th>21-25</th>
      <td>40.837696</td>
      <td>234</td>
    </tr>
    <tr>
      <th>26-30</th>
      <td>9.075044</td>
      <td>52</td>
    </tr>
    <tr>
      <th>31-35</th>
      <td>7.678883</td>
      <td>44</td>
    </tr>
    <tr>
      <th>36-40</th>
      <td>4.363002</td>
      <td>25</td>
    </tr>
    <tr>
      <th>41+</th>
      <td>0.523560</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
#calculate stats for each age group
groupby_age_stats = pd.DataFrame({"Average Price":groupby_age["Price"].mean()})
```


```python
groupby_age_stats["Total Purchase Value"] = groupby_age["Price"].sum()
```


```python
#add number of purchases to sum
num_purchases_age = []
for age_bin,group in groupby_age:
    num_purchases_age.append(len(group.index))
    
groupby_age_stats["Number of Purchases"] = num_purchases_age
```


```python
#normalized totals
purchase_value_list = groupby_age_stats["Total Purchase Value"].values
total_count_list = age_demographics["Total Count"].values
groupby_age_stats["Normalized Totals"] = purchase_value_list/total_count_list
groupby_age_stats
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Total Purchase Value</th>
      <th>Number of Purchases</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Bin</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(0, 10]</th>
      <td>3.019375</td>
      <td>96.62</td>
      <td>32</td>
      <td>4.391818</td>
    </tr>
    <tr>
      <th>(10, 15]</th>
      <td>2.873718</td>
      <td>224.15</td>
      <td>78</td>
      <td>4.150926</td>
    </tr>
    <tr>
      <th>(15, 20]</th>
      <td>2.873587</td>
      <td>528.74</td>
      <td>184</td>
      <td>3.803885</td>
    </tr>
    <tr>
      <th>(20, 25]</th>
      <td>2.959377</td>
      <td>902.61</td>
      <td>305</td>
      <td>3.857308</td>
    </tr>
    <tr>
      <th>(25, 30]</th>
      <td>2.892368</td>
      <td>219.82</td>
      <td>76</td>
      <td>4.227308</td>
    </tr>
    <tr>
      <th>(30, 35]</th>
      <td>3.073448</td>
      <td>178.26</td>
      <td>58</td>
      <td>4.051364</td>
    </tr>
    <tr>
      <th>(35, 40]</th>
      <td>2.897500</td>
      <td>127.49</td>
      <td>44</td>
      <td>5.099600</td>
    </tr>
    <tr>
      <th>(40, 45]</th>
      <td>2.880000</td>
      <td>8.64</td>
      <td>3</td>
      <td>2.880000</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
#groupby user
groupby_user = raw_df.groupby(by="SN")   
```


```python
#create data frame to store summary statistics for user
user_columns = [["User", "Total Purchase Value","Average Purchase Price","Number of Purchases"]]
user_stats = pd.DataFrame(columns=user_columns)
user_stats
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>User</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
#calculate user statistics
for user,group in groupby_user:
    sum_price_user = group["Price"].sum()
    average_price_user = group["Price"].mean()
    purchase_count_user = len(group.index)
    ##############################################################
    #how to save stats
    group_stats = {"User":user,"Total Purchase Value":sum_price_user,"Average Purchase Price":average_price_user,"Number of Purchases":purchase_count_user}
    user_stats = user_stats.append(group_stats,ignore_index=True)
```


```python
#sort users by total purchase value and display top 5 users
sortby_purchase_value_df = user_stats.sort_values(by='Total Purchase Value',ascending=False)
top_5_purchase_value = sortby_purchase_value_df.iloc[0:5,:]
top_5_purchase_value
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>User</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>538</th>
      <td>Undirrala66</td>
      <td>17.06</td>
      <td>3.412000</td>
      <td>5</td>
    </tr>
    <tr>
      <th>428</th>
      <td>Saedue76</td>
      <td>13.56</td>
      <td>3.390000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>354</th>
      <td>Mindimnya67</td>
      <td>12.74</td>
      <td>3.185000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>181</th>
      <td>Haellysu29</td>
      <td>12.73</td>
      <td>4.243333</td>
      <td>3</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Eoda93</td>
      <td>11.58</td>
      <td>3.860000</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
#group by item
groupby_item = raw_df.groupby(by="Item ID")
```


```python
#set up dataframe to store summary stats for each item
item_columns = [["Item ID","Item Name","Purchase Count","Item Price","Total Purchase Value"]]
item_stats = pd.DataFrame(columns=item_columns)
```


```python
#calculate summary stats for each item
for item,group in groupby_item:
    #find number of purchases of item
    num_purchases = len(group.index)

    #find total purchase value for item
    total_purchase_value = group["Price"].sum()

    #find item name
    name = group["SN"]
    name = name.iloc[0]

    #grab first value in specified column (all values should be the same for these columns)
    item_id = group["Item ID"]
    item_id = item_id.iloc[0]
    
    price = group["Price"]
    price = price.iloc[0]
    
    #create summary dataframe for this item and append it to dataframe for all items
    group_stats = {"Item ID":item_id,"Item Name":name,"Purchase Count":num_purchases,"Item Price":price,"Total Purchase Value":total_purchase_value}
    item_stats = item_stats.append(group_stats,ignore_index = True)
```


```python
#sort by num purchases and display data for top 5
sortby_num_purchases_df = item_stats.sort_values(by='Purchase Count',ascending=False)
top_5_num_purchases = sortby_num_purchases_df.iloc[0:5,:]
top_5_num_purchases
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>39</td>
      <td>Rinallorap73</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>84</td>
      <td>Koikirra25</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>31</td>
      <td>Marilsasya33</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>174</th>
      <td>175</td>
      <td>Yaristi64</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Aisur51</td>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
#sort by total purchase value and display data for top 5
sortby_purchase_value_df = item_stats.sort_values(by='Total Purchase Value',ascending=False)
top_5_purchase_value = sortby_purchase_value_df.iloc[0:5,:]
top_5_purchase_value
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>34</td>
      <td>Alallo58</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>115</td>
      <td>Undirrala66</td>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>32</td>
      <td>Saistyphos30</td>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>103</td>
      <td>Farenon57</td>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>107</td>
      <td>Frichossast75</td>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>
