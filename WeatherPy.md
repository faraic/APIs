
# WeatherPy
----

### Analysis
* As expected, the weather becomes significantly warmer as one approaches the equator (0 Deg. Latitude). More interestingly, however, is the fact that the southern hemisphere tends to be warmer this time of year than the northern hemisphere. This may be due to the tilt of the earth.
* There is no strong relationship between latitude and cloudiness. However, it is interesting to see that a strong band of cities sits at 0, 80, and 100% cloudiness.
* There is no strong relationship between latitude and wind speed. However, in northern hemispheres there is a flurry of cities with over 20 mph of wind.

---

#### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
import json

# Import API key
from api_keys import api_key

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []
countries=[]
lat=[]
long=[]
# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1])
  
    cityname=city.city_name
    countryname=city.country_code

# If the city is unique, then add it to a our cities list
    if cityname not in cities:
        cities.append(cityname)
        countries.append(countryname)
        lat.append(lat_lng[0])
        long.append(lat_lng[1])

```


```python
# len(latitude)
len(lat)
```




    609




```python
# len(cities)
len(cities)
```




    609




```python
# len(countries)
len(countries)
```




    609




```python
CityData_df=pd.DataFrame({"City": cities, "Country": countries, "Latitude":lat, "Longitude":long})
pd.options.display.float_format = "{:,.2f}".format
CityData_df
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>san carlos de bariloche</td>
      <td>ar</td>
      <td>-40.47</td>
      <td>-69.58</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-76.56</td>
      <td>-46.92</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tarko-sale</td>
      <td>ru</td>
      <td>63.98</td>
      <td>79.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-6.80</td>
      <td>-133.10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pinotepa nacional</td>
      <td>mx</td>
      <td>14.28</td>
      <td>-98.88</td>
    </tr>
    <tr>
      <th>5</th>
      <td>yellowknife</td>
      <td>ca</td>
      <td>79.87</td>
      <td>-108.61</td>
    </tr>
    <tr>
      <th>6</th>
      <td>amderma</td>
      <td>ru</td>
      <td>76.30</td>
      <td>59.23</td>
    </tr>
    <tr>
      <th>7</th>
      <td>itoman</td>
      <td>jp</td>
      <td>22.81</td>
      <td>129.11</td>
    </tr>
    <tr>
      <th>8</th>
      <td>albany</td>
      <td>au</td>
      <td>-62.94</td>
      <td>124.79</td>
    </tr>
    <tr>
      <th>9</th>
      <td>barabinsk</td>
      <td>ru</td>
      <td>55.25</td>
      <td>78.34</td>
    </tr>
    <tr>
      <th>10</th>
      <td>bargal</td>
      <td>so</td>
      <td>11.58</td>
      <td>57.33</td>
    </tr>
    <tr>
      <th>11</th>
      <td>portoferraio</td>
      <td>it</td>
      <td>41.98</td>
      <td>10.49</td>
    </tr>
    <tr>
      <th>12</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>10.69</td>
      <td>-118.56</td>
    </tr>
    <tr>
      <th>13</th>
      <td>cape town</td>
      <td>za</td>
      <td>-59.87</td>
      <td>-5.43</td>
    </tr>
    <tr>
      <th>14</th>
      <td>roma</td>
      <td>au</td>
      <td>-27.18</td>
      <td>144.64</td>
    </tr>
    <tr>
      <th>15</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-60.63</td>
      <td>-99.05</td>
    </tr>
    <tr>
      <th>16</th>
      <td>samalaeulu</td>
      <td>ws</td>
      <td>-5.50</td>
      <td>-167.38</td>
    </tr>
    <tr>
      <th>17</th>
      <td>jati</td>
      <td>id</td>
      <td>-6.38</td>
      <td>110.97</td>
    </tr>
    <tr>
      <th>18</th>
      <td>temryuk</td>
      <td>ru</td>
      <td>45.44</td>
      <td>37.48</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ilulissat</td>
      <td>gl</td>
      <td>87.37</td>
      <td>-41.71</td>
    </tr>
    <tr>
      <th>20</th>
      <td>qaanaaq</td>
      <td>gl</td>
      <td>74.10</td>
      <td>-88.97</td>
    </tr>
    <tr>
      <th>21</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-31.04</td>
      <td>-122.22</td>
    </tr>
    <tr>
      <th>22</th>
      <td>saint-michel-des-saints</td>
      <td>ca</td>
      <td>47.42</td>
      <td>-73.94</td>
    </tr>
    <tr>
      <th>23</th>
      <td>cherskiy</td>
      <td>ru</td>
      <td>82.85</td>
      <td>161.61</td>
    </tr>
    <tr>
      <th>24</th>
      <td>vurnary</td>
      <td>ru</td>
      <td>55.67</td>
      <td>47.00</td>
    </tr>
    <tr>
      <th>25</th>
      <td>kodiak</td>
      <td>us</td>
      <td>44.17</td>
      <td>-161.62</td>
    </tr>
    <tr>
      <th>26</th>
      <td>chiang rai</td>
      <td>th</td>
      <td>19.87</td>
      <td>100.16</td>
    </tr>
    <tr>
      <th>27</th>
      <td>nokha</td>
      <td>in</td>
      <td>27.23</td>
      <td>73.05</td>
    </tr>
    <tr>
      <th>28</th>
      <td>isangel</td>
      <td>vu</td>
      <td>-18.29</td>
      <td>176.50</td>
    </tr>
    <tr>
      <th>29</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-43.54</td>
      <td>-156.60</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>579</th>
      <td>alenquer</td>
      <td>br</td>
      <td>0.17</td>
      <td>-54.35</td>
    </tr>
    <tr>
      <th>580</th>
      <td>sturgis</td>
      <td>us</td>
      <td>44.64</td>
      <td>-103.44</td>
    </tr>
    <tr>
      <th>581</th>
      <td>basarabeasca</td>
      <td>md</td>
      <td>46.55</td>
      <td>29.03</td>
    </tr>
    <tr>
      <th>582</th>
      <td>yulara</td>
      <td>au</td>
      <td>-24.11</td>
      <td>127.97</td>
    </tr>
    <tr>
      <th>583</th>
      <td>zwedru</td>
      <td>lr</td>
      <td>5.45</td>
      <td>-8.98</td>
    </tr>
    <tr>
      <th>584</th>
      <td>cankuzo</td>
      <td>bi</td>
      <td>-3.19</td>
      <td>30.74</td>
    </tr>
    <tr>
      <th>585</th>
      <td>quepos</td>
      <td>cr</td>
      <td>8.24</td>
      <td>-84.85</td>
    </tr>
    <tr>
      <th>586</th>
      <td>barbar</td>
      <td>sd</td>
      <td>17.34</td>
      <td>34.76</td>
    </tr>
    <tr>
      <th>587</th>
      <td>kaka</td>
      <td>tm</td>
      <td>37.43</td>
      <td>60.44</td>
    </tr>
    <tr>
      <th>588</th>
      <td>atar</td>
      <td>mr</td>
      <td>21.01</td>
      <td>-14.81</td>
    </tr>
    <tr>
      <th>589</th>
      <td>bonnyville</td>
      <td>ca</td>
      <td>55.16</td>
      <td>-110.84</td>
    </tr>
    <tr>
      <th>590</th>
      <td>buala</td>
      <td>sb</td>
      <td>-6.33</td>
      <td>159.88</td>
    </tr>
    <tr>
      <th>591</th>
      <td>brigantine</td>
      <td>us</td>
      <td>38.17</td>
      <td>-72.65</td>
    </tr>
    <tr>
      <th>592</th>
      <td>almenara</td>
      <td>br</td>
      <td>-16.38</td>
      <td>-40.72</td>
    </tr>
    <tr>
      <th>593</th>
      <td>urucui</td>
      <td>br</td>
      <td>-7.09</td>
      <td>-44.93</td>
    </tr>
    <tr>
      <th>594</th>
      <td>harsewinkel</td>
      <td>de</td>
      <td>51.95</td>
      <td>8.25</td>
    </tr>
    <tr>
      <th>595</th>
      <td>mana</td>
      <td>gf</td>
      <td>11.06</td>
      <td>-52.49</td>
    </tr>
    <tr>
      <th>596</th>
      <td>tuburan</td>
      <td>ph</td>
      <td>6.25</td>
      <td>122.60</td>
    </tr>
    <tr>
      <th>597</th>
      <td>tumarbong</td>
      <td>ph</td>
      <td>10.32</td>
      <td>119.53</td>
    </tr>
    <tr>
      <th>598</th>
      <td>morehead</td>
      <td>pg</td>
      <td>-7.70</td>
      <td>141.85</td>
    </tr>
    <tr>
      <th>599</th>
      <td>alexandria</td>
      <td>ca</td>
      <td>45.37</td>
      <td>-74.64</td>
    </tr>
    <tr>
      <th>600</th>
      <td>christchurch</td>
      <td>nz</td>
      <td>-45.74</td>
      <td>176.16</td>
    </tr>
    <tr>
      <th>601</th>
      <td>ko samui</td>
      <td>th</td>
      <td>9.89</td>
      <td>99.89</td>
    </tr>
    <tr>
      <th>602</th>
      <td>harper</td>
      <td>lr</td>
      <td>-2.20</td>
      <td>-8.35</td>
    </tr>
    <tr>
      <th>603</th>
      <td>bima</td>
      <td>id</td>
      <td>-9.18</td>
      <td>118.60</td>
    </tr>
    <tr>
      <th>604</th>
      <td>kaele</td>
      <td>cm</td>
      <td>9.69</td>
      <td>14.52</td>
    </tr>
    <tr>
      <th>605</th>
      <td>grand-santi</td>
      <td>gf</td>
      <td>2.63</td>
      <td>-56.39</td>
    </tr>
    <tr>
      <th>606</th>
      <td>tomatlan</td>
      <td>mx</td>
      <td>18.18</td>
      <td>-107.64</td>
    </tr>
    <tr>
      <th>607</th>
      <td>pozo colorado</td>
      <td>py</td>
      <td>-24.97</td>
      <td>-59.61</td>
    </tr>
    <tr>
      <th>608</th>
      <td>santa fe</td>
      <td>ar</td>
      <td>-31.31</td>
      <td>-60.56</td>
    </tr>
  </tbody>
</table>
<p>609 rows × 4 columns</p>
</div>




```python
CityData_df["Date"]=""
CityData_df["Cloudiness"]=""
CityData_df["Humidity"]=""
CityData_df["Max Temp"]=""
CityData_df["Wind Speed"]=""

```

### Perform API Calls
* Perform a weather check on each city using a series of successive API calls.
* Include a print log of each city as it'sbeing processed (with the city number and city name).



```python
url="http://api.openweathermap.org/data/2.5/weather?"
units="imperial"
setcounter=1
counter=0
restcounter=0

print("Beginning Data Retrieval")
print("-------------------------")

for index, row in CityData_df.iterrows():
    
    try:
        
        query = url+"&APPID="+api_key+"&q="+row["City"].replace(" ","+")+","+row["Country"]+"&units="+units
        resp = requests.get(query).json()
        
        Date = resp["dt"]
        Cloudiness = resp["clouds"]["all"]
        Humidity = resp["main"]["humidity"]
        MaxTemperature = resp["main"]["temp_max"]            
        WindSpeed = resp["wind"]["speed"]
            
        CityData_df.set_value(index,"Date",Date,takeable=False)
        CityData_df.set_value(index,"Cloudiness",Cloudiness,takeable=False)
        CityData_df.set_value(index,"Humidity",Humidity,takeable=False)
        CityData_df.set_value(index,"Max Temp",MaxTemperature,takeable=False)
        CityData_df.set_value(index,"Wind Speed",WindSpeed,takeable=False)
        
    except:
        
        print("City not found. Skipping...")
        
    restcounter=restcounter+1
    counter=counter+1

    if restcounter==50:
        time.sleep(5)
        setcounter=setcounter+1
        restcounter=0
        counter=1
        
    print("Processing Record "+str(counter)+" of set "+ str(setcounter)+ " | "+ row["City"])
#     mute the query to hide the APPID
#     print(query)
```

    Beginning Data Retrieval
    -------------------------
    

    C:\Users\farai\Anaconda3\lib\site-packages\ipykernel_launcher.py:23: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\farai\Anaconda3\lib\site-packages\ipykernel_launcher.py:24: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\farai\Anaconda3\lib\site-packages\ipykernel_launcher.py:25: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\farai\Anaconda3\lib\site-packages\ipykernel_launcher.py:26: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\farai\Anaconda3\lib\site-packages\ipykernel_launcher.py:27: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    

    Processing Record 1 of set 1 | san carlos de bariloche
    Processing Record 2 of set 1 | ushuaia
    Processing Record 3 of set 1 | tarko-sale
    Processing Record 4 of set 1 | atuona
    City not found. Skipping...
    Processing Record 5 of set 1 | pinotepa nacional
    Processing Record 6 of set 1 | yellowknife
    City not found. Skipping...
    Processing Record 7 of set 1 | amderma
    Processing Record 8 of set 1 | itoman
    Processing Record 9 of set 1 | albany
    Processing Record 10 of set 1 | barabinsk
    City not found. Skipping...
    Processing Record 11 of set 1 | bargal
    Processing Record 12 of set 1 | portoferraio
    Processing Record 13 of set 1 | cabo san lucas
    Processing Record 14 of set 1 | cape town
    Processing Record 15 of set 1 | roma
    Processing Record 16 of set 1 | punta arenas
    City not found. Skipping...
    Processing Record 17 of set 1 | samalaeulu
    Processing Record 18 of set 1 | jati
    Processing Record 19 of set 1 | temryuk
    Processing Record 20 of set 1 | ilulissat
    Processing Record 21 of set 1 | qaanaaq
    Processing Record 22 of set 1 | rikitea
    Processing Record 23 of set 1 | saint-michel-des-saints
    Processing Record 24 of set 1 | cherskiy
    Processing Record 25 of set 1 | vurnary
    Processing Record 26 of set 1 | kodiak
    Processing Record 27 of set 1 | chiang rai
    Processing Record 28 of set 1 | nokha
    Processing Record 29 of set 1 | isangel
    City not found. Skipping...
    Processing Record 30 of set 1 | mataura
    Processing Record 31 of set 1 | sarab
    Processing Record 32 of set 1 | lebedyn
    Processing Record 33 of set 1 | hilo
    City not found. Skipping...
    Processing Record 34 of set 1 | taolanaro
    Processing Record 35 of set 1 | busselton
    City not found. Skipping...
    Processing Record 36 of set 1 | tome-acu
    City not found. Skipping...
    Processing Record 37 of set 1 | san quintin
    Processing Record 38 of set 1 | mahebourg
    Processing Record 39 of set 1 | new norfolk
    Processing Record 40 of set 1 | shache
    Processing Record 41 of set 1 | tuntum
    Processing Record 42 of set 1 | strezhevoy
    Processing Record 43 of set 1 | pevek
    Processing Record 44 of set 1 | bluff
    Processing Record 45 of set 1 | hermanus
    Processing Record 46 of set 1 | sobolevo
    Processing Record 47 of set 1 | souillac
    Processing Record 48 of set 1 | vao
    Processing Record 49 of set 1 | dikson
    City not found. Skipping...
    Processing Record 1 of set 2 | barentsburg
    City not found. Skipping...
    Processing Record 2 of set 2 | meyungs
    Processing Record 3 of set 2 | talnakh
    Processing Record 4 of set 2 | umm lajj
    Processing Record 5 of set 2 | mount gambier
    Processing Record 6 of set 2 | hobart
    Processing Record 7 of set 2 | viedma
    Processing Record 8 of set 2 | olinda
    Processing Record 9 of set 2 | korla
    Processing Record 10 of set 2 | manacapuru
    City not found. Skipping...
    Processing Record 11 of set 2 | bolungarvik
    Processing Record 12 of set 2 | saint-philippe
    Processing Record 13 of set 2 | nikolskoye
    Processing Record 14 of set 2 | port elizabeth
    Processing Record 15 of set 2 | mangai
    Processing Record 16 of set 2 | pulaski
    Processing Record 17 of set 2 | labytnangi
    Processing Record 18 of set 2 | lebu
    City not found. Skipping...
    Processing Record 19 of set 2 | wahran
    Processing Record 20 of set 2 | jamestown
    Processing Record 21 of set 2 | ponta delgada
    Processing Record 22 of set 2 | tuatapere
    Processing Record 23 of set 2 | king city
    Processing Record 24 of set 2 | valparaiso
    Processing Record 25 of set 2 | teguise
    Processing Record 26 of set 2 | port lincoln
    Processing Record 27 of set 2 | mamakan
    Processing Record 28 of set 2 | tagusao
    Processing Record 29 of set 2 | nizhniy kuranakh
    Processing Record 30 of set 2 | naze
    Processing Record 31 of set 2 | marawi
    City not found. Skipping...
    Processing Record 32 of set 2 | malwan
    Processing Record 33 of set 2 | ixtapa
    Processing Record 34 of set 2 | sinnamary
    Processing Record 35 of set 2 | troitsko-pechorsk
    City not found. Skipping...
    Processing Record 36 of set 2 | illoqqortoormiut
    Processing Record 37 of set 2 | chuy
    Processing Record 38 of set 2 | la celia
    Processing Record 39 of set 2 | malpe
    Processing Record 40 of set 2 | kushima
    Processing Record 41 of set 2 | kaitangata
    Processing Record 42 of set 2 | bandarbeyla
    Processing Record 43 of set 2 | talaya
    Processing Record 44 of set 2 | lazaro cardenas
    Processing Record 45 of set 2 | dunedin
    Processing Record 46 of set 2 | mecca
    Processing Record 47 of set 2 | nueve de julio
    Processing Record 48 of set 2 | kapaa
    Processing Record 49 of set 2 | torbay
    Processing Record 50 of set 2 | homer
    City not found. Skipping...
    Processing Record 1 of set 3 | caceres
    City not found. Skipping...
    Processing Record 2 of set 3 | hillsborough
    Processing Record 3 of set 3 | ponta do sol
    Processing Record 4 of set 3 | beloha
    Processing Record 5 of set 3 | marquette
    Processing Record 6 of set 3 | colares
    Processing Record 7 of set 3 | the pas
    Processing Record 8 of set 3 | kavieng
    Processing Record 9 of set 3 | ahipara
    Processing Record 10 of set 3 | ambilobe
    Processing Record 11 of set 3 | lagoa
    Processing Record 12 of set 3 | guerrero negro
    City not found. Skipping...
    Processing Record 13 of set 3 | belushya guba
    City not found. Skipping...
    Processing Record 14 of set 3 | mys shmidta
    Processing Record 15 of set 3 | esperance
    City not found. Skipping...
    Processing Record 16 of set 3 | marcona
    Processing Record 17 of set 3 | port alfred
    City not found. Skipping...
    Processing Record 18 of set 3 | grand river south east
    Processing Record 19 of set 3 | puerto ayora
    Processing Record 20 of set 3 | buraydah
    Processing Record 21 of set 3 | hithadhoo
    Processing Record 22 of set 3 | barrow
    Processing Record 23 of set 3 | brae
    Processing Record 24 of set 3 | maneadero
    Processing Record 25 of set 3 | kamaishi
    Processing Record 26 of set 3 | poum
    Processing Record 27 of set 3 | camara de lobos
    Processing Record 28 of set 3 | constitucion
    Processing Record 29 of set 3 | placetas
    Processing Record 30 of set 3 | arraial do cabo
    Processing Record 31 of set 3 | broome
    Processing Record 32 of set 3 | flinders
    Processing Record 33 of set 3 | miracema do tocantins
    Processing Record 34 of set 3 | la rioja
    Processing Record 35 of set 3 | yangcun
    Processing Record 36 of set 3 | araouane
    Processing Record 37 of set 3 | onega
    Processing Record 38 of set 3 | sao joao da barra
    City not found. Skipping...
    Processing Record 39 of set 3 | saleaula
    Processing Record 40 of set 3 | cidreira
    Processing Record 41 of set 3 | san cristobal
    Processing Record 42 of set 3 | port hardy
    Processing Record 43 of set 3 | tuktoyaktuk
    City not found. Skipping...
    Processing Record 44 of set 3 | alotau
    Processing Record 45 of set 3 | jalu
    Processing Record 46 of set 3 | turukhansk
    Processing Record 47 of set 3 | vaini
    Processing Record 48 of set 3 | san andres
    Processing Record 49 of set 3 | fortuna
    Processing Record 50 of set 3 | te anau
    City not found. Skipping...
    Processing Record 1 of set 4 | siwani
    Processing Record 2 of set 4 | cave spring
    Processing Record 3 of set 4 | aklavik
    Processing Record 4 of set 4 | muros
    Processing Record 5 of set 4 | westport
    Processing Record 6 of set 4 | shubarshi
    Processing Record 7 of set 4 | bredasdorp
    Processing Record 8 of set 4 | oranjemund
    Processing Record 9 of set 4 | castro
    Processing Record 10 of set 4 | la asuncion
    Processing Record 11 of set 4 | kipit
    Processing Record 12 of set 4 | lorengau
    Processing Record 13 of set 4 | sorong
    Processing Record 14 of set 4 | san patricio
    Processing Record 15 of set 4 | upernavik
    Processing Record 16 of set 4 | komsomolskiy
    Processing Record 17 of set 4 | paranhos
    City not found. Skipping...
    Processing Record 18 of set 4 | sataua
    Processing Record 19 of set 4 | eydhafushi
    Processing Record 20 of set 4 | sitka
    Processing Record 21 of set 4 | fare
    Processing Record 22 of set 4 | hammerfest
    City not found. Skipping...
    Processing Record 23 of set 4 | sinkat
    Processing Record 24 of set 4 | saint-augustin
    Processing Record 25 of set 4 | locri
    City not found. Skipping...
    Processing Record 26 of set 4 | vaitupu
    Processing Record 27 of set 4 | yima
    Processing Record 28 of set 4 | east london
    Processing Record 29 of set 4 | cockburn town
    Processing Record 30 of set 4 | anito
    Processing Record 31 of set 4 | butaritari
    Processing Record 32 of set 4 | carnarvon
    City not found. Skipping...
    Processing Record 33 of set 4 | nizhneyansk
    Processing Record 34 of set 4 | sovetskiy
    Processing Record 35 of set 4 | victoria
    City not found. Skipping...
    Processing Record 36 of set 4 | gat
    Processing Record 37 of set 4 | cayenne
    Processing Record 38 of set 4 | tasiilaq
    City not found. Skipping...
    Processing Record 39 of set 4 | bengkulu
    Processing Record 40 of set 4 | hasaki
    Processing Record 41 of set 4 | avarua
    Processing Record 42 of set 4 | klaksvik
    Processing Record 43 of set 4 | klyuchi
    Processing Record 44 of set 4 | moguer
    Processing Record 45 of set 4 | pundaguitan
    City not found. Skipping...
    Processing Record 46 of set 4 | tsihombe
    Processing Record 47 of set 4 | kargil
    City not found. Skipping...
    Processing Record 48 of set 4 | asau
    Processing Record 49 of set 4 | caravelas
    Processing Record 50 of set 4 | ribeira grande
    Processing Record 1 of set 5 | longyearbyen
    Processing Record 2 of set 5 | provideniya
    Processing Record 3 of set 5 | yumen
    Processing Record 4 of set 5 | kalevala
    Processing Record 5 of set 5 | nefteyugansk
    Processing Record 6 of set 5 | north platte
    Processing Record 7 of set 5 | remontnoye
    Processing Record 8 of set 5 | mount isa
    Processing Record 9 of set 5 | tinaquillo
    Processing Record 10 of set 5 | saint-francois
    Processing Record 11 of set 5 | puerto leguizamo
    Processing Record 12 of set 5 | haimen
    Processing Record 13 of set 5 | oksfjord
    Processing Record 14 of set 5 | hervey bay
    Processing Record 15 of set 5 | dukat
    Processing Record 16 of set 5 | awjilah
    Processing Record 17 of set 5 | oussouye
    Processing Record 18 of set 5 | georgetown
    Processing Record 19 of set 5 | sherghati
    Processing Record 20 of set 5 | elizabeth city
    Processing Record 21 of set 5 | aksarka
    Processing Record 22 of set 5 | velky tynec
    City not found. Skipping...
    Processing Record 23 of set 5 | codrington
    Processing Record 24 of set 5 | barra do corda
    City not found. Skipping...
    Processing Record 25 of set 5 | olafsvik
    Processing Record 26 of set 5 | araguaina
    Processing Record 27 of set 5 | tiksi
    Processing Record 28 of set 5 | bom jesus
    Processing Record 29 of set 5 | assiniboia
    City not found. Skipping...
    Processing Record 30 of set 5 | urumqi
    Processing Record 31 of set 5 | togur
    Processing Record 32 of set 5 | labuhan
    Processing Record 33 of set 5 | bambous virieux
    Processing Record 34 of set 5 | mar del plata
    Processing Record 35 of set 5 | jiwani
    Processing Record 36 of set 5 | saint-pierre
    Processing Record 37 of set 5 | burnie
    Processing Record 38 of set 5 | connersville
    Processing Record 39 of set 5 | garowe
    Processing Record 40 of set 5 | bethel
    Processing Record 41 of set 5 | katsuura
    Processing Record 42 of set 5 | marystown
    City not found. Skipping...
    Processing Record 43 of set 5 | yaan
    Processing Record 44 of set 5 | verkhnevilyuysk
    Processing Record 45 of set 5 | cabedelo
    Processing Record 46 of set 5 | namibe
    City not found. Skipping...
    Processing Record 47 of set 5 | ngukurr
    Processing Record 48 of set 5 | thompson
    Processing Record 49 of set 5 | presidencia roque saenz pena
    City not found. Skipping...
    Processing Record 50 of set 5 | lolua
    Processing Record 1 of set 6 | feodosiya
    Processing Record 2 of set 6 | nago
    Processing Record 3 of set 6 | south river
    Processing Record 4 of set 6 | khatanga
    Processing Record 5 of set 6 | key biscayne
    City not found. Skipping...
    Processing Record 6 of set 6 | pakwach
    Processing Record 7 of set 6 | slave lake
    Processing Record 8 of set 6 | egvekinot
    Processing Record 9 of set 6 | nyzhnya duvanka
    Processing Record 10 of set 6 | odessa
    City not found. Skipping...
    Processing Record 11 of set 6 | marzuq
    City not found. Skipping...
    Processing Record 12 of set 6 | sakakah
    Processing Record 13 of set 6 | hirara
    Processing Record 14 of set 6 | bahia de caraquez
    Processing Record 15 of set 6 | zinder
    Processing Record 16 of set 6 | mareeba
    Processing Record 17 of set 6 | kjollefjord
    Processing Record 18 of set 6 | tokur
    Processing Record 19 of set 6 | leshukonskoye
    Processing Record 20 of set 6 | geraldton
    Processing Record 21 of set 6 | parkes
    Processing Record 22 of set 6 | iqaluit
    Processing Record 23 of set 6 | panlaitan
    Processing Record 24 of set 6 | agua verde
    Processing Record 25 of set 6 | jablah
    Processing Record 26 of set 6 | abu dhabi
    Processing Record 27 of set 6 | alice springs
    Processing Record 28 of set 6 | bulgan
    Processing Record 29 of set 6 | mogadishu
    Processing Record 30 of set 6 | iquitos
    Processing Record 31 of set 6 | mbini
    Processing Record 32 of set 6 | arenapolis
    Processing Record 33 of set 6 | tahe
    Processing Record 34 of set 6 | zabol
    Processing Record 35 of set 6 | kichera
    Processing Record 36 of set 6 | canavieiras
    Processing Record 37 of set 6 | luderitz
    Processing Record 38 of set 6 | bayangol
    Processing Record 39 of set 6 | henties bay
    Processing Record 40 of set 6 | dicabisagan
    Processing Record 41 of set 6 | norman wells
    City not found. Skipping...
    Processing Record 42 of set 6 | faya
    City not found. Skipping...
    Processing Record 43 of set 6 | warqla
    Processing Record 44 of set 6 | hare bay
    Processing Record 45 of set 6 | langsa
    City not found. Skipping...
    Processing Record 46 of set 6 | formoso do araguaia
    Processing Record 47 of set 6 | kruisfontein
    Processing Record 48 of set 6 | carutapera
    Processing Record 49 of set 6 | hamilton
    Processing Record 50 of set 6 | meulaboh
    Processing Record 1 of set 7 | visby
    Processing Record 2 of set 7 | tura
    Processing Record 3 of set 7 | kez
    Processing Record 4 of set 7 | wonthaggi
    Processing Record 5 of set 7 | lock haven
    City not found. Skipping...
    Processing Record 6 of set 7 | sentyabrskiy
    City not found. Skipping...
    Processing Record 7 of set 7 | puerto cortes
    Processing Record 8 of set 7 | sao filipe
    Processing Record 9 of set 7 | nanortalik
    Processing Record 10 of set 7 | baykit
    Processing Record 11 of set 7 | saskylakh
    Processing Record 12 of set 7 | amarpur
    Processing Record 13 of set 7 | camopi
    City not found. Skipping...
    Processing Record 14 of set 7 | tumannyy
    Processing Record 15 of set 7 | gangapur
    Processing Record 16 of set 7 | uusikaupunki
    Processing Record 17 of set 7 | muyezerskiy
    Processing Record 18 of set 7 | omboue
    Processing Record 19 of set 7 | pathein
    Processing Record 20 of set 7 | dingle
    City not found. Skipping...
    Processing Record 21 of set 7 | sokoni
    Processing Record 22 of set 7 | mandiana
    Processing Record 23 of set 7 | vyborg
    Processing Record 24 of set 7 | los llanos de aridane
    Processing Record 25 of set 7 | otradnoye
    Processing Record 26 of set 7 | zalantun
    Processing Record 27 of set 7 | narsaq
    Processing Record 28 of set 7 | chadan
    Processing Record 29 of set 7 | santa maria
    City not found. Skipping...
    Processing Record 30 of set 7 | vestbygda
    Processing Record 31 of set 7 | calbuco
    Processing Record 32 of set 7 | cururupu
    City not found. Skipping...
    Processing Record 33 of set 7 | mormugao
    Processing Record 34 of set 7 | mehamn
    Processing Record 35 of set 7 | roald
    Processing Record 36 of set 7 | paraiso
    Processing Record 37 of set 7 | pontianak
    City not found. Skipping...
    Processing Record 38 of set 7 | esna
    Processing Record 39 of set 7 | fukue
    Processing Record 40 of set 7 | sanchazi
    Processing Record 41 of set 7 | kahului
    Processing Record 42 of set 7 | vuyyuru
    Processing Record 43 of set 7 | salinopolis
    Processing Record 44 of set 7 | kemlya
    Processing Record 45 of set 7 | kropotkin
    Processing Record 46 of set 7 | gornopravdinsk
    Processing Record 47 of set 7 | pacific grove
    Processing Record 48 of set 7 | villa maria
    Processing Record 49 of set 7 | bairnsdale
    Processing Record 50 of set 7 | minbu
    City not found. Skipping...
    Processing Record 1 of set 8 | lata
    City not found. Skipping...
    Processing Record 2 of set 8 | umzimvubu
    Processing Record 3 of set 8 | cap malheureux
    Processing Record 4 of set 8 | ansalta
    Processing Record 5 of set 8 | inhambane
    Processing Record 6 of set 8 | huambo
    Processing Record 7 of set 8 | ketchikan
    Processing Record 8 of set 8 | biloela
    Processing Record 9 of set 8 | vestmannaeyjar
    Processing Record 10 of set 8 | monrovia
    Processing Record 11 of set 8 | campbellsville
    City not found. Skipping...
    Processing Record 12 of set 8 | tabiauea
    Processing Record 13 of set 8 | camacha
    Processing Record 14 of set 8 | vila franca do campo
    City not found. Skipping...
    Processing Record 15 of set 8 | chagda
    Processing Record 16 of set 8 | ostrovnoy
    Processing Record 17 of set 8 | mutis
    Processing Record 18 of set 8 | thinadhoo
    Processing Record 19 of set 8 | bud
    City not found. Skipping...
    Processing Record 20 of set 8 | ye
    City not found. Skipping...
    Processing Record 21 of set 8 | mocambique
    Processing Record 22 of set 8 | challans
    Processing Record 23 of set 8 | bitung
    City not found. Skipping...
    Processing Record 24 of set 8 | roberto payan
    Processing Record 25 of set 8 | novokizhinginsk
    Processing Record 26 of set 8 | ambon
    Processing Record 27 of set 8 | tandil
    Processing Record 28 of set 8 | hami
    Processing Record 29 of set 8 | aketi
    Processing Record 30 of set 8 | neuquen
    City not found. Skipping...
    Processing Record 31 of set 8 | katha
    Processing Record 32 of set 8 | nsanje
    Processing Record 33 of set 8 | adrar
    Processing Record 34 of set 8 | parvatsar
    Processing Record 35 of set 8 | teluknaga
    Processing Record 36 of set 8 | foumban
    Processing Record 37 of set 8 | dawei
    Processing Record 38 of set 8 | khuzhir
    Processing Record 39 of set 8 | metro
    Processing Record 40 of set 8 | ancud
    Processing Record 41 of set 8 | englehart
    Processing Record 42 of set 8 | nishihara
    Processing Record 43 of set 8 | jilib
    Processing Record 44 of set 8 | honiara
    Processing Record 45 of set 8 | otofuke
    Processing Record 46 of set 8 | mansfield
    City not found. Skipping...
    Processing Record 47 of set 8 | tarudant
    Processing Record 48 of set 8 | sturgeon falls
    Processing Record 49 of set 8 | beyneu
    Processing Record 50 of set 8 | nhulunbuy
    Processing Record 1 of set 9 | raudeberg
    Processing Record 2 of set 9 | nemuro
    City not found. Skipping...
    Processing Record 3 of set 9 | skagastrond
    Processing Record 4 of set 9 | saldanha
    Processing Record 5 of set 9 | ajaccio
    City not found. Skipping...
    Processing Record 6 of set 9 | tenosique
    Processing Record 7 of set 9 | datia
    Processing Record 8 of set 9 | faanui
    Processing Record 9 of set 9 | bonthe
    Processing Record 10 of set 9 | amga
    Processing Record 11 of set 9 | meadow lake
    Processing Record 12 of set 9 | salalah
    Processing Record 13 of set 9 | mabaruma
    City not found. Skipping...
    Processing Record 14 of set 9 | airai
    Processing Record 15 of set 9 | kokoda
    Processing Record 16 of set 9 | tapiramuta
    Processing Record 17 of set 9 | chokurdakh
    Processing Record 18 of set 9 | eenhana
    Processing Record 19 of set 9 | launceston
    City not found. Skipping...
    Processing Record 20 of set 9 | goderich
    Processing Record 21 of set 9 | kindia
    Processing Record 22 of set 9 | damietta
    Processing Record 23 of set 9 | mariental
    Processing Record 24 of set 9 | abonnema
    Processing Record 25 of set 9 | margate
    Processing Record 26 of set 9 | mounana
    Processing Record 27 of set 9 | mwingi
    Processing Record 28 of set 9 | dongsheng
    Processing Record 29 of set 9 | lebanon
    Processing Record 30 of set 9 | urbino
    Processing Record 31 of set 9 | abha
    City not found. Skipping...
    Processing Record 32 of set 9 | haibowan
    Processing Record 33 of set 9 | teeli
    City not found. Skipping...
    Processing Record 34 of set 9 | payo
    Processing Record 35 of set 9 | college
    Processing Record 36 of set 9 | mandurah
    Processing Record 37 of set 9 | qingdao
    Processing Record 38 of set 9 | jacmel
    Processing Record 39 of set 9 | linqiong
    Processing Record 40 of set 9 | snyder
    City not found. Skipping...
    Processing Record 41 of set 9 | samusu
    Processing Record 42 of set 9 | leh
    Processing Record 43 of set 9 | menemen
    Processing Record 44 of set 9 | pattao
    Processing Record 45 of set 9 | sibolga
    Processing Record 46 of set 9 | lompoc
    Processing Record 47 of set 9 | padang
    Processing Record 48 of set 9 | luoyang
    Processing Record 49 of set 9 | solano
    Processing Record 50 of set 9 | manta
    Processing Record 1 of set 10 | caxias
    Processing Record 2 of set 10 | ossora
    Processing Record 3 of set 10 | miyako
    Processing Record 4 of set 10 | mubi
    City not found. Skipping...
    Processing Record 5 of set 10 | halalo
    Processing Record 6 of set 10 | jelenia gora
    Processing Record 7 of set 10 | zaltan
    Processing Record 8 of set 10 | praia da vitoria
    Processing Record 9 of set 10 | ellsworth
    Processing Record 10 of set 10 | campbell river
    Processing Record 11 of set 10 | portland
    Processing Record 12 of set 10 | miraflores
    Processing Record 13 of set 10 | port blair
    Processing Record 14 of set 10 | noumea
    Processing Record 15 of set 10 | hobyo
    Processing Record 16 of set 10 | houma
    Processing Record 17 of set 10 | maceio
    Processing Record 18 of set 10 | troitskoye
    Processing Record 19 of set 10 | dengzhou
    City not found. Skipping...
    Processing Record 20 of set 10 | buqayq
    Processing Record 21 of set 10 | la ronge
    Processing Record 22 of set 10 | tadine
    Processing Record 23 of set 10 | vyartsilya
    Processing Record 24 of set 10 | konakovo
    Processing Record 25 of set 10 | santa vitoria do palmar
    Processing Record 26 of set 10 | nakhon thai
    Processing Record 27 of set 10 | haines junction
    Processing Record 28 of set 10 | saint-louis
    Processing Record 29 of set 10 | muli
    Processing Record 30 of set 10 | srednekolymsk
    City not found. Skipping...
    Processing Record 31 of set 10 | krasnoselkup
    Processing Record 32 of set 10 | ilovlya
    Processing Record 33 of set 10 | lavrentiya
    Processing Record 34 of set 10 | grindavik
    Processing Record 35 of set 10 | yeniseysk
    Processing Record 36 of set 10 | sao jose da coroa grande
    Processing Record 37 of set 10 | eucaliptus
    Processing Record 38 of set 10 | merauke
    Processing Record 39 of set 10 | chara
    Processing Record 40 of set 10 | sandnessjoen
    Processing Record 41 of set 10 | polovinnoye
    Processing Record 42 of set 10 | bandipur
    Processing Record 43 of set 10 | bilibino
    Processing Record 44 of set 10 | dzilam gonzalez
    Processing Record 45 of set 10 | opuwo
    Processing Record 46 of set 10 | terrace
    Processing Record 47 of set 10 | kavaratti
    Processing Record 48 of set 10 | mosjoen
    Processing Record 49 of set 10 | kyren
    Processing Record 50 of set 10 | alegrete
    Processing Record 1 of set 11 | kastamonu
    Processing Record 2 of set 11 | oranjestad
    Processing Record 3 of set 11 | jaisalmer
    Processing Record 4 of set 11 | saint-joseph
    Processing Record 5 of set 11 | cassilandia
    Processing Record 6 of set 11 | lashio
    Processing Record 7 of set 11 | ust-maya
    Processing Record 8 of set 11 | veraval
    Processing Record 9 of set 11 | palm coast
    Processing Record 10 of set 11 | wamba
    Processing Record 11 of set 11 | balotra
    Processing Record 12 of set 11 | jiaohe
    City not found. Skipping...
    Processing Record 13 of set 11 | burica
    Processing Record 14 of set 11 | rio grande
    Processing Record 15 of set 11 | nizwa
    Processing Record 16 of set 11 | husavik
    Processing Record 17 of set 11 | natal
    City not found. Skipping...
    Processing Record 18 of set 11 | attawapiskat
    Processing Record 19 of set 11 | penzance
    Processing Record 20 of set 11 | skelleftea
    Processing Record 21 of set 11 | tessalit
    Processing Record 22 of set 11 | havoysund
    Processing Record 23 of set 11 | manokwari
    Processing Record 24 of set 11 | riyadh
    Processing Record 25 of set 11 | leningradskiy
    Processing Record 26 of set 11 | bafoussam
    Processing Record 27 of set 11 | yangjiang
    City not found. Skipping...
    Processing Record 28 of set 11 | kawana waters
    City not found. Skipping...
    Processing Record 29 of set 11 | dyakonovo
    Processing Record 30 of set 11 | dzhebariki-khaya
    Processing Record 31 of set 11 | matara
    Processing Record 32 of set 11 | ilanskiy
    Processing Record 33 of set 11 | port hedland
    Processing Record 34 of set 11 | san juan
    Processing Record 35 of set 11 | walvis bay
    Processing Record 36 of set 11 | wolfenbuttel
    Processing Record 37 of set 11 | rock springs
    Processing Record 38 of set 11 | waipawa
    Processing Record 39 of set 11 | the valley
    City not found. Skipping...
    Processing Record 40 of set 11 | zlatoustovsk
    Processing Record 41 of set 11 | singaraja
    City not found. Skipping...
    Processing Record 42 of set 11 | sorvag
    Processing Record 43 of set 11 | san jose
    Processing Record 44 of set 11 | waingapu
    Processing Record 45 of set 11 | schwalmtal
    Processing Record 46 of set 11 | kurumkan
    Processing Record 47 of set 11 | vardo
    Processing Record 48 of set 11 | dolores
    City not found. Skipping...
    Processing Record 49 of set 11 | santarem
    Processing Record 50 of set 11 | neryungri
    Processing Record 1 of set 12 | handan
    Processing Record 2 of set 12 | ginir
    Processing Record 3 of set 12 | arica
    City not found. Skipping...
    Processing Record 4 of set 12 | karaton
    Processing Record 5 of set 12 | trelew
    Processing Record 6 of set 12 | chernyshevskiy
    Processing Record 7 of set 12 | hovd
    Processing Record 8 of set 12 | birjand
    Processing Record 9 of set 12 | igrim
    Processing Record 10 of set 12 | houston
    Processing Record 11 of set 12 | pechenga
    City not found. Skipping...
    Processing Record 12 of set 12 | saint anthony
    Processing Record 13 of set 12 | lillooet
    Processing Record 14 of set 12 | xining
    Processing Record 15 of set 12 | pisco
    Processing Record 16 of set 12 | porto novo
    Processing Record 17 of set 12 | edd
    Processing Record 18 of set 12 | clyde river
    Processing Record 19 of set 12 | tarnow
    Processing Record 20 of set 12 | mandvi
    Processing Record 21 of set 12 | ulety
    Processing Record 22 of set 12 | baghdad
    Processing Record 23 of set 12 | ekhabi
    Processing Record 24 of set 12 | tazmalt
    Processing Record 25 of set 12 | aykhal
    Processing Record 26 of set 12 | west des moines
    Processing Record 27 of set 12 | belmonte
    City not found. Skipping...
    Processing Record 28 of set 12 | mahon
    Processing Record 29 of set 12 | alyangula
    Processing Record 30 of set 12 | boa vista
    Processing Record 31 of set 12 | alenquer
    Processing Record 32 of set 12 | sturgis
    Processing Record 33 of set 12 | basarabeasca
    Processing Record 34 of set 12 | yulara
    Processing Record 35 of set 12 | zwedru
    Processing Record 36 of set 12 | cankuzo
    Processing Record 37 of set 12 | quepos
    City not found. Skipping...
    Processing Record 38 of set 12 | barbar
    Processing Record 39 of set 12 | kaka
    Processing Record 40 of set 12 | atar
    Processing Record 41 of set 12 | bonnyville
    Processing Record 42 of set 12 | buala
    Processing Record 43 of set 12 | brigantine
    Processing Record 44 of set 12 | almenara
    Processing Record 45 of set 12 | urucui
    Processing Record 46 of set 12 | harsewinkel
    Processing Record 47 of set 12 | mana
    Processing Record 48 of set 12 | tuburan
    Processing Record 49 of set 12 | tumarbong
    Processing Record 50 of set 12 | morehead
    Processing Record 1 of set 13 | alexandria
    Processing Record 2 of set 13 | christchurch
    Processing Record 3 of set 13 | ko samui
    Processing Record 4 of set 13 | harper
    Processing Record 5 of set 13 | bima
    Processing Record 6 of set 13 | kaele
    Processing Record 7 of set 13 | grand-santi
    Processing Record 8 of set 13 | tomatlan
    Processing Record 9 of set 13 | pozo colorado
    Processing Record 10 of set 13 | santa fe
    


```python
CityData_df.head(4)
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Date</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>san carlos de bariloche</td>
      <td>ar</td>
      <td>-40.47</td>
      <td>-69.58</td>
      <td>1557302400</td>
      <td>90</td>
      <td>76</td>
      <td>33.80</td>
      <td>2.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-76.56</td>
      <td>-46.92</td>
      <td>1557302400</td>
      <td>0</td>
      <td>52</td>
      <td>42.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tarko-sale</td>
      <td>ru</td>
      <td>63.98</td>
      <td>79.57</td>
      <td>1557304322</td>
      <td>84</td>
      <td>64</td>
      <td>34.27</td>
      <td>15.75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-6.80</td>
      <td>-133.10</td>
      <td>1557304451</td>
      <td>18</td>
      <td>78</td>
      <td>82.87</td>
      <td>14.09</td>
    </tr>
  </tbody>
</table>
</div>




```python
CityData_df.count()
```




    City          609
    Country       609
    Latitude      609
    Longitude     609
    Date          609
    Cloudiness    609
    Humidity      609
    Max Temp      609
    Wind Speed    609
    dtype: int64




```python
CityData_df.to_csv("CityData")
```

### Convert Raw Data to DataFrame
* Export the city data into a .csv.
* Display the DataFrame


```python
CityData_df.to_csv("CityData")
CityData_df.head(3)
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Date</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>san carlos de bariloche</td>
      <td>ar</td>
      <td>-40.47</td>
      <td>-69.58</td>
      <td>1557302400</td>
      <td>90</td>
      <td>76</td>
      <td>33.80</td>
      <td>2.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-76.56</td>
      <td>-46.92</td>
      <td>1557302400</td>
      <td>0</td>
      <td>52</td>
      <td>42.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tarko-sale</td>
      <td>ru</td>
      <td>63.98</td>
      <td>79.57</td>
      <td>1557304322</td>
      <td>84</td>
      <td>64</td>
      <td>34.27</td>
      <td>15.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
CityData_df.count()
```




    City          609
    Country       609
    Latitude      609
    Longitude     609
    Date          609
    Cloudiness    609
    Humidity      609
    Max Temp      609
    Wind Speed    609
    dtype: int64



### Plotting the Data
* Use proper labeling of the plots using plot titles (including date of analysis) and axes labels.
* Save the plotted figures as .pngs.

#### Latitude vs. Temperature Plot


```python
plt.scatter(pd.to_numeric(CityData_df["Latitude"]), pd.to_numeric(CityData_df["Max Temp"]),c="blue", edgecolor="black")
plt.title("City Latitude vs. Max Temperature (04/07/2019)")
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.ylim(0, 120)
plt.xlim(-95, 95)
plt.grid()
plt.savefig("LatTemp.png")
plt.show()
```


![png](output_19_0.png)


#### Latitude vs. Humidity Plot


```python
plt.scatter(pd.to_numeric(CityData_df["Latitude"]), pd.to_numeric(CityData_df["Humidity"]),c="blue", edgecolor="black")
plt.title("City Latitude vs. Humidity (04/07/2019)")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.ylim(0, 110)
plt.xlim(-95, 95)
plt.grid()
plt.savefig("LatHum.png")
plt.show()
```


![png](output_21_0.png)


#### Latitude vs. Cloudiness Plot


```python
plt.scatter(pd.to_numeric(CityData_df["Latitude"]), pd.to_numeric(CityData_df["Cloudiness"]),c="blue", edgecolor="black")
plt.title("City Latitude vs. Cloudiness (04/07/2019)")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.ylim(-10, 110)
plt.xlim(-95, 95)
plt.grid()
plt.savefig("LatCloud.png")
plt.show()
```


![png](output_23_0.png)


#### Latitude vs. Wind Speed Plot


```python
plt.scatter(pd.to_numeric(CityData_df["Latitude"]), pd.to_numeric(CityData_df["Wind Speed"]),c="blue", edgecolor="black")
plt.title("City Latitude vs. Wind Speed (04/07/2019)")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.ylim(-1, 45)
plt.xlim(-95, 95)
plt.grid()
plt.savefig("LatWind.png")
plt.show()
```


![png](output_25_0.png)

