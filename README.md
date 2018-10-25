
# API Exercises

Here are two exercises that involve working with APIs and dictionaries.

One is using the Open Brewery API found at https://www.openbrewerydb.org/, and the other is using the API for UK Police Data, found at https://data.police.uk/docs/.

You can complete them in either order!

Remember that you can create new cells with esc + a or b

## Breweries

### Q1: Load the first page of results with 50 results per page


```python
import requests
url = 'https://api.openbrewerydb.org/breweries?page=1&per_page=50'
r = requests.get(url)
data = r.json()
data[:3]
```




    [{'id': 2,
      'name': 'Avondale Brewing Co',
      'brewery_type': 'micro',
      'street': '201 41st St S',
      'city': 'Birmingham',
      'state': 'Alabama',
      'postal_code': '35222-1932',
      'country': 'United States',
      'longitude': '-86.774322',
      'latitude': '33.524521',
      'phone': '2057775456',
      'website_url': 'http://www.avondalebrewing.com',
      'updated_at': '2018-08-23T23:19:57.825Z'},
     {'id': 4,
      'name': 'Band of Brothers Brewing Company',
      'brewery_type': 'micro',
      'street': '1605 23rd Ave',
      'city': 'Tuscaloosa',
      'state': 'Alabama',
      'postal_code': '35401-4653',
      'country': 'United States',
      'longitude': '-87.5621551272424',
      'latitude': '33.1984907123707',
      'phone': '2052665137',
      'website_url': 'http://www.bandofbrosbrewing.com',
      'updated_at': '2018-08-23T23:19:59.462Z'},
     {'id': 44,
      'name': 'Trim Tab Brewing',
      'brewery_type': 'micro',
      'street': '2721 5th Ave S',
      'city': 'Birmingham',
      'state': 'Alabama',
      'postal_code': '35233-3401',
      'country': 'United States',
      'longitude': '-86.7914000624146',
      'latitude': '33.5128492349817',
      'phone': '2057030536',
      'website_url': 'http://www.trimtabbrewing.com',
      'updated_at': '2018-08-23T23:20:31.423Z'}]



### Q2: This is only the first 50 results.  Get the next 50 and put them together.


```python
url2 = 'https://api.openbrewerydb.org/breweries?page=2&per_page=50'
r = requests.get(url2)
data2 = r.json()
full_data = data + data2
len(full_data)
```




    100



### Q3: How many of these 100 breweries in are in Alaska?


```python
c = 0
for x in full_data:
    if x['state'] == 'Alaska':
        c += 1
print( c)
```

    3


### Q4: Of these 100 breweries, what are the different unique brewery types?


```python
set([x['brewery_type'] for x in full_data])

```




    {'brewpub', 'contract', 'micro', 'planning', 'proprietor', 'regional'}



### Q5: What is the cloest brewery to "Devil's Potion Brewing Company LLC" ?
* Hint 1: Use Euclidian distance w/ longitude and latitude (assume longitude and latitude are a Carteisan coordinate system)
* Hint 2: You'll have to ignore the entries with "none" for latitude or longitude


```python
for x in full_data:
    if x['name'] == "Devil's Potion Brewing Company LLC":
        print(x)
```

    {'id': 525, 'name': "Devil's Potion Brewing Company LLC", 'brewery_type': 'planning', 'street': '', 'city': 'Escondido', 'state': 'California', 'postal_code': '92026-3187', 'country': 'United States', 'longitude': '-117.0814849', 'latitude': '33.1216751', 'phone': '7605329091', 'website_url': 'http://www.devilspotion.com', 'updated_at': '2018-08-23T23:27:21.742Z'}



```python

lat1 = 33.1216751
long1 = -117.0814849

d = {}
for x in full_data:
    try:
        distance = ((long1 - float(x['longitude'])) **2 + (lat1 - float(x['latitude']))**2)**0.5
    except:
        distance = 0
    if distance > 0:
        d[x['name']] = distance
```


```python
min(d, key=d.get)
```




    'Port Brewing Co / The Lost Abbey'




```python
d['Port Brewing Co / The Lost Abbey']
```




    0.07051129653338688



### Q6: Write a function to find the closest brewery to any other given brewery


```python
def brew_distance(x):
    for i in full_data:
        if i['name'] == x:
            lat1 = i['latitude']
            long1 = i['longitude']
    if lat1:
        pass
    else:
        print('error')
        return None
    d = {}
    for z in full_data:
        try:
            distance = ((float(long1) - float(z['longitude'])) **2 + (float(lat1) - float(z['latitude']))**2)**0.5
        except:
            distance = 0
        if distance > 0:
            d[z['name']] = distance
    print(min(d, key=d.get))

```


```python
brew_distance('Copper Hop Brewing Co/Copper Hop Ranch')
```

    error



```python
brew_distance('Jackrabbit Brewing Co')
```

    Urban Roots Brewing


### Q7: How would you get the first 10 pages from this API and put them all together using a for loop?


```python
data = []
for i in range(1,11):
    url = 'https://api.openbrewerydb.org/breweries?page=' + str(i) + 'per_page=50'
    r = requests.get(url)
    data += r.json()
len(data)
```




    200



# Crime in the UK

### We will be analyzing different crimes reported in the UK as provided by https://data.police.uk/docs/

# Exploratory analysis
##### 1. How many total crimes were there at latitude : 52.63902 and -1.131321 on November of 2017.
Use the street level crimes data, the documentation for the API can be found at https://data.police.uk/docs/method/crime-street/


```python
import json
import requests
response = requests.get('https://data.police.uk/api/crimes-street/all-crime?lat=52.63902&lng=-1.131321&date=2017-11')
all_crimes = response.json()
len(all_crimes)

```




    1334



##### 2. We've queried the API once, but it could get annoying to retype the url over and over again, create a function `make_api_request` that enables you to query the API.


 The parameters for the function should be:
* lat (float) : latitude
* lng (float) : longitude
* date (string): Date in the format YYYY-MM
    * default value = `None`
    
And it should return a json object of 

for more information on default values check out http://blog.thedigitalcatonline.com/blog/2015/02/11/default-arguments-in-python/


```python
def make_api_request(lat,lng,date=None):
    if date:
        url = 'https://data.police.uk/api/crimes-street/all-crime?lat=' + str(lat) + '&lng=' + str(lng) + '&date=' + date 
    else :
        url = 'https://data.police.uk/api/crimes-street/all-crime?lat=' + str(lat) + '&lng=' + str(lng)
    response = requests.get(url)
    return response.json()
    
# response = requests.get('https://data.police.uk/api/crimes-street/all-crime?lat=52.629729&lng=-1.131592&date=2017-01')
crimes = make_api_request(52.63902,-1.131321,'2017-11')

```


```python
crimes[:3]
```




    [{'category': 'anti-social-behaviour',
      'location_type': 'Force',
      'location': {'latitude': '52.635184',
       'street': {'id': 883410, 'name': 'On or near Shopping Area'},
       'longitude': '-1.135455'},
      'context': '',
      'outcome_status': None,
      'persistent_id': '',
      'id': 61222401,
      'location_subtype': '',
      'month': '2017-11'},
     {'category': 'anti-social-behaviour',
      'location_type': 'Force',
      'location': {'latitude': '52.637674',
       'street': {'id': 883469, 'name': 'On or near Tudor Road'},
       'longitude': '-1.147819'},
      'context': '',
      'outcome_status': None,
      'persistent_id': '',
      'id': 61223188,
      'location_subtype': '',
      'month': '2017-11'},
     {'category': 'anti-social-behaviour',
      'location_type': 'Force',
      'location': {'latitude': '52.629501',
       'street': {'id': 883363, 'name': 'On or near Grange Lane'},
       'longitude': '-1.136424'},
      'context': '',
      'outcome_status': None,
      'persistent_id': '',
      'id': 61222667,
      'location_subtype': '',
      'month': '2017-11'}]



##### 3. Write a function `categories_of_crime` that will determine the count of each type of crime for a given latitude and longitude. This is labelled as 'category' in the records. Your function should call the `make_api_request` function you created.

The parameters for the function should be:

* lat (float) : latitude
* lng (float) : longitude
* date (str) default = None

The function should return:
* a dictionary with the count of each type of crime



Once you've created the function, try it with these locations
* lat, lng of 51.5017861,-0.1432319   (Buckingham Palace)
* lat, lng of 53.480161, -2.245163     (Manchester)

**Bonus**: 
* Write a function that determines the difference between Buckingham Palace and Edinburgh Castle in terms of the number of crimes in each category.
    * In which category is there the largest absolute difference between the category of crime?
* Create a histogram depiction of the categories of crime


```python
buckingham = (51.5017861,-0.1432319)
edinburgh = (55.948811,-3.197982)
manchester = (53.480161,-2.245163)
```


```python
def categories_of_crime(lat,lng,date = None):
    crimes = make_api_request(lat,lng,date)
    category_dict = {}
    for crime in crimes:
        if crime['category'] in category_dict:
            category_dict[crime['category']] +=1
        else:
            category_dict[crime['category']] = 1
    return category_dict
```


```python
buck_categories = categories_of_crime(buckingham[0],buckingham[1])
print('Buckingham Palace crime categories:',buck_categories)
```

    Buckingham Palace crime categories: {'anti-social-behaviour': 608, 'bicycle-theft': 65, 'burglary': 138, 'criminal-damage-arson': 82, 'drugs': 74, 'other-theft': 810, 'possession-of-weapons': 26, 'public-order': 174, 'robbery': 141, 'shoplifting': 329, 'theft-from-the-person': 586, 'vehicle-crime': 102, 'violent-crime': 553, 'other-crime': 22}



```python
edin_categories = categories_of_crime(edinburgh[0],edinburgh[1])
print('Edinburgh Castle crime categories',edin_categories)
```

    Edinburgh Castle crime categories {'criminal-damage-arson': 1, 'drugs': 1, 'other-theft': 9, 'possession-of-weapons': 1, 'public-order': 1, 'shoplifting': 4, 'violent-crime': 7, 'other-crime': 2}



```python
manch_categories = categories_of_crime(manchester[0],manchester[1])
print(manch_categories)
```

    {'anti-social-behaviour': 422, 'bicycle-theft': 86, 'burglary': 95, 'criminal-damage-arson': 132, 'drugs': 42, 'other-theft': 357, 'possession-of-weapons': 25, 'public-order': 289, 'robbery': 134, 'shoplifting': 159, 'theft-from-the-person': 321, 'vehicle-crime': 284, 'violent-crime': 607, 'other-crime': 25}


##### 4. Create a function `find_outcome_statuses` that will determine outcome statuses for a given latitude and longitude and date (optional)?
Investigate the data to determine where the outcome statuses are located.

**NOTE**: You'll notice that some of these crimes do not have crime outcomes. Make these into the category of "Not Resolved."

**NOTE 2**: These might take a long time to execute if you do not specify a month

**Bonus**: What is the ratio of crimes investigated to those not investigated? Is it higher near London or Manchester?


```python
from collections import Counter

def find_outcome_statuses(lat,lng,date=None):
    outcome_status = []
    crimes = make_api_request(lat,lng,date)
    for crime in crimes:
        if crime['outcome_status']:
            outcome_status.append(crime['outcome_status']['category'])
        else:
            outcome_status.append('Not Resolved')
    d = Counter(outcome_status)    
    return d

```


```python
buck_outcomes = find_outcome_statuses(buckingham[0],buckingham[1])
print(buck_outcomes)
```

    Counter({'Under investigation': 2730, 'Not Resolved': 608, 'Investigation complete; no suspect identified': 201, 'Awaiting court outcome': 85, 'Local resolution': 66, 'Offender given a caution': 15, 'Offender given penalty notice': 3, 'Offender given a drugs possession warning': 2})



```python
manch_outcomes = find_outcome_statuses(manchester[0],manchester[1],'2017-10')
print(manch_outcomes)
```

    Counter({'Investigation complete; no suspect identified': 1834, 'Not Resolved': 445, 'Unable to prosecute suspect': 376, 'Status update unavailable': 266, 'Further investigation is not in the public interest': 92, 'Court result unavailable': 72, 'Local resolution': 44, 'Awaiting court outcome': 19, 'Offender sent to prison': 17, 'Offender given community sentence': 14, 'Formal action is not in the public interest': 13, 'Offender given a drugs possession warning': 11, 'Offender given a caution': 9, 'Offender given suspended prison sentence': 8, 'Offender fined': 4, 'Offender given penalty notice': 2, 'Defendant sent to Crown Court': 2, 'Defendant found not guilty': 2, 'Court case unable to proceed': 2, 'Offender given conditional discharge': 1, 'Offender given absolute discharge': 1})


##### 5. Write a function `month_highest_crimes` that will return the month that had the highest number of crimes for a latitude, longitude and a year.

Inputs
* lat (float) : latitude
* lng (float) : longitude
* year (str) : in the format YYYY

Output
* month with highest crime (int)

**Bonus** Make a graph of how the number of crimes changed over time for a year. This will likely require a new function. Is seasonality a factor? Do the type of crimes change over time?


```python
def month_highest_crimes(lat,lng,year):
    month_crimes = {}
    for month in range(1,13):
        month_crimes[str(month)] = len(make_api_request(lat,lng,year+'-' +str(month)))
    return max(month_crimes, key=month_crimes.get)

```


```python
manch_highest_month = month_highest_crimes(manchester[0],manchester[1],'2016')
manch_highest_month
```




    '12'



### Bonus Open Ended Questions

1. Take a look at the https://data.police.uk/docs/method/stops-street/ API. Is there a correlation between gender and being stopped and searched? How about race and being stopped and searched?


```python


```
