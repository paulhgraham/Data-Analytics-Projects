
Pandas-HW/README.md

Congratulations! After a lot of hard work in the data munging mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli. 

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

Your final report should include each of the following:

##Player Count
-Total Number of Players

##Purchasing Analysis (Total)
-Number of Unique Items
-Average Purchase Price
-Total Number of Purchases
-Total Revenue

##Gender Demographics
-Percentage and Count of Male Players
-Percentage and Count of Female Players
-Percentage and Count of Other / Non-Disclosed

##Purchasing Analysis (Gender) 

The below each broken by gender
-Purchase Count
-Average Purchase Price
-Total Purchase Value
-Normalized Totals

##Age Demographics

The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
-Purchase Count
-Average Purchase Price
-Total Purchase Value
-Normalized Totals

##Top Spenders

Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
-SN
-Purchase Count
-Average Purchase Price
-Total Purchase Value

##Most Popular Items

Identify the 5 most popular items by purchase count, then list (in a table):
-Item ID
-Item Name
-Purchase Count
-Item Price
-Total Purchase Value

##Most Profitable Items

Identify the 5 most profitable items by total purchase value, then list (in a table):
-Item ID
-Item Name
-Purchase Count
-Item Price
-Total Purchase Value

##As final considerations:

Your script must work for both data-sets given.
You must use the Pandas Library and the Jupyter Notebook.
You must submit a link to your Jupyter Notebook with the viewable Data Frames. 
You must include an exported markdown version of your Notebook called  README.md in your GitHub repository.

You must include a written description of three observable trends based on the data. 
See Example Solution for a reference on expected format.


```python
import pandas as pd
from sklearn import preprocessing

json_path = 'purchase_data.json'

pymoli_sales = pd.read_json(json_path, orient='records')
#orient = records turns into pandas 
```

Player Count


```python
player_demographics = pymoli_sales.loc[:,['Age','Gender','SN']]
player_demographics.count()
```




    Age       780
    Gender    780
    SN        780
    dtype: int64



Player Count (Duplicate Check)


```python
#duplcate check: player_demographics.duplicated()

duplicates = player_demographics.drop_duplicates()
numplayers = duplicates.count()[0]
pd.DataFrame({'Total Players':[numplayers]})
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Total)


```python
#Number of Unique Items
unique_items = pymoli_sales['Item Name'].nunique()

#Average Purchase Price
total_revenue = pymoli_sales['Price'].sum()
mean_price = "${0:,.2f}".format(total_revenue / numplayers)

#Total Number of Purchases
num_purchases = pymoli_sales['Price'].sum() 

#Total Revenue 
purchasing_analysis = pd.DataFrame({"Unique Items":[unique_items],"Average Purchase Price":[mean_price], "Number of Purchases":[num_purchases],"Total Revenue":[total_revenue]})
purchasing_analysis

#########$ to total revenue 
#########not in alphabetical order 
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
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
      <th>Unique Items</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$3.99</td>
      <td>2286.33</td>
      <td>2286.33</td>
      <td>179</td>
    </tr>
  </tbody>
</table>
</div>



Gender Demographics


```python
#Percentage of Players 
normed = pymoli_sales.groupby(['Gender','SN']).count().reset_index()['Gender'].value_counts(normalize=True)*100

#Total Count 
absolute = pymoli_sales.groupby(['Gender','SN']).count().reset_index()['Gender'].value_counts(normalize=False)
gender_demographics = pd.concat([normed,absolute], axis=1)

#Rename column one and two
gender_demographics.columns.values[0] = 'Percentage of Players'
gender_demographics.columns.values[1] = 'Total Count'

gender_demographics
#######format percent 
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.151832</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.396161</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Gender) 


```python
purchasing_analysis_gender = pd.DataFrame(pymoli_sales['Gender'].value_counts())

#The below each broken by gender
#Purchase Count
purchasing_analysis_gender['Purchase Count'] = pymoli_sales['Gender'].value_counts()

#Average Purchase Price
purchasing_analysis_gender['Average Purchase Price'] = pymoli_sales.groupby(["Gender"])["Price"].mean()

#Total Purchase Value
purchasing_analysis_gender['Total Purchase Value'] = pymoli_sales.groupby(["Gender"])["Price"].sum()

#Normalized Totals
purchasing_analysis_gender['Normalized Totals'] = purchasing_analysis_gender['Total Purchase Value'] / purchasing_analysis_gender['Purchase Count']
pd.concat([normed,absolute], axis=1)

purchasing_analysis_gender.drop(['Gender'], axis=1)
#########$ to average purchase price 
#########$ to normalized totals
#########$ to total purchase value 
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
      <td>2.950521</td>
      <td>1867.68</td>
      <td>2.950521</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.815515</td>
      <td>382.91</td>
      <td>2.815515</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>3.249091</td>
    </tr>
  </tbody>
</table>
</div>



Age Demographics


```python
#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
player_demographics = pd.DataFrame(pymoli_sales)

age_bins = [0,9.99,14.99,19.99,24.99,29.99,34.99,39.99,40]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
player_demographics['Age Ranges'] = pd.cut(player_demographics["Age"], age_bins, labels = group_names)

#Percentage of players column and Total count
pdemo = player_demographics.groupby(['Age Ranges']).agg([lambda x: x.sum()/ player_demographics['Price'].sum()*100,'count'])['Price']

#Rename columns
pdemo.columns = ['Percentage of Players', 'Total Count']
pdemo
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Ranges</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.650392</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.240420</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>16.901322</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>42.809656</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.197574</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.627364</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.222343</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.973031</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Age)


```python
#Calculate on age; sum, mean, count 
purchasing_analysis_age = pymoli_sales.groupby('Age Ranges').agg(['sum','mean','count'])['Price']

age_bins = [0,9.99,14.99,19.99,24.99,29.99,34.99,39.99,40]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']

#Rename columns
purchasing_analysis_age.columns = ['Total Purchase Value','Average Purchase Price','Purchase Count']

#Normalized Totals
purchasing_analysis_age['Normalized Totals'] = purchasing_analysis_age["Total Purchase Value"] / purchasing_analysis_age['Purchase Count']
purchasing_analysis_age
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
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Ranges</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>83.46</td>
      <td>2.980714</td>
      <td>28</td>
      <td>2.980714</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>96.95</td>
      <td>2.770000</td>
      <td>35</td>
      <td>2.770000</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>386.42</td>
      <td>2.905414</td>
      <td>133</td>
      <td>2.905414</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>978.77</td>
      <td>2.913006</td>
      <td>336</td>
      <td>2.913006</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>370.33</td>
      <td>2.962640</td>
      <td>125</td>
      <td>2.962640</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>197.25</td>
      <td>3.082031</td>
      <td>64</td>
      <td>3.082031</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>119.40</td>
      <td>2.842857</td>
      <td>42</td>
      <td>2.842857</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>45.11</td>
      <td>3.222143</td>
      <td>14</td>
      <td>3.222143</td>
    </tr>
  </tbody>
</table>
</div>



Top Spenders


```python
#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
top_spenders = pd.DataFrame(pymoli_sales['SN'].value_counts())

#Purchase Count
top_spenders['Purchase Count'] = pymoli_sales.groupby(["SN"])["Price"].count()

#Average Purchase Price
top_spenders['Average Purchase Price'] = pymoli_sales.groupby(["SN"])["Price"].mean()

#Total Purchase Value
top_spenders['Total Purchase Value'] = pymoli_sales.groupby(["SN"])["Price"].sum()

top_spenders.nlargest(5,'Purchase Count').drop(['SN'], axis=1)

#########$ to average purchase price 
#########$ to total purchase value 
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>3.4120</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.3900</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.1850</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Hailaphos89</th>
      <td>4</td>
      <td>1.4675</td>
      <td>5.87</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>2.4925</td>
      <td>9.97</td>
    </tr>
  </tbody>
</table>
</div>



Most Popular Items


```python
#Identify the 5 most popular items by purchase count, then list (in a table):
most_popular_items = pd.DataFrame(pymoli_sales['Item Name'].value_counts())

#Purchase Count
most_popular_items['Purchase Count'] = pymoli_sales['Item Name'].value_counts()

#Item Purchase Price
pymoli_sales['Item Price'] = pymoli_sales["Price"]

#Total Purchase Value
most_popular_items['Total Purchase Value'] = pymoli_sales.groupby(["Item Name"])["Price"].sum()

mp = most_popular_items.nlargest(5,'Purchase Count').drop(['Item Name'], axis=1)
mp['Item Price'] = mp.index.map(lambda x: price_dict.get(x))
mp
######Error on Final Critic data (total purchase value) 
#########reorder columns
#########$ to item price 
#########$ to total purchase value 
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
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>38.60</td>
      <td>4.62</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>24.53</td>
      <td>2.23</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>25.85</td>
      <td>2.35</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>34.65</td>
      <td>4.15</td>
    </tr>
    <tr>
      <th>Serenity</th>
      <td>9</td>
      <td>13.41</td>
      <td>1.49</td>
    </tr>
  </tbody>
</table>
</div>



Most Profitable Items


```python
##Most Profitable Items
#Identify the 5 most popular items by purchase count, then list (in a table):
most_profitable_items = pd.DataFrame(pymoli_sales['Item Name'].value_counts())

#Item Name
most_profitable_items['Item Name'] = pymoli_sales.groupby(["Item ID"]).max()['Price']

#Item Purchase Price
most_profitable_items['Item Price'] = pymoli_sales.groupby(["Item Name"]).max()['Price']

#Total Purchase Value
most_profitable_items['Total Purchase Value'] = pymoli_sales.groupby(["Item Name"])["Price"].sum()

#Toal Purchase Count
most_profitable_items['Purchase Count'] = pymoli_sales['Item Name'].value_counts()

most_profitable_items.head().drop(['Item Name'], axis=1)
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
      <th>Item Price</th>
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>4.62</td>
      <td>38.60</td>
      <td>14</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>2.23</td>
      <td>24.53</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>2.35</td>
      <td>25.85</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>4.15</td>
      <td>34.65</td>
      <td>10</td>
    </tr>
    <tr>
      <th>Serenity</th>
      <td>1.49</td>
      <td>13.41</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>


