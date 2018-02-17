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


## Load Dataframe 

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

<div>
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

<div>
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

<div>
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
count    780.000000 </br>
mean      22.729487 </br>
std        6.930604 </br>
min        7.000000 </br>
25%       19.000000 </br>
50%       22.000000 </br>
75%       25.000000 </br>
max       45.000000 </br>

<div>
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

## Age Purchasing Analysis
<div>
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

<div>
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

<div>
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
