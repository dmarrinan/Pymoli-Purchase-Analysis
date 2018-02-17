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
Observations in Data for File 1¶
1) The average item's purchase price was highest for other/nongender users. The average item's purchase price for men was greater than that for women.

2) The largest portion of users were between the ages of 20-24. Over 40% of users fell in this age range.

3) The most profitable items were bought fewer times for a greater cost than the most popular items.

In [1]:

#import relevant libraries
import pandas as pd
import json
In [2]:

#set names of files to read in 
file_name = "purchase_data.json"
In [3]:

raw_df = pd.read_json(file_name, orient="records")
raw_df.head()
Out[3]:
Age	Gender	Item ID	Item Name	Price	SN
0	38	Male	165	Bone Crushing Silver Skewer	3.37	Aelalis34
1	21	Male	119	Stormbringer, Dark Blade of Ending Misery	2.32	Eolo46
2	34	Male	174	Primitive Blade	2.46	Assastnya25
3	21	Male	92	Final Critic	1.36	Pheusrical25
4	23	Male	63	Stormfury Mace	1.27	Aela59
Player Count
In [4]:

#calculate number of players and print results
num_users_df = pd.DataFrame()
num_users = raw_df["SN"].nunique()
num_users_df["Total Players"] = num_users
num_users
Out[4]:
573
Purchasing Analysis (Total)
In [5]:

#set up data frame to store purchasing analysis
purchasing_analysis = pd.DataFrame()
In [6]:

#find number of unique items
num_unique_items = raw_df["Item Name"].nunique()
purchasing_analysis["Number of Unique Items"] = [num_unique_items]
In [7]:

#find average price of items
average_price = raw_df["Price"].mean()
purchasing_analysis["Average Price"] = [average_price] 
In [8]:

#find the total number of purchases
num_purchases = len(raw_df.index)
purchasing_analysis["Number of Purchases"] = [num_purchases]
In [9]:

#find the total revenue
total_revenue = raw_df["Price"].sum()
purchasing_analysis["Total Revenue"] = [total_revenue] 
In [10]:

#display results
purchasing_analysis
Out[10]:
Number of Unique Items	Average Price	Number of Purchases	Total Revenue
0	179	2.931192	780	2286.33
Gender Demographics
In [11]:

#create gender demographics dataframe
gender_demographics = pd.DataFrame(columns = ["Percentage of Players","Total Count"],index = ["Male","Female","Other/Non-Disclosed"])
In [12]:

#filter by gender to get df full of male users
male_demographics = pd.DataFrame()
male_df = raw_df.loc[raw_df["Gender"]=="Male"]
In [13]:

#filter by gender to get df full of female users
female_df = raw_df.loc[raw_df["Gender"]=="Female"]
In [14]:

#get other/Non-Disclosed users
other_gender_filter = (raw_df["Gender"]!="Male")&(raw_df["Gender"]!="Female")
other_gender_df = raw_df.loc[other_gender_filter]
In [15]:

#calculate number of males
num_males = male_df["SN"].nunique()
gender_demographics.loc["Male"]["Total Count"] = num_males
num_males
Out[15]:
465
In [16]:

#calculate number of females
num_females = female_df["SN"].nunique()
gender_demographics.loc["Female"]["Total Count"] = num_females
In [17]:

#calculate number of users that are of other/non-disclosed gender
num_other_gender = other_gender_df["SN"].nunique()
gender_demographics.loc["Other/Non-Disclosed"]["Total Count"] = num_other_gender
In [18]:

#calculate percentage of users that are male
percent_male = num_males/num_users*100
gender_demographics.loc["Male"]["Percentage of Players"] = percent_male
percent_male
Out[18]:
81.15183246073299
In [19]:

#calculate percentage of users that are female
percent_female = num_females/num_users*100
gender_demographics.loc["Female"]["Percentage of Players"] = percent_female
In [20]:

#calculate percntage of users that are of other/non-disclosed gender
percent_other_gender = num_other_gender/num_users*100
gender_demographics.loc["Other/Non-Disclosed"]["Percentage of Players"] = percent_other_gender
In [21]:

#display gender demographics
gender_demographics
Out[21]:
Percentage of Players	Total Count
Male	81.1518	465
Female	17.452	100
Other/Non-Disclosed	1.39616	8
Purchasing Analysis (Gender)
In [22]:

#create purchasing analysis gender dataframe
purchase_gender_index = ["Male","Female","Other/Non-Disclosed"]
purchase_gender_columns = ["Purchase Count","Average Purchase Price","Total Purchase Value", "Normalized Totals"]
purchasing_analysis_gender = pd.DataFrame(index=purchase_gender_index,columns = purchase_gender_columns)
purchasing_analysis_gender.index
Out[22]:
Index(['Male', 'Female', 'Other/Non-Disclosed'], dtype='object')
In [23]:

#calculate number of purchases by male users
num_purchases_male = len(male_df.index)
purchasing_analysis_gender.loc["Male"]["Purchase Count"] = num_purchases_male
In [24]:

#calculate number of purchases by female users
num_purchases_female = len(female_df.index)
purchasing_analysis_gender.loc["Female"]["Purchase Count"] =num_purchases_female
In [25]:

#calculate number of purchases by users of other/non-disclosed gender
num_purchases_other_gender = len(other_gender_df.index)
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Purchase Count"] =num_purchases_other_gender
In [26]:

#find average purchase price for items by male users
average_price_male = male_df["Price"].mean()
purchasing_analysis_gender.loc["Male"]["Average Purchase Price"] = average_price_male
In [27]:

#find average purchase price for items by female users
average_price_female = female_df["Price"].mean()
purchasing_analysis_gender.loc["Female"]["Average Purchase Price"] = average_price_female
In [28]:

#find average purchase price for items by users with other/nonspecified genders
average_price_other_gender = other_gender_df["Price"].mean()
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Average Purchase Price"] = average_price_other_gender
In [29]:

#find total purchase value for male users
total_value_male = male_df["Price"].sum()
purchasing_analysis_gender.loc["Male"]["Total Purchase Value"] = total_value_male
In [30]:

#find total purchase value for female users
total_value_female = female_df["Price"].sum()
purchasing_analysis_gender.loc["Female"]["Total Purchase Value"] = total_value_female
In [31]:

#find total purchase value for male users
total_value_other_gender = other_gender_df["Price"].sum()
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Total Purchase Value"] = total_value_other_gender
In [32]:

#find percentage of total purchase value by male users
purchasing_analysis_gender.loc["Male"]["Normalized Totals"] = total_value_male/num_males
In [33]:

#find percentage of total purchase value by female users
purchasing_analysis_gender.loc["Female"]["Normalized Totals"] = total_value_female/num_females
In [34]:

#find percentage of total purchase value by male users
purchasing_analysis_gender.loc["Other/Non-Disclosed"]["Normalized Totals"] = total_value_other_gender/num_other_gender
In [35]:

#display purchasing analysis by gender
purchasing_analysis_gender
Out[35]:
Purchase Count	Average Purchase Price	Total Purchase Value	Normalized Totals
Male	633	2.95052	1867.68	4.01652
Female	136	2.81551	382.91	3.8291
Other/Non-Disclosed	11	3.24909	35.74	4.4675
Age Demographics
In [36]:

#find info on age distribution
raw_df["Age"].describe()
Out[36]:
count    780.000000
mean      22.729487
std        6.930604
min        7.000000
25%       19.000000
50%       22.000000
75%       25.000000
max       45.000000
Name: Age, dtype: float64
In [37]:

#set age bins
if raw_df["Age"].max() == 40:
    bins = [0, 10, 15, 20, 25,30,35,40]
    bin_index = ["<11","11-15","16-20","21-25","26-30","31-35","36-40"]
else:
    bins = [0, 10, 15, 20, 25,30,35,40,raw_df["Age"].max()]
    bin_index = ["<11","11-15","16-20","21-25","26-30","31-35","36-40","41+"]
In [38]:

#bin by age
df_age_bins = pd.cut(raw_df["Age"],bins)
In [39]:

#add age bin column to data frame
age_df = raw_df
age_df["Age Bin"] = df_age_bins
In [40]:

#group by age bin
groupby_age = age_df.groupby(by="Age Bin")
In [41]:

num_users_age_bin = []
age_demo_columns = ["Percentage of Players","Total Count"]
age_demographics = pd.DataFrame(columns = age_demo_columns,index=bin_index)
​
for age_bin,group in groupby_age:
    #num_per_age = group["SN"].value_counts()
    num_users_age_bin.append(group["SN"].nunique())
​
age_demographics.loc[:,"Total Count"] = num_users_age_bin
age_demographics["Percentage of Players"] = age_demographics["Total Count"]/num_users*100
age_demographics
Out[41]:
Percentage of Players	Total Count
<11	3.839442	22
11-15	9.424084	54
16-20	24.258290	139
21-25	40.837696	234
26-30	9.075044	52
31-35	7.678883	44
36-40	4.363002	25
41+	0.523560	3
In [42]:

#calculate stats for each age group
groupby_age_stats = pd.DataFrame({"Average Price":groupby_age["Price"].mean()})
In [43]:

groupby_age_stats["Total Purchase Value"] = groupby_age["Price"].sum()
In [44]:

#add number of purchases to sum
num_purchases_age = []
for age_bin,group in groupby_age:
    num_purchases_age.append(len(group.index))
    
groupby_age_stats["Number of Purchases"] = num_purchases_age
In [45]:

#normalized totals
purchase_value_list = groupby_age_stats["Total Purchase Value"].values
total_count_list = age_demographics["Total Count"].values
groupby_age_stats["Normalized Totals"] = purchase_value_list/total_count_list
groupby_age_stats
Out[45]:
Average Price	Total Purchase Value	Number of Purchases	Normalized Totals
Age Bin				
(0, 10]	3.019375	96.62	32	4.391818
(10, 15]	2.873718	224.15	78	4.150926
(15, 20]	2.873587	528.74	184	3.803885
(20, 25]	2.959377	902.61	305	3.857308
(25, 30]	2.892368	219.82	76	4.227308
(30, 35]	3.073448	178.26	58	4.051364
(35, 40]	2.897500	127.49	44	5.099600
(40, 45]	2.880000	8.64	3	2.880000
Top Spenders
In [46]:

#groupby user
groupby_user = raw_df.groupby(by="SN")   
In [47]:

#create data frame to store summary statistics for user
user_columns = [["User", "Total Purchase Value","Average Purchase Price","Number of Purchases"]]
user_stats = pd.DataFrame(columns=user_columns)
user_stats
Out[47]:
User	Total Purchase Value	Average Purchase Price	Number of Purchases
In [48]:

#calculate user statistics
for user,group in groupby_user:
    sum_price_user = group["Price"].sum()
    average_price_user = group["Price"].mean()
    purchase_count_user = len(group.index)
    ##############################################################
    #how to save stats
    group_stats = {"User":user,"Total Purchase Value":sum_price_user,"Average Purchase Price":average_price_user,"Number of Purchases":purchase_count_user}
    user_stats = user_stats.append(group_stats,ignore_index=True)
In [49]:

#sort users by total purchase value and display top 5 users
sortby_purchase_value_df = user_stats.sort_values(by='Total Purchase Value',ascending=False)
top_5_purchase_value = sortby_purchase_value_df.iloc[0:5,:]
top_5_purchase_value
Out[49]:
User	Total Purchase Value	Average Purchase Price	Number of Purchases
538	Undirrala66	17.06	3.412000	5
428	Saedue76	13.56	3.390000	4
354	Mindimnya67	12.74	3.185000	4
181	Haellysu29	12.73	4.243333	3
120	Eoda93	11.58	3.860000	3
Most Popular Items
In [50]:

#group by item
groupby_item = raw_df.groupby(by="Item ID")
In [51]:

#set up dataframe to store summary stats for each item
item_columns = [["Item ID","Item Name","Purchase Count","Item Price","Total Purchase Value"]]
item_stats = pd.DataFrame(columns=item_columns)
In [52]:

#calculate summary stats for each item
for item,group in groupby_item:
    #find number of purchases of item
    num_purchases = len(group.index)
​
    #find total purchase value for item
    total_purchase_value = group["Price"].sum()
​
    #find item name
    name = group["SN"]
    name = name.iloc[0]
​
    #grab first value in specified column (all values should be the same for these columns)
    item_id = group["Item ID"]
    item_id = item_id.iloc[0]
    
    price = group["Price"]
    price = price.iloc[0]
    
    #create summary dataframe for this item and append it to dataframe for all items
    group_stats = {"Item ID":item_id,"Item Name":name,"Purchase Count":num_purchases,"Item Price":price,"Total Purchase Value":total_purchase_value}
    item_stats = item_stats.append(group_stats,ignore_index = True)
In [53]:

#sort by num purchases and display data for top 5
sortby_num_purchases_df = item_stats.sort_values(by='Purchase Count',ascending=False)
top_5_num_purchases = sortby_num_purchases_df.iloc[0:5,:]
top_5_num_purchases
Out[53]:
Item ID	Item Name	Purchase Count	Item Price	Total Purchase Value
39	39	Rinallorap73	11	2.35	25.85
84	84	Koikirra25	11	2.23	24.53
31	31	Marilsasya33	9	2.07	18.63
174	175	Yaristi64	9	1.24	11.16
13	13	Aisur51	9	1.49	13.41
Most Profitable Items
In [54]:

#sort by total purchase value and display data for top 5
sortby_purchase_value_df = item_stats.sort_values(by='Total Purchase Value',ascending=False)
top_5_purchase_value = sortby_purchase_value_df.iloc[0:5,:]
top_5_purchase_value
Out[54]:
Item ID	Item Name	Purchase Count	Item Price	Total Purchase Value
34	34	Alallo58	9	4.14	37.26
115	115	Undirrala66	7	4.25	29.75
32	32	Saistyphos30	6	4.95	29.70
103	103	Farenon57	6	4.87	29.22
107	107	Frichossast75	8	3.61	28.88