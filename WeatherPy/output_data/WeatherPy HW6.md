
Analysis
1. Most data displayed in Humidity vs. Latitude shows that at any random point on earth, you are more likely to experience high amounts of humidity than little or none. Based on an observation of the eye, humidity more than 40% is highly likely for any random point on the globe in this sample size.

2. Further data would need to be used to calculate a confidence interval for the analysis that the higher latitude you get, the more likely you are to experience higher wind speed. The data point, run on this code as the analysis is being written, at 65 Latitude has wind almost hitting 35 mph and should be further investigated if it's an outlier.   

3. An overall observation that may not be seen unless an observer takes a step back, is to realize this data only displays one day. Although this data is accurate for the day the code was run, it isn't conclusive of an average trend in data across the globe. In order to understand if most areas of the world are humid based on a random sample for example, the average of several years of daily data would need to be calculated and displayed. 


P.S. I had a fair amount of trouble with this homework, so my tutor helped me a lot through some of the snags I was experiencing.


```
import csv
import seaborn
import requests
import time
import numpy
import pandas as pd
import matplotlib.pyplot as plt
from citipy import citipy
import random
import urllib

api_key = "4328b000df419885e1b4acf29ef2cd9f"

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

Generate Cities List


```
lat_lngs = []
cities = []

# random lat and lng combinations
for x in range(1500):
    lats = numpy.random.uniform(low=-90.000, high=90.000)
    lngs = numpy.random.uniform(low=-180.000, high=180.000)

    city = citipy.nearest_city(lats, lngs).city_name
    
    if city not in cities:
        cities.append(city)

len(cities)
```




    611




```
url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=" + api_key 

city_data = []

print("Retrieving Data              ")
print("-----------------------------")

record_count = 1

for i, city in enumerate(cities):
    
    if (i % 500 == 0 and i < 502):
        record_count = 0
    city_url = url + "&q=" + urllib.request.pathname2url(city)

    print("Processing Record %s of 500 | %s" % (record_count, city))
    print(city_url)    

    record_count += 1

    # API request
    try:
        city_weather = requests.get(city_url).json()

        city_lat = city_weather["coord"]["lat"]
        city_lng = city_weather["coord"]["lon"]
        city_max_temp = city_weather["main"]["temp_max"]
        city_humidity = city_weather["main"]["humidity"]
        city_clouds = city_weather["clouds"]["all"]
        city_wind = city_weather["wind"]["speed"]
        city_country = city_weather["sys"]["country"]
        city_date = city_weather["dt"]

        city_data.append({"City": city, 
                          "Lat": city_lat, 
                          "Lng": city_lng, 
                          "Max Temp": city_max_temp,
                          "Humidity": city_humidity,
                          "Cloudiness": city_clouds,
                          "Wind Speed": city_wind,
                          "Country": city_country,
                          "Date": city_date})

    except:
        print("Error locating City data. Skipping...")
        pass
              
print("-----------------------------")
print("Data Load Complete           ")
print("-----------------------------")
```

    Retrieving Data              
    -----------------------------
    Processing Record 0 of 500 | barbar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=barbar
    Error locating City data. Skipping...
    Processing Record 1 of 500 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=punta%20arenas
    Processing Record 2 of 500 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bredasdorp
    Processing Record 3 of 500 | beyneu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=beyneu
    Processing Record 4 of 500 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=taolanaro
    Error locating City data. Skipping...
    Processing Record 5 of 500 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=qasigiannguit
    Processing Record 6 of 500 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=jamestown
    Processing Record 7 of 500 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chokurdakh
    Processing Record 8 of 500 | naze
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=naze
    Processing Record 9 of 500 | kuito
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kuito
    Processing Record 10 of 500 | lompoc
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lompoc
    Processing Record 11 of 500 | buritizeiro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=buritizeiro
    Processing Record 12 of 500 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ponta%20do%20sol
    Processing Record 13 of 500 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rikitea
    Processing Record 14 of 500 | serov
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=serov
    Processing Record 15 of 500 | kerman
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kerman
    Processing Record 16 of 500 | kamenka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kamenka
    Processing Record 17 of 500 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=new%20norfolk
    Processing Record 18 of 500 | albany
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=albany
    Processing Record 19 of 500 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fairbanks
    Processing Record 20 of 500 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ushuaia
    Processing Record 21 of 500 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tuatapere
    Processing Record 22 of 500 | jiuquan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=jiuquan
    Processing Record 23 of 500 | castro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=castro
    Processing Record 24 of 500 | vanimo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vanimo
    Processing Record 25 of 500 | banff
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=banff
    Processing Record 26 of 500 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=richards%20bay
    Processing Record 27 of 500 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=qaanaaq
    Processing Record 28 of 500 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kapaa
    Processing Record 29 of 500 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mataura
    Processing Record 30 of 500 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint-pierre
    Processing Record 31 of 500 | korla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=korla
    Error locating City data. Skipping...
    Processing Record 32 of 500 | broken hill
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=broken%20hill
    Processing Record 33 of 500 | kozulka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kozulka
    Processing Record 34 of 500 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hobart
    Processing Record 35 of 500 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lebu
    Processing Record 36 of 500 | rey bouba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rey%20bouba
    Processing Record 37 of 500 | sao gabriel da cachoeira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sao%20gabriel%20da%20cachoeira
    Processing Record 38 of 500 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=atuona
    Processing Record 39 of 500 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mar%20del%20plata
    Processing Record 40 of 500 | gustavo diaz ordaz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gustavo%20diaz%20ordaz
    Processing Record 41 of 500 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hilo
    Processing Record 42 of 500 | singleton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=singleton
    Processing Record 43 of 500 | caravelas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=caravelas
    Processing Record 44 of 500 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cidreira
    Processing Record 45 of 500 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=thinadhoo
    Processing Record 46 of 500 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=isangel
    Processing Record 47 of 500 | bontang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bontang
    Processing Record 48 of 500 | cape town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cape%20town
    Processing Record 49 of 500 | upernavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=upernavik
    Processing Record 50 of 500 | hofn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hofn
    Processing Record 51 of 500 | ruatoria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ruatoria
    Error locating City data. Skipping...
    Processing Record 52 of 500 | otradnoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=otradnoye
    Processing Record 53 of 500 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sao%20filipe
    Processing Record 54 of 500 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20patricio
    Processing Record 55 of 500 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vila%20velha
    Processing Record 56 of 500 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vaini
    Processing Record 57 of 500 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nikolskoye
    Processing Record 58 of 500 | dikson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dikson
    Processing Record 59 of 500 | grindavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=grindavik
    Processing Record 60 of 500 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=iqaluit
    Processing Record 61 of 500 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sisimiut
    Processing Record 62 of 500 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cherskiy
    Processing Record 63 of 500 | kamenskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kamenskoye
    Error locating City data. Skipping...
    Processing Record 64 of 500 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=srednekolymsk
    Processing Record 65 of 500 | xuddur
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=xuddur
    Processing Record 66 of 500 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hermanus
    Processing Record 67 of 500 | volot
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=volot
    Processing Record 68 of 500 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hithadhoo
    Processing Record 69 of 500 | eyl
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=eyl
    Processing Record 70 of 500 | antofagasta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=antofagasta
    Processing Record 71 of 500 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=arraial%20do%20cabo
    Processing Record 72 of 500 | amderma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=amderma
    Error locating City data. Skipping...
    Processing Record 73 of 500 | urbana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=urbana
    Processing Record 74 of 500 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=geraldton
    Processing Record 75 of 500 | cairns
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cairns
    Processing Record 76 of 500 | collierville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=collierville
    Processing Record 77 of 500 | mount isa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mount%20isa
    Processing Record 78 of 500 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pevek
    Processing Record 79 of 500 | myanaung
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=myanaung
    Processing Record 80 of 500 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kloulklubed
    Processing Record 81 of 500 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=attawapiskat
    Error locating City data. Skipping...
    Processing Record 82 of 500 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=narsaq
    Processing Record 83 of 500 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kodiak
    Processing Record 84 of 500 | broome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=broome
    Processing Record 85 of 500 | kibala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kibala
    Processing Record 86 of 500 | paamiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=paamiut
    Processing Record 87 of 500 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=souillac
    Processing Record 88 of 500 | katherine
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=katherine
    Processing Record 89 of 500 | pringsewu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pringsewu
    Processing Record 90 of 500 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=illoqqortoormiut
    Error locating City data. Skipping...
    Processing Record 91 of 500 | beira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=beira
    Processing Record 92 of 500 | paraul
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=paraul
    Processing Record 93 of 500 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=qaqortoq
    Processing Record 94 of 500 | mareeba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mareeba
    Processing Record 95 of 500 | summerville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=summerville
    Processing Record 96 of 500 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=busselton
    Processing Record 97 of 500 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bluff
    Processing Record 98 of 500 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=puerto%20ayora
    Processing Record 99 of 500 | iranduba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=iranduba
    Processing Record 100 of 500 | bangolo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bangolo
    Processing Record 101 of 500 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=yellowknife
    Processing Record 102 of 500 | warqla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=warqla
    Error locating City data. Skipping...
    Processing Record 103 of 500 | turayf
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=turayf
    Processing Record 104 of 500 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ribeira%20grande
    Processing Record 105 of 500 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=avarua
    Processing Record 106 of 500 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saskylakh
    Processing Record 107 of 500 | nioro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nioro
    Processing Record 108 of 500 | sumbe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sumbe
    Processing Record 109 of 500 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=taoudenni
    Processing Record 110 of 500 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vila%20franca%20do%20campo
    Processing Record 111 of 500 | hammerfest
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hammerfest
    Processing Record 112 of 500 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kruisfontein
    Processing Record 113 of 500 | las palmas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=las%20palmas
    Processing Record 114 of 500 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=victoria
    Processing Record 115 of 500 | fare
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fare
    Processing Record 116 of 500 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tasiilaq
    Processing Record 117 of 500 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=port%20elizabeth
    Processing Record 118 of 500 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hamilton
    Processing Record 119 of 500 | beloha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=beloha
    Processing Record 120 of 500 | airai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=airai
    Processing Record 121 of 500 | chuy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chuy
    Processing Record 122 of 500 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nizhneyansk
    Error locating City data. Skipping...
    Processing Record 123 of 500 | dasoguz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dasoguz
    Processing Record 124 of 500 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=butaritari
    Processing Record 125 of 500 | khomeynishahr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=khomeynishahr
    Error locating City data. Skipping...
    Processing Record 126 of 500 | zyryanka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=zyryanka
    Processing Record 127 of 500 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=port%20alfred
    Processing Record 128 of 500 | kollumerland
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kollumerland
    Error locating City data. Skipping...
    Processing Record 129 of 500 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=khatanga
    Processing Record 130 of 500 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sentyabrskiy
    Error locating City data. Skipping...
    Processing Record 131 of 500 | gobabis
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gobabis
    Processing Record 132 of 500 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=zhigansk
    Processing Record 133 of 500 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=constitucion
    Processing Record 134 of 500 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=severo-kurilsk
    Processing Record 135 of 500 | anito
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=anito
    Processing Record 136 of 500 | igarka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=igarka
    Processing Record 137 of 500 | camacha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=camacha
    Processing Record 138 of 500 | santa rosa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=santa%20rosa
    Processing Record 139 of 500 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tuktoyaktuk
    Processing Record 140 of 500 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=plettenberg%20bay
    Processing Record 141 of 500 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=barentsburg
    Error locating City data. Skipping...
    Processing Record 142 of 500 | bergen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bergen
    Error locating City data. Skipping...
    Processing Record 143 of 500 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mount%20gambier
    Processing Record 144 of 500 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=longyearbyen
    Processing Record 145 of 500 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=carnarvon
    Processing Record 146 of 500 | tashla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tashla
    Processing Record 147 of 500 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lavrentiya
    Processing Record 148 of 500 | chukhloma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chukhloma
    Processing Record 149 of 500 | lingao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lingao
    Processing Record 150 of 500 | viedma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=viedma
    Processing Record 151 of 500 | atocha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=atocha
    Processing Record 152 of 500 | griffith
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=griffith
    Processing Record 153 of 500 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=komsomolskiy
    Processing Record 154 of 500 | hazorasp
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hazorasp
    Processing Record 155 of 500 | sharjah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sharjah
    Processing Record 156 of 500 | rancho palos verdes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rancho%20palos%20verdes
    Processing Record 157 of 500 | neiafu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=neiafu
    Processing Record 158 of 500 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=port%20lincoln
    Processing Record 159 of 500 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=puerto%20escondido
    Processing Record 160 of 500 | ukiah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ukiah
    Processing Record 161 of 500 | amarwara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=amarwara
    Processing Record 162 of 500 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=barrow
    Processing Record 163 of 500 | babrala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=babrala
    Processing Record 164 of 500 | aurad
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aurad
    Processing Record 165 of 500 | dunedin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dunedin
    Processing Record 166 of 500 | aljezur
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aljezur
    Processing Record 167 of 500 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mahebourg
    Processing Record 168 of 500 | maldonado
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=maldonado
    Processing Record 169 of 500 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tiksi
    Processing Record 170 of 500 | puerto el triunfo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=puerto%20el%20triunfo
    Processing Record 171 of 500 | niteroi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=niteroi
    Processing Record 172 of 500 | bulawayo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bulawayo
    Processing Record 173 of 500 | inta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=inta
    Processing Record 174 of 500 | chabahar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chabahar
    Processing Record 175 of 500 | tres passos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tres%20passos
    Processing Record 176 of 500 | torres
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=torres
    Processing Record 177 of 500 | ruteng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ruteng
    Processing Record 178 of 500 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tabiauea
    Error locating City data. Skipping...
    Processing Record 179 of 500 | peru
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=peru
    Processing Record 180 of 500 | penzance
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=penzance
    Processing Record 181 of 500 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sinnamary
    Processing Record 182 of 500 | aitape
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aitape
    Processing Record 183 of 500 | caconda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=caconda
    Processing Record 184 of 500 | ekangala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ekangala
    Processing Record 185 of 500 | moshupa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=moshupa
    Processing Record 186 of 500 | chama
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chama
    Processing Record 187 of 500 | rafaela
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rafaela
    Processing Record 188 of 500 | chicama
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chicama
    Processing Record 189 of 500 | vardo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vardo
    Processing Record 190 of 500 | kriel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kriel
    Processing Record 191 of 500 | maragogi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=maragogi
    Processing Record 192 of 500 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mys%20shmidta
    Error locating City data. Skipping...
    Processing Record 193 of 500 | te anau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=te%20anau
    Processing Record 194 of 500 | halifax
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=halifax
    Processing Record 195 of 500 | simao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=simao
    Processing Record 196 of 500 | cazaje
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cazaje
    Error locating City data. Skipping...
    Processing Record 197 of 500 | walvis bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=walvis%20bay
    Processing Record 198 of 500 | saleaula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saleaula
    Error locating City data. Skipping...
    Processing Record 199 of 500 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=umzimvubu
    Error locating City data. Skipping...
    Processing Record 200 of 500 | santa marta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=santa%20marta
    Processing Record 201 of 500 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=esperance
    Processing Record 202 of 500 | birnin kebbi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=birnin%20kebbi
    Processing Record 203 of 500 | samusu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=samusu
    Error locating City data. Skipping...
    Processing Record 204 of 500 | yumen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=yumen
    Processing Record 205 of 500 | goderich
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=goderich
    Processing Record 206 of 500 | kikwit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kikwit
    Processing Record 207 of 500 | sorong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sorong
    Processing Record 208 of 500 | saint-leu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint-leu
    Processing Record 209 of 500 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=grand%20river%20south%20east
    Error locating City data. Skipping...
    Processing Record 210 of 500 | dingle
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dingle
    Processing Record 211 of 500 | mahon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mahon
    Processing Record 212 of 500 | mega
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mega
    Processing Record 213 of 500 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20cristobal
    Processing Record 214 of 500 | panguna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=panguna
    Processing Record 215 of 500 | opuwo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=opuwo
    Processing Record 216 of 500 | aflu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aflu
    Error locating City data. Skipping...
    Processing Record 217 of 500 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bengkulu
    Error locating City data. Skipping...
    Processing Record 218 of 500 | kongsberg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kongsberg
    Processing Record 219 of 500 | hobyo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hobyo
    Processing Record 220 of 500 | bilma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bilma
    Processing Record 221 of 500 | campbell river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=campbell%20river
    Processing Record 222 of 500 | namibe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=namibe
    Processing Record 223 of 500 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saldanha
    Processing Record 224 of 500 | nokaneng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nokaneng
    Processing Record 225 of 500 | lerwick
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lerwick
    Processing Record 226 of 500 | kazachinskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kazachinskoye
    Processing Record 227 of 500 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=torbay
    Processing Record 228 of 500 | staraya russa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=staraya%20russa
    Processing Record 229 of 500 | chumikan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chumikan
    Processing Record 230 of 500 | lorengau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lorengau
    Processing Record 231 of 500 | port blair
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=port%20blair
    Processing Record 232 of 500 | vieste
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vieste
    Processing Record 233 of 500 | lata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lata
    Processing Record 234 of 500 | east london
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=east%20london
    Processing Record 235 of 500 | pisco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pisco
    Processing Record 236 of 500 | annau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=annau
    Processing Record 237 of 500 | erenhot
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=erenhot
    Processing Record 238 of 500 | ancud
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ancud
    Processing Record 239 of 500 | ndele
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ndele
    Error locating City data. Skipping...
    Processing Record 240 of 500 | kuche
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kuche
    Error locating City data. Skipping...
    Processing Record 241 of 500 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ostrovnoy
    Processing Record 242 of 500 | lazaro cardenas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lazaro%20cardenas
    Processing Record 243 of 500 | shelburne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=shelburne
    Processing Record 244 of 500 | haibowan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=haibowan
    Error locating City data. Skipping...
    Processing Record 245 of 500 | machico
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=machico
    Processing Record 246 of 500 | hambantota
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hambantota
    Processing Record 247 of 500 | kulim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kulim
    Processing Record 248 of 500 | la ronge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=la%20ronge
    Processing Record 249 of 500 | kokopo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kokopo
    Processing Record 250 of 500 | vagay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vagay
    Processing Record 251 of 500 | hutchinson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hutchinson
    Processing Record 252 of 500 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=georgetown
    Processing Record 253 of 500 | yulara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=yulara
    Processing Record 254 of 500 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint-philippe
    Processing Record 255 of 500 | novodugino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=novodugino
    Processing Record 256 of 500 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tumannyy
    Error locating City data. Skipping...
    Processing Record 257 of 500 | manta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=manta
    Processing Record 258 of 500 | temaraia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=temaraia
    Error locating City data. Skipping...
    Processing Record 259 of 500 | tairua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tairua
    Processing Record 260 of 500 | keokuk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=keokuk
    Processing Record 261 of 500 | saint andrews
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint%20andrews
    Processing Record 262 of 500 | chipinge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chipinge
    Processing Record 263 of 500 | suzun
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=suzun
    Processing Record 264 of 500 | presidencia roque saenz pena
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=presidencia%20roque%20saenz%20pena
    Processing Record 265 of 500 | djibo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=djibo
    Processing Record 266 of 500 | isla mujeres
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=isla%20mujeres
    Processing Record 267 of 500 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=belushya%20guba
    Error locating City data. Skipping...
    Processing Record 268 of 500 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=beringovskiy
    Processing Record 269 of 500 | asosa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=asosa
    Processing Record 270 of 500 | isugod
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=isugod
    Processing Record 271 of 500 | trabzon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=trabzon
    Processing Record 272 of 500 | sistranda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sistranda
    Processing Record 273 of 500 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kaitangata
    Processing Record 274 of 500 | san pedro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20pedro
    Processing Record 275 of 500 | fort-shevchenko
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fort-shevchenko
    Processing Record 276 of 500 | kalabo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kalabo
    Processing Record 277 of 500 | puyo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=puyo
    Processing Record 278 of 500 | fortuna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fortuna
    Processing Record 279 of 500 | amazar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=amazar
    Processing Record 280 of 500 | ivybridge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ivybridge
    Processing Record 281 of 500 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=comodoro%20rivadavia
    Processing Record 282 of 500 | hirado
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hirado
    Processing Record 283 of 500 | pontianak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pontianak
    Processing Record 284 of 500 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=thompson
    Processing Record 285 of 500 | russell
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=russell
    Processing Record 286 of 500 | abnub
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=abnub
    Processing Record 287 of 500 | mana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mana
    Processing Record 288 of 500 | gari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gari
    Processing Record 289 of 500 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=egvekinot
    Processing Record 290 of 500 | busca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=busca
    Processing Record 291 of 500 | ibra
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ibra
    Processing Record 292 of 500 | lyngseidet
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lyngseidet
    Processing Record 293 of 500 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=clyde%20river
    Processing Record 294 of 500 | sitka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sitka
    Processing Record 295 of 500 | gorey
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gorey
    Processing Record 296 of 500 | peace river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=peace%20river
    Processing Record 297 of 500 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=skalistyy
    Error locating City data. Skipping...
    Processing Record 298 of 500 | sangolqui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sangolqui
    Processing Record 299 of 500 | seddon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=seddon
    Processing Record 300 of 500 | beisfjord
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=beisfjord
    Processing Record 301 of 500 | muskogee
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=muskogee
    Processing Record 302 of 500 | dese
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dese
    Processing Record 303 of 500 | ouesso
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ouesso
    Processing Record 304 of 500 | doha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=doha
    Processing Record 305 of 500 | novyy urengoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=novyy%20urengoy
    Processing Record 306 of 500 | starkville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=starkville
    Processing Record 307 of 500 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=norman%20wells
    Processing Record 308 of 500 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cabo%20san%20lucas
    Processing Record 309 of 500 | mossendjo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mossendjo
    Processing Record 310 of 500 | praya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=praya
    Processing Record 311 of 500 | ormara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ormara
    Processing Record 312 of 500 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=palabuhanratu
    Error locating City data. Skipping...
    Processing Record 313 of 500 | alekseyevsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=alekseyevsk
    Processing Record 314 of 500 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tilichiki
    Processing Record 315 of 500 | ascension
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ascension
    Error locating City data. Skipping...
    Processing Record 316 of 500 | hachinohe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hachinohe
    Processing Record 317 of 500 | vestnes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vestnes
    Processing Record 318 of 500 | letnyaya stavka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=letnyaya%20stavka
    Processing Record 319 of 500 | uray
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=uray
    Processing Record 320 of 500 | haapiti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=haapiti
    Processing Record 321 of 500 | morondava
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=morondava
    Processing Record 322 of 500 | ucluelet
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ucluelet
    Processing Record 323 of 500 | kavieng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kavieng
    Processing Record 324 of 500 | haines junction
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=haines%20junction
    Processing Record 325 of 500 | mayo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mayo
    Processing Record 326 of 500 | paramirim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=paramirim
    Processing Record 327 of 500 | mizdah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mizdah
    Processing Record 328 of 500 | orel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=orel
    Processing Record 329 of 500 | skovorodino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=skovorodino
    Processing Record 330 of 500 | lodja
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lodja
    Processing Record 331 of 500 | atar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=atar
    Processing Record 332 of 500 | halalo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=halalo
    Error locating City data. Skipping...
    Processing Record 333 of 500 | marigot
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=marigot
    Processing Record 334 of 500 | urumqi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=urumqi
    Error locating City data. Skipping...
    Processing Record 335 of 500 | dwarka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dwarka
    Processing Record 336 of 500 | mildura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mildura
    Processing Record 337 of 500 | gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gornopravdinsk
    Processing Record 338 of 500 | visnes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=visnes
    Processing Record 339 of 500 | muzhi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=muzhi
    Processing Record 340 of 500 | montgomery
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=montgomery
    Processing Record 341 of 500 | heyang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=heyang
    Processing Record 342 of 500 | novosysoyevka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=novosysoyevka
    Processing Record 343 of 500 | fukue
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fukue
    Processing Record 344 of 500 | laguna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=laguna
    Processing Record 345 of 500 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=coquimbo
    Processing Record 346 of 500 | daultala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=daultala
    Processing Record 347 of 500 | sangar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sangar
    Processing Record 348 of 500 | luderitz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=luderitz
    Processing Record 349 of 500 | quartucciu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=quartucciu
    Processing Record 350 of 500 | cleethorpes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cleethorpes
    Processing Record 351 of 500 | belyy yar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=belyy%20yar
    Processing Record 352 of 500 | salekhard
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=salekhard
    Processing Record 353 of 500 | nkoteng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nkoteng
    Processing Record 354 of 500 | kalulushi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kalulushi
    Processing Record 355 of 500 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vaitupu
    Error locating City data. Skipping...
    Processing Record 356 of 500 | thano bula khan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=thano%20bula%20khan
    Error locating City data. Skipping...
    Processing Record 357 of 500 | sept-iles
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sept-iles
    Processing Record 358 of 500 | hackettstown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hackettstown
    Processing Record 359 of 500 | kismayo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kismayo
    Error locating City data. Skipping...
    Processing Record 360 of 500 | lephepe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lephepe
    Error locating City data. Skipping...
    Processing Record 361 of 500 | birjand
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=birjand
    Processing Record 362 of 500 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=guerrero%20negro
    Processing Record 363 of 500 | tessalit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tessalit
    Processing Record 364 of 500 | miles city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=miles%20city
    Processing Record 365 of 500 | bikaner
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bikaner
    Processing Record 366 of 500 | afmadu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=afmadu
    Error locating City data. Skipping...
    Processing Record 367 of 500 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sao%20joao%20da%20barra
    Processing Record 368 of 500 | sakakah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sakakah
    Error locating City data. Skipping...
    Processing Record 369 of 500 | saint george
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint%20george
    Processing Record 370 of 500 | barberton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=barberton
    Processing Record 371 of 500 | evensk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=evensk
    Processing Record 372 of 500 | corvallis
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=corvallis
    Processing Record 373 of 500 | bethel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bethel
    Processing Record 374 of 500 | tautira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tautira
    Processing Record 375 of 500 | tarana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tarana
    Processing Record 376 of 500 | hutang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hutang
    Processing Record 377 of 500 | tarko-sale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tarko-sale
    Processing Record 378 of 500 | mombetsu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mombetsu
    Processing Record 379 of 500 | dudinka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dudinka
    Processing Record 380 of 500 | portland
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=portland
    Processing Record 381 of 500 | freeport
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=freeport
    Processing Record 382 of 500 | akyab
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=akyab
    Error locating City data. Skipping...
    Processing Record 383 of 500 | jijiga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=jijiga
    Processing Record 384 of 500 | vao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vao
    Processing Record 385 of 500 | katobu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=katobu
    Processing Record 386 of 500 | staraya poltavka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=staraya%20poltavka
    Processing Record 387 of 500 | hualmay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hualmay
    Processing Record 388 of 500 | sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sao%20felix%20do%20xingu
    Processing Record 389 of 500 | moose jaw
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=moose%20jaw
    Processing Record 390 of 500 | kemijarvi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kemijarvi
    Error locating City data. Skipping...
    Processing Record 391 of 500 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nouadhibou
    Processing Record 392 of 500 | abu dhabi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=abu%20dhabi
    Processing Record 393 of 500 | kjopsvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kjopsvik
    Processing Record 394 of 500 | yatou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=yatou
    Processing Record 395 of 500 | orocue
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=orocue
    Processing Record 396 of 500 | ankpa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ankpa
    Processing Record 397 of 500 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sobolevo
    Processing Record 398 of 500 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bambous%20virieux
    Processing Record 399 of 500 | coihaique
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=coihaique
    Processing Record 400 of 500 | great falls
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=great%20falls
    Processing Record 401 of 500 | staraya toropa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=staraya%20toropa
    Processing Record 402 of 500 | maanshan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=maanshan
    Processing Record 403 of 500 | madang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=madang
    Processing Record 404 of 500 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=batemans%20bay
    Processing Record 405 of 500 | gravdal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gravdal
    Processing Record 406 of 500 | sulangan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sulangan
    Processing Record 407 of 500 | rajpipla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rajpipla
    Processing Record 408 of 500 | jalu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=jalu
    Processing Record 409 of 500 | tabialan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tabialan
    Error locating City data. Skipping...
    Processing Record 410 of 500 | sukhobuzimskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sukhobuzimskoye
    Processing Record 411 of 500 | pandaria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pandaria
    Processing Record 412 of 500 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dongsheng
    Processing Record 413 of 500 | ust-shonosha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ust-shonosha
    Processing Record 414 of 500 | murmashi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=murmashi
    Processing Record 415 of 500 | mbandaka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mbandaka
    Processing Record 416 of 500 | leninskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=leninskoye
    Processing Record 417 of 500 | erzincan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=erzincan
    Processing Record 418 of 500 | sansai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sansai
    Error locating City data. Skipping...
    Processing Record 419 of 500 | baturaja
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=baturaja
    Processing Record 420 of 500 | arlit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=arlit
    Processing Record 421 of 500 | flin flon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=flin%20flon
    Processing Record 422 of 500 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fort%20nelson
    Processing Record 423 of 500 | san quintin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20quintin
    Processing Record 424 of 500 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=klaksvik
    Processing Record 425 of 500 | havoysund
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=havoysund
    Processing Record 426 of 500 | kyzyl-suu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kyzyl-suu
    Processing Record 427 of 500 | madera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=madera
    Processing Record 428 of 500 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ilulissat
    Processing Record 429 of 500 | ngukurr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ngukurr
    Error locating City data. Skipping...
    Processing Record 430 of 500 | businga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=businga
    Processing Record 431 of 500 | manaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=manaia
    Processing Record 432 of 500 | rambha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=rambha
    Processing Record 433 of 500 | graciano sanchez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=graciano%20sanchez
    Processing Record 434 of 500 | skibbereen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=skibbereen
    Processing Record 435 of 500 | male
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=male
    Processing Record 436 of 500 | dali
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dali
    Processing Record 437 of 500 | gazanjyk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gazanjyk
    Processing Record 438 of 500 | kieta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kieta
    Processing Record 439 of 500 | gazojak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gazojak
    Processing Record 440 of 500 | hargeysa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hargeysa
    Processing Record 441 of 500 | lagoa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lagoa
    Processing Record 442 of 500 | alofi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=alofi
    Processing Record 443 of 500 | kiunga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kiunga
    Processing Record 444 of 500 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=barra%20do%20garcas
    Processing Record 445 of 500 | tarboro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tarboro
    Processing Record 446 of 500 | mendahara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mendahara
    Error locating City data. Skipping...
    Processing Record 447 of 500 | washington
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=washington
    Processing Record 448 of 500 | mantua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mantua
    Processing Record 449 of 500 | aripuana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aripuana
    Processing Record 450 of 500 | conway
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=conway
    Processing Record 451 of 500 | marcona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=marcona
    Error locating City data. Skipping...
    Processing Record 452 of 500 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kavaratti
    Processing Record 453 of 500 | mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mitsamiouli
    Processing Record 454 of 500 | kayes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kayes
    Processing Record 455 of 500 | williams lake
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=williams%20lake
    Processing Record 456 of 500 | myitkyina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=myitkyina
    Processing Record 457 of 500 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pangnirtung
    Processing Record 458 of 500 | hailey
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hailey
    Processing Record 459 of 500 | galiwinku
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=galiwinku
    Error locating City data. Skipping...
    Processing Record 460 of 500 | toliary
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=toliary
    Error locating City data. Skipping...
    Processing Record 461 of 500 | kahului
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kahului
    Processing Record 462 of 500 | mazagao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mazagao
    Processing Record 463 of 500 | talaya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=talaya
    Processing Record 464 of 500 | santa maria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=santa%20maria
    Processing Record 465 of 500 | ola
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ola
    Processing Record 466 of 500 | iracoubo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=iracoubo
    Processing Record 467 of 500 | ostersund
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ostersund
    Processing Record 468 of 500 | amapa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=amapa
    Processing Record 469 of 500 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=los%20llanos%20de%20aridane
    Processing Record 470 of 500 | escanaba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=escanaba
    Processing Record 471 of 500 | fonte boa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fonte%20boa
    Processing Record 472 of 500 | maniitsoq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=maniitsoq
    Processing Record 473 of 500 | college
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=college
    Processing Record 474 of 500 | dandong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dandong
    Processing Record 475 of 500 | porto novo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=porto%20novo
    Processing Record 476 of 500 | mahajanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mahajanga
    Processing Record 477 of 500 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kudahuvadhoo
    Processing Record 478 of 500 | ca mau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ca%20mau
    Processing Record 479 of 500 | darhan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=darhan
    Processing Record 480 of 500 | riihimaki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=riihimaki
    Processing Record 481 of 500 | fairhope
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=fairhope
    Processing Record 482 of 500 | vredendal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vredendal
    Processing Record 483 of 500 | margate
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=margate
    Processing Record 484 of 500 | roma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=roma
    Processing Record 485 of 500 | samana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=samana
    Processing Record 486 of 500 | aklavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=aklavik
    Processing Record 487 of 500 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint-augustin
    Processing Record 488 of 500 | bafia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bafia
    Processing Record 489 of 500 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=provideniya
    Processing Record 490 of 500 | mishan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mishan
    Processing Record 491 of 500 | lolua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lolua
    Error locating City data. Skipping...
    Processing Record 492 of 500 | malatya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=malatya
    Error locating City data. Skipping...
    Processing Record 493 of 500 | ittiri
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ittiri
    Processing Record 494 of 500 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=codrington
    Processing Record 495 of 500 | kourou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kourou
    Processing Record 496 of 500 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ketchikan
    Processing Record 497 of 500 | ahipara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ahipara
    Processing Record 498 of 500 | chara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chara
    Processing Record 499 of 500 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ambilobe
    Processing Record 0 of 500 | chokwe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chokwe
    Error locating City data. Skipping...
    Processing Record 1 of 500 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=leningradskiy
    Processing Record 2 of 500 | bosaso
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bosaso
    Processing Record 3 of 500 | hays
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hays
    Processing Record 4 of 500 | dukat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dukat
    Processing Record 5 of 500 | riberalta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=riberalta
    Processing Record 6 of 500 | the valley
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=the%20valley
    Processing Record 7 of 500 | svetlyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=svetlyy
    Error locating City data. Skipping...
    Processing Record 8 of 500 | mitu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mitu
    Processing Record 9 of 500 | buala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=buala
    Processing Record 10 of 500 | pasuruan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pasuruan
    Processing Record 11 of 500 | quelimane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=quelimane
    Processing Record 12 of 500 | udachnyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=udachnyy
    Processing Record 13 of 500 | salalah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=salalah
    Processing Record 14 of 500 | nam tha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nam%20tha
    Error locating City data. Skipping...
    Processing Record 15 of 500 | bull savanna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bull%20savanna
    Processing Record 16 of 500 | adre
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=adre
    Processing Record 17 of 500 | ugoofaaru
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ugoofaaru
    Processing Record 18 of 500 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=okhotsk
    Processing Record 19 of 500 | kutum
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kutum
    Processing Record 20 of 500 | nome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nome
    Processing Record 21 of 500 | tubod
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tubod
    Processing Record 22 of 500 | phalaborwa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=phalaborwa
    Processing Record 23 of 500 | wulanhaote
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=wulanhaote
    Error locating City data. Skipping...
    Processing Record 24 of 500 | celestun
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=celestun
    Processing Record 25 of 500 | singkang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=singkang
    Processing Record 26 of 500 | abu kamal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=abu%20kamal
    Processing Record 27 of 500 | maumere
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=maumere
    Processing Record 28 of 500 | tacuarembo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tacuarembo
    Processing Record 29 of 500 | dvinskoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=dvinskoy
    Processing Record 30 of 500 | pobe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pobe
    Processing Record 31 of 500 | simeonovgrad
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=simeonovgrad
    Processing Record 32 of 500 | grand-santi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=grand-santi
    Processing Record 33 of 500 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cap%20malheureux
    Processing Record 34 of 500 | katsuura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=katsuura
    Processing Record 35 of 500 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=faanui
    Processing Record 36 of 500 | trinidad
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=trinidad
    Processing Record 37 of 500 | vallenar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vallenar
    Processing Record 38 of 500 | lakatoro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=lakatoro
    Processing Record 39 of 500 | zapolyarnyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=zapolyarnyy
    Processing Record 40 of 500 | pervomaysk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=pervomaysk
    Processing Record 41 of 500 | devyatka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=devyatka
    Error locating City data. Skipping...
    Processing Record 42 of 500 | bilibino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bilibino
    Processing Record 43 of 500 | basco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=basco
    Processing Record 44 of 500 | antibes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=antibes
    Processing Record 45 of 500 | tir pol
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tir%20pol
    Error locating City data. Skipping...
    Processing Record 46 of 500 | poyarkovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=poyarkovo
    Processing Record 47 of 500 | weyburn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=weyburn
    Processing Record 48 of 500 | mattru
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mattru
    Processing Record 49 of 500 | matagami
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=matagami
    Processing Record 50 of 500 | obihiro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=obihiro
    Processing Record 51 of 500 | palmer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=palmer
    Processing Record 52 of 500 | tabou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tabou
    Processing Record 53 of 500 | wellsford
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=wellsford
    Processing Record 54 of 500 | junction city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=junction%20city
    Processing Record 55 of 500 | deggendorf
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=deggendorf
    Processing Record 56 of 500 | anadyr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=anadyr
    Processing Record 57 of 500 | kubrat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kubrat
    Processing Record 58 of 500 | san jeronimo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20jeronimo
    Processing Record 59 of 500 | andenes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=andenes
    Error locating City data. Skipping...
    Processing Record 60 of 500 | jatara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=jatara
    Processing Record 61 of 500 | satitoa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=satitoa
    Error locating City data. Skipping...
    Processing Record 62 of 500 | bardiyah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bardiyah
    Error locating City data. Skipping...
    Processing Record 63 of 500 | raudeberg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=raudeberg
    Processing Record 64 of 500 | camana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=camana
    Error locating City data. Skipping...
    Processing Record 65 of 500 | mecca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=mecca
    Processing Record 66 of 500 | killybegs
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=killybegs
    Processing Record 67 of 500 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nanortalik
    Processing Record 68 of 500 | tubruq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tubruq
    Error locating City data. Skipping...
    Processing Record 69 of 500 | helsinge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=helsinge
    Processing Record 70 of 500 | ondjiva
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ondjiva
    Processing Record 71 of 500 | coffs harbour
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=coffs%20harbour
    Processing Record 72 of 500 | faya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=faya
    Processing Record 73 of 500 | cape coast
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=cape%20coast
    Processing Record 74 of 500 | palizada
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=palizada
    Processing Record 75 of 500 | candolim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=candolim
    Processing Record 76 of 500 | sterling
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sterling
    Processing Record 77 of 500 | biak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=biak
    Processing Record 78 of 500 | kimbe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=kimbe
    Processing Record 79 of 500 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=yarmouth
    Processing Record 80 of 500 | hvide sande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hvide%20sande
    Processing Record 81 of 500 | touros
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=touros
    Processing Record 82 of 500 | husavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=husavik
    Processing Record 83 of 500 | presidente olegario
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=presidente%20olegario
    Processing Record 84 of 500 | khash
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=khash
    Processing Record 85 of 500 | bloemhof
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bloemhof
    Processing Record 86 of 500 | noumea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=noumea
    Processing Record 87 of 500 | tambacounda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=tambacounda
    Processing Record 88 of 500 | abha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=abha
    Processing Record 89 of 500 | chlorakas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=chlorakas
    Error locating City data. Skipping...
    Processing Record 90 of 500 | matale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=matale
    Processing Record 91 of 500 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hasaki
    Processing Record 92 of 500 | bulungu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=bulungu
    Processing Record 93 of 500 | houma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=houma
    Processing Record 94 of 500 | san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=san%20carlos%20de%20bariloche
    Processing Record 95 of 500 | nantucket
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=nantucket
    Processing Record 96 of 500 | afrikanda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=afrikanda
    Processing Record 97 of 500 | vostok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=vostok
    Processing Record 98 of 500 | sanlucar de barrameda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=sanlucar%20de%20barrameda
    Processing Record 99 of 500 | westpunt
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=westpunt
    Error locating City data. Skipping...
    Processing Record 100 of 500 | hami
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=hami
    Processing Record 101 of 500 | gushi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=gushi
    Processing Record 102 of 500 | turukhansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=turukhansk
    Processing Record 103 of 500 | along
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=along
    Processing Record 104 of 500 | xinxiang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=xinxiang
    Processing Record 105 of 500 | homer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=homer
    Processing Record 106 of 500 | devils lake
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=devils%20lake
    Processing Record 107 of 500 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=half%20moon%20bay
    Processing Record 108 of 500 | saint-louis
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=saint-louis
    Processing Record 109 of 500 | ornskoldsvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=ornskoldsvik
    Processing Record 110 of 500 | axim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=4328b000df419885e1b4acf29ef2cd9f&q=axim
    -----------------------------
    Data Load Complete           
    -----------------------------



```
city_data_pd = pd.DataFrame(city_data)

lats = city_data_pd["Lat"]
max_temps = city_data_pd["Max Temp"]
humidity = city_data_pd["Humidity"]
cloudiness = city_data_pd["Cloudiness"]
wind_speed = city_data_pd["Wind Speed"]

# Export the City_Data into a csv
city_data_pd.to_csv(output_data_file, index_label="City_ID")

city_data_pd.count()
```




    City          546
    Cloudiness    546
    Country       546
    Date          546
    Humidity      546
    Lat           546
    Lng           546
    Max Temp      546
    Wind Speed    546
    dtype: int64




```
city_data_pd.head()
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
      <th>0</th>
      <td>punta arenas</td>
      <td>75</td>
      <td>CL</td>
      <td>1529902800</td>
      <td>93</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>35.60</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bredasdorp</td>
      <td>92</td>
      <td>ZA</td>
      <td>1529899200</td>
      <td>76</td>
      <td>-34.53</td>
      <td>20.04</td>
      <td>51.80</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>beyneu</td>
      <td>12</td>
      <td>KZ</td>
      <td>1529905025</td>
      <td>14</td>
      <td>45.32</td>
      <td>55.19</td>
      <td>94.97</td>
      <td>4.61</td>
    </tr>
    <tr>
      <th>3</th>
      <td>qasigiannguit</td>
      <td>88</td>
      <td>GL</td>
      <td>1529902200</td>
      <td>93</td>
      <td>68.82</td>
      <td>-51.19</td>
      <td>35.60</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>4</th>
      <td>jamestown</td>
      <td>92</td>
      <td>AU</td>
      <td>1529905026</td>
      <td>73</td>
      <td>-33.21</td>
      <td>138.60</td>
      <td>49.25</td>
      <td>4.50</td>
    </tr>
  </tbody>
</table>
</div>



Latitude vs. Humidity Plot


```
plt.scatter(lats, 
            humidity,
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.6, label="Cities")

plt.title("City Latitude vs. Humidity for (%s)" % time.strftime("%x"))
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)

plt.savefig("output_data/Fig1.png")

plt.show()
```


![png](output_8_0.png)


Latitude vs Temperature Plot


```
plt.scatter(lats, 
            max_temps,
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.6, label="Cities")

plt.title("City Latitude vs. Max Temperature for (%s)" % time.strftime("%x"))
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

plt.savefig("output_data/Fig2.png")

plt.show()
```


![png](output_10_0.png)


Latitude vs. Cloudiness Plot


```
plt.scatter(lats, 
            cloudiness,
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.6, label="Cities")

plt.title("City Latitude vs. Cloudiness for (%s)" % time.strftime("%x"))
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)

plt.savefig("output_data/Fig3.png")

# Show plot
plt.show()
```


![png](output_12_0.png)


Latitude vs. Wind Speed Plot


```
plt.scatter(lats, 
            wind_speed,
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.6, label="Cities")

plt.title("City Latitude vs. Wind Speed for (%s)" % time.strftime("%x"))
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)

plt.savefig("output_data/Fig4.png")

plt.show()
```


![png](output_14_0.png)

