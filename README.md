
# WeatherPy
----

#### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
import openweathermapy.core as owm

# Import API key
from api_keys import api_key

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"
# output_data_file = "cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)
# list(cities)
```




    605



### Perform API Calls
* Perform a weather check on each city using a series of successive API calls.
* Include a print log of each city as it'sbeing processed (with the city number and city name).



```python
#create base url
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

#build query url
query_url = f"{url}units={units}&appid={api_key}&q="

#create list for dataframe
lats = []
lngs = []
temp_max = []
wind_speed = []
cloudiness = []
humidity = []
country = []
cities_df = []
dates = []

#set initial count 
count_one = 0
set_one = 1

#beginning statement
print(f"Beginning Data Retrieval")
print(f"-------------------------------")

for city in cities:
    try:
        response = requests.get(query_url + city.replace(" ","&")).json()
        lats.append(response["coord"]["lat"])
        lngs.append(response["coord"]["lon"])
        temp_max.append(response["main"]["temp_max"])
        wind_speed.append(response["wind"]["speed"])
        cloudiness.append(response["clouds"]["all"])
        humidity.append(response["main"]["humidity"])
        country.append(response["sys"]["country"])
        dates.append(response["dt"])
        
        if count_one > 48:
            count_one = 1
            set_one = set_one + 1
            cities_df.append(city)
        else:
            count_one = count_one + 1
            cities_df.append(city)
        print(f"Processing Record {count_one} of Set {set_one} | {city}\n{query_url}{city}")
    except Exception:
        print("City not found. Skipping...")
        
print("-------------------------------")
print("Data Retrieval Complete")
print("-------------------------------")
```

    Beginning Data Retrieval
    -------------------------------
    Processing Record 1 of Set 1 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bandarbeyla
    Processing Record 2 of Set 1 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saldanha
    Processing Record 3 of Set 1 | laguna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=laguna
    City not found. Skipping...
    Processing Record 4 of Set 1 | uruzgan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=uruzgan
    City not found. Skipping...
    Processing Record 5 of Set 1 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=khatanga
    Processing Record 6 of Set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=rikitea
    Processing Record 7 of Set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ushuaia
    Processing Record 8 of Set 1 | moen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=moen
    Processing Record 9 of Set 1 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=iqaluit
    Processing Record 10 of Set 1 | nanakuli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nanakuli
    Processing Record 11 of Set 1 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=puerto ayora
    Processing Record 12 of Set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port alfred
    Processing Record 13 of Set 1 | sambava
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sambava
    Processing Record 14 of Set 1 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nanortalik
    Processing Record 15 of Set 1 | dingle
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dingle
    City not found. Skipping...
    Processing Record 16 of Set 1 | sitka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sitka
    Processing Record 17 of Set 1 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hithadhoo
    City not found. Skipping...
    Processing Record 18 of Set 1 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=faanui
    Processing Record 19 of Set 1 | wyndham
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=wyndham
    Processing Record 20 of Set 1 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=beringovskiy
    Processing Record 21 of Set 1 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=chokurdakh
    Processing Record 22 of Set 1 | kamina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kamina
    Processing Record 23 of Set 1 | tarnobrzeg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tarnobrzeg
    Processing Record 24 of Set 1 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=victoria
    Processing Record 25 of Set 1 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=meulaboh
    Processing Record 26 of Set 1 | flin flon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=flin flon
    Processing Record 27 of Set 1 | krutinka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=krutinka
    Processing Record 28 of Set 1 | ribas do rio pardo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ribas do rio pardo
    Processing Record 29 of Set 1 | bulgan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bulgan
    Processing Record 30 of Set 1 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=severo-kurilsk
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 31 of Set 1 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=codrington
    Processing Record 32 of Set 1 | bartica
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bartica
    Processing Record 33 of Set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bredasdorp
    Processing Record 34 of Set 1 | aracatuba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aracatuba
    Processing Record 35 of Set 1 | alice springs
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=alice springs
    Processing Record 36 of Set 1 | constantine
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=constantine
    Processing Record 37 of Set 1 | altay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=altay
    Processing Record 38 of Set 1 | hirara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hirara
    Processing Record 39 of Set 1 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ostrovnoy
    Processing Record 40 of Set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vaini
    Processing Record 41 of Set 1 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=castro
    Processing Record 42 of Set 1 | vytegra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vytegra
    Processing Record 43 of Set 1 | port hedland
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port hedland
    Processing Record 44 of Set 1 | gornyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gornyy
    Processing Record 45 of Set 1 | victor harbor
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=victor harbor
    Processing Record 46 of Set 1 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=georgetown
    Processing Record 47 of Set 1 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hobart
    Processing Record 48 of Set 1 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=plettenberg bay
    Processing Record 49 of Set 1 | maniitsoq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=maniitsoq
    Processing Record 1 of Set 2 | cape town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cape town
    Processing Record 2 of Set 2 | east london
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=east london
    Processing Record 3 of Set 2 | arvika
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=arvika
    City not found. Skipping...
    Processing Record 4 of Set 2 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=busselton
    Processing Record 5 of Set 2 | gat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gat
    City not found. Skipping...
    Processing Record 6 of Set 2 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=narsaq
    Processing Record 7 of Set 2 | tripoli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tripoli
    Processing Record 8 of Set 2 | ouallam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ouallam
    Processing Record 9 of Set 2 | dakar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dakar
    Processing Record 10 of Set 2 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port lincoln
    Processing Record 11 of Set 2 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=zhigansk
    Processing Record 12 of Set 2 | dudinka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dudinka
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 13 of Set 2 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=torbay
    Processing Record 14 of Set 2 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=clyde river
    Processing Record 15 of Set 2 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=longyearbyen
    Processing Record 16 of Set 2 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=guerrero negro
    Processing Record 17 of Set 2 | tandil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tandil
    Processing Record 18 of Set 2 | airai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=airai
    Processing Record 19 of Set 2 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=butaritari
    City not found. Skipping...
    Processing Record 20 of Set 2 | khani
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=khani
    Processing Record 21 of Set 2 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=atuona
    Processing Record 22 of Set 2 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=albany
    Processing Record 23 of Set 2 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hermanus
    Processing Record 24 of Set 2 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=avarua
    Processing Record 25 of Set 2 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kavaratti
    Processing Record 26 of Set 2 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cidreira
    Processing Record 27 of Set 2 | grindavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=grindavik
    Processing Record 28 of Set 2 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bluff
    Processing Record 29 of Set 2 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=punta arenas
    Processing Record 30 of Set 2 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ilulissat
    Processing Record 31 of Set 2 | broome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=broome
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 32 of Set 2 | ruwi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ruwi
    Processing Record 33 of Set 2 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saskylakh
    Processing Record 34 of Set 2 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=thompson
    Processing Record 35 of Set 2 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sobolevo
    Processing Record 36 of Set 2 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=yellowknife
    City not found. Skipping...
    Processing Record 37 of Set 2 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kodiak
    Processing Record 38 of Set 2 | dikson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dikson
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 39 of Set 2 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lavrentiya
    Processing Record 40 of Set 2 | upernavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=upernavik
    Processing Record 41 of Set 2 | sampit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sampit
    Processing Record 42 of Set 2 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jamestown
    Processing Record 43 of Set 2 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mataura
    City not found. Skipping...
    Processing Record 44 of Set 2 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tasiilaq
    Processing Record 45 of Set 2 | athabasca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=athabasca
    City not found. Skipping...
    Processing Record 46 of Set 2 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mys shmidta
    Processing Record 47 of Set 2 | shache
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shache
    Processing Record 48 of Set 2 | stange
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=stange
    Processing Record 49 of Set 2 | ballina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ballina
    Processing Record 1 of Set 3 | lorengau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lorengau
    Processing Record 2 of Set 3 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pevek
    City not found. Skipping...
    Processing Record 3 of Set 3 | anadyr
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=anadyr
    Processing Record 4 of Set 3 | vardo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vardo
    Processing Record 5 of Set 3 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=barrow
    Processing Record 6 of Set 3 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=provideniya
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 7 of Set 3 | avera
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=avera
    Processing Record 8 of Set 3 | yertsevo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=yertsevo
    Processing Record 9 of Set 3 | barrhead
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=barrhead
    Processing Record 10 of Set 3 | alofi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=alofi
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 11 of Set 3 | salalah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=salalah
    Processing Record 12 of Set 3 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tuktoyaktuk
    Processing Record 13 of Set 3 | lompoc
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lompoc
    City not found. Skipping...
    Processing Record 14 of Set 3 | linqiong
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=linqiong
    Processing Record 15 of Set 3 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mar del plata
    Processing Record 16 of Set 3 | palmer
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=palmer
    Processing Record 17 of Set 3 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=new norfolk
    Processing Record 18 of Set 3 | bethel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bethel
    Processing Record 19 of Set 3 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=norman wells
    Processing Record 20 of Set 3 | leh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=leh
    Processing Record 21 of Set 3 | okoneshnikovo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=okoneshnikovo
    Processing Record 22 of Set 3 | lagoa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lagoa
    Processing Record 23 of Set 3 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pacific grove
    Processing Record 24 of Set 3 | fort morgan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fort morgan
    Processing Record 25 of Set 3 | dunedin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dunedin
    Processing Record 26 of Set 3 | chara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=chara
    Processing Record 27 of Set 3 | tigil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tigil
    Processing Record 28 of Set 3 | moerai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=moerai
    Processing Record 29 of Set 3 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=souillac
    Processing Record 30 of Set 3 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hilo
    Processing Record 31 of Set 3 | puerto del rosario
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=puerto del rosario
    City not found. Skipping...
    Processing Record 32 of Set 3 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vila franca do campo
    City not found. Skipping...
    Processing Record 33 of Set 3 | cabaiguan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cabaiguan
    Processing Record 34 of Set 3 | college
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=college
    Processing Record 35 of Set 3 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=komsomolskiy
    Processing Record 36 of Set 3 | cairns
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cairns
    City not found. Skipping...
    Processing Record 37 of Set 3 | nabire
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nabire
    City not found. Skipping...
    Processing Record 38 of Set 3 | bac lieu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bac lieu
    Processing Record 39 of Set 3 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ponta do sol
    City not found. Skipping...
    Processing Record 40 of Set 3 | praya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=praya
    Processing Record 41 of Set 3 | jinchang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jinchang
    Processing Record 42 of Set 3 | lima
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lima
    Processing Record 43 of Set 3 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bambous virieux
    City not found. Skipping...
    Processing Record 44 of Set 3 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sisimiut
    Processing Record 45 of Set 3 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nikolskoye
    Processing Record 46 of Set 3 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cockburn town
    Processing Record 47 of Set 3 | bonoua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bonoua
    Processing Record 48 of Set 3 | soyo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=soyo
    Processing Record 49 of Set 3 | altamont
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=altamont
    Processing Record 1 of Set 4 | nemuro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nemuro
    City not found. Skipping...
    Processing Record 2 of Set 4 | roxana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=roxana
    Processing Record 3 of Set 4 | fortuna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fortuna
    Processing Record 4 of Set 4 | novobirilyussy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=novobirilyussy
    Processing Record 5 of Set 4 | batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=batagay-alyta
    Processing Record 6 of Set 4 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shenjiamen
    Processing Record 7 of Set 4 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cherskiy
    Processing Record 8 of Set 4 | gobabis
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gobabis
    Processing Record 9 of Set 4 | haines junction
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=haines junction
    City not found. Skipping...
    Processing Record 10 of Set 4 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=isangel
    Processing Record 11 of Set 4 | seminole
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=seminole
    Processing Record 12 of Set 4 | kargasok
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kargasok
    Processing Record 13 of Set 4 | nizhnevartovsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nizhnevartovsk
    Processing Record 14 of Set 4 | karratha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=karratha
    Processing Record 15 of Set 4 | kirakira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kirakira
    Processing Record 16 of Set 4 | bulaevo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bulaevo
    Processing Record 17 of Set 4 | quelimane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=quelimane
    Processing Record 18 of Set 4 | severodvinsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=severodvinsk
    Processing Record 19 of Set 4 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kapaa
    Processing Record 20 of Set 4 | labuhan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=labuhan
    Processing Record 21 of Set 4 | kahului
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kahului
    City not found. Skipping...
    Processing Record 22 of Set 4 | seydi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=seydi
    Processing Record 23 of Set 4 | kaduqli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kaduqli
    Processing Record 24 of Set 4 | paka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=paka
    City not found. Skipping...
    Processing Record 25 of Set 4 | zaragoza
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=zaragoza
    City not found. Skipping...
    Processing Record 26 of Set 4 | boiro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=boiro
    Processing Record 27 of Set 4 | guiyang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=guiyang
    City not found. Skipping...
    Processing Record 28 of Set 4 | paamiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=paamiut
    Processing Record 29 of Set 4 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tiksi
    Processing Record 30 of Set 4 | eureka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=eureka
    Processing Record 31 of Set 4 | kadirli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kadirli
    Processing Record 32 of Set 4 | luderitz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=luderitz
    Processing Record 33 of Set 4 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=taoudenni
    Processing Record 34 of Set 4 | atar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=atar
    Processing Record 35 of Set 4 | nome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nome
    Processing Record 36 of Set 4 | armenis
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=armenis
    Processing Record 37 of Set 4 | ancud
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ancud
    Processing Record 38 of Set 4 | amahai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=amahai
    Processing Record 39 of Set 4 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lebu
    Processing Record 40 of Set 4 | sergeyevka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sergeyevka
    Processing Record 41 of Set 4 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=egvekinot
    Processing Record 42 of Set 4 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=comodoro rivadavia
    Processing Record 43 of Set 4 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=carnarvon
    Processing Record 44 of Set 4 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=geraldton
    Processing Record 45 of Set 4 | preston
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=preston
    Processing Record 46 of Set 4 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mahebourg
    Processing Record 47 of Set 4 | bilma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bilma
    Processing Record 48 of Set 4 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=qaanaaq
    Processing Record 49 of Set 4 | beian
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=beian
    Processing Record 1 of Set 5 | camacha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=camacha
    Processing Record 2 of Set 5 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=coquimbo
    Processing Record 3 of Set 5 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vila velha
    Processing Record 4 of Set 5 | igarka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=igarka
    City not found. Skipping...
    Processing Record 5 of Set 5 | farmington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=farmington
    Processing Record 6 of Set 5 | miri
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=miri
    Processing Record 7 of Set 5 | binzhou
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=binzhou
    Processing Record 8 of Set 5 | vinogradnyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vinogradnyy
    Processing Record 9 of Set 5 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=klaksvik
    Processing Record 10 of Set 5 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fairbanks
    Processing Record 11 of Set 5 | loksa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=loksa
    Processing Record 12 of Set 5 | buala
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=buala
    Processing Record 13 of Set 5 | waipawa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=waipawa
    City not found. Skipping...
    Processing Record 14 of Set 5 | seoul
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=seoul
    Processing Record 15 of Set 5 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=esperance
    Processing Record 16 of Set 5 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tuatapere
    Processing Record 17 of Set 5 | fare
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fare
    City not found. Skipping...
    Processing Record 18 of Set 5 | manaus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=manaus
    Processing Record 19 of Set 5 | haverfordwest
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=haverfordwest
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 20 of Set 5 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=arraial do cabo
    Processing Record 21 of Set 5 | san isidro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=san isidro
    Processing Record 22 of Set 5 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saint-pierre
    Processing Record 23 of Set 5 | semey
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=semey
    Processing Record 24 of Set 5 | yasnyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=yasnyy
    Processing Record 25 of Set 5 | kavieng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kavieng
    Processing Record 26 of Set 5 | akcakoca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=akcakoca
    Processing Record 27 of Set 5 | acapulco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=acapulco
    Processing Record 28 of Set 5 | kpandae
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kpandae
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 29 of Set 5 | nianzishan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nianzishan
    Processing Record 30 of Set 5 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=santa cruz
    Processing Record 31 of Set 5 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=constitucion
    Processing Record 32 of Set 5 | miramar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=miramar
    City not found. Skipping...
    Processing Record 33 of Set 5 | margate
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=margate
    Processing Record 34 of Set 5 | katikati
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=katikati
    Processing Record 35 of Set 5 | san carlos
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=san carlos
    Processing Record 36 of Set 5 | lac du bonnet
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lac du bonnet
    Processing Record 37 of Set 5 | waingapu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=waingapu
    Processing Record 38 of Set 5 | orje
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=orje
    Processing Record 39 of Set 5 | whitianga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=whitianga
    City not found. Skipping...
    Processing Record 40 of Set 5 | deori khas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=deori khas
    Processing Record 41 of Set 5 | ahipara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ahipara
    Processing Record 42 of Set 5 | karatau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=karatau
    Processing Record 43 of Set 5 | adrar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=adrar
    Processing Record 44 of Set 5 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=half moon bay
    Processing Record 45 of Set 5 | hof
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hof
    City not found. Skipping...
    Processing Record 46 of Set 5 | artigas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=artigas
    Processing Record 47 of Set 5 | kawalu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kawalu
    Processing Record 48 of Set 5 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port macquarie
    City not found. Skipping...
    Processing Record 49 of Set 5 | tsotilion
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tsotilion
    Processing Record 1 of Set 6 | bud
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bud
    Processing Record 2 of Set 6 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ribeira grande
    Processing Record 3 of Set 6 | roma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=roma
    Processing Record 4 of Set 6 | atambua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=atambua
    Processing Record 5 of Set 6 | pendra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pendra
    Processing Record 6 of Set 6 | berlevag
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=berlevag
    City not found. Skipping...
    Processing Record 7 of Set 6 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cap malheureux
    Processing Record 8 of Set 6 | aykhal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aykhal
    City not found. Skipping...
    Processing Record 9 of Set 6 | lerwick
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lerwick
    City not found. Skipping...
    Processing Record 10 of Set 6 | ewa beach
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ewa beach
    Processing Record 11 of Set 6 | faridkot
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=faridkot
    Processing Record 12 of Set 6 | pedasi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pedasi
    Processing Record 13 of Set 6 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=rio gallegos
    Processing Record 14 of Set 6 | touros
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=touros
    Processing Record 15 of Set 6 | igrim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=igrim
    Processing Record 16 of Set 6 | dingzhou
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=dingzhou
    Processing Record 17 of Set 6 | arman
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=arman
    Processing Record 18 of Set 6 | bestobe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bestobe
    Processing Record 19 of Set 6 | bertoua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bertoua
    Processing Record 20 of Set 6 | hualmay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hualmay
    Processing Record 21 of Set 6 | merauke
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=merauke
    Processing Record 22 of Set 6 | san ramon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=san ramon
    Processing Record 23 of Set 6 | arica
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=arica
    Processing Record 24 of Set 6 | mamlyutka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mamlyutka
    Processing Record 25 of Set 6 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ust-kuyga
    City not found. Skipping...
    Processing Record 26 of Set 6 | chaumont
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=chaumont
    Processing Record 27 of Set 6 | furano
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=furano
    Processing Record 28 of Set 6 | nhulunbuy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nhulunbuy
    City not found. Skipping...
    Processing Record 29 of Set 6 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hamilton
    Processing Record 30 of Set 6 | hofn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hofn
    City not found. Skipping...
    Processing Record 31 of Set 6 | ruteng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ruteng
    Processing Record 32 of Set 6 | tezu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tezu
    Processing Record 33 of Set 6 | rovaniemi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=rovaniemi
    City not found. Skipping...
    Processing Record 34 of Set 6 | amberieu-en-bugey
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=amberieu-en-bugey
    Processing Record 35 of Set 6 | bratsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bratsk
    Processing Record 36 of Set 6 | bose
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bose
    Processing Record 37 of Set 6 | atoyac
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=atoyac
    Processing Record 38 of Set 6 | kununurra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kununurra
    Processing Record 39 of Set 6 | nova olimpia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nova olimpia
    Processing Record 40 of Set 6 | maracacume
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=maracacume
    Processing Record 41 of Set 6 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saint-joseph
    Processing Record 42 of Set 6 | cheney
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cheney
    Processing Record 43 of Set 6 | aginskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aginskoye
    Processing Record 44 of Set 6 | pemberton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pemberton
    Processing Record 45 of Set 6 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saint-philippe
    City not found. Skipping...
    Processing Record 46 of Set 6 | kargil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kargil
    Processing Record 47 of Set 6 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=richards bay
    Processing Record 48 of Set 6 | ilhabela
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ilhabela
    City not found. Skipping...
    Processing Record 49 of Set 6 | talnakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=talnakh
    Processing Record 1 of Set 7 | aracati
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aracati
    Processing Record 2 of Set 7 | piranhas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=piranhas
    Processing Record 3 of Set 7 | itarema
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=itarema
    City not found. Skipping...
    City not found. Skipping...
    Processing Record 4 of Set 7 | tiznit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tiznit
    Processing Record 5 of Set 7 | kaduna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kaduna
    Processing Record 6 of Set 7 | kidal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kidal
    Processing Record 7 of Set 7 | acajutla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=acajutla
    Processing Record 8 of Set 7 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ambilobe
    Processing Record 9 of Set 7 | necochea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=necochea
    Processing Record 10 of Set 7 | lincoln
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lincoln
    Processing Record 11 of Set 7 | hornepayne
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hornepayne
    Processing Record 12 of Set 7 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port elizabeth
    Processing Record 13 of Set 7 | meadow lake
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=meadow lake
    City not found. Skipping...
    Processing Record 14 of Set 7 | sarakhs
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sarakhs
    Processing Record 15 of Set 7 | champerico
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=champerico
    City not found. Skipping...
    Processing Record 16 of Set 7 | aswan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aswan
    Processing Record 17 of Set 7 | itaituba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=itaituba
    Processing Record 18 of Set 7 | okha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=okha
    Processing Record 19 of Set 7 | vostok
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vostok
    Processing Record 20 of Set 7 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sao joao da barra
    Processing Record 21 of Set 7 | shimoda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shimoda
    Processing Record 22 of Set 7 | pisco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pisco
    Processing Record 23 of Set 7 | noumea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=noumea
    Processing Record 24 of Set 7 | narwar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=narwar
    Processing Record 25 of Set 7 | fomboni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fomboni
    Processing Record 26 of Set 7 | mitha tiwana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mitha tiwana
    Processing Record 27 of Set 7 | bom jesus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bom jesus
    Processing Record 28 of Set 7 | deori
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=deori
    Processing Record 29 of Set 7 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saint-francois
    Processing Record 30 of Set 7 | grand centre
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=grand centre
    City not found. Skipping...
    Processing Record 31 of Set 7 | viravanallur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=viravanallur
    Processing Record 32 of Set 7 | poum
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=poum
    City not found. Skipping...
    Processing Record 33 of Set 7 | puksoozero
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=puksoozero
    Processing Record 34 of Set 7 | warrnambool
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=warrnambool
    City not found. Skipping...
    Processing Record 35 of Set 7 | sur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sur
    Processing Record 36 of Set 7 | otradnoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=otradnoye
    Processing Record 37 of Set 7 | talara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=talara
    Processing Record 38 of Set 7 | pihuamo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pihuamo
    Processing Record 39 of Set 7 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sinnamary
    Processing Record 40 of Set 7 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=vestmannaeyjar
    Processing Record 41 of Set 7 | muisne
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=muisne
    Processing Record 42 of Set 7 | flinders
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=flinders
    Processing Record 43 of Set 7 | maryborough
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=maryborough
    Processing Record 44 of Set 7 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hasaki
    Processing Record 45 of Set 7 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kaitangata
    City not found. Skipping...
    Processing Record 46 of Set 7 | tura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tura
    Processing Record 47 of Set 7 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=saint-augustin
    Processing Record 48 of Set 7 | sonkovo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sonkovo
    Processing Record 49 of Set 7 | asahikawa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=asahikawa
    Processing Record 1 of Set 8 | les cayes
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=les cayes
    Processing Record 2 of Set 8 | waitati
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=waitati
    Processing Record 3 of Set 8 | wilmington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=wilmington
    Processing Record 4 of Set 8 | marawi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=marawi
    Processing Record 5 of Set 8 | tomatlan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tomatlan
    City not found. Skipping...
    Processing Record 6 of Set 8 | pereslavl-zalesskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pereslavl-zalesskiy
    Processing Record 7 of Set 8 | kemi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kemi
    City not found. Skipping...
    Processing Record 8 of Set 8 | tokur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tokur
    Processing Record 9 of Set 8 | loandjili
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=loandjili
    Processing Record 10 of Set 8 | kieta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kieta
    Processing Record 11 of Set 8 | beroroha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=beroroha
    Processing Record 12 of Set 8 | bowen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bowen
    Processing Record 13 of Set 8 | falavarjan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=falavarjan
    Processing Record 14 of Set 8 | shanting
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shanting
    City not found. Skipping...
    Processing Record 15 of Set 8 | amapa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=amapa
    Processing Record 16 of Set 8 | barkly west
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=barkly west
    Processing Record 17 of Set 8 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port-gentil
    Processing Record 18 of Set 8 | erzin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=erzin
    Processing Record 19 of Set 8 | zhangjiakou
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=zhangjiakou
    Processing Record 20 of Set 8 | oktyabrskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=oktyabrskoye
    Processing Record 21 of Set 8 | namibe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=namibe
    Processing Record 22 of Set 8 | nargana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nargana
    Processing Record 23 of Set 8 | helong
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=helong
    Processing Record 24 of Set 8 | ilebo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=ilebo
    Processing Record 25 of Set 8 | voh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=voh
    Processing Record 26 of Set 8 | nikel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nikel
    Processing Record 27 of Set 8 | kjopsvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kjopsvik
    Processing Record 28 of Set 8 | gryazi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gryazi
    Processing Record 29 of Set 8 | shelburne
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shelburne
    Processing Record 30 of Set 8 | smithers
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=smithers
    Processing Record 31 of Set 8 | finnsnes
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=finnsnes
    Processing Record 32 of Set 8 | santa rosa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=santa rosa
    Processing Record 33 of Set 8 | luwuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=luwuk
    City not found. Skipping...
    Processing Record 34 of Set 8 | lata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lata
    Processing Record 35 of Set 8 | gorom-gorom
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gorom-gorom
    Processing Record 36 of Set 8 | matam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=matam
    Processing Record 37 of Set 8 | inhambane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=inhambane
    Processing Record 38 of Set 8 | jumla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jumla
    Processing Record 39 of Set 8 | san quintin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=san quintin
    Processing Record 40 of Set 8 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=fort nelson
    Processing Record 41 of Set 8 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pangnirtung
    Processing Record 42 of Set 8 | rawson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=rawson
    Processing Record 43 of Set 8 | nioro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nioro
    Processing Record 44 of Set 8 | mianyang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mianyang
    Processing Record 45 of Set 8 | port-cartier
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port-cartier
    Processing Record 46 of Set 8 | bubaque
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=bubaque
    Processing Record 47 of Set 8 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kalmunai
    Processing Record 48 of Set 8 | arlit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=arlit
    City not found. Skipping...
    Processing Record 49 of Set 8 | sampang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sampang
    City not found. Skipping...
    Processing Record 1 of Set 9 | pontianak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pontianak
    Processing Record 2 of Set 9 | gewane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=gewane
    Processing Record 3 of Set 9 | caraz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=caraz
    Processing Record 4 of Set 9 | plouzane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=plouzane
    Processing Record 5 of Set 9 | lukovetskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lukovetskiy
    Processing Record 6 of Set 9 | beloha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=beloha
    Processing Record 7 of Set 9 | nago
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=nago
    Processing Record 8 of Set 9 | shingu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=shingu
    Processing Record 9 of Set 9 | hambantota
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hambantota
    Processing Record 10 of Set 9 | pombas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pombas
    Processing Record 11 of Set 9 | tranas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=tranas
    Processing Record 12 of Set 9 | lahaina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lahaina
    Processing Record 13 of Set 9 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=cabo san lucas
    City not found. Skipping...
    Processing Record 14 of Set 9 | karasjok
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=karasjok
    Processing Record 15 of Set 9 | pestravka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=pestravka
    Processing Record 16 of Set 9 | jodhpur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jodhpur
    City not found. Skipping...
    Processing Record 17 of Set 9 | sigiriya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sigiriya
    Processing Record 18 of Set 9 | terra santa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=terra santa
    Processing Record 19 of Set 9 | jacareacanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jacareacanga
    Processing Record 20 of Set 9 | kerema
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kerema
    Processing Record 21 of Set 9 | aquiraz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=aquiraz
    Processing Record 22 of Set 9 | abadan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=abadan
    Processing Record 23 of Set 9 | houma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=houma
    Processing Record 24 of Set 9 | namatanai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=namatanai
    Processing Record 25 of Set 9 | jacqueville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=jacqueville
    City not found. Skipping...
    Processing Record 26 of Set 9 | kingsland
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kingsland
    Processing Record 27 of Set 9 | makungu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=makungu
    Processing Record 28 of Set 9 | port blair
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=port blair
    Processing Record 29 of Set 9 | kautokeino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=kautokeino
    Processing Record 30 of Set 9 | sayansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=sayansk
    Processing Record 31 of Set 9 | zarubino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=zarubino
    Processing Record 32 of Set 9 | teya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=teya
    Processing Record 33 of Set 9 | snyder
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=snyder
    Processing Record 34 of Set 9 | chake chake
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=chake chake
    Processing Record 35 of Set 9 | conde
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=conde
    Processing Record 36 of Set 9 | puerto gaitan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=puerto gaitan
    Processing Record 37 of Set 9 | hay river
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=hay river
    Processing Record 38 of Set 9 | lethem
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=lethem
    Processing Record 39 of Set 9 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=mount gambier
    Processing Record 40 of Set 9 | basco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=basco
    Processing Record 41 of Set 9 | naze
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=naze
    Processing Record 42 of Set 9 | khandyga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=535398c1cd6b981223d813b07b095dea&q=khandyga
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    City not found. Skipping...
    -------------------------------
    Data Retrieval Complete
    -------------------------------
    

### Convert Raw Data to DataFrame
* Export the city data into a .csv.
* Display the DataFrame


```python
weather_df = pd.DataFrame({"City": cities_df,
                           "Cloudiness": cloudiness,
                           "Country": country,
                           "Date": dates,
                           "Humidity": humidity,
                           "Lat": lats,
                           "Lng": lngs,
                           "Max Temp": temp_max,
                           "Wind Speed": wind_speed})
```


```python
weather_df.count()
```




    City          434
    Cloudiness    434
    Country       434
    Date          434
    Humidity      434
    Lat           434
    Lng           434
    Max Temp      434
    Wind Speed    434
    dtype: int64




```python
weather_df.head()
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
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>bandarbeyla</td>
      <td>79</td>
      <td>SO</td>
      <td>1569788070</td>
      <td>80</td>
      <td>9.49</td>
      <td>50.81</td>
      <td>76.68</td>
      <td>17.27</td>
    </tr>
    <tr>
      <td>1</td>
      <td>saldanha</td>
      <td>0</td>
      <td>PT</td>
      <td>1569788071</td>
      <td>69</td>
      <td>41.42</td>
      <td>-6.55</td>
      <td>66.19</td>
      <td>3.71</td>
    </tr>
    <tr>
      <td>2</td>
      <td>laguna</td>
      <td>75</td>
      <td>BZ</td>
      <td>1569788071</td>
      <td>70</td>
      <td>16.17</td>
      <td>-88.94</td>
      <td>89.60</td>
      <td>6.93</td>
    </tr>
    <tr>
      <td>3</td>
      <td>uruzgan</td>
      <td>0</td>
      <td>AF</td>
      <td>1569788071</td>
      <td>33</td>
      <td>32.93</td>
      <td>66.63</td>
      <td>46.73</td>
      <td>3.00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>khatanga</td>
      <td>76</td>
      <td>RU</td>
      <td>1569788072</td>
      <td>81</td>
      <td>71.98</td>
      <td>102.47</td>
      <td>25.17</td>
      <td>6.78</td>
    </tr>
  </tbody>
</table>
</div>



### Plotting the Data
* Use proper labeling of the plots using plot titles (including date of analysis) and axes labels.
* Save the plotted figures as .pngs.

#### Latitude vs. Temperature Plot


```python
#create scatter plot, Lat vs. Temp
plt.scatter(weather_df["Lat"], weather_df["Max Temp"], facecolors = "blue", edgecolors = "black",  marker = "o", s = 10)

#indicate title, ylabel, xlabel, and etc..
plt.title("City Latitude vs. Max Temperature")
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)
plt.ylim(-80, 150)
plt.xlim(-80, 100)

#show plot
plt.show()

#save the figure
plt.savefig("Temperature(F)_vs_Latitude.png")
```


![png](output_12_0.png)



    <Figure size 432x288 with 0 Axes>


#### Latitude vs. Humidity Plot


```python
#create scatterplot, Lat vs. Humidity
plt.scatter(weather_df["Lat"], weather_df["Humidity"], facecolors = "b", edgecolors = "black",  marker = "o", s = 10)

#indicate title, ylabel, xlabel, and etc..
plt.title("City Latitude vs. Humidity")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.ylim(-20, 120)
plt.xlim(-80, 100)

#show plot
plt.show()

#save the figure
plt.savefig("Humidity(%)_vs_Latitude.png")
```


![png](output_14_0.png)



    <Figure size 432x288 with 0 Axes>


#### Latitude vs. Cloudiness Plot


```python
#create scatterplot, Lat vs. Cloudiness
plt.scatter(weather_df["Lat"], weather_df["Cloudiness"], facecolors = "b", edgecolors = "black",  marker = "o", s = 10)

#indicate title, ylabel, xlabel, and etc..
plt.title("City Latitude vs. Cloudiness")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.ylim(-20, 120)
plt.xlim(-80, 100)

#show plot
plt.show()

#save the figure
plt.savefig("Cloudiness(%)_vs_Latitude.png")
```


![png](output_16_0.png)



    <Figure size 432x288 with 0 Axes>


#### Latitude vs. Wind Speed Plot


```python
#create scatterplot, Lat vs. Wind Speed
plt.scatter(weather_df["Lat"], weather_df["Wind Speed"], facecolors = "b", edgecolors = "black",  marker = "o", s = 10)

#indicate title, ylabel, xlabel, and etc..
plt.title("City Latitude vs. Wind Speed")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)
plt.ylim(-5, 40)
plt.xlim(-80, 100)

#show plot
plt.show()

#save the figure
plt.savefig("Wind_Speed(mph)_vs_Latitude.png")
```


![png](output_18_0.png)



    <Figure size 432x288 with 0 Axes>


# Three Observable Trend
---
### 1. Temperatures gradually decrease as latitude go south and north. 
### 2. Temperatures are high when latitude is close to 0.
### 3. Humidity and cloudiness do not show correlation with latitude



```python

```
