
### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%). 

### Luke's Analysis
* Even though male players make up the vast majority of players, both genders and those who don't identify tend to spend close to the same total average amount. There isn't a large disparity between the sexes when it comes to average spending.
* Far and away, the most spending by age group is 20-24, followed at a distance by 15-19. The other age groups are far removed from either of these groups.
* If you want to be cool, buy an Oathbringer. EVerybody's doing it. Lisosia93 probably bought one...
-----

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

## Player Count

* Display the total number of players



```python
player_count = purchase_data['SN'].nunique()
pd.DataFrame([player_count], columns = ["Total Players"])
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

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
unique_items = purchase_data['Item ID'].nunique()
avg_price = (purchase_data['Price'].sum()/purchase_data['Price'].count()).round(2)
avg_price
tot_purchase = purchase_data['Item ID'].count()
tot_purchase
purchase_data.dropna()

tot_revenue = purchase_data['Price'].sum()
tot_revenue

purch_analysis = pd.DataFrame([{"Number of Unique Items": unique_items,
                                "Number of Purchases": tot_purchase,
                                "Average Price": avg_price,
                                "Total Revenue": tot_revenue}])

purch_analysis
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.05</td>
      <td>780</td>
      <td>183</td>
      <td>2379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
total_gender = purchase_data["Gender"].count()
male_total = purchase_data["Gender"].value_counts()['Male']
female_total = purchase_data["Gender"].value_counts()['Female']
other_total = total_gender - male_total - female_total

male_percent = (male_total / total_gender) * 100
female_percent = (female_total / total_gender) * 100
other_percent = (other_total / total_gender) * 100

gender_dem = pd.DataFrame({"":["Male", "Female", "Other/Non-Disclosed"],
                            "Percentage of Players":[male_percent, female_percent, other_percent],
                          "Total Count":[male_total, female_total, other_total]})
gender_dem["Percentage of Players"] = gender_dem["Percentage of Players"].map("{:.2f}%".format)
gender_dem = gender_dem.set_index('')
gender_dem
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
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>83.59%</td>
      <td>652</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>14.49%</td>
      <td>113</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.92%</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, etc. by gender


* For normalized purchasing, divide total purchase value by purchase count, by gender


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
male_purchase = purchase_data["Gender"].value_counts()['Male']
female_purchase = purchase_data["Gender"].value_counts()['Female']
other_purchase = total_gender - male_total - female_total

male_avg = purchase_data[purchase_data["Gender"] == 'Male']['Price'].mean()
female_avg = purchase_data[purchase_data["Gender"] =='Female']['Price'].mean()
other_avg = purchase_data[purchase_data["Gender"] =='Other / Non-Disclosed']['Price'].mean()

male_sum = purchase_data[purchase_data["Gender"] == 'Male']['Price'].sum()
female_sum = purchase_data[purchase_data["Gender"] =='Female']['Price'].sum()
other_sum = purchase_data[purchase_data["Gender"] =='Other / Non-Disclosed']['Price'].sum()

male_norm = male_sum/male_total
female_norm = female_sum/female_total
other_norm = other_sum/other_total

gender_dem = pd.DataFrame({"Gender":["Male", "Female", "Other / Non-Disclosed"],
                            "Purchase Count":[male_purchase, female_purchase, other_purchase],
                           "Average Purchase Price":[male_avg, female_avg, other_avg],
                           "Total Purchase Value":[male_norm, female_norm, other_norm],
                          "Normalized Totals": [male_norm,female_norm, other_norm]})
gender_dem["Average Purchase Price"] = gender_dem["Average Purchase Price"].map("${:.2f}".format)
gender_dem["Total Purchase Value"] = gender_dem["Total Purchase Value"].map("${:.2f}".format)
gender_dem["Normalized Totals"] = gender_dem["Normalized Totals"].map("${:.2f}".format)
gender_dem
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
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>652</td>
      <td>$3.02</td>
      <td>$3.02</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>113</td>
      <td>$3.20</td>
      <td>$3.20</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>15</td>
      <td>$3.35</td>
      <td>$3.35</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
bin_df = purchase_data.copy()
bin_df["Age Group"] = pd.cut(bin_df["Age"],age_bins, labels=group_names)
group_bin = bin_df.groupby(["Age Group"])

bin_count = group_bin["SN"].count()
count_tot = purchase_data["SN"].count()
percentage = (bin_count/count_tot)*100
percentage

age_percentage = pd.DataFrame({"Percentage of Players": percentage,
                               "Total Count":bin_count,})
age_percentage["Percentage of Players"] = age_percentage["Percentage of Players"].map("{:.2f}%".format)
age_percentage
                  
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
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>2.95%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.59%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>46.79%</td>
      <td>365</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>12.95%</td>
      <td>101</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>9.36%</td>
      <td>73</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.26%</td>
      <td>41</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.67%</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, etc. in the table below


* Calculate Normalized Purchasing


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
age_bins = [0,10,15,20,25,30,35,40,200]
group_names = ['Under 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', 'Over 40']
bin_df = purchase_data.copy()
bin_df["Age Group"] = pd.cut(bin_df["Age"],age_bins, labels=group_names)
bin_column = pd.cut(bin_df["Age"], age_bins, labels = group_names)
group_bin = bin_df.groupby(["Age Group"])


bin_purch_count = group_bin["Age"].count()
bin_avg_price = group_bin["Price"].mean()
bin_avg_total = group_bin["Price"].sum()

bin_dup = purchase_data.drop_duplicates(subset="SN", keep="first")
bin_dup["Age Group"] = pd.cut(bin_df["Age"],age_bins, labels=group_names)
bin_dup = bin_dup.groupby(["Age Group"])


bin_norm = (bin_df["Price"].sum()/bin_dup["SN"].count())

age_purchase = pd.DataFrame({"Purchase Count":bin_purch_count,
                           "Average Purchase Price":bin_avg_price,
                           "Total Purchase Value":bin_avg_total,
                          "Normalized Totals": bin_norm})
age_purchase["Average Purchase Price"] = age_purchase["Average Purchase Price"].map("${:.2f}".format)
age_purchase["Total Purchase Value"] = age_purchase["Total Purchase Value"].map("${:.2f}".format)
age_purchase["Normalized Totals"] = age_purchase["Normalized Totals"].map("${:.2f}".format)
age_purchase = age_purchase[["Purchase Count","Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_purchase.head(10)
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:14: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    




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
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Under 10</th>
      <td>32</td>
      <td>$3.40</td>
      <td>$108.96</td>
      <td>$99.16</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>54</td>
      <td>$2.90</td>
      <td>$156.60</td>
      <td>$58.04</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>200</td>
      <td>$3.11</td>
      <td>$621.56</td>
      <td>$15.87</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>325</td>
      <td>$3.02</td>
      <td>$981.64</td>
      <td>$10.26</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>77</td>
      <td>$2.88</td>
      <td>$221.42</td>
      <td>$40.34</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>52</td>
      <td>$2.99</td>
      <td>$155.71</td>
      <td>$64.32</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>33</td>
      <td>$3.40</td>
      <td>$112.35</td>
      <td>$91.53</td>
    </tr>
    <tr>
      <th>Over 40</th>
      <td>7</td>
      <td>$3.08</td>
      <td>$21.53</td>
      <td>$339.97</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
groupSN=purchase_data.groupby(["SN"])
groupSNtotal=purchase_data.groupby(["SN"])['Price'].sum()
groupIDtotal=purchase_data.groupby(["SN"])['Price'].count()

groupSNavg= round(groupSNtotal/groupIDtotal,2)

top_spender=pd.DataFrame({"Purchase Count": groupIDtotal,
                          "Average Purchase Price":groupSNavg,
                          "Total Purchase Value":groupSNtotal})


top_spender= top_spender.sort_values("Total Purchase Value", ascending=False)
top_spender= top_spender[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
top_spender["Average Purchase Price"] = top_spender["Average Purchase Price"].map("${:.2f}".format)
top_spender["Total Purchase Value"] = top_spender["Total Purchase Value"].map("${:.2f}".format)
top_spender.reset_index(inplace=True)
top_spender.round(2)
top_spender.head()
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
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lisosia93</td>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Idastidru52</td>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chamjask73</td>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Iral74</td>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iskadarya95</td>
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


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
premergeone = purchase_data.groupby("Item Name").sum().reset_index()
premergetwo = purchase_data.groupby("Item ID").sum().reset_index()
premergethree = purchase_data.groupby("Item Name").count().reset_index()


mergeone = pd.merge(premergeone, premergetwo, on="Price")
mergetwo = pd.merge(premergethree, mergeone, on="Item Name")


mergetwo["Gender"] = (mergetwo["Price_y"]/mergetwo["Item ID"]).round(2)

mergetwo_renamed = mergetwo.rename(columns={"Age": "Purchase Count", "Gender": "Item Price", "Item ID": "null", "Price_y": "Total Purchase Value", "Item ID_y": "Item ID"})

#grab columns we are looking for
clean_df = mergetwo_renamed[["Item ID", "Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

prefinal_df = clean_df.set_index(['Item Name', 'Item ID'])
popular_items_final = prefinal_df.sort_values('Purchase Count', ascending=False).head(5)
popular_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level0_row0" class="row_heading level0 row0" >Oathbreaker, Last Hope of the Breaking Storm</th> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level1_row0" class="row_heading level1 row0" >178</th> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row0_col0" class="data row0 col0" >12</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row0_col1" class="data row0 col1" >$4.23</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row0_col2" class="data row0 col2" >$50.76</td> 
    </tr>    <tr> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level0_row1" class="row_heading level0 row1" >Nirvana</th> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level1_row1" class="row_heading level1 row1" >82</th> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row1_col0" class="data row1 col0" >9</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row1_col1" class="data row1 col1" >$4.90</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row1_col2" class="data row1 col2" >$44.10</td> 
    </tr>    <tr> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level0_row2" class="row_heading level0 row2" >Fiery Glass Crusader</th> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level1_row2" class="row_heading level1 row2" >145</th> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row2_col0" class="data row2 col0" >9</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row2_col1" class="data row2 col1" >$4.58</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row2_col2" class="data row2 col2" >$41.22</td> 
    </tr>    <tr> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level0_row3" class="row_heading level0 row3" >Extraction, Quickblade Of Trembling Hands</th> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level1_row3" class="row_heading level1 row3" >108</th> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row3_col0" class="data row3 col0" >9</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row3_col1" class="data row3 col1" >$3.53</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row3_col2" class="data row3 col2" >$31.77</td> 
    </tr>    <tr> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level0_row4" class="row_heading level0 row4" >Singed Scalpel</th> 
        <th id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781level1_row4" class="row_heading level1 row4" >103</th> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row4_col0" class="data row4 col0" >8</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row4_col1" class="data row4 col1" >$4.35</td> 
        <td id="T_cf9ae34c_7f0d_11e8_ac7a_a41731b30781row4_col2" class="data row4 col2" >$34.80</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
profit_items_final = prefinal_df.sort_values('Total Purchase Value', ascending=False).head()
profit_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level0_row0" class="row_heading level0 row0" >Oathbreaker, Last Hope of the Breaking Storm</th> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level1_row0" class="row_heading level1 row0" >178</th> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row0_col0" class="data row0 col0" >12</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row0_col1" class="data row0 col1" >$4.23</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row0_col2" class="data row0 col2" >$50.76</td> 
    </tr>    <tr> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level0_row1" class="row_heading level0 row1" >Nirvana</th> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level1_row1" class="row_heading level1 row1" >82</th> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row1_col0" class="data row1 col0" >9</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row1_col1" class="data row1 col1" >$4.90</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row1_col2" class="data row1 col2" >$44.10</td> 
    </tr>    <tr> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level0_row2" class="row_heading level0 row2" >Fiery Glass Crusader</th> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level1_row2" class="row_heading level1 row2" >145</th> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row2_col0" class="data row2 col0" >9</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row2_col1" class="data row2 col1" >$4.58</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row2_col2" class="data row2 col2" >$41.22</td> 
    </tr>    <tr> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level0_row3" class="row_heading level0 row3" >Singed Scalpel</th> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level1_row3" class="row_heading level1 row3" >103</th> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row3_col0" class="data row3 col0" >8</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row3_col1" class="data row3 col1" >$4.35</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row3_col2" class="data row3 col2" >$34.80</td> 
    </tr>    <tr> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level0_row4" class="row_heading level0 row4" >Lightning, Etcher of the King</th> 
        <th id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781level1_row4" class="row_heading level1 row4" >59</th> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row4_col0" class="data row4 col0" >8</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row4_col1" class="data row4 col1" >$4.23</td> 
        <td id="T_9f98c1cc_7f0c_11e8_a48d_a41731b30781row4_col2" class="data row4 col2" >$33.84</td> 
    </tr></tbody> 
</table> 


