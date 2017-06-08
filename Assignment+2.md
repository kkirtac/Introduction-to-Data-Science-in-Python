
---

_You are currently looking at **version 1.2** of this notebook. To download notebooks and datafiles, as well as get help on Jupyter notebooks in the Coursera platform, visit the [Jupyter Notebook FAQ](https://www.coursera.org/learn/python-data-analysis/resources/0dhYG) course resource._

---

# Assignment 2 - Pandas Introduction
All questions are weighted the same in this assignment.
## Part 1
The following code loads the olympics dataset (olympics.csv), which was derrived from the Wikipedia entry on [All Time Olympic Games Medals](https://en.wikipedia.org/wiki/All-time_Olympic_Games_medal_table), and does some basic data cleaning. 

The columns are organized as # of Summer games, Summer medals, # of Winter games, Winter medals, total # number of games, total # of medals. Use this dataset to answer the questions below.


```python
import pandas as pd

df = pd.read_csv('olympics.csv', index_col=0, skiprows=1)

for col in df.columns:
    if col[:2]=='01':
        df.rename(columns={col:'Gold'+col[4:]}, inplace=True)
    if col[:2]=='02':
        df.rename(columns={col:'Silver'+col[4:]}, inplace=True)
    if col[:2]=='03':
        df.rename(columns={col:'Bronze'+col[4:]}, inplace=True)
    if col[:1]=='№':
        df.rename(columns={col:'#'+col[1:]}, inplace=True)

names_ids = df.index.str.split('\s\(') # split the index by '('

df.index = names_ids.str[0] # the [0] element is the country name (new index) 
df['ID'] = names_ids.str[1].str[:3] # the [1] element is the abbreviation or ID (take first 3 characters from that)

df = df.drop('Totals')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th># Summer</th>
      <th>Gold</th>
      <th>Silver</th>
      <th>Bronze</th>
      <th>Total</th>
      <th># Winter</th>
      <th>Gold.1</th>
      <th>Silver.1</th>
      <th>Bronze.1</th>
      <th>Total.1</th>
      <th># Games</th>
      <th>Gold.2</th>
      <th>Silver.2</th>
      <th>Bronze.2</th>
      <th>Combined total</th>
      <th>ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>AFG</td>
    </tr>
    <tr>
      <th>Algeria</th>
      <td>12</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
      <td>15</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
      <td>15</td>
      <td>ALG</td>
    </tr>
    <tr>
      <th>Argentina</th>
      <td>23</td>
      <td>18</td>
      <td>24</td>
      <td>28</td>
      <td>70</td>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>41</td>
      <td>18</td>
      <td>24</td>
      <td>28</td>
      <td>70</td>
      <td>ARG</td>
    </tr>
    <tr>
      <th>Armenia</th>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>9</td>
      <td>12</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>11</td>
      <td>1</td>
      <td>2</td>
      <td>9</td>
      <td>12</td>
      <td>ARM</td>
    </tr>
    <tr>
      <th>Australasia</th>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
      <td>12</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
      <td>12</td>
      <td>ANZ</td>
    </tr>
  </tbody>
</table>
</div>



### Question 0 (Example)

What is the first country in df?

*This function should return a Series.*


```python
# You should write your whole answer within the function provided. The autograder will call
# this function and compare the return value against the correct solution value
def answer_zero():
    # This function returns the row for Afghanistan, which is a Series object. The assignment
    # question description will tell you the general format the autograder is expecting
    return df.iloc[0]

# You can examine what your function returns by calling it in the cell. If you have questions
# about the assignment formats, check out the discussion forums for any FAQs
answer_zero() 
```




    # Summer           13
    Gold                0
    Silver              0
    Bronze              2
    Total               2
    # Winter            0
    Gold.1              0
    Silver.1            0
    Bronze.1            0
    Total.1             0
    # Games            13
    Gold.2              0
    Silver.2            0
    Bronze.2            2
    Combined total      2
    ID                AFG
    Name: Afghanistan, dtype: object



### Question 1
Which country has won the most gold medals in summer games?

*This function should return a single string value.*


```python
def answer_one():
    series = df['Gold'].sort_values(ascending=False)
    return series.index[0]

answer_one()
```




    'United States'




```python
df.loc['United States']
```




    # Summer            26
    Gold               976
    Silver             757
    Bronze             666
    Total             2399
    # Winter            22
    Gold.1              96
    Silver.1           102
    Bronze.1            84
    Total.1            282
    # Games             48
    Gold.2            1072
    Silver.2           859
    Bronze.2           750
    Combined total    2681
    ID                 USA
    Name: United States, dtype: object



### Question 2
Which country had the biggest difference between their summer and winter gold medal counts?

*This function should return a single string value.*


```python
def answer_two():
    series = df['Gold']-df['Gold.1']
    abss = series.abs()
    bsss = abss.sort_values(ascending=False)
    return bsss.index[0]

answer_two()
```




    'United States'



### Question 3
Which country has the biggest difference between their summer gold medal counts and winter gold medal counts relative to their total gold medal count? 

$$\frac{Summer~Gold - Winter~Gold}{Total~Gold}$$

Only include countries that have won at least 1 gold in both summer and winter.

*This function should return a single string value.*


```python
def answer_three():
    at_least_1 = df[(df['Gold.1'] >= 1) & (df['Gold'] >= 1)]   
    val = (((at_least_1['Gold'] - at_least_1['Gold.1'])/at_least_1['Gold.2']).sort_values(ascending=False))

    return val.index[0]

answer_three()
```




    'Bulgaria'



### Question 4
Write a function that creates a Series called "Points" which is a weighted value where each gold medal (`Gold.2`) counts for 3 points, silver medals (`Silver.2`) for 2 points, and bronze medals (`Bronze.2`) for 1 point. The function should return only the column (a Series object) which you created.

*This function should return a Series named `Points` of length 146*


```python
def answer_four():
    Points = df['Gold.2']*3 + df['Silver.2']*2 + df['Bronze.2']*1
    print(len(Points))
    return Points

answer_four()
```

    146





    Afghanistan                            2
    Algeria                               27
    Argentina                            130
    Armenia                               16
    Australasia                           22
    Australia                            923
    Austria                              569
    Azerbaijan                            43
    Bahamas                               24
    Bahrain                                1
    Barbados                               1
    Belarus                              154
    Belgium                              276
    Bermuda                                1
    Bohemia                                5
    Botswana                               2
    Brazil                               184
    British West Indies                    2
    Bulgaria                             411
    Burundi                                3
    Cameroon                              12
    Canada                               846
    Chile                                 24
    China                               1120
    Colombia                              29
    Costa Rica                             7
    Ivory Coast                            2
    Croatia                               67
    Cuba                                 420
    Cyprus                                 2
                                        ... 
    Spain                                268
    Sri Lanka                              4
    Sudan                                  2
    Suriname                               4
    Sweden                              1217
    Switzerland                          630
    Syria                                  6
    Chinese Taipei                        32
    Tajikistan                             4
    Tanzania                               4
    Thailand                              44
    Togo                                   1
    Tonga                                  2
    Trinidad and Tobago                   27
    Tunisia                               19
    Turkey                               191
    Uganda                                14
    Ukraine                              220
    United Arab Emirates                   3
    United States                       5684
    Uruguay                               16
    Uzbekistan                            38
    Venezuela                             18
    Vietnam                                4
    Virgin Islands                         2
    Yugoslavia                           171
    Independent Olympic Participants       4
    Zambia                                 3
    Zimbabwe                              18
    Mixed team                            38
    dtype: int64



## Part 2
For the next set of questions, we will be using census data from the [United States Census Bureau](http://www.census.gov/popest/data/counties/totals/2015/CO-EST2015-alldata.html). Counties are political and geographic subdivisions of states in the United States. This dataset contains population data for counties and states in the US from 2010 to 2015. [See this document](http://www.census.gov/popest/data/counties/totals/2015/files/CO-EST2015-alldata.pdf) for a description of the variable names.

The census dataset (census.csv) should be loaded as census_df. Answer questions using this as appropriate.

### Question 5
Which state has the most counties in it? (hint: consider the sumlevel key carefully! You'll need this for future questions too...)

*This function should return a single string value.*


```python
import numpy as np
census_df = pd.read_csv('census.csv')

census_df = census_df.set_index('CTYNAME')
    
census_df = census_df[census_df['SUMLEV'] == 50]
    
data = census_df[['POPESTIMATE2010', 'POPESTIMATE2011', 'POPESTIMATE2012', 
                      'POPESTIMATE2013', 'POPESTIMATE2014', 'POPESTIMATE2015']]

data
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>POPESTIMATE2010</th>
      <th>POPESTIMATE2011</th>
      <th>POPESTIMATE2012</th>
      <th>POPESTIMATE2013</th>
      <th>POPESTIMATE2014</th>
      <th>POPESTIMATE2015</th>
    </tr>
    <tr>
      <th>CTYNAME</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Autauga County</th>
      <td>54660</td>
      <td>55253</td>
      <td>55175</td>
      <td>55038</td>
      <td>55290</td>
      <td>55347</td>
    </tr>
    <tr>
      <th>Baldwin County</th>
      <td>183193</td>
      <td>186659</td>
      <td>190396</td>
      <td>195126</td>
      <td>199713</td>
      <td>203709</td>
    </tr>
    <tr>
      <th>Barbour County</th>
      <td>27341</td>
      <td>27226</td>
      <td>27159</td>
      <td>26973</td>
      <td>26815</td>
      <td>26489</td>
    </tr>
    <tr>
      <th>Bibb County</th>
      <td>22861</td>
      <td>22733</td>
      <td>22642</td>
      <td>22512</td>
      <td>22549</td>
      <td>22583</td>
    </tr>
    <tr>
      <th>Blount County</th>
      <td>57373</td>
      <td>57711</td>
      <td>57776</td>
      <td>57734</td>
      <td>57658</td>
      <td>57673</td>
    </tr>
    <tr>
      <th>Bullock County</th>
      <td>10887</td>
      <td>10629</td>
      <td>10606</td>
      <td>10628</td>
      <td>10829</td>
      <td>10696</td>
    </tr>
    <tr>
      <th>Butler County</th>
      <td>20944</td>
      <td>20673</td>
      <td>20408</td>
      <td>20261</td>
      <td>20276</td>
      <td>20154</td>
    </tr>
    <tr>
      <th>Calhoun County</th>
      <td>118437</td>
      <td>117768</td>
      <td>117286</td>
      <td>116575</td>
      <td>115993</td>
      <td>115620</td>
    </tr>
    <tr>
      <th>Chambers County</th>
      <td>34098</td>
      <td>33993</td>
      <td>34075</td>
      <td>34153</td>
      <td>34052</td>
      <td>34123</td>
    </tr>
    <tr>
      <th>Cherokee County</th>
      <td>25976</td>
      <td>26080</td>
      <td>26023</td>
      <td>26084</td>
      <td>25995</td>
      <td>25859</td>
    </tr>
    <tr>
      <th>Chilton County</th>
      <td>43665</td>
      <td>43739</td>
      <td>43697</td>
      <td>43795</td>
      <td>43921</td>
      <td>43943</td>
    </tr>
    <tr>
      <th>Choctaw County</th>
      <td>13841</td>
      <td>13593</td>
      <td>13543</td>
      <td>13378</td>
      <td>13289</td>
      <td>13170</td>
    </tr>
    <tr>
      <th>Clarke County</th>
      <td>25767</td>
      <td>25570</td>
      <td>25144</td>
      <td>25116</td>
      <td>24847</td>
      <td>24675</td>
    </tr>
    <tr>
      <th>Clay County</th>
      <td>13880</td>
      <td>13670</td>
      <td>13456</td>
      <td>13467</td>
      <td>13538</td>
      <td>13555</td>
    </tr>
    <tr>
      <th>Cleburne County</th>
      <td>14973</td>
      <td>14971</td>
      <td>14921</td>
      <td>15028</td>
      <td>15072</td>
      <td>15018</td>
    </tr>
    <tr>
      <th>Coffee County</th>
      <td>50177</td>
      <td>50448</td>
      <td>51173</td>
      <td>50755</td>
      <td>50831</td>
      <td>51211</td>
    </tr>
    <tr>
      <th>Colbert County</th>
      <td>54514</td>
      <td>54443</td>
      <td>54472</td>
      <td>54471</td>
      <td>54480</td>
      <td>54354</td>
    </tr>
    <tr>
      <th>Conecuh County</th>
      <td>13208</td>
      <td>13121</td>
      <td>12996</td>
      <td>12875</td>
      <td>12662</td>
      <td>12672</td>
    </tr>
    <tr>
      <th>Coosa County</th>
      <td>11758</td>
      <td>11348</td>
      <td>11195</td>
      <td>11059</td>
      <td>10807</td>
      <td>10724</td>
    </tr>
    <tr>
      <th>Covington County</th>
      <td>37796</td>
      <td>38060</td>
      <td>37818</td>
      <td>37830</td>
      <td>37888</td>
      <td>37835</td>
    </tr>
    <tr>
      <th>Crenshaw County</th>
      <td>13853</td>
      <td>13896</td>
      <td>13951</td>
      <td>13932</td>
      <td>13948</td>
      <td>13963</td>
    </tr>
    <tr>
      <th>Cullman County</th>
      <td>80473</td>
      <td>80469</td>
      <td>80374</td>
      <td>80756</td>
      <td>81221</td>
      <td>82005</td>
    </tr>
    <tr>
      <th>Dale County</th>
      <td>50358</td>
      <td>50109</td>
      <td>50324</td>
      <td>49833</td>
      <td>49501</td>
      <td>49565</td>
    </tr>
    <tr>
      <th>Dallas County</th>
      <td>43803</td>
      <td>43178</td>
      <td>42777</td>
      <td>42021</td>
      <td>41662</td>
      <td>41131</td>
    </tr>
    <tr>
      <th>DeKalb County</th>
      <td>71142</td>
      <td>71387</td>
      <td>70942</td>
      <td>70869</td>
      <td>71012</td>
      <td>71130</td>
    </tr>
    <tr>
      <th>Elmore County</th>
      <td>79465</td>
      <td>80012</td>
      <td>80432</td>
      <td>80883</td>
      <td>81022</td>
      <td>81468</td>
    </tr>
    <tr>
      <th>Escambia County</th>
      <td>38309</td>
      <td>38213</td>
      <td>38034</td>
      <td>37857</td>
      <td>37784</td>
      <td>37789</td>
    </tr>
    <tr>
      <th>Etowah County</th>
      <td>104442</td>
      <td>104236</td>
      <td>104235</td>
      <td>103852</td>
      <td>103452</td>
      <td>103057</td>
    </tr>
    <tr>
      <th>Fayette County</th>
      <td>17231</td>
      <td>17062</td>
      <td>16960</td>
      <td>16857</td>
      <td>16842</td>
      <td>16759</td>
    </tr>
    <tr>
      <th>Franklin County</th>
      <td>31734</td>
      <td>31729</td>
      <td>31648</td>
      <td>31507</td>
      <td>31592</td>
      <td>31696</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Washburn County</th>
      <td>15930</td>
      <td>15784</td>
      <td>15831</td>
      <td>15657</td>
      <td>15678</td>
      <td>15552</td>
    </tr>
    <tr>
      <th>Washington County</th>
      <td>131967</td>
      <td>132225</td>
      <td>132649</td>
      <td>132758</td>
      <td>133301</td>
      <td>133674</td>
    </tr>
    <tr>
      <th>Waukesha County</th>
      <td>390076</td>
      <td>390808</td>
      <td>392710</td>
      <td>394025</td>
      <td>395335</td>
      <td>396488</td>
    </tr>
    <tr>
      <th>Waupaca County</th>
      <td>52422</td>
      <td>52342</td>
      <td>52035</td>
      <td>52213</td>
      <td>52088</td>
      <td>51945</td>
    </tr>
    <tr>
      <th>Waushara County</th>
      <td>24506</td>
      <td>24581</td>
      <td>24484</td>
      <td>24332</td>
      <td>24173</td>
      <td>24033</td>
    </tr>
    <tr>
      <th>Winnebago County</th>
      <td>167059</td>
      <td>167630</td>
      <td>168717</td>
      <td>169486</td>
      <td>169639</td>
      <td>169546</td>
    </tr>
    <tr>
      <th>Wood County</th>
      <td>74807</td>
      <td>74647</td>
      <td>74384</td>
      <td>73996</td>
      <td>73597</td>
      <td>73435</td>
    </tr>
    <tr>
      <th>Albany County</th>
      <td>36428</td>
      <td>36908</td>
      <td>37396</td>
      <td>37647</td>
      <td>37918</td>
      <td>37956</td>
    </tr>
    <tr>
      <th>Big Horn County</th>
      <td>11672</td>
      <td>11745</td>
      <td>11785</td>
      <td>12002</td>
      <td>11919</td>
      <td>12022</td>
    </tr>
    <tr>
      <th>Campbell County</th>
      <td>46244</td>
      <td>46600</td>
      <td>47881</td>
      <td>48121</td>
      <td>48243</td>
      <td>49220</td>
    </tr>
    <tr>
      <th>Carbon County</th>
      <td>15837</td>
      <td>15817</td>
      <td>15678</td>
      <td>15787</td>
      <td>15856</td>
      <td>15559</td>
    </tr>
    <tr>
      <th>Converse County</th>
      <td>13826</td>
      <td>13728</td>
      <td>14025</td>
      <td>14343</td>
      <td>14172</td>
      <td>14236</td>
    </tr>
    <tr>
      <th>Crook County</th>
      <td>7114</td>
      <td>7129</td>
      <td>7148</td>
      <td>7160</td>
      <td>7264</td>
      <td>7444</td>
    </tr>
    <tr>
      <th>Fremont County</th>
      <td>40222</td>
      <td>40591</td>
      <td>41129</td>
      <td>41024</td>
      <td>40717</td>
      <td>40315</td>
    </tr>
    <tr>
      <th>Goshen County</th>
      <td>13408</td>
      <td>13597</td>
      <td>13666</td>
      <td>13565</td>
      <td>13509</td>
      <td>13383</td>
    </tr>
    <tr>
      <th>Hot Springs County</th>
      <td>4813</td>
      <td>4818</td>
      <td>4846</td>
      <td>4846</td>
      <td>4793</td>
      <td>4741</td>
    </tr>
    <tr>
      <th>Johnson County</th>
      <td>8581</td>
      <td>8636</td>
      <td>8610</td>
      <td>8619</td>
      <td>8552</td>
      <td>8585</td>
    </tr>
    <tr>
      <th>Laramie County</th>
      <td>92271</td>
      <td>92663</td>
      <td>94894</td>
      <td>96006</td>
      <td>96469</td>
      <td>97121</td>
    </tr>
    <tr>
      <th>Lincoln County</th>
      <td>18091</td>
      <td>18022</td>
      <td>17943</td>
      <td>18328</td>
      <td>18564</td>
      <td>18722</td>
    </tr>
    <tr>
      <th>Natrona County</th>
      <td>75472</td>
      <td>76420</td>
      <td>78699</td>
      <td>81156</td>
      <td>81603</td>
      <td>82178</td>
    </tr>
    <tr>
      <th>Niobrara County</th>
      <td>2492</td>
      <td>2485</td>
      <td>2475</td>
      <td>2548</td>
      <td>2530</td>
      <td>2542</td>
    </tr>
    <tr>
      <th>Park County</th>
      <td>28259</td>
      <td>28473</td>
      <td>28863</td>
      <td>29237</td>
      <td>29126</td>
      <td>29228</td>
    </tr>
    <tr>
      <th>Platte County</th>
      <td>8678</td>
      <td>8701</td>
      <td>8732</td>
      <td>8728</td>
      <td>8776</td>
      <td>8812</td>
    </tr>
    <tr>
      <th>Sheridan County</th>
      <td>29146</td>
      <td>29275</td>
      <td>29594</td>
      <td>29794</td>
      <td>30020</td>
      <td>30009</td>
    </tr>
    <tr>
      <th>Sublette County</th>
      <td>10244</td>
      <td>10142</td>
      <td>10418</td>
      <td>10086</td>
      <td>10039</td>
      <td>9899</td>
    </tr>
    <tr>
      <th>Sweetwater County</th>
      <td>43593</td>
      <td>44041</td>
      <td>45104</td>
      <td>45162</td>
      <td>44925</td>
      <td>44626</td>
    </tr>
    <tr>
      <th>Teton County</th>
      <td>21297</td>
      <td>21482</td>
      <td>21697</td>
      <td>22347</td>
      <td>22905</td>
      <td>23125</td>
    </tr>
    <tr>
      <th>Uinta County</th>
      <td>21102</td>
      <td>20912</td>
      <td>20989</td>
      <td>21022</td>
      <td>20903</td>
      <td>20822</td>
    </tr>
    <tr>
      <th>Washakie County</th>
      <td>8545</td>
      <td>8469</td>
      <td>8443</td>
      <td>8443</td>
      <td>8316</td>
      <td>8328</td>
    </tr>
    <tr>
      <th>Weston County</th>
      <td>7181</td>
      <td>7114</td>
      <td>7065</td>
      <td>7160</td>
      <td>7185</td>
      <td>7234</td>
    </tr>
  </tbody>
</table>
<p>3142 rows × 6 columns</p>
</div>




```python
def answer_five():
    census_df = pd.read_csv('census.csv')
    unique_states = census_df['STNAME'].unique()
    census_df = census_df.set_index(['STNAME', 'CTYNAME'])
    d = {}
    for state in unique_states:
        d[state] = census_df.loc[state]['STATE'].count()
        
    s = pd.Series(d)
    ss = s.sort_values(ascending=False)
    
    return ss.index[0]

answer_five()
```




    'Texas'



### Question 6
Only looking at the three most populous counties for each state, what are the three most populous states (in order of highest population to lowest population)? Use `CENSUS2010POP`.

*This function should return a list of string values.*


```python
import numpy as np

def answer_six():
    census_df = pd.read_csv('census.csv')
    
    sumlev = census_df.SUMLEV.values == 50
    data = census_df[['CENSUS2010POP', 'STNAME', 'CTYNAME']].values[sumlev]

    s = pd.Series(data[:, 0], [data[:, 1], data[:, 2]], dtype=np.int64)

    def sum_largest(x, n=3):
        return x.nlargest(n).sum()

    return s.groupby(level=0).apply(sum_largest).nlargest(3).index.tolist()

answer_six()
```




    ['California', 'Texas', 'Illinois']



### Question 7
Which county has had the largest absolute change in population within the period 2010-2015? (Hint: population values are stored in columns POPESTIMATE2010 through POPESTIMATE2015, you need to consider all six columns.)

e.g. If County Population in the 5 year period is 100, 120, 80, 105, 100, 130, then its largest change in the period would be |130-80| = 50.

*This function should return a single string value.*


```python
def answer_seven():
    census_df = pd.read_csv('census.csv')

    sumlev = census_df.SUMLEV.values == 50
    census_df = census_df.set_index('CTYNAME')
    
    census_df = census_df[census_df['SUMLEV'] == 50]
    
    data = census_df[['POPESTIMATE2010', 'POPESTIMATE2011', 'POPESTIMATE2012', 
                      'POPESTIMATE2013', 'POPESTIMATE2014', 'POPESTIMATE2015']]
    
    def abs_change(x):
        return x.max() - x.min()
    
    return data.apply(abs_change, axis=1).nlargest(1).index[0]

answer_seven()
```




    'Harris County'



### Question 8
In this datafile, the United States is broken up into four regions using the "REGION" column. 

Create a query that finds the counties that belong to regions 1 or 2, whose name starts with 'Washington', and whose POPESTIMATE2015 was greater than their POPESTIMATE 2014.

*This function should return a 5x2 DataFrame with the columns = ['STNAME', 'CTYNAME'] and the same index ID as the census_df (sorted ascending by index).*


```python
def answer_eight():
    census_df = pd.read_csv('census.csv')
    census_df = census_df.set_index('CTYNAME')
    census_df = census_df[((census_df['REGION'] == 1) | (census_df['REGION'] == 2)) & 
           ((census_df['POPESTIMATE2015']) > (census_df['POPESTIMATE2014'])) ]

    census_df = census_df[census_df.index.map(lambda s: s.startswith('Washington'))]
    census_df = census_df.reset_index()
    census_df = census_df.sort_index()
    
    return census_df[['STNAME', 'CTYNAME']]

answer_eight()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>STNAME</th>
      <th>CTYNAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Iowa</td>
      <td>Washington County</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Minnesota</td>
      <td>Washington County</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Pennsylvania</td>
      <td>Washington County</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rhode Island</td>
      <td>Washington County</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wisconsin</td>
      <td>Washington County</td>
    </tr>
  </tbody>
</table>
</div>


