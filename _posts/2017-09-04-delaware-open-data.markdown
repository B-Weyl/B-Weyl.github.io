---
layout: post
title:  "Delaware Open Data"
date:   2017-09-04 00:41:44 -0400
categories: Blog Post
---

# Births in Delaware from 2009 to 2014

Using the following [dataset](https://dev.socrata.com/foundry/data.delaware.gov/y8fa-dqxh) we can analyze the births in Delaware from 2009 to 2014. I will be using the requests library to do so.

    import requests
    import matplotlib.pyplot as plt
    url = 'https://data.delaware.gov/resource/y8fa-dqxh.json?$limit=100000'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()

We can now use our response object to analyze the data. Delaware is a small state, let's find out how many births, of the total 72456, were in each county. Delaware has three counties: New Castle, Kent, Sussex.


We see the totals for each county.


    kent, sussex, ncc = 0, 0, 0
    for item in data:
        if item['county_of_residence'] == 'Sussex':
            sussex += 1
        elif item['county_of_residence'] == 'New Castle':
            ncc += 1
        else:
            kent += 1
    print(ncc, kent, sussex)

New Castle | Kent | Sussex
---: | ---: | ---:
39,827 | 19,154 | 13,475



We can plot that data to show the following:


    labels = 'NCC', 'Kent', 'Sussex'
    sizes = [ncc, kent, sussex]
    fig1, ax1 = plt.subplots()
    ax1.pie(sizes, labels=labels, autopct='%1.1f%%', shadow=False, startangle=90)
    ax1.axis('equal')
    plt.show()

![chart1](http://i.imgur.com/5qki6p6.png)


We can also use the same method as above to take a look at the age of the mothers at the time they gave birth.Unforuntaely our dataset is limited to three different age ranges but we will make due with what we have. Again we can iterate through each of the items in the dataset and count each occurence in each range:

    twenty, thirtyfive, less_than_twenty = 0, 0, 0
    for item in data:
        if item['mom_s_age'] == '20-34':
            twenty += 1
        elif item['mom_s_age'] == '35+':
            thirtyfive += 1
        else:
            less_than_twenty += 1
    print(less_than_twenty, twenty, thirtyfive)

Under Twenty | Twenty to Thirty-four | Thirty-Five Plus
---: | ---: | ---:
5,271 | 56,402 | 10,783


Doing so we see that there were 5,271 births from mothers aged less than 20, 56,402 from mothers aged 20-34, and 10,783 aged 35+. Now let's filter the data by county and age. We can do that for each county using the following:
## Sussex
```python
    sussex_twenty, sussex_thirtyfive, sussex_less_than_twenty = 0, 0, 0
    for item in data:
        if item['county_of_residence'] == 'Sussex':
            if item['mom_s_age'] == '20-34':
                sussex_twenty += 1
            elif item['mom_s_age'] == '35+':
                sussex_thirtyfive += 1
            else:
                sussex_less_than_twenty += 1
    print(sussex_less_than_twenty, sussex_twenty, sussex_thirtyfive)
```

Under Twenty | Twenty to Thirty-four | Thirty-Five Plus
---: | ---: | ---:
1,268 | 10,603 | 1,604


## Kent
```python
    kent_twenty, kent_thirtyfive, kent_less_than_twenty = 0, 0, 0
    for item in data:
        if item['county_of_residence'] == 'Kent':
            if item['mom_s_age'] == '20-34':
                kent_twenty += 1
            elif item['mom_s_age'] == '35+':
                kent_thirtyfive += 1
            else:
                kent_less_than_twenty += 1
    print(kent_less_than_twenty, kent_twenty, kent_thirtyfive)
```

Under Twenty | Twenty to Thirty-four | Thirty-Five Plus
---: | ---: | ---:
1,015 | 10,869 | 1,420


## New Castle
```python
    ncc_twenty, ncc_thirtyfive, ncc_less_than_twenty = 0, 0, 0
    for item in data:
        if item['county_of_residence'] == 'New Castle':
            if item['mom_s_age'] == '20-34':
                ncc_twenty += 1
            elif item['mom_s_age'] == '35+':
                ncc_thirtyfive += 1
            else:
                ncc_less_than_twenty += 1
    print(ncc_less_than_twenty, ncc_twenty, ncc_thirtyfive)
```

Under Twenty | Twenty to Thirty-four | Thirty-Five Plus
---: | ---: | ---:
2,814 | 30,579 | 6,434
