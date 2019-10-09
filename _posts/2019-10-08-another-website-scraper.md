---
layout: post
title:  "Yet Another News Website Scraper Using Requests and BeautifulSoup"
date:   2019-10-08 11:05:00 -0400
categories: Blog Post
---

# Another Local New Headline Web Scraper

Keeping with the same theme of the last [post](https://b-weyl.github.io/blog/post/2017/09/04/news-scraper.html) I did on this topic, I value being able to run a simple script from my terminal versus navigating a web browser to consume my local news. Again for this script I will be using Requests and BeautifulSoup. This website was much easier to get a scraper up and running for. I do not know if I should attribute that to an increase in skill on my part or a more simplistic website design.


Here is the full code I run:

```
import requests
from bs4 import BeautifulSoup

URL = 'https://townsquaredelaware.com/'


def get_content(url):
    response = requests.get(url)
    if response.status_code == 200:
        page_content = response.text
    return page_content


def parse_headlines(content):
    soup = BeautifulSoup(content, 'html.parser')
    headline_titles, headline_urls = [], []
    headlines = []
    for link in soup.find_all('a',
                              {'class': 'vce-featured-header-background'}):
        headline_urls.append(link.get('href'))
    for link in soup.find_all('a', {'class': 'vce-featured-link-article'}):
        headline_titles.append(link.get('title'))

    for headline in headlines:
        if headline not in headline_titles:
            headline_titles.append(headline)
    return zip(headline_titles, headline_urls)


def parse_trending(content):
    soup = BeautifulSoup(content, 'html.parser')
    trending_titles, trending_urls = [], []
    trending = soup.find_all('a', {'class': 'wpp-post-title'})
    for trend in trending:
        if trend not in trending_titles:
            trending_urls.append(trend.get('href'))
    for trend in trending:
        if trend not in trending_titles:
            trending_titles.append(trend.get('title'))
    return zip(trending_titles, trending_urls)


def display_headline_data(data):
    print("Displaying headlines from Town Square Delaware..." + '\n')
    for headline, url in data:
        print(f"{headline}" + '\n' + f"{url}")


def display_trending_data(data):
    print('\n')
    print("Displaying trending stories from Town Square Delaware..." + '\n')
    for headline, url in data:
        print(f"{headline}" + '\n' + f"{url}")


def main():
    site = get_content(URL)
    headline_data = parse_headlines(site)
    trending_data = parse_trending(site)
    display_headline_data(headline_data)
    display_trending_data(trending_data)


if __name__ == "__main__":
    main()

```

I also went ahead and added aliases to my `.zshrc` to be able to run these scripts more often and easier. I use ZSH and iTerm on my Macbook Pro so in order to accomplish that I added the following lines to my `.zshrc` file:

```
alias do="python3 Path/To/Program/program.py"
alias tsd="python3 Path/To/Program/program.py"
```


While I was at it, I also added an alias for the previous webscraper I wrote, so now when I want to run either I can simply just run `do` or `tsd` from anywhere in my terminal since I have provided the full path to the program. Be sure to restart your terminal so that changes can be picked up and the aliases can be set. If you do not use ZSH, the process for doing this should be the same with the exception of adding aliases to your `.bash_profile` instead.
