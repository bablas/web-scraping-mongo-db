

```python
# Declare Dependencies 
from bs4 import BeautifulSoup
from splinter import Browser
import pandas as pd
import requests
```


```python
browser = Browser('chrome')
```

# NASA Mars News


```python
# Splinter browser visit
url = 'https://mars.nasa.gov/news/'
browser.visit(url)
```


```python
# Beautiful Soup 
html = browser.html

soup = BeautifulSoup(html, 'html.parser')
news_title = soup.find('div', class_='content_title').find('a').text
news_p = soup.find('div', class_='article_teaser_body').text

print(news_title)
print(news_p)
```

    The Mast Is Raised for NASA's Mars 2020 Rover
    Engineers at JPL take a group selfie after attaching the remote sensing mast to the Mars 2020 rover.


# JPL Mars Space Images


```python
# Splinter Browser
image_url_featured = 'https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars'
browser.visit(image_url_featured)
```


```python
#Beautiful Soup
html_image = browser.html

soup = BeautifulSoup(html_image, 'html.parser')
featured_image_url  = soup.find('article')['style'].replace('background-image: url(','').replace(');', '')[1:-1]
main_url = 'https://www.jpl.nasa.gov'

#Website URL
featured_image_url = main_url + featured_image_url
print(featured_image_url) 
```

    https://www.jpl.nasa.gov/spaceimages/images/wallpaper/PIA18280-1920x1200.jpg


# Mars Weather 


```python
# Splinter Browser
weather_url = 'https://twitter.com/marswxreport?lang=en'
browser.visit(weather_url)
```


```python
#Beautiful Soup
html_weather = browser.html


soup = BeautifulSoup(html_weather, 'html.parser')
latest_tweets = soup.find_all('div', class_='js-tweet-text-container')

#Loop to find tweet
for tweet in latest_tweets: 
    weather_tweet = tweet.find('p').text
    if 'insight' and 'sol' in weather_tweet:
        print(weather_tweet)
        break
    else: 
        pass

```

    InSight sol 197 (2019-06-16) low -105.2ºC (-157.4ºF) high -23.3ºC (-9.9ºF)
    winds from the SSE at 4.1 m/s (9.2 mph) gusting to 15.9 m/s (35.6 mph)
    pressure at 7.60 hPapic.twitter.com/FslJFt8muB


# Mars Facts


```python
# Scrape URL
facts_url = 'http://space-facts.com/mars/'


mars_facts = pd.read_html(facts_url)
# print(mars_facts)


mars_df = mars_facts[0]
print(mars_df)


mars_df.columns = ['Description','Value']
mars_df.set_index('Description', inplace=True)
mars_df.to_html()

data = mars_df.to_dict(orient='records') 
mars_df
```

    [                      0                              1
    0  Equatorial Diameter:                       6,792 km
    1       Polar Diameter:                       6,752 km
    2                 Mass:  6.42 x 10^23 kg (10.7% Earth)
    3                Moons:            2 (Phobos & Deimos)
    4       Orbit Distance:       227,943,824 km (1.52 AU)
    5         Orbit Period:           687 days (1.9 years)
    6  Surface Temperature:                  -153 to 20 °C
    7         First Record:              2nd millennium BC
    8          Recorded By:           Egyptian astronomers]
                          0                              1
    0  Equatorial Diameter:                       6,792 km
    1       Polar Diameter:                       6,752 km
    2                 Mass:  6.42 x 10^23 kg (10.7% Earth)
    3                Moons:            2 (Phobos & Deimos)
    4       Orbit Distance:       227,943,824 km (1.52 AU)
    5         Orbit Period:           687 days (1.9 years)
    6  Surface Temperature:                  -153 to 20 °C
    7         First Record:              2nd millennium BC
    8          Recorded By:           Egyptian astronomers





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
      <th>Value</th>
    </tr>
    <tr>
      <th>Description</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Equatorial Diameter:</th>
      <td>6,792 km</td>
    </tr>
    <tr>
      <th>Polar Diameter:</th>
      <td>6,752 km</td>
    </tr>
    <tr>
      <th>Mass:</th>
      <td>6.42 x 10^23 kg (10.7% Earth)</td>
    </tr>
    <tr>
      <th>Moons:</th>
      <td>2 (Phobos &amp; Deimos)</td>
    </tr>
    <tr>
      <th>Orbit Distance:</th>
      <td>227,943,824 km (1.52 AU)</td>
    </tr>
    <tr>
      <th>Orbit Period:</th>
      <td>687 days (1.9 years)</td>
    </tr>
    <tr>
      <th>Surface Temperature:</th>
      <td>-153 to 20 °C</td>
    </tr>
    <tr>
      <th>First Record:</th>
      <td>2nd millennium BC</td>
    </tr>
    <tr>
      <th>Recorded By:</th>
      <td>Egyptian astronomers</td>
    </tr>
  </tbody>
</table>
</div>



# Mars Hemispheres


```python
# Splinter Browser
hemispheres_url = 'https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars'
browser.visit(hemispheres_url)
```


```python
#Beautiful Soup
html_hemispheres = browser.html

soup = BeautifulSoup(html_hemispheres, 'html.parser')

items = soup.find_all('div', class_='item')

hemisphere_image_urls = []


hemispheres_main_url = 'https://astrogeology.usgs.gov'


for i in items: 
    title = i.find('h3').text
    partial_img_url = i.find('a', class_='itemLink product-item')['href']
    browser.visit(hemispheres_main_url + partial_img_url)
    partial_img_html = browser.html
    soup = BeautifulSoup( partial_img_html, 'html.parser')
    img_url = hemispheres_main_url + soup.find('img', class_='wide-image')['src']
    hemisphere_image_urls.append({"title" : title, "img_url" : img_url})
    


hemisphere_image_urls
```




    [{'title': 'Cerberus Hemisphere Enhanced',
      'img_url': 'https://astrogeology.usgs.gov/cache/images/cfa62af2557222a02478f1fcd781d445_cerberus_enhanced.tif_full.jpg'},
     {'title': 'Schiaparelli Hemisphere Enhanced',
      'img_url': 'https://astrogeology.usgs.gov/cache/images/3cdd1cbf5e0813bba925c9030d13b62e_schiaparelli_enhanced.tif_full.jpg'},
     {'title': 'Syrtis Major Hemisphere Enhanced',
      'img_url': 'https://astrogeology.usgs.gov/cache/images/ae209b4e408bb6c3e67b6af38168cf28_syrtis_major_enhanced.tif_full.jpg'},
     {'title': 'Valles Marineris Hemisphere Enhanced',
      'img_url': 'https://astrogeology.usgs.gov/cache/images/7cf2da4bf549ed01c17f206327be4db7_valles_marineris_enhanced.tif_full.jpg'}]




```python

```
