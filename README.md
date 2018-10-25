
# API Exercises

Here are two exercises that involve working with APIs and dictionaries.

One is using the Open Brewery API found at https://www.openbrewerydb.org/, and the other is using the API for UK Police Data, found at https://data.police.uk/docs/.

You can complete them in either order!

Remember that you can create new cells with esc + a or b

## Breweries

### Q1: Load the first page of results with 50 results per page


```python
url = 'https://api.openbrewerydb.org/breweries?page=1&per_page=50'

```

### Q2: This is only the first 50 results.  Get the next 50 and put them together.


```python


```

### Q3: How many of these 100 breweries in are in Alaska?


```python


```

### Q4: Of these 100 breweries, what are the different unique brewery types?


```python


```

### Q5: What is the cloest brewery to "Devil's Potion Brewing Company LLC" ?
* Hint 1: Use Euclidian distance w/ longitude and latitude (assume longitude and latitude are a Carteisan coordinate system)
* Hint 2: You'll have to ignore the entries with "none" for latitude or longitude


```python


```

### Q6: Write a function to find the closest brewery to any other given brewery


```python


```

### Q7: How would you get the first 10 pages from this API and put them all together using a for loop?


```python


```

# Crime in the UK

### We will be analyzing different crimes reported in the UK as provided by https://data.police.uk/docs/

# Exploratory analysis
##### 1. How many total crimes were there at latitude : 52.63902 and -1.131321 on November of 2017.
Use the street level crimes data, the documentation for the API can be found at https://data.police.uk/docs/method/crime-street/


```python


```

##### 2. We've queried the API once, but it could get annoying to retype the url over and over again, create a function `make_api_request` that enables you to query the API.


 The parameters for the function should be:
* lat (float) : latitude
* lng (float) : longitude
* date (string): Date in the format YYYY-MM
    * default value = `None`
    
And it should return a json object of 

for more information on default values check out http://blog.thedigitalcatonline.com/blog/2015/02/11/default-arguments-in-python/


```python


```

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


```python


```

**Bonus**: 
* Write a function that determines the difference between Buckingham Palace and Edinburgh Castle in terms of the number of crimes in each category.
    * In which category is there the largest absolute difference between the category of crime?
* Create a histogram depiction of the categories of crime


```python


```

##### 4. Create a function `find_outcome_statuses` that will determine outcome statuses for a given latitude and longitude and date (optional)?
Investigate the data to determine where the outcome statuses are located.

**NOTE**: You'll notice that some of these crimes do not have crime outcomes. Make these into the category of "Not Resolved."

**NOTE 2**: These might take a long time to execute if you do not specify a month

**Bonus**: What is the ratio of crimes investigated to those not investigated? Is it higher near London or Manchester?


```python


```

##### 5. Write a function `month_highest_crimes` that will return the month that had the highest number of crimes for a latitude, longitude and a year.

Inputs
* lat (float) : latitude
* lng (float) : longitude
* year (str) : in the format YYYY

Output
* month with highest crime (int)

**Bonus** Make a graph of how the number of crimes changed over time for a year. This will likely require a new function. Is seasonality a factor? Do the type of crimes change over time?


```python


```

### Bonus Open Ended Questions

1. Take a look at the https://data.police.uk/docs/method/stops-street/ API. Is there a correlation between gender and being stopped and searched? How about race and being stopped and searched?


```python


```
