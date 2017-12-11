

```python
import pandas as pd
import numpy as np
```


```python
HofP=pd.read_json('./HeroesOfPymoli/purchase_data.json')
HofP.head(10)
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
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>



### Player Count


```python
pd.DataFrame(data={'Total Players': [HofP['SN'].nunique()]})
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



### Purchasing Analysis (Total)


```python
keys =['Number of Unique Items','Average Price','Number of Purchases','Total Revenue']
unique_items= HofP['Item ID'].nunique()                  
ave_price = HofP['Price'].mean()
total_purchase = HofP['Item ID'].count()
total_revenue = HofP['Price'].sum()
purchase_ana = {'Number of Unique Items': [unique_items],'Average Price': [ave_price],
                'Number of Purchases': [total_purchase],'Total Revenue': [total_revenue]}
purchase_ana_df = pd.DataFrame(data=purchase_ana, columns=keys)
purchase_ana_df['Average Price'] = purchase_ana_df['Average Price'].map('${:,.2f}'.format)
purchase_ana_df['Total Revenue'] = purchase_ana_df['Total Revenue'].map('${:,.2f}'.format)
purchase_ana_df
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
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



### Gender Demographics


```python
cols=['Percentage of Players',' Total Count ']
genders=[HofP['Gender'].unique()]
gender_grp_counts=pd.DataFrame(HofP.groupby(HofP['Gender'])['SN'].nunique(), 
                               index=genders)
per_plyr=pd.DataFrame(gender_grp_counts['SN']/gender_grp_counts['SN'].sum()*100)
res=per_plyr.set_index(genders).join(gender_grp_counts.set_index(genders),
                                                     lsuffix='0', rsuffix='1')
res['SN0'] = res['SN0'].map('{:,.2f}'.format)
res.rename(columns={'SN0':cols[0],'SN1':cols[1]})
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
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Gender)


```python
HofP['Total Purchase Value']=HofP['Price']
HofP['Normalized Totals']=pd.DataFrame(HofP.groupby(['Gender'])['Price']
                                       .apply(lambda x: (x) / x.count()))
pur_ana=pd.DataFrame(HofP.groupby(['Gender']).agg({'Item ID': 'count',
                                                    'Price': 'mean',
                                                   'Normalized Totals':'sum',
                                                     })).rename(columns={'Item ID':'Purchase Count',
                                                                         'Price':'Average Price'})
pur_ana['Total Purchase Value']= pur_ana['Purchase Count']*pur_ana['Average Price']
pur_ana['Total Purchase Value']= pur_ana['Total Purchase Value'].map('${:,.2f}'.format)
pur_ana['Normalized Totals']= pur_ana['Normalized Totals'].map('${:,.2f}'.format)
pur_ana['Average Price']= pur_ana['Average Price'].map('${:,.2f}'.format)
pur_ana
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
      <th>Average Price</th>
      <th>Normalized Totals</th>
      <th>Total Purchase Value</th>
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
      <td>$2.82</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$3.25</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
HofP1=HofP
HofP1['Percent Count']=HofP['SN']
HofP1['Total Count']=HofP['SN']

def Pyr_count(SN):
    return (len(SN.unique()))
def Perc_Pyr(SN):
    return (len(SN.unique())/HofP['SN'].nunique()*100)

rlabel=[[ "{0}-{1}".format(i+1, i + 4) for i in range(HofP['Age'].min()-1,
                                                       HofP['Age'].max()+4, 4) ]]
rlabel[0][0]='<'+ rlabel[0][0][-2:]
rlabel[0][len(rlabel[0])-1]=rlabel[0][len(rlabel[0])-1][:2]+'+'

a=HofP1.groupby(pd.cut(HofP1['Age'] , 
                    bins=np.arange(HofP1['Age'].min()-1,
                                   HofP1['Age'].max()+8,4),
                      right=True)).agg({'Percent Count':Perc_Pyr,
                                       'Total Count':Pyr_count}
                                      )
a.index=rlabel
a.fillna(0, inplace=True)
a['Total Count']=a['Total Count'].astype(int)
a['Percent Count']=a['Percent Count'].map('{:,.2f}'.format)
a
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
      <th>Percent Count</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>11-14</th>
      <td>3.49</td>
      <td>20</td>
    </tr>
    <tr>
      <th>15-18</th>
      <td>14.66</td>
      <td>84</td>
    </tr>
    <tr>
      <th>19-22</th>
      <td>31.06</td>
      <td>178</td>
    </tr>
    <tr>
      <th>23-26</th>
      <td>26.70</td>
      <td>153</td>
    </tr>
    <tr>
      <th>27-30</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>31-34</th>
      <td>5.93</td>
      <td>34</td>
    </tr>
    <tr>
      <th>35-38</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>39-42</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
    <tr>
      <th>43-46</th>
      <td>0.35</td>
      <td>2</td>
    </tr>
    <tr>
      <th>47+</th>
      <td>0.00</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Age)


```python
def norm(price):
    price_list=list(price)
    norm=map(lambda x: (x-np.min(price_list)) / (np.max(price_list)-np.min(price_list)),price)
    norm=(sum(list(norm)))
    return norm
def total_pur_val(price):
    return np.mean(price)*len(price)

HofP1['Normalized Totals']=HofP1['Price']
a=HofP1.groupby(pd.cut(HofP1['Age'] , 
                    bins=np.arange(HofP1['Age'].min()-1,
                                   HofP1['Age'].max()+8,4),
                      right=True)).agg({'Item ID':'count',
                                       'Price':'mean',
                                        'Total Purchase Value': total_pur_val,
                                       'Normalized Totals':norm}
                                      ).rename(columns={'Item ID':'Purchase Count',
                                                       'Price':'Average Purchase Price'})

a['Average Purchase Price']=a['Average Purchase Price'].map('${:,.2f}'.format)
a['Total Purchase Value']=a['Total Purchase Value'].map('${:,.2f}'.format)
a['Normalized Totals']=a['Normalized Totals'].map('${:,.2f}'.format)
a.index=rlabel
a.sort_index(0)
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11-14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$12.92</td>
    </tr>
    <tr>
      <th>15-18</th>
      <td>111</td>
      <td>$2.88</td>
      <td>$319.32</td>
      <td>$52.29</td>
    </tr>
    <tr>
      <th>19-22</th>
      <td>231</td>
      <td>$2.93</td>
      <td>$676.20</td>
      <td>$111.80</td>
    </tr>
    <tr>
      <th>23-26</th>
      <td>207</td>
      <td>$2.94</td>
      <td>$608.02</td>
      <td>$100.72</td>
    </tr>
    <tr>
      <th>27-30</th>
      <td>63</td>
      <td>$2.98</td>
      <td>$187.99</td>
      <td>$31.40</td>
    </tr>
    <tr>
      <th>31-34</th>
      <td>46</td>
      <td>$3.07</td>
      <td>$141.24</td>
      <td>$23.94</td>
    </tr>
    <tr>
      <th>35-38</th>
      <td>37</td>
      <td>$2.81</td>
      <td>$104.06</td>
      <td>$18.37</td>
    </tr>
    <tr>
      <th>39-42</th>
      <td>20</td>
      <td>$3.13</td>
      <td>$62.56</td>
      <td>$11.19</td>
    </tr>
    <tr>
      <th>43-46</th>
      <td>2</td>
      <td>$3.26</td>
      <td>$6.53</td>
      <td>$1.00</td>
    </tr>
    <tr>
      <th>47+</th>
      <td>0</td>
      <td>$nan</td>
      <td>$nan</td>
      <td>$nan</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$16.49</td>
    </tr>
  </tbody>
</table>
</div>



### Top Spenders


```python
HofP1['Normalized Totals']=HofP1['Price']
Tp_Spendr=pd.DataFrame(HofP1.groupby(['SN']).agg({'Item ID': 'count',
                                                    'Price': 'mean',
                                                   'Total Purchase Value':total_pur_val,
                                                     })).rename(columns={'Item ID':'Purchase Count',
                                                                         'Price':'Average Purchase Price'})
Tp_Spendr['Average Purchase Price']=Tp_Spendr['Average Purchase Price'].map('${:,.2f}'.format)
Tp_Spendr['Total Purchase Value']=Tp_Spendr['Total Purchase Value'].map('${:,.2f}'.format)
Tp_Spendr=Tp_Spendr.sort_values(by=['Purchase Count'], ascending=False)
Tp_Spendr.head(5)
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
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Sondastan54</th>
      <td>4</td>
      <td>$2.56</td>
      <td>$10.24</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items


```python
Tp_items=pd.DataFrame(HofP1.groupby(['Item ID', 'Item Name']).agg({'Item ID': 'count',
                                                    'Price': 'mean',
                                                   'Total Purchase Value':total_pur_val,
                                                     })).rename(columns={'Item ID':'Purchase Count',
                                                                         'Price':'Item Price'})
Tp_items['Item Price']=Tp_items['Item Price'].map('${:,.2f}'.format)
Tp_items['Total Purchase Value']=Tp_items['Total Purchase Value'].map('${:,.2f}'.format)
Tp_items=Tp_items.sort_values(by=['Purchase Count'], ascending=False)
Tp_items.head(5)
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



### Most Profitable Items


```python
Tp_items=Tp_items.sort_values(by=['Total Purchase Value'], ascending=False)
Tp_items.head(5)
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
      <th>170</th>
      <th>Shadowsteel</th>
      <td>5</td>
      <td>$1.98</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>21</th>
      <th>Souleater</th>
      <td>3</td>
      <td>$3.27</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>37</th>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>5</td>
      <td>$1.93</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>127</th>
      <th>Heartseeker, Reaver of Souls</th>
      <td>3</td>
      <td>$3.21</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>120</th>
      <th>Agatha</th>
      <td>5</td>
      <td>$1.91</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>


