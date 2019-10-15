
# Bart Stations

This notebook shows how you can crawl the Bart website, extract station names and addresses and plot them in Google Maps


```python
import requests
c = requests.get('https://www.bart.gov/stations').content
```


```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(c)
```


```python
links = soup.find_all('a')
```


```python
# It looks like Bart assigns a 4 character code for every station
import re
station_exp = re.compile('/stations/.{4}$')
```


```python
hrefs = [link.attrs['href'] for link in links]
stations = [href for href in hrefs if station_exp.match(href)]
```


```python
base_url = 'https://www.bart.gov'
address = re.compile('/planner\?origin=(.*)$')
print("station_name,station_address")
for s in stations:
    c = requests.get(f'{base_url}{s}').content
    station_name = BeautifulSoup(c).find_all('h1')[0].text
    links = BeautifulSoup(c).find_all('a', href=True)
    hrefs = [link.attrs['href'] for link in links]
    addresses = [address.match(href).group(1) for href in hrefs if address.match(href)]
    print(f'"{station_name}","{addresses[0]}"')
```

    station_name,station_address
    "12th St. Oakland City Center Station","1245 Broadway, Oakland, CA 94612"
    "16th St. Mission Station","2000 Mission Street, San Francisco, CA 94110"
    "19th St. Oakland Station","1900 Broadway, Oakland, CA 94612"
    "24th St. Mission Station","2800 Mission Street, San Francisco, CA 94110"
    "Antioch Station","1600 Slatten Ranch Road, Antioch, CA 94509"
    "Ashby Station","3100 Adeline Street, Berkeley, CA 94703"
    "Balboa Park Station","401 Geneva Avenue, San Francisco, CA 94112"
    "Bay Fair Station","15242 Hesperian Blvd., San Leandro, CA 94578"
    "Castro Valley Station","3301 Norbridge Dr., Castro Valley, CA 94546"
    "Civic Center / UN Plaza Station","1150 Market Street, San Francisco, CA 94102"
    "Coliseum Station","7200 San Leandro St., Oakland, CA 94621"
    "Colma Station","365 D Street, Colma, CA 94014"
    "Concord Station","1451 Oakland Avenue, Concord, CA 94520"
    "Daly City Station","500 John Daly Blvd., Daly City, CA 94014"
    "Downtown Berkeley Station","2160 Shattuck Avenue, Berkeley, CA 94704"
    "Dublin / Pleasanton Station","5801 Owens Dr., Pleasanton, CA 94588"
    "El Cerrito del Norte Station","6400 Cutting Blvd., El Cerrito, CA 94530"
    "El Cerrito Plaza Station","6699 Fairmount Avenue, El Cerrito, CA 94530"
    "Embarcadero Station","298 Market Street, San Francisco, CA 94111"
    "Fremont Station","2000 BART Way, Fremont, CA 94536"
    "Fruitvale Station","3401 East 12th Street, Oakland, CA 94601"
    "Glen Park Station","2901 Diamond Street, San Francisco, CA 94131"
    "Hayward Station","699 'B' Street, Hayward, CA 94541"
    "Lafayette Station","3601 Deer Hill Road, Lafayette, CA 94549"
    "Lake Merritt Station","800 Madison Street, Oakland, CA 94607"
    "MacArthur Station","555 40th Street, Oakland, CA 94609"
    "Millbrae Station","200 North Rollins Road, Millbrae, CA 94030"
    "Montgomery St. Station","598 Market Street, San Francisco, CA 94104"
    "North Berkeley Station","1750 Sacramento Street, Berkeley, CA 94702"
    "North Concord / Martinez Station","3700 Port Chicago Highway, Concord, CA 94520"
    "Oakland International Airport Station","4 Airport Drive, Oakland, CA 94621"
    "Orinda Station","11 Camino Pablo, Orinda, CA 94563"
    "Pittsburg / Bay Point Station","1700 West Leland Road, Pittsburg, CA 94565"
    "Pittsburg Center Station","2099 Railroad Avenue, Pittsburg, CA 94565"
    "Pleasant Hill / Contra Costa Centre Station","1365 Treat Blvd., Walnut Creek, CA 94597"
    "Powell St. Station","899 Market Street, San Francisco, CA 94102"
    "Richmond Station","1700 Nevin Avenue, Richmond, CA 94801"
    "Rockridge Station","5660 College Avenue, Oakland, CA 94618"
    "San Bruno Station","1151 Huntington Avenue, San Bruno, CA 94066"
    "San Francisco International Airport Station","International Terminal, Level 3, San Francisco Int'l Airport, CA 94128"
    "San Leandro Station","1401 San Leandro Blvd., San Leandro, CA 94577"
    "South Hayward Station","28601 Dixon Street, Hayward, CA 94544"
    "South San Francisco Station","1333 Mission Road, South San Francisco, CA 94080"
    "Union City Station","10 Union Square, Union City, CA 94587"
    "Walnut Creek Station","200 Ygnacio Valley Road, Walnut Creek, CA 94596"
    "Warm Springs / South Fremont Station","45193 Warm Springs Blvd, Fremont, CA 94539"
    "West Dublin / Pleasanton Station","6501 Golden Gate Drive, Dublin, CA 94568"
    "West Oakland Station","1451 7th Street, Oakland, CA 94607"


### Import into Google Maps

Save the above result as a CSV file, then import into Google My Maps

The result is here: https://drive.google.com/open?id=1nvmxajzh09Da_vERTH3c49G98UCNr4Mv&usp=sharing

![Bart Stations](bart_stations.png)
