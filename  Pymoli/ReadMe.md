
### Heroes Of Pymoli Data Analysis 

OBSERVED TREND 1
The male players are 4.5 times more than female players.
OBSERVED TREND 2 
The most game players are 15 - 29 years. I am not the only woman who doesn't play games.
OBSERVED TREND 3
Even the most profitable items are all under $5. Really couldn't see how that are profitable.


```python
import pandas as pd
import numpy as np
```


```python
df = pd.read_json("raw_data/purchase_data.json")
df.head()
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



## Total Number of Players


```python
total_unique_players=df["SN"].nunique()
# Create a new table consolodating above calculations
total = pd.DataFrame({"Total Players": [total_unique_players]
                    })
total
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



## Purchasing Analysis (Total)


```python
#Number of Unique Items
unique_count_items = len(df["Item ID"].unique())
#Total Number of Purchases
total_purchase = df["Item ID"].count()
#Average Purchase Price
avg_price = (df['Price'].sum()) / total_purchase
#Total Revenue 
total_rev= df['Price'].sum()
# Create a new table consolodating above calculations
purchasing_summary = pd.DataFrame({"Number of Unique Items": [unique_count_items],
                                   "Average Price": [avg_price],
                                   "Number of Purchases": [total_purchase],
                                   "Total Revenue": [total_rev]
                            })
purchasing_summary["Total Revenue"] = purchasing_summary["Total Revenue"].map("${0:,.2f}".format)
purchasing_summary["Average Price"] = purchasing_summary["Average Price"].map("${0:,.2f}".format)
purchasing_summary
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>183</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
total_purchase_players=len(df["SN"])
gender_players=df.groupby('Gender')['SN'].count()
purchase_players_percentage=gender_players*100 / total_purchase_players
Gender_Demographics = pd.concat([purchase_players_percentage, gender_players], axis=1)
Gender_Demographics.columns = ['Percentage of Players','Total Count']
Gender_Demographics
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.435897</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.153846</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.410256</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
purchase_count_Gender=df.groupby('Gender')['Item ID'].count()
avg_price_Gender=df.groupby('Gender')['Price'].mean()
total_purchase_value_Gender =df.groupby('Gender')['Price'].sum()
unique_players_Gender=(df.groupby('Gender')["SN"].nunique())
normalized_total_Gender=total_purchase_value_Gender/unique_players_Gender
Purchasing_Analysis_Gender = pd.concat([purchase_count_Gender, avg_price_Gender, total_purchase_value_Gender, normalized_total_Gender], axis=1)
Purchasing_Analysis_Gender.columns = ['Purchase Count','Average Purchase Price','Total Purchase Value', 'Normalized Totals']
Purchasing_Analysis_Gender
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
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
      <td>136</td>
      <td>2.815515</td>
      <td>382.91</td>
      <td>3.829100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.950521</td>
      <td>1867.68</td>
      <td>4.016516</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>4.467500</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
#Generate category dataframe
bins = [0, 10, 15, 20, 25, 30, 35, 40, 100]
group_names = ['<10', '10-14', '15-19', '20-24','25-29', '30-34','35-39', '>40']
df['categories']=pd.cut(df['Age'], bins, labels=group_names, right=False)
categories_df=df
categories_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>categories</th>
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
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_count_categories =df.groupby('categories')['SN'].count()
total_players=df["SN"].count()
Percentage_Players = total_count_categories *100 / total_players
Age_Demographics_temp =pd.concat([Percentage_Players, total_count_categories], axis=1)
Age_Demographics=Age_Demographics_temp.reindex(["<10", "10-14", "15-19", "20-24","25-29", "30-34","35-39", ">40"])
Age_Demographics.columns=["Percentage of Players","Total Count"]
Age_Demographics
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>categories</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.589744</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.487179</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.051282</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>43.076923</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.025641</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.205128</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.384615</td>
      <td>42</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>2.179487</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
Purchase_Count_Age=categories_df.groupby('categories')['Item ID'].count()
#Average Purchase Price 
Average_Purchase_Price_Age = df.groupby('categories')['Price'].mean()
total_purchase_value_Age =df.groupby('categories')['Price'].sum()
unique_players_Age =df.groupby('categories')['SN'].nunique()
normalized_total_Age=total_purchase_value_Age/unique_players_Age
Purchasing_Analysis_Age_temp =pd.concat([Purchase_Count_Age, Average_Purchase_Price_Age,total_purchase_value_Age,normalized_total_Age], axis=1)
Purchasing_Analysis_Age=Purchasing_Analysis_Age_temp.reindex(["<10", "10-14", "15-19", "20-24","25-29", "30-34","35-39", ">40"])
Purchasing_Analysis_Age
Purchasing_Analysis_Age.columns=["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]
Purchasing_Analysis_Age["Average Purchase Price"] = Purchasing_Analysis_Age["Average Purchase Price"].map("${0:,.2f}".format)
Purchasing_Analysis_Age["Normalized Totals"] = Purchasing_Analysis_Age["Normalized Totals"].map("${0:,.2f}".format)
Purchasing_Analysis_Age["Total Purchase Value"] = Purchasing_Analysis_Age["Total Purchase Value"].map("${0:,.2f}".format)
Purchasing_Analysis_Age
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
    <tr>
      <th>categories</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
#generate dataframe with only the 5 top shopppers
Top_Purchase =df.groupby('SN')["Price"].sum().nlargest(5)
item=df.loc[df['SN'].isin(Top_Purchase.index)]
item.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>categories</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>21</td>
      <td>Male</td>
      <td>96</td>
      <td>Blood-Forged Skeletal Spine</td>
      <td>4.77</td>
      <td>Haellysu29</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>79</th>
      <td>29</td>
      <td>Male</td>
      <td>144</td>
      <td>Blood Infused Guardian</td>
      <td>2.86</td>
      <td>Undirrala66</td>
      <td>25-29</td>
    </tr>
    <tr>
      <th>107</th>
      <td>29</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Undirrala66</td>
      <td>25-29</td>
    </tr>
    <tr>
      <th>108</th>
      <td>22</td>
      <td>Male</td>
      <td>35</td>
      <td>Heartless Bone Dualblade</td>
      <td>2.63</td>
      <td>Eoda93</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>131</th>
      <td>29</td>
      <td>Male</td>
      <td>62</td>
      <td>Piece Maker</td>
      <td>4.36</td>
      <td>Undirrala66</td>
      <td>25-29</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_count = item.groupby('SN')['Item ID'].count()
Average_Purchase_Price = Top_Purchase / item_count
Top_Spenders =pd.concat([item_count, Average_Purchase_Price, Top_Purchase], axis=1)
Top_Spenders.columns=["Purchase Count","Average Purchase Price","Total Purchase Value"]
Top_Spenders["Average Purchase Price"] = Top_Spenders["Average Purchase Price"].map("${0:,.2f}".format)
Top_Spenders["Total Purchase Value"] = Top_Spenders["Total Purchase Value"].map("${0:,.2f}".format)
Top_Spenders
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
popular_item =df.groupby('Item ID')['SN'].count().nlargest(5)
item_puchase=df.loc[df['Item ID'].isin(popular_item.index)]
item_puchase.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>categories</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>35</th>
      <td>21</td>
      <td>Female</td>
      <td>13</td>
      <td>Serenity</td>
      <td>1.49</td>
      <td>Aisur51</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>56</th>
      <td>23</td>
      <td>Male</td>
      <td>31</td>
      <td>Trickster</td>
      <td>2.07</td>
      <td>Marilsasya33</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>57</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alallo58</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>61</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Rinallorap73</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>62</th>
      <td>19</td>
      <td>Female</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeri84</td>
      <td>15-19</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_purchase_total = item_puchase.groupby(['Item ID','Item Name'])['Price'].sum()
item_purchase_count = item_puchase.groupby(['Item ID','Item Name'])['Item ID'].count()
item_purchase_price = item_purchase_total / item_purchase_count
Most_Popular_Items_temp =pd.concat([item_purchase_count, item_purchase_price,item_purchase_total], axis=1)
Most_Popular_Items_temp.columns=["Purchase Count","Item Price","Total Purchase Value"]
Most_Popular_Items= Most_Popular_Items_temp
Most_Popular_Items["Item Price"] = Most_Popular_Items["Item Price"].map("${0:,.2f}".format)
Most_Popular_Items["Total Purchase Value"] = Most_Popular_Items["Total Purchase Value"].map("${0:,.2f}".format)
Most_Popular_Items
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
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
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
profit_item =df.groupby('Item ID')["Price"].sum().nlargest(5)
profit_item_puchase = df.loc[df['Item ID'].isin(profit_item.index)]
profit_item_purchase_total = profit_item_puchase.groupby(['Item ID','Item Name'])['Price'].sum()
profit_item_purchase_count = profit_item_puchase.groupby(['Item ID','Item Name'])['Item ID'].count()
profit_item_purchase_price = profit_item_purchase_total / profit_item_purchase_count
Most_Profitable_Items_temp =pd.concat([profit_item_purchase_count, profit_item_purchase_price,profit_item_purchase_total], axis=1)
Most_Profitable_Items_temp.columns=["Purchase Count", "Item Price", "Total Purchase Value"]
Most_Profitable_Items = Most_Profitable_Items_temp
Most_Profitable_Items["Item Price"] = Most_Profitable_Items["Item Price"].map("${0:,.2f}".format)
Most_Profitable_Items["Total Purchase Value"] = Most_Profitable_Items["Total Purchase Value"].map("${0:,.2f}".format)
Most_Profitable_Items
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
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
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
  </tbody>
</table>
</div>


