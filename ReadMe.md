
WeatherPy
In this example, you'll be creating a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, you'll be utilizing a simple Python library, the OpenWeatherMap API, and a little common sense to create a representative model of weather across world cities.

Your objective is to build a series of scatter plots to showcase the following relationships:

Temperature (F) vs. Latitude
Humidity (%) vs. Latitude
Cloudiness (%) vs. Latitude
Wind Speed (mph) vs. Latitude
Your final notebook must:

Randomly select at least 500 unique (non-repeat) cities based on latitude and longitude.
Perform a weather check on each of the cities using a series of successive API calls.
Include a print log of each city as it's being processed with the city number, city name, and requested URL.
Save both a CSV of all data retrieved and png images for each scatter plot.
As final considerations:

You must use the Matplotlib and Seaborn libraries.
You must include a written description of three observable trends based on the data.
You must use proper labeling of your plots, including aspects like: Plot Titles (with date of analysis) and Axes Labels.
You must include an exported markdown version of your Notebook called  README.md in your GitHub repository.
See Example Solution for a reference on expected format.
Hints and Considerations
You may want to start this assignment by refreshing yourself on 4th grade geography, in particular, the geographic coordinate system.

Next, spend the requisite time necessary to study the OpenWeatherMap API. Based on your initial study, you should be able to answer basic questions about the API: Where do you request the API key? Which Weather API in particular will you need? What URL endpoints does it expect? What JSON structure does it respond with? Before you write a line of code, you should be aiming to have a crystal clear understanding of your intended outcome.

Though we've never worked with the citipy Python library, push yourself to decipher how it works, and why it might be relevant. Before you try to incorporate the library into your analysis, start by creating simple test cases outside your main script to confirm that you are using it correctly. Too often, when introduced to a new library, students get bogged down by the most minor of errors -- spending hours investigating their entire code -- when, in fact, a simple and focused test would have shown their basic utilization of the library was wrong from the start. Don't let this be you!

Part of our expectation in this challenge is that you will use critical thinking skills to understand how and why we're recommending the tools we are. What is Citipy for? Why would you use it in conjunction with the OpenWeatherMap API? How would you do so?

In building your script, pay attention to the cities you are using in your query pool. Are you getting coverage of the full gamut of latitudes and longitudes? Or are you simply choosing 500 cities concentrated in one region of the world? Even if you were a geographic genius, simply rattling 500 cities based on your human selection would create a biased dataset. Be thinking of how you should counter this. (Hint: Consider the full range of latitudes).

Lastly, remember -- this is a challenging activity. Push yourself! If you complete this task, then you can safely say that you've gained a strong mastery of the core foundations of data analytics and it will only go better from here. Good luck!

# Observations
- There is a direction correlation between latitude and maximum temperature. The farther away from 0 latitude, the lower the max temperature for a city
- Humidity appears to increase as a city draws closer to 0 latitude up to a limit.
- city windspeeds appear to increase the farther away a city is from latitude 0


```python
# Dependencies
import csv
import matplotlib.pyplot as plt
import requests as req
import seaborn as sns
import pandas as pd
import json
import numpy as np
from citipy import citipy
```


```python
%matplotlib inline
```


```python
# Save config information.
api_key = "2a0e8e14dd60d3518271c5f0803eaf63"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Build partial query URL
query_url = f"{url}appid={api_key}&units={units}&q="

```


```python
#Randomly select 500+ cities
random_lat = []
random_lon = []
cities = []
country_code = []
name = []
weather_json = []
weather_dict = {}
for x in range (1400):
    random_lat = np.random.uniform(low=-90, high=90, size=1)
    random_lon =np.random.uniform(low=-180, high=180, size=1)
    cityname = citipy.nearest_city(random_lat, random_lon)
    if cityname not in cities:
        cities.append(cityname)
print(len(cities))
    
```

    608



```python
#Report on pulled data
c=1
for city in cities:
    name.append(city.city_name)
    country_code.append(city.country_code)
    response = req.get(query_url+city.city_name).json()
    print("Processing Record "+str(c)+" of "+ str(len(cities))+" "+query_url+city.city_name)
    weather_json.append(response)
    c +=1
    
print("-"*100)
print("                Data Retrieval Complete")
print("-"*100)
```

    Processing Record 1 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bredasdorp
    Processing Record 2 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kuna
    Processing Record 3 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=petrovec
    Processing Record 4 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ahipara
    Processing Record 5 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=belleville
    Processing Record 6 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cape town
    Processing Record 7 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=busselton
    Processing Record 8 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=atuona
    Processing Record 9 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sola
    Processing Record 10 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nogliki
    Processing Record 11 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cherskiy
    Processing Record 12 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bograd
    Processing Record 13 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-kan
    Processing Record 14 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ponta do sol
    Processing Record 15 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mahebourg
    Processing Record 16 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ushuaia
    Processing Record 17 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=east london
    Processing Record 18 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nelson bay
    Processing Record 19 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kenai
    Processing Record 20 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bluff
    Processing Record 21 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bathsheba
    Processing Record 22 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=inhambane
    Processing Record 23 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=taolanaro
    Processing Record 24 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=rikitea
    Processing Record 25 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lebu
    Processing Record 26 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=khatanga
    Processing Record 27 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mar del plata
    Processing Record 28 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hilo
    Processing Record 29 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=westport
    Processing Record 30 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=punta arenas
    Processing Record 31 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=point pleasant
    Processing Record 32 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cidreira
    Processing Record 33 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pak phanang
    Processing Record 34 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=portland
    Processing Record 35 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hobart
    Processing Record 36 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sinjai
    Processing Record 37 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chuy
    Processing Record 38 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=avarua
    Processing Record 39 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=beloha
    Processing Record 40 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hermanus
    Processing Record 41 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=upernavik
    Processing Record 42 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=northam
    Processing Record 43 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=carmen
    Processing Record 44 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=warmbad
    Processing Record 45 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jamsa
    Processing Record 46 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mrirt
    Processing Record 47 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kavieng
    Processing Record 48 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ribeira grande
    Processing Record 49 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nuristan
    Processing Record 50 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bamnet narong
    Processing Record 51 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=banda aceh
    Processing Record 52 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nikolskoye
    Processing Record 53 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=brokopondo
    Processing Record 54 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=norman wells
    Processing Record 55 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port alfred
    Processing Record 56 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=butaritari
    Processing Record 57 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port macquarie
    Processing Record 58 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pasewalk
    Processing Record 59 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kulmbach
    Processing Record 60 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=qaanaaq
    Processing Record 61 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dikson
    Processing Record 62 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mocambique
    Processing Record 63 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chifeng
    Processing Record 64 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=belushya guba
    Processing Record 65 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=puerto del rosario
    Processing Record 66 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=qianan
    Processing Record 67 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=padang
    Processing Record 68 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mataura
    Processing Record 69 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vaini
    Processing Record 70 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=peniche
    Processing Record 71 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=new norfolk
    Processing Record 72 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yumen
    Processing Record 73 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tarko-sale
    Processing Record 74 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=svecha
    Processing Record 75 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tasiilaq
    Processing Record 76 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gorno-chuyskiy
    Processing Record 77 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=albany
    Processing Record 78 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yellowknife
    Processing Record 79 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lenoir city
    Processing Record 80 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vaitupu
    Processing Record 81 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tra vinh
    Processing Record 82 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=maragogi
    Processing Record 83 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zhangye
    Processing Record 84 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=oistins
    Processing Record 85 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=blama
    Processing Record 86 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=grand river south east
    Processing Record 87 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lampazos de naranjo
    Processing Record 88 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gazli
    Processing Record 89 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pozo colorado
    Processing Record 90 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tiksi
    Processing Record 91 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ugoofaaru
    Processing Record 92 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=carnarvon
    Processing Record 93 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=puerto ayora
    Processing Record 94 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cabo san lucas
    Processing Record 95 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=wentzville
    Processing Record 96 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=faanui
    Processing Record 97 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=paamiut
    Processing Record 98 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=neuquen
    Processing Record 99 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lira
    Processing Record 100 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=alberton
    Processing Record 101 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=barrow
    Processing Record 102 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jalu
    Processing Record 103 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=taldan
    Processing Record 104 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jamestown
    Processing Record 105 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=srednekolymsk
    Processing Record 106 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=arraial do cabo
    Processing Record 107 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lorengau
    Processing Record 108 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bengkulu
    Processing Record 109 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bolungarvik
    Processing Record 110 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hulyaypole
    Processing Record 111 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vikulovo
    Processing Record 112 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gongzhuling
    Processing Record 113 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dromolaxia
    Processing Record 114 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dawei
    Processing Record 115 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=arawa
    Processing Record 116 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=samarai
    Processing Record 117 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=samusu
    Processing Record 118 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kuah
    Processing Record 119 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=iqaluit
    Processing Record 120 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=buluang
    Processing Record 121 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sinkat
    Processing Record 122 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=udachnyy
    Processing Record 123 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=marcona
    Processing Record 124 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cukai
    Processing Record 125 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=praia
    Processing Record 126 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=labrea
    Processing Record 127 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=luderitz
    Processing Record 128 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zhurivka
    Processing Record 129 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tilichiki
    Processing Record 130 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=abashiri
    Processing Record 131 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=anshun
    Processing Record 132 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hofn
    Processing Record 133 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kodiak
    Processing Record 134 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vardo
    Processing Record 135 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=barentsburg
    Processing Record 136 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kelowna
    Processing Record 137 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=anloga
    Processing Record 138 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=souillac
    Processing Record 139 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nyurba
    Processing Record 140 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bhainsdehi
    Processing Record 141 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=skarzysko-kamienna
    Processing Record 142 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bubaque
    Processing Record 143 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=serebryansk
    Processing Record 144 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bambous virieux
    Processing Record 145 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=klaksvik
    Processing Record 146 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kapaa
    Processing Record 147 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=japura
    Processing Record 148 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=asau
    Processing Record 149 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=manavalakurichi
    Processing Record 150 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cockburn harbour
    Processing Record 151 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lingyuan
    Processing Record 152 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=huarmey
    Processing Record 153 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=thilogne
    Processing Record 154 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lyuban
    Processing Record 155 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mugreyevskiy
    Processing Record 156 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nome
    Processing Record 157 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=palana
    Processing Record 158 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=antofagasta
    Processing Record 159 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=le vauclin
    Processing Record 160 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=college
    Processing Record 161 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=haines junction
    Processing Record 162 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=fortuna
    Processing Record 163 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=taitung
    Processing Record 164 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sorkjosen
    Processing Record 165 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=shakhrinau
    Processing Record 166 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=acapulco
    Processing Record 167 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=illoqqortoormiut
    Processing Record 168 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tuktoyaktuk
    Processing Record 169 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sigli
    Processing Record 170 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bilibino
    Processing Record 171 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lima
    Processing Record 172 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hithadhoo
    Processing Record 173 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=severo-kurilsk
    Processing Record 174 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dingle
    Processing Record 175 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=carutapera
    Processing Record 176 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=esperance
    Processing Record 177 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=boa vista
    Processing Record 178 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=newport
    Processing Record 179 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port elizabeth
    Processing Record 180 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ginir
    Processing Record 181 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=attawapiskat
    Processing Record 182 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port hardy
    Processing Record 183 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=husavik
    Processing Record 184 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=anupgarh
    Processing Record 185 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=muriwai beach
    Processing Record 186 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint-pierre
    Processing Record 187 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kaolack
    Processing Record 188 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=marechal candido rondon
    Processing Record 189 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pangnirtung
    Processing Record 190 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=labuhan
    Processing Record 191 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=georgetown
    Processing Record 192 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pauini
    Processing Record 193 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bur gabo
    Processing Record 194 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=katobu
    Processing Record 195 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cochrane
    Processing Record 196 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=olafsvik
    Processing Record 197 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=castro
    Processing Record 198 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lompoc
    Processing Record 199 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kabul
    Processing Record 200 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=leningradskiy
    Processing Record 201 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=manuk mangkaw
    Processing Record 202 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zhemchuzhnyy
    Processing Record 203 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint-lo
    Processing Record 204 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=grindavik
    Processing Record 205 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=acari
    Processing Record 206 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=katsuura
    Processing Record 207 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=celestun
    Processing Record 208 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yar-sale
    Processing Record 209 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gotsu
    Processing Record 210 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint anthony
    Processing Record 211 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mirnyy
    Processing Record 212 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=delijan
    Processing Record 213 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=henties bay
    Processing Record 214 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=moindou
    Processing Record 215 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bereda
    Processing Record 216 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=victoria
    Processing Record 217 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=skjervoy
    Processing Record 218 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ituni
    Processing Record 219 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gao
    Processing Record 220 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=daru
    Processing Record 221 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mys shmidta
    Processing Record 222 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=narsaq
    Processing Record 223 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nanortalik
    Processing Record 224 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=savannakhet
    Processing Record 225 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sentyabrskiy
    Processing Record 226 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=matara
    Processing Record 227 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=abu samrah
    Processing Record 228 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=west fargo
    Processing Record 229 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=guerrero negro
    Processing Record 230 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lubango
    Processing Record 231 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=torbay
    Processing Record 232 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sorland
    Processing Record 233 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=azuaga
    Processing Record 234 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gawler
    Processing Record 235 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dryden
    Processing Record 236 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hasaki
    Processing Record 237 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=brae
    Processing Record 238 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=thompson
    Processing Record 239 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mehamn
    Processing Record 240 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=evensk
    Processing Record 241 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lolua
    Processing Record 242 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=karratha
    Processing Record 243 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tanout
    Processing Record 244 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=opuwo
    Processing Record 245 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saskylakh
    Processing Record 246 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=porto novo
    Processing Record 247 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vangaindrano
    Processing Record 248 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kruisfontein
    Processing Record 249 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=feijo
    Processing Record 250 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=namibe
    Processing Record 251 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint-philippe
    Processing Record 252 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=rumoi
    Processing Record 253 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bera
    Processing Record 254 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=himora
    Processing Record 255 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=caucaia
    Processing Record 256 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=codrington
    Processing Record 257 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hambantota
    Processing Record 258 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=avera
    Processing Record 259 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=talnakh
    Processing Record 260 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=belmonte
    Processing Record 261 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=verkhovye
    Processing Record 262 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=werder
    Processing Record 263 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lavrentiya
    Processing Record 264 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=longyearbyen
    Processing Record 265 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tezu
    Processing Record 266 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=samalaeulu
    Processing Record 267 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=grand gaube
    Processing Record 268 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saldanha
    Processing Record 269 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=abidjan
    Processing Record 270 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=iskateley
    Processing Record 271 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ostersund
    Processing Record 272 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pontal do parana
    Processing Record 273 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=concordia
    Processing Record 274 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dongsheng
    Processing Record 275 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sechura
    Processing Record 276 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=teguldet
    Processing Record 277 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ostrovnoy
    Processing Record 278 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=half moon bay
    Processing Record 279 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vestmannaeyjar
    Processing Record 280 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=te anau
    Processing Record 281 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=barcelos
    Processing Record 282 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ulladulla
    Processing Record 283 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=luang prabang
    Processing Record 284 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=fairbanks
    Processing Record 285 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nanded
    Processing Record 286 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pangody
    Processing Record 287 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jawhar
    Processing Record 288 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=majene
    Processing Record 289 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=san cristobal
    Processing Record 290 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vila do maio
    Processing Record 291 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sulphur springs
    Processing Record 292 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=shaoguan
    Processing Record 293 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tamandare
    Processing Record 294 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=parambu
    Processing Record 295 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kurunegala
    Processing Record 296 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=loikaw
    Processing Record 297 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=torres
    Processing Record 298 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ancud
    Processing Record 299 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cap malheureux
    Processing Record 300 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=airai
    Processing Record 301 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tomatlan
    Processing Record 302 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tateyama
    Processing Record 303 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=canchungo
    Processing Record 304 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kentau
    Processing Record 305 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=seymchan
    Processing Record 306 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dalvik
    Processing Record 307 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=laguna
    Processing Record 308 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kalininsk
    Processing Record 309 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port-gentil
    Processing Record 310 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=atikokan
    Processing Record 311 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pevek
    Processing Record 312 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lata
    Processing Record 313 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=marietta
    Processing Record 314 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nuuk
    Processing Record 315 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=oum hadjer
    Processing Record 316 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kinkala
    Processing Record 317 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=homer
    Processing Record 318 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=touros
    Processing Record 319 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=raudeberg
    Processing Record 320 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=speightstown
    Processing Record 321 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mentok
    Processing Record 322 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tsotilion
    Processing Record 323 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=amderma
    Processing Record 324 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ojiya
    Processing Record 325 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nizwa
    Processing Record 326 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bethel
    Processing Record 327 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kieta
    Processing Record 328 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yashkul
    Processing Record 329 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=marsh harbour
    Processing Record 330 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nsanje
    Processing Record 331 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dekar
    Processing Record 332 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kalmar
    Processing Record 333 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kokstad
    Processing Record 334 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=xuddur
    Processing Record 335 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=haldia
    Processing Record 336 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lasa
    Processing Record 337 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sao filipe
    Processing Record 338 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=manaus
    Processing Record 339 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=rumford
    Processing Record 340 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=abu dhabi
    Processing Record 341 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chapais
    Processing Record 342 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=rio verde de mato grosso
    Processing Record 343 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jumla
    Processing Record 344 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nanhai
    Processing Record 345 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nizhneyansk
    Processing Record 346 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kaitangata
    Processing Record 347 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tsihombe
    Processing Record 348 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pahrump
    Processing Record 349 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chokurdakh
    Processing Record 350 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=altamira
    Processing Record 351 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hobyo
    Processing Record 352 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lashio
    Processing Record 353 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cockburn town
    Processing Record 354 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sao joao da barra
    Processing Record 355 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=naldurg
    Processing Record 356 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=goderich
    Processing Record 357 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=elizabeth city
    Processing Record 358 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=snezhnogorsk
    Processing Record 359 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pisco
    Processing Record 360 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=morondava
    Processing Record 361 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tepic
    Processing Record 362 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sembakung
    Processing Record 363 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=novoagansk
    Processing Record 364 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lazaro cardenas
    Processing Record 365 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=alekseyevsk
    Processing Record 366 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=umzimvubu
    Processing Record 367 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vostok
    Processing Record 368 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=port blair
    Processing Record 369 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=borgarnes
    Processing Record 370 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yulara
    Processing Record 371 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tahlequah
    Processing Record 372 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vryburg
    Processing Record 373 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mitsamiouli
    Processing Record 374 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ulaangom
    Processing Record 375 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=isiro
    Processing Record 376 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=namatanai
    Processing Record 377 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=parrita
    Processing Record 378 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=qostanay
    Processing Record 379 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=diamantino
    Processing Record 380 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=donji vakuf
    Processing Record 381 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dudinka
    Processing Record 382 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sebeta
    Processing Record 383 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cruzeiro do sul
    Processing Record 384 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mpika
    Processing Record 385 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=constitucion
    Processing Record 386 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jacqueville
    Processing Record 387 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=monduli
    Processing Record 388 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=alta floresta
    Processing Record 389 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=stornoway
    Processing Record 390 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sidi ali
    Processing Record 391 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nelson
    Processing Record 392 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=richards bay
    Processing Record 393 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=prokopyevsk
    Processing Record 394 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tura
    Processing Record 395 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pavelets
    Processing Record 396 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zabol
    Processing Record 397 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ixtapa
    Processing Record 398 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=polunochnoye
    Processing Record 399 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kautokeino
    Processing Record 400 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zheleznodorozhnyy
    Processing Record 401 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ilulissat
    Processing Record 402 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tuatapere
    Processing Record 403 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pedernales
    Processing Record 404 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gourdon
    Processing Record 405 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=straumen
    Processing Record 406 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=palmer
    Processing Record 407 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint-augustin
    Processing Record 408 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=san policarpo
    Processing Record 409 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=san patricio
    Processing Record 410 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chebarkul
    Processing Record 411 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zyryanka
    Processing Record 412 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=aksarka
    Processing Record 413 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sao jose da coroa grande
    Processing Record 414 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kahului
    Processing Record 415 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=isangel
    Processing Record 416 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=guangyuan
    Processing Record 417 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=camacha
    Processing Record 418 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-kamchatsk
    Processing Record 419 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-koksa
    Processing Record 420 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=merauke
    Processing Record 421 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=severnyy
    Processing Record 422 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=fare
    Processing Record 423 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=clyde river
    Processing Record 424 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=santa cruz
    Processing Record 425 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nautla
    Processing Record 426 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=inuvik
    Processing Record 427 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tautira
    Processing Record 428 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=vylkove
    Processing Record 429 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=victor harbor
    Processing Record 430 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=winnemucca
    Processing Record 431 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=colac
    Processing Record 432 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=seoul
    Processing Record 433 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=honningsvag
    Processing Record 434 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=wattegama
    Processing Record 435 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=berbera
    Processing Record 436 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=iskandar
    Processing Record 437 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=the valley
    Processing Record 438 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kjollefjord
    Processing Record 439 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=khani
    Processing Record 440 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chacabuco
    Processing Record 441 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=manggar
    Processing Record 442 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=karaton
    Processing Record 443 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jiddah
    Processing Record 444 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=eureka
    Processing Record 445 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=byron bay
    Processing Record 446 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jinan
    Processing Record 447 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=esso
    Processing Record 448 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nanakuli
    Processing Record 449 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mnogovershinnyy
    Processing Record 450 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hvide sande
    Processing Record 451 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=atar
    Processing Record 452 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=aykhal
    Processing Record 453 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mastung
    Processing Record 454 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=marsabit
    Processing Record 455 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=achisay
    Processing Record 456 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=thanh hoa
    Processing Record 457 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-kuyga
    Processing Record 458 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=el tigre
    Processing Record 459 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=puerto plata
    Processing Record 460 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=loandjili
    Processing Record 461 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=louisbourg
    Processing Record 462 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jaumave
    Processing Record 463 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cruz grande
    Processing Record 464 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kamenskoye
    Processing Record 465 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=adrar
    Processing Record 466 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=garden city
    Processing Record 467 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cockburn town
    Processing Record 468 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=qinhuangdao
    Processing Record 469 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bambanglipuro
    Processing Record 470 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=shaoyang
    Processing Record 471 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pincher creek
    Processing Record 472 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=castanos
    Processing Record 473 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=honiara
    Processing Record 474 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kilindoni
    Processing Record 475 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=north platte
    Processing Record 476 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=douglas
    Processing Record 477 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cairns
    Processing Record 478 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=provideniya
    Processing Record 479 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=partizanske
    Processing Record 480 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pyay
    Processing Record 481 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tarakan
    Processing Record 482 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=talara
    Processing Record 483 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=la sarre
    Processing Record 484 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=teya
    Processing Record 485 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mogok
    Processing Record 486 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=haibowan
    Processing Record 487 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mbacke
    Processing Record 488 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=krasnoborsk
    Processing Record 489 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=waipawa
    Processing Record 490 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=luba
    Processing Record 491 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=grand baie
    Processing Record 492 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=troitsko-pechorsk
    Processing Record 493 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=praia da vitoria
    Processing Record 494 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zaruma
    Processing Record 495 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sibolga
    Processing Record 496 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=maceio
    Processing Record 497 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yialos
    Processing Record 498 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=magadan
    Processing Record 499 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tupik
    Processing Record 500 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kwinana
    Processing Record 501 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=machali
    Processing Record 502 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=nicoya
    Processing Record 503 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=winneba
    Processing Record 504 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=fort nelson
    Processing Record 505 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=luena
    Processing Record 506 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lafiagi
    Processing Record 507 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=labuan
    Processing Record 508 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zaykovo
    Processing Record 509 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=acajutla
    Processing Record 510 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kaseda
    Processing Record 511 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=shingu
    Processing Record 512 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=santa maria
    Processing Record 513 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=maumere
    Processing Record 514 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=halalo
    Processing Record 515 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=thunder bay
    Processing Record 516 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=eldikan
    Processing Record 517 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tadine
    Processing Record 518 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=khorixas
    Processing Record 519 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=cape coast
    Processing Record 520 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kazalinsk
    Processing Record 521 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tombouctou
    Processing Record 522 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kegayli
    Processing Record 523 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=caravelas
    Processing Record 524 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=baglan
    Processing Record 525 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-nera
    Processing Record 526 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tumannyy
    Processing Record 527 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=isla mujeres
    Processing Record 528 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=buhovo
    Processing Record 529 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=khandyga
    Processing Record 530 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pilot butte
    Processing Record 531 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=berlevag
    Processing Record 532 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint george
    Processing Record 533 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=balykshi
    Processing Record 534 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ponta do sol
    Processing Record 535 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=andenes
    Processing Record 536 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=plettenberg bay
    Processing Record 537 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=los patios
    Processing Record 538 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=veraval
    Processing Record 539 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=guarapari
    Processing Record 540 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=duluth
    Processing Record 541 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ahuimanu
    Processing Record 542 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=salinas
    Processing Record 543 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-ishim
    Processing Record 544 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=matamoros
    Processing Record 545 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=dali
    Processing Record 546 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=la ronge
    Processing Record 547 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bosaso
    Processing Record 548 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kapit
    Processing Record 549 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mount gambier
    Processing Record 550 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sitka
    Processing Record 551 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=comodoro rivadavia
    Processing Record 552 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hokitika
    Processing Record 553 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=broome
    Processing Record 554 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=waingapu
    Processing Record 555 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=hervey bay
    Processing Record 556 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=tixpehual
    Processing Record 557 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=colares
    Processing Record 558 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pimampiro
    Processing Record 559 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ust-maya
    Processing Record 560 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=gladstone
    Processing Record 561 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=malanje
    Processing Record 562 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lovington
    Processing Record 563 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=pascagoula
    Processing Record 564 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=zhezkazgan
    Processing Record 565 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=rio gallegos
    Processing Record 566 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=viesca
    Processing Record 567 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=eenhana
    Processing Record 568 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kunda
    Processing Record 569 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=riyadh
    Processing Record 570 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=ukiah
    Processing Record 571 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=qabis
    Processing Record 572 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kijang
    Processing Record 573 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=petropavlovsk-kamchatskiy
    Processing Record 574 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=chengzihe
    Processing Record 575 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=alihe
    Processing Record 576 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=salalah
    Processing Record 577 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=jutai
    Processing Record 578 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sur
    Processing Record 579 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=taganak
    Processing Record 580 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bakchar
    Processing Record 581 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=letlhakane
    Processing Record 582 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kloulklubed
    Processing Record 583 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=anadyr
    Processing Record 584 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=itarema
    Processing Record 585 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=san jeronimo
    Processing Record 586 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=auch
    Processing Record 587 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=roald
    Processing Record 588 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=saint simons
    Processing Record 589 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=aksay
    Processing Record 590 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=mapiripan
    Processing Record 591 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=bell ville
    Processing Record 592 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=yerbogachen
    Processing Record 593 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=omsukchan
    Processing Record 594 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=high level
    Processing Record 595 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=razole
    Processing Record 596 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=boali
    Processing Record 597 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=karakendzha
    Processing Record 598 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=virginia beach
    Processing Record 599 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=koindu
    Processing Record 600 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=wasilla
    Processing Record 601 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=manzanillo
    Processing Record 602 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=lagos
    Processing Record 603 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=karpathos
    Processing Record 604 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=kalaiya
    Processing Record 605 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=san vicente
    Processing Record 606 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=sioux lookout
    Processing Record 607 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=uarini
    Processing Record 608 of 608 http://api.openweathermap.org/data/2.5/weather?appid=2a0e8e14dd60d3518271c5f0803eaf63&units=imperial&q=myski
    ----------------------------------------------------------------------------------------------------
                    Data Retrieval Complete
    ----------------------------------------------------------------------------------------------------



```python
#Fill data into DataFrame
lat_data = []
temp_data = []
city_data = []
country = []
Humi_data = []
cld_data = []
wind_data = []
long_data = []
temp_max = []
for data in weather_json:
    try:
        lat1 = data.get("coord").get("lat")
        lat_data.append(lat1)
        temp1 = data.get("main").get("temp")
        temp_data.append(temp1)
        city1 = data.get("name")
        city_data.append(city1)
        country1 = data.get("sys").get("country")
        country.append(country1)
        Humi1 = data.get("main").get("humidity")
        Humi_data.append(Humi1)
        cld1 = data.get("clouds").get("all")
        cld_data.append(cld1)
        wind1 = data.get("wind").get("speed")
        wind_data.append(wind1)
        long1 = data.get("coord").get("lon")
        long_data.append(long1)
        temp1 = data.get("main").get("temp_max")
        temp_max.append(temp1)

    except:
        pass

    continue

weather_dict = {"Temperature (F)": temp_data, "Latitude": lat_data, "Longitude":long_data, "city":city_data, "Country":country,
                "Humidity": Humi_data, "Cloudiness":cld_data,"Wind Speed":wind_data, "Max_Temp":temp_max}
weather_df = pd.DataFrame(weather_dict)
weather_df.set_index("city", inplace=True)
weather_df
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
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Max_Temp</th>
      <th>Temperature (F)</th>
      <th>Wind Speed</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
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
      <th>Bredasdorp</th>
      <td>88</td>
      <td>ZA</td>
      <td>100</td>
      <td>-34.53</td>
      <td>20.04</td>
      <td>65.87</td>
      <td>65.87</td>
      <td>12.33</td>
    </tr>
    <tr>
      <th>Kuna</th>
      <td>90</td>
      <td>US</td>
      <td>59</td>
      <td>43.49</td>
      <td>-116.42</td>
      <td>35.60</td>
      <td>34.65</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>Petrovec</th>
      <td>20</td>
      <td>MK</td>
      <td>84</td>
      <td>41.94</td>
      <td>21.61</td>
      <td>6.80</td>
      <td>6.80</td>
      <td>2.15</td>
    </tr>
    <tr>
      <th>Ahipara</th>
      <td>20</td>
      <td>NZ</td>
      <td>78</td>
      <td>-35.17</td>
      <td>173.16</td>
      <td>72.21</td>
      <td>72.21</td>
      <td>14.00</td>
    </tr>
    <tr>
      <th>Belleville</th>
      <td>40</td>
      <td>CA</td>
      <td>86</td>
      <td>44.16</td>
      <td>-77.39</td>
      <td>37.40</td>
      <td>37.40</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>Cape Town</th>
      <td>20</td>
      <td>ZA</td>
      <td>82</td>
      <td>-33.93</td>
      <td>18.42</td>
      <td>64.40</td>
      <td>64.40</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>Busselton</th>
      <td>0</td>
      <td>AU</td>
      <td>79</td>
      <td>-33.64</td>
      <td>115.35</td>
      <td>77.03</td>
      <td>77.03</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>Atuona</th>
      <td>92</td>
      <td>PF</td>
      <td>100</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>80.27</td>
      <td>80.27</td>
      <td>19.04</td>
    </tr>
    <tr>
      <th>Sola</th>
      <td>0</td>
      <td>FI</td>
      <td>83</td>
      <td>62.78</td>
      <td>29.36</td>
      <td>-9.41</td>
      <td>-9.41</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>Nogliki</th>
      <td>44</td>
      <td>RU</td>
      <td>96</td>
      <td>51.80</td>
      <td>143.14</td>
      <td>20.10</td>
      <td>20.10</td>
      <td>6.73</td>
    </tr>
    <tr>
      <th>Cherskiy</th>
      <td>80</td>
      <td>RU</td>
      <td>72</td>
      <td>68.75</td>
      <td>161.30</td>
      <td>0.89</td>
      <td>0.89</td>
      <td>2.93</td>
    </tr>
    <tr>
      <th>Bograd</th>
      <td>40</td>
      <td>RU</td>
      <td>71</td>
      <td>54.22</td>
      <td>90.86</td>
      <td>6.80</td>
      <td>6.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>Ust-Kan</th>
      <td>80</td>
      <td>RU</td>
      <td>71</td>
      <td>50.93</td>
      <td>84.77</td>
      <td>26.94</td>
      <td>26.94</td>
      <td>6.73</td>
    </tr>
    <tr>
      <th>Ponta do Sol</th>
      <td>32</td>
      <td>BR</td>
      <td>91</td>
      <td>-20.63</td>
      <td>-46.00</td>
      <td>64.83</td>
      <td>64.83</td>
      <td>2.59</td>
    </tr>
    <tr>
      <th>Mahebourg</th>
      <td>20</td>
      <td>MU</td>
      <td>74</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>84.20</td>
      <td>84.20</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>Ushuaia</th>
      <td>75</td>
      <td>AR</td>
      <td>75</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>46.40</td>
      <td>46.40</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>East London</th>
      <td>75</td>
      <td>ZA</td>
      <td>94</td>
      <td>-33.02</td>
      <td>27.91</td>
      <td>68.00</td>
      <td>68.00</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>Nelson Bay</th>
      <td>40</td>
      <td>AU</td>
      <td>57</td>
      <td>-32.72</td>
      <td>152.14</td>
      <td>75.20</td>
      <td>75.20</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>Kenai</th>
      <td>1</td>
      <td>US</td>
      <td>61</td>
      <td>60.55</td>
      <td>-151.26</td>
      <td>12.20</td>
      <td>7.61</td>
      <td>8.41</td>
    </tr>
    <tr>
      <th>Bluff</th>
      <td>20</td>
      <td>AU</td>
      <td>60</td>
      <td>-23.58</td>
      <td>149.07</td>
      <td>89.36</td>
      <td>89.36</td>
      <td>9.19</td>
    </tr>
    <tr>
      <th>Bathsheba</th>
      <td>40</td>
      <td>BB</td>
      <td>94</td>
      <td>13.22</td>
      <td>-59.52</td>
      <td>71.60</td>
      <td>71.60</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>Inhambane</th>
      <td>20</td>
      <td>MZ</td>
      <td>90</td>
      <td>-23.87</td>
      <td>35.38</td>
      <td>78.78</td>
      <td>78.78</td>
      <td>9.53</td>
    </tr>
    <tr>
      <th>Rikitea</th>
      <td>76</td>
      <td>PF</td>
      <td>100</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>78.69</td>
      <td>78.69</td>
      <td>13.78</td>
    </tr>
    <tr>
      <th>Lebu</th>
      <td>48</td>
      <td>ET</td>
      <td>56</td>
      <td>8.96</td>
      <td>38.73</td>
      <td>62.99</td>
      <td>62.99</td>
      <td>3.94</td>
    </tr>
    <tr>
      <th>Khatanga</th>
      <td>80</td>
      <td>RU</td>
      <td>72</td>
      <td>71.98</td>
      <td>102.47</td>
      <td>-6.45</td>
      <td>-6.45</td>
      <td>6.40</td>
    </tr>
    <tr>
      <th>Mar del Plata</th>
      <td>20</td>
      <td>AR</td>
      <td>44</td>
      <td>-46.43</td>
      <td>-67.52</td>
      <td>63.03</td>
      <td>63.03</td>
      <td>19.04</td>
    </tr>
    <tr>
      <th>Hilo</th>
      <td>90</td>
      <td>US</td>
      <td>83</td>
      <td>19.71</td>
      <td>-155.08</td>
      <td>69.80</td>
      <td>58.60</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>Westport</th>
      <td>0</td>
      <td>IE</td>
      <td>99</td>
      <td>53.80</td>
      <td>-9.52</td>
      <td>29.06</td>
      <td>29.06</td>
      <td>23.40</td>
    </tr>
    <tr>
      <th>Punta Arenas</th>
      <td>75</td>
      <td>CL</td>
      <td>75</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>46.40</td>
      <td>46.40</td>
      <td>28.86</td>
    </tr>
    <tr>
      <th>Point Pleasant</th>
      <td>90</td>
      <td>US</td>
      <td>57</td>
      <td>40.08</td>
      <td>-74.07</td>
      <td>53.60</td>
      <td>49.80</td>
      <td>12.88</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Sur</th>
      <td>0</td>
      <td>OM</td>
      <td>93</td>
      <td>22.57</td>
      <td>59.53</td>
      <td>74.51</td>
      <td>74.51</td>
      <td>1.59</td>
    </tr>
    <tr>
      <th>Taganak</th>
      <td>75</td>
      <td>PH</td>
      <td>70</td>
      <td>6.08</td>
      <td>118.30</td>
      <td>87.80</td>
      <td>87.80</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>Bakchar</th>
      <td>80</td>
      <td>RU</td>
      <td>63</td>
      <td>57.02</td>
      <td>82.07</td>
      <td>0.26</td>
      <td>0.26</td>
      <td>6.51</td>
    </tr>
    <tr>
      <th>Letlhakane</th>
      <td>80</td>
      <td>BW</td>
      <td>95</td>
      <td>-21.42</td>
      <td>25.59</td>
      <td>67.58</td>
      <td>67.58</td>
      <td>9.98</td>
    </tr>
    <tr>
      <th>Kloulklubed</th>
      <td>90</td>
      <td>PW</td>
      <td>58</td>
      <td>7.04</td>
      <td>134.26</td>
      <td>87.80</td>
      <td>86.86</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>Anadyr</th>
      <td>90</td>
      <td>RU</td>
      <td>94</td>
      <td>64.73</td>
      <td>177.51</td>
      <td>32.00</td>
      <td>32.00</td>
      <td>40.26</td>
    </tr>
    <tr>
      <th>Itarema</th>
      <td>48</td>
      <td>BR</td>
      <td>99</td>
      <td>-2.92</td>
      <td>-39.92</td>
      <td>76.08</td>
      <td>76.08</td>
      <td>3.94</td>
    </tr>
    <tr>
      <th>San Jeronimo</th>
      <td>92</td>
      <td>PE</td>
      <td>99</td>
      <td>-13.65</td>
      <td>-73.37</td>
      <td>45.62</td>
      <td>45.62</td>
      <td>0.58</td>
    </tr>
    <tr>
      <th>Auch</th>
      <td>90</td>
      <td>FR</td>
      <td>87</td>
      <td>43.65</td>
      <td>0.59</td>
      <td>46.40</td>
      <td>44.62</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>Roald</th>
      <td>40</td>
      <td>NO</td>
      <td>53</td>
      <td>62.58</td>
      <td>6.12</td>
      <td>21.20</td>
      <td>10.17</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>Saint Simons</th>
      <td>75</td>
      <td>US</td>
      <td>72</td>
      <td>31.14</td>
      <td>-81.39</td>
      <td>69.80</td>
      <td>67.55</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>Aksay</th>
      <td>92</td>
      <td>RU</td>
      <td>96</td>
      <td>47.27</td>
      <td>39.86</td>
      <td>34.05</td>
      <td>34.05</td>
      <td>17.13</td>
    </tr>
    <tr>
      <th>Mapiripan</th>
      <td>8</td>
      <td>CO</td>
      <td>65</td>
      <td>2.89</td>
      <td>-72.13</td>
      <td>80.49</td>
      <td>80.49</td>
      <td>4.16</td>
    </tr>
    <tr>
      <th>Bell Ville</th>
      <td>32</td>
      <td>AR</td>
      <td>90</td>
      <td>-32.63</td>
      <td>-62.69</td>
      <td>65.78</td>
      <td>65.78</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>Yerbogachen</th>
      <td>36</td>
      <td>RU</td>
      <td>53</td>
      <td>61.28</td>
      <td>108.01</td>
      <td>-4.88</td>
      <td>-4.88</td>
      <td>3.71</td>
    </tr>
    <tr>
      <th>Omsukchan</th>
      <td>64</td>
      <td>RU</td>
      <td>71</td>
      <td>62.53</td>
      <td>155.80</td>
      <td>-4.20</td>
      <td>-4.20</td>
      <td>5.73</td>
    </tr>
    <tr>
      <th>High Level</th>
      <td>20</td>
      <td>CA</td>
      <td>66</td>
      <td>58.52</td>
      <td>-117.13</td>
      <td>15.80</td>
      <td>15.80</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>Razole</th>
      <td>0</td>
      <td>IN</td>
      <td>96</td>
      <td>16.48</td>
      <td>81.84</td>
      <td>81.26</td>
      <td>81.26</td>
      <td>2.48</td>
    </tr>
    <tr>
      <th>Boali</th>
      <td>20</td>
      <td>CF</td>
      <td>94</td>
      <td>4.81</td>
      <td>18.11</td>
      <td>71.60</td>
      <td>71.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>Virginia Beach</th>
      <td>90</td>
      <td>US</td>
      <td>100</td>
      <td>36.85</td>
      <td>-75.98</td>
      <td>57.20</td>
      <td>55.40</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>Koindu</th>
      <td>64</td>
      <td>LR</td>
      <td>98</td>
      <td>8.20</td>
      <td>-10.37</td>
      <td>68.03</td>
      <td>68.03</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>Wasilla</th>
      <td>1</td>
      <td>US</td>
      <td>36</td>
      <td>61.58</td>
      <td>-149.44</td>
      <td>19.40</td>
      <td>18.00</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>Manzanillo</th>
      <td>0</td>
      <td>CU</td>
      <td>100</td>
      <td>20.34</td>
      <td>-77.12</td>
      <td>74.51</td>
      <td>74.51</td>
      <td>16.69</td>
    </tr>
    <tr>
      <th>Lagos</th>
      <td>20</td>
      <td>NG</td>
      <td>94</td>
      <td>6.46</td>
      <td>3.39</td>
      <td>78.80</td>
      <td>78.80</td>
      <td>5.17</td>
    </tr>
    <tr>
      <th>Karpathos</th>
      <td>0</td>
      <td>GR</td>
      <td>66</td>
      <td>35.51</td>
      <td>27.21</td>
      <td>55.40</td>
      <td>55.40</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>Kalaiya</th>
      <td>0</td>
      <td>NP</td>
      <td>43</td>
      <td>27.03</td>
      <td>85.00</td>
      <td>86.12</td>
      <td>86.12</td>
      <td>4.72</td>
    </tr>
    <tr>
      <th>San Vicente</th>
      <td>0</td>
      <td>SV</td>
      <td>69</td>
      <td>13.64</td>
      <td>-88.78</td>
      <td>75.20</td>
      <td>73.06</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>Sioux Lookout</th>
      <td>90</td>
      <td>CA</td>
      <td>54</td>
      <td>50.10</td>
      <td>-91.92</td>
      <td>28.40</td>
      <td>28.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>Uarini</th>
      <td>64</td>
      <td>BR</td>
      <td>83</td>
      <td>-2.98</td>
      <td>-65.16</td>
      <td>78.24</td>
      <td>78.24</td>
      <td>5.73</td>
    </tr>
    <tr>
      <th>Myski</th>
      <td>92</td>
      <td>RU</td>
      <td>82</td>
      <td>53.71</td>
      <td>87.80</td>
      <td>15.47</td>
      <td>15.47</td>
      <td>4.16</td>
    </tr>
  </tbody>
</table>
<p>549 rows × 8 columns</p>
</div>




```python
#Save DataFrame to CSV
weather_df.to_csv("my_weather.csv",encoding="utf-8",index=False)
```


```python
plt.scatter(weather_df["Latitude"], 
            weather_df["Max_Temp"], c="gold", 
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="Temp")

# Incorporate the other graph properties
plt.title("City Max Temperature vs. Latitude (02/24/2018)")
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.xlim((-90,90))
plt.grid(True)
sns.set()
plt.savefig("Temperature (F) vs. Latitude.png")
plt.show()
```


![png](output_9_0.png)



```python
plt.scatter(weather_df["Latitude"], 
            weather_df["Humidity"], c="blue", 
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="Humidity")

# Incorporate the other graph properties
plt.title("City Humidity vs. Latitude (02/24/2018)")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.xlim((-90,90))
plt.grid(True)
sns.set()
plt.savefig("Humidity (%) vs. Latitude.png")
plt.show()
```


![png](output_10_0.png)



```python
plt.scatter(weather_df["Latitude"], 
            weather_df["Cloudiness"], c="green", 
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="Cloudiness")

# Incorporate the other graph properties
plt.title("City Cloudiness vs. Latitude (02/24/2018)")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.xlim((-90,90))
plt.grid(True)
sns.set()
plt.savefig("Cloudiness (%) vs. Latitude.png")
plt.show()
```


![png](output_11_0.png)



```python

plt.scatter(weather_df["Latitude"], 
            weather_df["Wind Speed"], c="red", 
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="wind speed")

# Incorporate the other graph properties
plt.title("City Wind Speed vs. Latitude (02/24/2018)")
plt.ylabel("Wind Speed (MPH)")
plt.xlabel("Latitude")
plt.xlim((-90,90))
plt.grid(True)
sns.set()
plt.savefig("Wind Speed (MPH) vs. Latitude.png")
plt.show()
```


![png](output_12_0.png)

