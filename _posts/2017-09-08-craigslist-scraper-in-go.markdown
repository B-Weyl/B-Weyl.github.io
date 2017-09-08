---
layout: post
title:  "Web Scraping with Go"
date:   2017-09-08 00:41:44 -0400
categories: Blog Post
---

Since I feel that I have grown comfortable with Python, I am trying to broaden my knowledge base of langauges. I figure a good way to do so is to attempt some of the things I have done in Python in other languages. I have decided to start off with trying to learn some Go.

One of the projects I had created with Python was a craigslist scraper, that grabbed all of the posts on a craigslist form (in my case, missed connections because I think that some of them are strangely hilarious) from the command line. I decided to give this a shot using Go since I had used a script in Go that someone had made to scrape [Y News Combinator](https://news.ycombinator.com/news) front page. I figure I can at least use this as a skeleton to get my scraper of craigslist to start working. To my surprise it took only a few minutes for me to get it up and working.

Go is structured quite differently than python. You need to include all of the packages that you will be using in an import statement sat the top of the program like so:

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/yhat/scrape"
	"golang.org/x/net/html"
	"golang.org/x/net/html/atom"
)
```

`fmt` is the standard go package that implements formatted I/O similar to that of C's printf and scanf. `Net/Http` provides client server implementation in Go. `github.com/yhat/scrape` is a highlevel interface for go web scraping written by glamp on github, and the last two packages implement html5 tokinization and parsing in Go.

Now that we have that covered we can move onto the scraping. We can make an HTTP request using :
```go
func main() {
	resp, err := http.Get("https://delaware.craigslist.org/search/mis")
	if err != nil {
		panic(err)
	}
	root, err := html.Parse(resp.Body)
	if err != nil {
		panic(err)
	}
```

Now that we have the request, we can look into it for a specific node. In our case the node we want is result-info. We can get that information by using a matcher function which will return true if the node is found in the http request:

```go
matcher := func(n *html.Node) bool {
		if n.DataAtom == atom.A && n.Parent != nil {
			return scrape.Attr(n.Parent, "class") == "result-info"
		}
		return false
	}
```

We can now go through all of the nodes (articles) that match by going through all of the articles and printing out a numbered list of the headline and its url:

```go
articles := scrape.FindAll(root, matcher)
	for i, article := range articles {
		fmt.Printf("%2d %s (%s)\n", i, scrape.Text(article), scrape.Attr(article, "href"))
	}
```

And just like that, we are done. Let's take a look at it's output:

```
➜  Examples go run clist.go
 0 COLLEEN AT DOVER LOWES IS AWESOME - m4w (/mis/d/colleen-at-dover-lowes-is/6295022337.html)
 1 Elkton Super Walmart Late Tuesday Night, Sept 5th - m4w (/mis/d/elkton-super-walmart-late/6297528100.html)
 2 North Shore - m4m (/mis/d/north-shore-m4m/6297429766.html)
 3 Planet Fitness - w4m (/mis/d/planet-fitness-w4m/6297325100.html)
 4 red speedo like suit on poodle beach on thursday afternoon - m4m (/mis/d/red-speedo-like-suit-on/6297199417.html)
 5 Club fitness Thursday evening 8-24-17 - m4m (/mis/d/club-fitness-thursday/6279025302.html)
 6 Omar, where you at - m4m (/mis/d/omar-where-you-at-m4m/6272203023.html)
 7 Ymca Dover - m4m (/mis/d/ymca-dover-m4m/6274379536.html)
 8 You can't let go - m4w (/mis/d/you-cant-let-go-m4w/6295926877.html)
 9 Doghouse customer on Rt 13 - m4w (/mis/d/doghouse-customer-on-rt-13-m4w/6289970160.html)
10 DISPLACED FAMILY IN NEED OF FOOD - w4w (/mis/d/displaced-family-in-need-of/6290869441.html)
```

