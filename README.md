
# Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (82%). There also exists, a smaller, but notable proportion of female players (16%).

* Our peak age demographic falls between 20-24 (42%) with secondary groups falling between 15-19 (17.80%) and 25-29 (15.48%).

* Our players are putting in significant cash during the lifetime of their gameplay. Across all major age and gender demographics, the average purchase for a user is roughly $491.   
-----


```python
import pandas as pd
import numpy

```

## Player Count


```python
file = 'purchase_data.json'

df = pd.read_json(file)
sn_count=len(df["SN"].value_counts())
total_players=pd.DataFrame([sn_count],columns=["Total Players"])
print(total_players)





```

       Total Players
    0            573
    

## Purchasing Analysis (Total)


```python
unique_things=len(df["Item ID"].value_counts())
total_sales=round(df["Price"].sum(),2)
mean_price=round(df["Price"].mean(),2)
purchase_amount=df["Price"].count()
print("Purchasing Analyis-Total")
print("The number of unique items was " + str(unique_things) + ".")
print("The total revenue was $"+str(total_sales)+".")
print("The purchasing amount was $"+str(purchase_amount)+".")
```

    Purchasing Analyis-Total
    The number of unique items was 183.
    The total revenue was $2286.33.
    The purchasing amount was $780.
    

## Gender Demographics


```python


```


## Purchasing Analysis (Gender)


```python


```

## Age Demographics


```python
players_df = df.drop_duplicates(['SN'], keep ='last')
players_count=players_df.count()
# Determine the number of male, female, and other players
malePlayer_df=players_df.loc[players_df["Gender"]=="Male"]
malePlayers=len(malePlayer_df)
femalePlayer_df=players_df.loc[players_df["Gender"]=="Female"]
femalePlayers=len(femalePlayer_df)
otherPlayer_df=players_df.loc[players_df["Gender"]=="Other / Non-Disclosed"]
otherPlayers=len(otherPlayer_df)

print("There were",malePlayers,"males,",femalePlayers,"females, and",otherPlayers,"other (non-disclosed) players.")
print("There were",str(100*round(malePlayers/sn_count,4)),"% male,",str(100*round(femalePlayers/sn_count,4)),"% female, and",str(100*round(otherPlayers/sn_count,4)),"% other (non-disclosed) of total players, respectively.")

      
      
```

    There were 465 males, 100 females, and 8 other (non-disclosed) players.
    There were 81.15 % male, 17.45 % female, and 1.4000000000000001 % other (non-disclosed) of total players, respectively.
    

## Purchasing Analysis (Age)


```python
male_df=df.loc[df["Gender"]=="Male"]
male_purchase_count=male_df["Item ID"].count()
male_df=df.loc[df["Gender"]=="Male"]
male_mean_price=round(male_df["Price"].mean(),2)
male_total_price=round(male_df["Price"].sum(),2)
male_player_df=players_df.loc[players_df["Gender"]=="Male"]
male_players=len(male_player_df)
normal_male_total_price=round(male_total_price/male_players,2)
print("----------------------------")
print("PURCHASING ANALYSIS (GENDER)")
print("The total purchases made by male players were " + str(male_purchase_count) + ".")
print("The average purchase price for males was $"+str(male_mean_price)+".")
print("The total value purchased by males was $"+str(male_total_price)+".")
print("The average male players bought a total of $"+str(male_players)+".\n")


female_df=df.loc[df["Gender"]=="Female"]
female_purchase_count=female_df["Item ID"].count()
female_df=df.loc[df["Gender"]=="Female"]
female_mean_price=round(female_df["Price"].mean(),2)
female_total_price=round(female_df["Price"].sum(),2)
female_player_df=players_df.loc[players_df["Gender"]=="Female"]
female_players=len(female_player_df)
normal_female_total_price=round(female_total_price/female_players,2)
print("The total purchases made by female players were " + str(female_purchase_count) + ".")
print("The average purchase price for males was $"+str(female_mean_price)+".")
print("The total value purchased by males was $"+str(female_total_price)+".")
print("The average male players bought a total of $"+str(female_players)+".\n")


other_df=df.loc[df["Gender"]=="Other / Non-Disclosed"]
other_purchase_count=other_df["Item ID"].count()
other_df=df.loc[df["Gender"]=="Other / Non-Disclosed"]
other_mean_price=round(other_df["Price"].mean(),2)
other_total_price=round(other_df["Price"].sum(),2)
other_player_df=players_df.loc[players_df["Gender"]=="Other / Non-Disclosed"]
other_players=len(other_player_df)
normal_other_total_price=round(other_total_price/other_players,2)
print("The total purchases made by other/non-disclosed players were " + str(other_purchase_count) + ".")
print("The average purchase price for others was $"+str(other_mean_price)+".")
print("The total value purchased by others was $"+str(other_total_price)+".")
print("The average other players bought a total of $"+str(other_players)+".")


```

    ----------------------------
    PURCHASING ANALYSIS (GENDER)
    The total purchases made by male players were 633.
    The average purchase price for males was $2.95.
    The total value purchased by males was $1867.68.
    The average male players bought a total of $465.
    
    The total purchases made by female players were 136.
    The average purchase price for males was $2.82.
    The total value purchased by males was $382.91.
    The average male players bought a total of $100.
    
    The total purchases made by other/non-disclosed players were 11.
    The average purchase price for others was $3.25.
    The total value purchased by others was $35.74.
    The average other players bought a total of $8.
    

## Top Spenders


```python
bins=[10,15,20,25,30,35,40,120]
group_labels=["10 and younger", "11 to 15", "16 to 20", "21 to 25", "30 to 35", "35 to 40", "41 and older"]


df["Age Range"]=pd.cut(df["Age"],bins,labels=group_labels)
players_df["Age Range"]=pd.cut(df["Age"],bins,labels=group_labels)
obj1=df.groupby("Age Range")

count_in_obj1=obj1["SN"].count()
count_table=pd.DataFrame({"Purchase Count":count_in_obj1})

avgprice_in_obj1=round(obj1["Price"].mean(),2)
avgprice_table=pd.DataFrame({"Average Price ($)":avgprice_in_obj1})

totprice_in_obj1=obj1["Price"].sum()
totprice_table=pd.DataFrame({"Purchase Value ($)":totprice_in_obj1})

age_df=pd.merge(count_table,avgprice_table,left_index=True,right_index=True).merge(totprice_table,left_index=True,
            right_index=True)

objAge=players_df.groupby("Age Range")

count_in_objAge=objAge["Age"].count()
player_count_table=pd.DataFrame({"Purchase Count":count_in_objAge})

totprice_in_objage=objAge["Price"].sum()
player_totprice_table=pd.DataFrame({"Purchase Value ($)":totprice_in_objage})

age_df["Normalized Price ($)"]=round((player_totprice_table["Purchase Value ($)"]/
                                             player_count_table["Purchase Count"]),2)
age_df
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:6: SettingWithCopyWarning: 
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
      <th>Average Price ($)</th>
      <th>Purchase Value ($)</th>
      <th>Normalized Price ($)</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10 and younger</th>
      <td>78</td>
      <td>2.87</td>
      <td>224.15</td>
      <td>2.84</td>
    </tr>
    <tr>
      <th>11 to 15</th>
      <td>184</td>
      <td>2.87</td>
      <td>528.74</td>
      <td>2.86</td>
    </tr>
    <tr>
      <th>16 to 20</th>
      <td>305</td>
      <td>2.96</td>
      <td>902.61</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>21 to 25</th>
      <td>76</td>
      <td>2.89</td>
      <td>219.82</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>30 to 35</th>
      <td>58</td>
      <td>3.07</td>
      <td>178.26</td>
      <td>2.99</td>
    </tr>
    <tr>
      <th>35 to 40</th>
      <td>44</td>
      <td>2.90</td>
      <td>127.49</td>
      <td>2.74</td>
    </tr>
    <tr>
      <th>41 and older</th>
      <td>3</td>
      <td>2.88</td>
      <td>8.64</td>
      <td>2.88</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
objSN=df.groupby("SN")

count_in_objSN=objSN["Age"].count()
SN_count_table=pd.DataFrame({"Purchase Count":count_in_objSN})

average_price_in_objSN=round(objSN["Price"].mean(),2)
SN_average_price_table=pd.DataFrame({"Average Price ($)":average_price_in_objSN})

totalprice_in_objSN=objSN["Price"].sum()
SN_total_price_table=pd.DataFrame({"Total Purchase Value ($)":totalprice_in_objSN})

SN_df=pd.merge(SN_count_table,SN_average_price_table,left_index=True,right_index=True).merge(
    SN_total_price_table,left_index=True,right_index=True)

sorted_SN=SN_df.sort_values(by="Total Purchase Value ($)",ascending=False)

print("----------------------------------------------------------------------")
print("TOP SPENDERS")
sorted_SN.head()
```

    ----------------------------------------------------------------------
    TOP SPENDERS
    




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
      <th>Average Price ($)</th>
      <th>Total Purchase Value ($)</th>
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
      <td>3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
objid=df.groupby("Item ID")


count_in_objid=objid["Item ID"].count()
ID_count_table=pd.DataFrame({"Purchase Count":count_in_objid})

averageprice_in_objid=round(objid["Price"].mean(),2)
ID_average_price_table=pd.DataFrame({"Item Price ($)":averageprice_in_objid})

totalprice_in_objid=objid["Price"].sum()
Id_totalprice_table=pd.DataFrame({"Item Total Sales ($)":totalprice_in_objid})

singleID_df = df.drop_duplicates(['Item ID'], keep ='last')

reduced_singleID_df=singleID_df.loc[:,["Item ID","Item Name"]]


ID_df=pd.merge(reduced_singleID_df,ID_count_table,left_index=True,right_index=True).merge(ID_average_price_table,
               left_index=True,right_index=True).merge(Id_totalprice_table,left_index=True,right_index=True)


indexed_ID_df=ID_df.set_index("Item ID")

sorted_ID=indexed_ID_df.sort_values(by="Purchase Count",ascending=False)

print("----------------------------------------------------------------------")
print("MOST POPULAR ITEMS")
sorted_ID.head()
```

    ----------------------------------------------------------------------
    MOST POPULAR ITEMS
    




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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price ($)</th>
      <th>Item Total Sales ($)</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Splinter</td>
      <td>5</td>
      <td>4.83</td>
      <td>24.15</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Flux, Destroyer of Due Diligence</td>
      <td>4</td>
      <td>1.27</td>
      <td>5.08</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Exiled Mithril Longsword</td>
      <td>4</td>
      <td>1.55</td>
      <td>6.20</td>
    </tr>
    <tr>
      <th>155</th>
      <td>War-Forged Gold Deflector</td>
      <td>4</td>
      <td>4.89</td>
      <td>19.56</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lightning, Etcher of the King</td>
      <td>3</td>
      <td>3.47</td>
      <td>10.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
sorted_ID=indexed_ID_df.sort_values(by="Item Total Sales ($)",ascending=False)

print("----------------------------------------------------------------------")
print("MOST PROFITABLE ITEMS")
sorted_ID.head()
```

    ----------------------------------------------------------------------
    MOST PROFITABLE ITEMS
    




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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price ($)</th>
      <th>Item Total Sales ($)</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Splinter</td>
      <td>5</td>
      <td>4.83</td>
      <td>24.15</td>
    </tr>
    <tr>
      <th>155</th>
      <td>War-Forged Gold Deflector</td>
      <td>4</td>
      <td>4.89</td>
      <td>19.56</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lightning, Etcher of the King</td>
      <td>3</td>
      <td>3.47</td>
      <td>10.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Phantomlight</td>
      <td>3</td>
      <td>3.27</td>
      <td>9.81</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Persuasion</td>
      <td>2</td>
      <td>4.10</td>
      <td>8.20</td>
    </tr>
  </tbody>
</table>
</div>


