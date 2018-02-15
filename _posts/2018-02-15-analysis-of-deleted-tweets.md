---
layout: post
title:  "Analysis of Twtter's 200k Deleted Tweets"
date:   2018-02-15 14:20:00 -0400
categories: Blog Post
---

# Deleted Russian Troll Tweets

NBC posted over 200,000 tweets that Twitter deleted that belonged to Russian Troll accounts. The tweets were made available in a .CSV format with data such as the username, hashtags, tweet text, and mentions. Along with the tweets, they also posted a .CSV that contained accounts that were suspended from Twitter for being a Russian Troll account. This post will go over my analysis and filtering of that data and my findings along the way. All of the code that I used to create graphs and filter the data are available in the [github repo](https://github.com/B-Weyl/Analysis-of-Deleted-Tweets/blob/master/tweets.py).

## Hashtags in the Dataset

In total, there were 12,936 unique hashtags contained in the dataset provided. The most popular hashtags, after removing a few extraneous hashtags like "tcot", "ccot", and "", were :


#politics | #maga | #trump | #news | #neverhillary | #pjnet | #trumppence2016 | #trump2016
---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
8,332 | 6,886 | 5,546 | 4,538 | 3,031 | 2,826 | 2,184 | 2,130 |


As expected most of the hashtags tend to have a political meaning to them. There are however, some hashtags that jump out as not following along the lines of political content and could warrant further analysis. Those hashtags include pjnet, merkelmussbleiben, and christmasaftermath to name a few.


## Users in the Dataset

In total, there were 454 unique usernames that tweeted in the dataset that was provided. Here is a chart of the top ten most active users in the dataset along with their tweet count :


ameliebaldwin | hyddrox | giselleevns | patriotblake | thefoundingson | melvinsroberts | mrclydepratt | brianaregland | leroylovesusa | baobaeham
---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
9,269 | 6,813 | 6,652 | 4,140 | 3,663 | 3,346 | 3,263 | 3,261 | 3,229 | 3,215 |


You can note that there is quite a big difference of volume in tweets from the most active account to the tenth most active account. Also note the common theme of "patriotic" or "American" username choices like **PATRIOT**blake, the**FOUNDING**son, leeroyloves**USA**, etc.


While 454 is the total number of users that authored tweets that were removed from Twitter, there is more information that can be found from the dataset regarding users. Through the tweet's text and those mentioned in the tweet, there is a total of 213,186 users mentioned (*note this is not unique mentions of a username*). There is a total of 53,345 additional unique usernames that appear in either the text or the mentions of the supplied tweets. This shows that near all the tweets contained in the dataset are tweets that are in response to a tweet from a different original author, where the originating author is automatically tagged or the tweets contain mentions to an individual. I believe this is done to maximize the impact of each tweet. By replying to a tweet from another popular user the troll account is able to obtain the maximum impact and views for their tweet, especially when boosted by other fake accounts since Twitter shows the most popular and interactive replies first when viewing a tweet.

Here is a table of the most mentioned users in either the text or in the mentions of the tweets :


realDonaldTrump | midnight | blicqer | HillaryClinton | Conservatexian | POTUS | FoxNews | YouTube | PrisonPlanet | nineoh
---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
3,707 | 2,523 | 2,236 | 1,949 | 1,103 | 726 | 716 | 550 | 545 | 536 |


## Words in the dataset

Below is a table of the most commonly used words in the dataset. *Note that stopwords and other common words and words with no context were removed from the display to display more meaningful results.*


Trump | Clinton | Hillary | Obama | will | people | like | Donald | realDonaldTrump | politics
---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
23,730 | 10,395 | 9,657 | 7,504 | 7,006 | 5,193 | 4,808 | 4,756 | 4,245 | 3,970 |




## Work in Progress

Todo : Map patterns from top tweet contributers. Determine the main target of their tweets along with the message they were trying to convey. Show other users interactions with these removed "troll account" wit accounts that were not removed from Twitter and are still active to this day.

**Please note that this post is still a work in progress and will continue to be updated.**