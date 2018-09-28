---
layout: post
title:  "Webscraping with Go Revisited"
date:   2018-09-27 10:20:00 -0400
categories: Blog Post
---

# Motivation

In a previous post, located here, I detailed using Go to webscrape posts from Craigslist missed connections. The code used was heavily based off an article I found detailing a webscraper written in Go to scrape the posts from hackernews. I had updated the code to run against craigslist and had been happy with the results. I thought I could take it a step further and adapt the same code to cover scraping a local news website for my state. I believe I was also able to get the scraper working for that site as well. However, after trying to revisit the code and rerun it, it seems that my local news site has undergone a website redesign rendering my current code useless. I welcomed the challenge of trying to modify the code once again to work with the new website design but it proved much more challenging that I had thought.

# Introducing goQuery

I decided I needed to branch out and look for a different way of processing html other than using `yhat/scrape`. I stumbled upon gocolly, but it seemed to be more of a webcrawler instead of a webscraper. One of the libraries that gocolly uses is goQuery which is essentially a port of jquery to Go. GoQuery has decent documentation and an example that I was able to use as a base for my code.

# The Code

This is the example from the goQuery github:

```
package main

import (
  "fmt"
  "log"
  "net/http"

  "github.com/PuerkitoBio/goquery"
)

func ExampleScrape() {
  // Request the HTML page.
  res, err := http.Get("http://metalsucks.net")
  if err != nil {
    log.Fatal(err)
  }
  defer res.Body.Close()
  if res.StatusCode != 200 {
    log.Fatalf("status code error: %d %s", res.StatusCode, res.Status)
  }

  // Load the HTML document
  doc, err := goquery.NewDocumentFromReader(res.Body)
  if err != nil {
    log.Fatal(err)
  }

  // Find the review items
  doc.Find(".sidebar-reviews article .content-block").Each(func(i int, s *goquery.Selection) {
    // For each item found, get the band and title
    band := s.Find("a").Text()
    title := s.Find("i").Text()
    fmt.Printf("Review %d: %s - %s\n", i, band, title)
  })
}

func main() {
  ExampleScrape()
}
```

Using the outlined code above I was able to create the following to scrape the crime news from my local news website.
```
package main

import (
	"fmt"
	"log"
	"net/http"

	"github.com/PuerkitoBio/goquery"
)

func NewsScrape() {
	// Request the HTML page.
	res, err := http.Get("https://www.delawareonline.com/news/crime")
	if err != nil {
		log.Fatal(err)
	}
	defer res.Body.Close()
	if res.StatusCode != 200 {
		log.Fatalf("status code error: %d %s", res.StatusCode, res.Status)
	}

	// Load the HTML document
	doc, err := goquery.NewDocumentFromReader(res.Body)
	if err != nil {
		log.Fatal(err)
	}

	theTitles := make([]string, 0)
	theLinks := make([]string, 0)

	// Find the article titles
	doc.Find(".flm-bundle a .flm-hed").Each(func(i int, s *goquery.Selection) {
		title := s.Find("span").Text()
		theTitles = append(theTitles, title)
		// fmt.Printf("Article %d: - %s\n", i, title)
	})
	// Find the article links or hrefs
	doc.Find("a.flm-asset-link").Each(func(i int, s *goquery.Selection) {
		urlToSite, _ := s.Attr("href")
		theLinks = append(theLinks, urlToSite)
		// fmt.Printf("Link %d: - %s\n", i, urlToSite)
	})
	var baseURL = "https://www.delawareonline.com"

	for i := range theLinks {
		fmt.Printf("%2d %s (%s)\n", i, theTitles[i], baseURL+theLinks[i])
	}
}

func main() {
	NewsScrape()
}
```

# Going forward

There were only minor changes needed to modify the example to meet my needs. This exercise served as a way to learn and become more comfortable with using another language other than Python. I plan on using Go for future exercises due to its speed as well as its simplicity.

