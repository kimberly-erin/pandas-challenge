
# Observations

Observation 1: The largest gaming demographic age is between 15 and 29, with the peak years being 20-24.

Observation 2: While those identifying as male are the most profitable group, those who do not provide a gender preference are more likely to spend more.

Observation 3: The average paid per item is significantly higher than the price of some items including those on the most sold list.

More information is needed to know total number of players by gender so that insight into what percentage of each demographic has purchased items.


```python
import pandas as pd
```

# file path
file = "purchase_data.json"
# read json file
pymoli_df = pd.read_json(file)
pymoli_df.head()


```python
# Total number of players
player_total = pymoli_df["SN"].nunique()
player_total
```




    573




```python
# Purchasing Analysis (Total)
items = pymoli_df["Item Name"].nunique()
price = pymoli_df["Price"].mean()
purchases = pymoli_df["Item Name"].count()
revenue = pymoli_df["Price"].sum()

purchase_df = pd.DataFrame({"Unique Items Purchased": [items],
                     "Average Purchase Price": [price],
                     "Total Number of Purchases": [purchases],
                     "Total Revenue": [revenue]
})

purchase_df = purchase_df[["Unique Items Purchased", "Average Purchase Price", "Total Number of Purchases", "Total Revenue"]]

purchase_df["Average Purchase Price"] = purchase_df["Average Purchase Price"].map("${:.2f}".format)
purchase_df["Total Revenue"] = purchase_df["Total Revenue"].map("${:.2f}".format)

purchase_df
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
      <th>Unique Items Purchased</th>
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
gender_df = pymoli_df.groupby(["Gender"])

gender_count = gender_df["SN"].nunique()
player_total = pymoli_df["SN"].nunique()
gender_percent = gender_count/player_total*100

gender_summary_df = pd.DataFrame({"Percentage of Players": gender_percent, "Total Players": gender_count})

gender_summary_df["Percentage of Players"]=gender_summary_df["Percentage of Players"].map("{:.2f}%".format)

gender_summary_df
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
      <th>Total Players</th>
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
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchase Analysis (Gender)
purchase_gender_df = pymoli_df.groupby(["Gender"])

pcount = purchase_gender_df["Item Name"].count()
aveprice = purchase_gender_df["Price"].mean()
totpurchase = purchase_gender_df["Price"].sum()
normtotals = purchase_gender_df["Price"].sum()/purchase_gender_df["SN"].nunique()

gen_purchase_summary_df = pd.DataFrame({"Purchase Count": pcount, 
                                        "Average Purchase Price": aveprice, 
                                        "Total Purchase Amount": totpurchase,
                                        "Normalized Totals": normtotals
})

gen_purchase_summary_df=gen_purchase_summary_df[["Purchase Count", "Average Purchase Price", "Total Purchase Amount", "Normalized Totals"]]

gen_purchase_summary_df["Average Purchase Price"]= gen_purchase_summary_df["Average Purchase Price"].map("${:.2f}".format)
gen_purchase_summary_df["Total Purchase Amount"]=gen_purchase_summary_df["Total Purchase Amount"].map("${:.2f}".format)
gen_purchase_summary_df["Normalized Totals"]=gen_purchase_summary_df["Normalized Totals"].map("${:.2f}".format)

gen_purchase_summary_df
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
      <th>Total Purchase Amount</th>
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
bins = [0,9,14,19,24,29,34,39,100]
binnames = ["<10","10-14","15-19","20-24","25-29","30-34","35-39",">40"]
pymoli_df["Age Groups"] = pd.cut(pymoli_df["Age"],bins,labels=binnames)
age_df = pymoli_df.groupby(["Age Groups"])

pcount = age_df["Item Name"].count()
aveprice = age_df["Price"].mean()
totpurchase = age_df["Price"].sum()
normtotals = age_df["Price"].sum()/age_df["SN"].nunique()

age_summary_df = pd.DataFrame({"Purchase Count": pcount, 
                                        "Average Purchase Price": aveprice, 
                                        "Total Purchase Amount": totpurchase,
                                        "Normalized Totals": normtotals
})

age_summary_df=age_summary_df[["Purchase Count", "Average Purchase Price", "Total Purchase Amount", "Normalized Totals"]]

age_summary_df["Average Purchase Price"]= age_summary_df["Average Purchase Price"].map("${:.2f}".format)
age_summary_df["Total Purchase Amount"]=age_summary_df["Total Purchase Amount"].map("${:.2f}".format)
age_summary_df["Normalized Totals"]=age_summary_df["Normalized Totals"].map("${:.2f}".format)

age_summary_df
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
      <th>Total Purchase Amount</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Groups</th>
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




```python
# Top Spenders
spender_df = pymoli_df.groupby(["SN"])

pcount = spender_df["Item Name"].count()
aveprice = spender_df["Price"].mean()
totpurchase = spender_df["Price"].sum()

spender_summary_df = pd.DataFrame({"Purchase Count": pcount,"Average Purchase Price":aveprice,"Total Purchase Amount":totpurchase})
spender_summary_df = spender_summary_df[["Purchase Count", "Average Purchase Price", "Total Purchase Amount"]]

spender_summary_df = spender_summary_df.sort_values(by=['Total Purchase Amount'],ascending=False)


spender_summary_df["Average Purchase Price"]= spender_summary_df["Average Purchase Price"].map("${:.2f}".format)
spender_summary_df["Total Purchase Amount"]= spender_summary_df["Total Purchase Amount"].map("${:.2f}".format)



spender_summary_df.head()

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
      <th>Total Purchase Amount</th>
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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most popular items
item_df = pymoli_df.groupby(["Item ID","Item Name"])

pcount = item_df["Item Name"].count()
totpurchase = item_df["Price"].sum()
price = item_df["Price"].mean()

item_summary_df = pd.DataFrame({"Purchase Count": pcount, "Item Price":price, "Total Purchase Value":totpurchase })
item_summary_df = item_summary_df[["Purchase Count", "Item Price", "Total Purchase Value"]]

item_summary_df = item_summary_df.sort_values(by=['Purchase Count'],ascending=False)


item_summary_df["Item Price"]= item_summary_df["Item Price"].map("${:.2f}".format)
item_summary_df["Total Purchase Value"]= item_summary_df["Total Purchase Value"].map("${:.2f}".format)



item_summary_df.head()

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
      <th>Purchase Count</th>
      <th>Item Price</th>
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
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most profitable item
profit_df = pymoli_df.groupby(["Item ID","Item Name"])

pcount = item_df["Item Name"].count()
totpurchase = item_df["Price"].sum()
price = item_df["Price"].mean()

profit_summary_df = pd.DataFrame({"Purchase Count": pcount, "Item Price":price, "Total Purchase Value":totpurchase })
profit_summary_df = profit_summary_df[["Purchase Count", "Item Price", "Total Purchase Value"]]

profit_summary_df = profit_summary_df.sort_values(by=['Total Purchase Value'],ascending=False)


profit_summary_df["Item Price"]= profit_summary_df["Item Price"].map("${:.2f}".format)
profit_summary_df["Total Purchase Value"]= profit_summary_df["Total Purchase Value"].map("${:.2f}".format)



profit_summary_df.head()

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
      <th>Purchase Count</th>
      <th>Item Price</th>
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
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
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
  </tbody>
</table>
</div>


