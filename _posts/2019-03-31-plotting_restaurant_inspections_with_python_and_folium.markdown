---
layout: post
title:  "Plotting restuarants inspections with Python and Folium (Part One)"
date:   2019-04-01 10:20:00 -0400
categories: Blog Post
---

# Motivation

I've been searching for a project to really test my knowledge of programming and offer me some experience
with understanding, analyzing, and displaying data. I wanted to find a project that would offer me those goals as well
as be something that I found interesting and kept my attention. I love the idea of somehow incorporating local data into
what I am working on and after a few quick Google searches, I came across the website for Delaware Open Data. In fact, most states now keep large and open collections of data from state agencies and other offices. Depending on your state, their website might have this data displayed and laid out in an organized and easy to access fashion. I decided to see what information I could use from my states' (Delaware) website to use for a potential project. I came across the dataset that contained a record of Restaurant Inspection reports and decided I would use that information to create a visual map of the various restaurants located in Delaware and display inspections data about them.

This is documentation of my steps taken, which will hopefully turn into a complete project in the end. There will be a few parts to this post. I will be posting sections once I feel that they are adequately completed and ready to be published.


# Step 1 - Get the Data

In order to display data on a map, I need data. The inspections data is stored on a website in a giant html table. This does not make retrieving the data ideal, however, it also does not make it impossible. I threw together a quick script with `requests` and `BeautifulSoup` and in a few minutes had access to all of the data in the table and wrote that data into a text file.

```
import requests
from bs4 import BeautifulSoup

# the url that stores the information we want, sorted to display all information
url = 'https://dhss.delaware.gov/dhss/dph/hsp/Default.aspx?listAll=1&sort=Establishment'

# get the page, if we are successful give us the page contents
# print it once to make sure everything looks good, then we can comment out or remove
# I left it commented for the sake of clarity.

response = requests.get(url)
if response.status_code == 200:
    page_content = response.text
# print(page_content)

soup = BeautifulSoup(page_content, 'html.parser')

table = soup.find('table')

list_of_rows = []
for row in table.findAll('tr'):
    list_of_cells = []
    for cell in row.findAll(['td']):
        text = cell.text
        text = text.strip()
        list_of_cells.append(str(text + ';'))

    list_of_rows.append(list_of_cells)

for item in list_of_rows:
    with open('inspections.txt', 'a') as f:
        f.write(str(item))
        f.write('\n')

```

The data contains the following information:

```Establishments,	Address,	City,	Zip,	County,	Inspection Type,	Inspection Date,	Violations/COS * ```


So now that we have all of that information, we need a way to display it.


# Step 2 - Displaying the Data

This next part took me way longer than I am willing to admit. At first, the obvious way to plot the data was of course Google Maps. However, it seems that Google Maps has changed up its pricing for use of its maps api. I believe, and of course I could be wrong, you need to enter some form of credit card information in order to use their service. I believe you are given a $300 free credit per month so unless your project is huge you *shouldn't* ever been charged. However, I do not like the idea of having my credit card attached to something like that so I looked elsewhere for a different service.

I had already heard of OpenStreetMaps before so that is where I went next. I skimmed through the documentation and tried to get started creating a map to plot points on. There was only one major draw back for me. Well, actually two; HTML and JS. While I am familiar with both, I am nowhere near the level I need to be to write both. This put me off of this project for quite sometime, until I was randomly browsing one day and came across Folium.

Folium seemed to be everything I needed. No writing HTML files or writing JS, just using pure python to create what I wanted. Sign me up.

Of course things are never that easy so I ran into my next roadbock. I had the restaurant's address, city, zip, couty, etc... but in order to plot points on the map, I needed to have that same data but in coordinate form. The solution in practice, sounds easy, but of course is not. I needed to write something to take my addresses for the restaurants and convert them into lat and long points so that I could map them. I'm not sure if my Google skills are severely lacking or what but it really took a long time to come up with a solution that I could use. I ended up using MapQuest Developer since they had a few option for their Geocoding API that I could use to convert an address into coordinates.


I created a modified text file of all of the restaurant addresses in the following format: ```  Restaurant_Name, Address,City, ZIP```

I would pass in each line of the file into a call to the Geocoding API that would give me exactly the latitude and longitude I was looking for, right? Wrong. The call to the API returns quite a lot of data. Way more data than I was interested in. It took a couple minutes of trial and error guessing to be able to parse out the latitude and longitude from the request. I added all of the tuples of lats and longs to yet another text file.


```
addresses = set()
with open('inspections.txt', 'r') as f:
    for line in f:
        elements = line.split(',')
        for element in elements:
            element = element.replace(';', '')

        addresses.add(
            ''.join(elements[0] + elements[1] + elements[2] + elements[3]))

# print(addresses)


def build_request(location):
# key removed - use your own
    geocode_response = requests.get(
        'http://www.mapquestapi.com/geocoding/v1/address?key=KEY&location={}'.
        format(location))
    if geocode_response.status_code == 200:
        response_data = geocode_response.text
    else:
        print("There was an error")
    return response_data


def parse(data):
    parsed_data = json.loads(data)
    return parsed_data


def lat_long(parsed_data):
    lat = parsed_data['results'][0]['locations'][0]['latLng']['lat']
    lng = parsed_data['results'][0]['locations'][0]['latLng']['lng']
    return lat, lng


latlongs = set()
with open('coords.txt', 'a') as coords_file:
    for address in addresses:
        new_address = address.replace("'", "").replace(';', '').replace(
            '[', '')
        data_latlong = lat_long(parse(build_request(new_address)))
        # latlongs.add(data_latlong)
        coords_file.write(str(data_latlong))
```


With this completed I am ready to plot the restaurant locations onto a map. In the next part of this post I will continue on with this work.