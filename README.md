
### Heroes Of Pymoli Data Analysis
* Of the 576 players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%). Although there is a huge difference in the games's gender demographic, there is not much difference between the amout each gender spends on average. On the contrary female players spend a little more than male players(\$4.5 compared to \$4.0). We can further look into this gender gap and make a business strategy on attracting more female players.


* Item no. 19 is one of the five most popular items in the game and has its cost significanly lower than other 4 popular items(\$1.02 vs \$4.31(on average)for other items). Looking at the popularity we might consider increasing the price of this item.


* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).


-----


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

##### Getting dataframes in bordered table format


```python
%%HTML
<style type="text/css">
table.dataframe td, table.dataframe th {
    border: 1px  black solid !important;
  color: black !important;
}
```


<style type="text/css">
table.dataframe td, table.dataframe th {
    border: 1px  black solid !important;
  color: black !important;
}



```python
#purchase_data.head()
```

## Player Count

* Display the total number of players



```python
# Counting unique values from SN column to get total number of players
player_count = purchase_data["SN"].nunique()
#total_players
```


```python
total_players = pd.DataFrame([player_count], columns = ["Total Players"])
```

# Total number of players


```python
total_players
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Basic calculations to obtain number of unique items, average price, etc.


* a summary data frame to hold the results


```python
unique_items_count = purchase_data["Item ID"].nunique()
#unique_items_count
```


```python
avg_price = purchase_data["Price"].mean()
#avg_price
```


```python
purchase_count = purchase_data["Purchase ID"].nunique()
#purchase_count
```


```python
total_revenue = purchase_data["Price"].sum()
#total_revenue
```


```python
purchasing_summary = pd.DataFrame({"Number of Unique Items": [unique_items_count],"Average Price":[avg_price],\
                                  "Number of Purchases":[purchase_count],"Total Revenue":[total_revenue]})

purchasing_summary["Average Price"] = purchasing_summary["Average Price"].map("${:,.2f}".format)
purchasing_summary["Total Revenue"] = purchasing_summary["Total Revenue"].map("${:,.2f}".format)
```

# Game's purchasing data analysis


```python
purchasing_summary
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
gender_count = purchase_data.groupby("Gender")["SN"].nunique()
#gender_count
```


```python
percent_players = (gender_count/player_count)*100
#percent_players
```


```python
gender_demographics = pd.DataFrame({"Total Count":gender_count,"Percentage of Players":percent_players})
gender_demographics["Percentage of Players"] = gender_demographics["Percentage of Players"].map("{:,.2f}".format)
```

# Game's gender demographics


```python
gender_demographics.sort_values(by=['Total Count'],ascending = False)
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender

* a summary data frame to hold the results


```python
# Purchase data analysis by gender
purchase_count_by_gender = purchase_data.groupby("Gender")["Purchase ID"].nunique()
#purchase_count_by_gender
```


```python
avg_purchase_price_by_gender = purchase_data.groupby("Gender")["Price"].mean()
#avg_purchase_price_by_gender
```


```python
total_purchase_price_by_gender = purchase_data.groupby("Gender")["Price"].sum()
#total_purchase_price_by_gender
```


```python
avg_purchase_per_person = (total_purchase_price_by_gender/gender_count)
#avg_purchase_per_person
```


```python
gender_purchase_summary = pd.DataFrame({"Purchase Count":purchase_count_by_gender ,"Average Purchase Price":avg_purchase_price_by_gender,\
                                        "Total Purchase Value":total_purchase_price_by_gender,"Avg Total Purchase per Person":\
                                       avg_purchase_per_person})
gender_purchase_summary["Average Purchase Price"] = gender_purchase_summary["Average Purchase Price"].map("${:,.2f}".format)
gender_purchase_summary["Total Purchase Value"] = gender_purchase_summary["Total Purchase Value"].map("${:,.2f}".format)
gender_purchase_summary["Avg Total Purchase per Person"] = gender_purchase_summary["Avg Total Purchase per Person"].map("${:,.2f}".format)
```

# Game's purchasing analysis based on Gender


```python
gender_purchase_summary
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
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins.


* Calculate the numbers and percentages by age group


* a summary data frame to hold the results


```python
#Establish bins for ages
ages = [0,9,14,19,24,29,34,39,max(purchase_data["Age"])+1]
group_names = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

#Categorize the existing players using the age bins
purchase_data["Age Ranges"] = pd.cut(purchase_data["Age"],ages,labels=group_names)
```


```python
total_count_by_ages = purchase_data.groupby("Age Ranges")["SN"].nunique()
#total_count_by_ages
```


```python
percent_players_by_ages = (total_count_by_ages/player_count)*100
#percent_players_by_ages
```


```python
age_demographics = pd.DataFrame({"Total Count":total_count_by_ages, "Percentage of Players":percent_players_by_ages})
age_demographics["Percentage of Players"] = age_demographics["Percentage of Players"].map("{:,.2f}".format)
```

# Game's age demographics


```python
age_demographics
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
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
      <td>17</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* a summary data frame to hold the results


```python
# Purchase data analysis by age
purchase_count_by_age = purchase_data.groupby("Age Ranges")["Purchase ID"].nunique()
#purchase_count_by_age
```


```python
avg_purchase_price_by_age = purchase_data.groupby("Age Ranges")["Price"].mean()
#avg_purchase_price_by_age
```


```python
total_purchase_by_age = purchase_data.groupby("Age Ranges")["Price"].sum()
#total_purchase_by_age
```


```python
avg_purchase_pp_by_age = (total_purchase_by_age/total_count_by_ages)
```


```python
age_purchase_summary = pd.DataFrame({"Purchase Count":purchase_count_by_age,"Average Purchase Price":avg_purchase_price_by_age,\
                                     "Total Purchase Value":total_purchase_by_age, \
                                     "Avg Total Purchase per Person":avg_purchase_pp_by_age})
age_purchase_summary["Average Purchase Price"] = age_purchase_summary["Average Purchase Price"].map("${:,.2f}".format)
age_purchase_summary["Total Purchase Value"] = age_purchase_summary["Total Purchase Value"].map("${:,.2f}".format)
age_purchase_summary["Avg Total Purchase per Person"] = age_purchase_summary["Avg Total Purchase per Person"].map("${:,.2f}".format)
```

# Game's purchasing analysis based on Age


```python
age_purchase_summary
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
      <th>Avg Total Purchase per Person</th>
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
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
total_purchase_by_spender = purchase_data.groupby("SN")["Price"].sum()
#total_purchase_by_spender
```


```python
purchase_count_by_spender = purchase_data.groupby("SN")["Purchase ID"].count()
#purchase_count_by_spender
```


```python
avg_purchase_by_spender = purchase_data.groupby("SN")["Price"].mean()
#avg_purchase_by_spender
```


```python
spender_data_summary = pd.DataFrame({"Purchase Count":purchase_count_by_spender,\
                                     "Average Purchase Price":avg_purchase_by_spender,\
                                     "Total Purchase Value":total_purchase_by_spender})
spender_data_summary["Average Purchase Price"] = spender_data_summary["Average Purchase Price"].map("${:,.2f}".format)
spender_data_summary["Total Purchase Value"] = spender_data_summary["Total Purchase Value"].map("{:,.2f}".format)
spender_data_summary["Total Purchase Value"] = spender_data_summary["Total Purchase Value"].astype(str).astype(float)
```


```python
spender_data_summary_fmt = spender_data_summary.sort_values(by=["Total Purchase Value"],ascending=False)
```


```python
spender_data_summary_fmt["Total Purchase Value"] = spender_data_summary_fmt["Total Purchase Value"].astype(str).astype(float).map("${:,.2f}".format)
```

# Top 5 Spenders in the game


```python
spender_data_summary_fmt.head(5)
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
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* a summary data frame to hold the results


```python
purchase_count_by_item = purchase_data.groupby(["Item ID","Item Name"])["Purchase ID"].count()
#purchase_count_by_item
```


```python
item_price_by_item = purchase_data.groupby(["Item ID","Item Name"])["Price"].unique()
```


```python
total_purchase_by_item = purchase_data.groupby(["Item ID","Item Name"])["Price"].sum()
#total_purchase_by_item
```


```python
popular_items = pd.DataFrame({"Item Price":item_price_by_item,"Purchase Count":purchase_count_by_item,\
                              "Total Purchase Value":total_purchase_by_item})
```


```python
#sorting
popular_items_fmt = popular_items.sort_values(by=["Purchase Count"],ascending = False)
```


```python
popular_items_fmt["Item Price"] = popular_items_fmt["Item Price"].astype(float).map("${:,.2f}".format)
popular_items_fmt["Total Purchase Value"] = popular_items_fmt["Total Purchase Value"].astype(str).astype(float).map("${:,.2f}".format)
```


```python
popular_items_fmt.head()
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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>$4.23</td>
      <td>12</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>$4.58</td>
      <td>9</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>$3.53</td>
      <td>9</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>$4.90</td>
      <td>9</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>$1.02</td>
      <td>8</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



# 5 most popular items in the game


```python
popular_items_fmt.head(5)
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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>$4.23</td>
      <td>12</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>$4.58</td>
      <td>9</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>$3.53</td>
      <td>9</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>$4.90</td>
      <td>9</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>$1.02</td>
      <td>8</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order

# 5 most profitable items in the game


```python
profitable_items_fmt = popular_items.sort_values(by=["Total Purchase Value"],ascending = False)
```


```python
profitable_items_fmt["Item Price"] = profitable_items_fmt["Item Price"].astype(float).map("${:,.2f}".format)
profitable_items_fmt["Total Purchase Value"] = profitable_items_fmt["Total Purchase Value"].astype(str).astype(float).map("${:,.2f}".format)
```


```python
profitable_items_fmt.head(5)
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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>$4.23</td>
      <td>12</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>$4.90</td>
      <td>9</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>$4.58</td>
      <td>9</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>$4.88</td>
      <td>8</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$4.35</td>
      <td>8</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


