---
layout: post
title:  "A Deep Dive into Delaware Restaurants"
date:   2019-10-07 10:20:00 -0400
categories: Blog Post
---

# The Data

All the data that I will be using for this analysis is provided by the state of Delaware. It is apart of their "Delaware Open Data" initiative that launched back on January 27th by Gov. Jack Markell with the goal of "offering access in a central location and will facilitate better data sharing and collaboration across public agencies, nonprofits and the private sector to spur innovation and develop applications that can benefit our communities.” [Read this article for more information on the matter.](https://www.govtech.com/state/Data-Governance-Council-to-Revamp-Delawares-Open-Data-Efforts.html)

## Getting the data

This is largely the same from my [previous](https://b-weyl.github.io/blog/post/2019/04/01/plotting_restaurant_inspections_with_python_and_folium.html) post, with the exception of instead of creating a map and displaying the locations of the restaurants, I am just going to analyze the data provided. I have still written the data from the table on the Delaware Open Data websites table to a `txt` file for ease of accessing.

### Some basic information about the restaurants in Delaware

At the time of writing this article, August 28th 2019, there are 11,338 documented inspections on the website. I will do my best in keeping this updated as frequently as possible. I am currently trying to automate a way to always a current up-to-date file of the inspections. The inspections seem to be updated on a random basis and not at a regular schedule.

There are 2,476 restaurants that have violations in Delaware. That falls to roughly 60.2% of places having accumulated at least one violation. This leaves a remaining 1,635 places that have no violations to their name. Some places have a advantage over other restaurants with being newer places and thus having less time for them to be inspected.

The restaurant in Delaware with the most inspections to this date is `LA DELICIA` located at 585 FOREST AVE in Dover Delaware. They have a total of 53 violations to their name. The next highest total number of violations is 42, courtesy of `ATAMI GRILL` located at 833 N MARKET ST. Wilmington, Delaware 19801.

Despite having a staggering number of health and safety violations, `LA DELICIA` has overall positive reviews on Facebook and Yelp. On the other hand, `ATAMI GRILL` does not quite have the same luck. Most of the reviews from various sites including Facebook, Yelp, and Tripadvisor average a 2.5 out of 5 star rating. Most of the reviews detail bad delivery service since the place is mostly for delivery which would explain why people would still get food from a place with so many violations. Maybe the people who order from the place have never physically been inside of the establishment and are not aware of the cleanliness of the place.

I plan for this to be an ongoing project that I will try to keep up-to-date in my free time. This will vary depending on the amount of free time that I have to be able to work on this project and how frequently the data provided by the state is updated.
