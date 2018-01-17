---
layout: post
title:  "Twitter Bio Search"
date:   2018-01-14 13:22:00 -0400
categories: Blog Post
---

# Searching Twitter User Descriptions

## Inspriation and Motivation

I have recently created a program that could display trending twitter topics of specified locations, compare trends, and display same tweets of a topic and I was
thinking about what else I could do with Twitter and Python. I had the idea of being able to parse through Twitter User descriptions and find useful information in the user's bio. For example,
during a sports game, one could search through users who tweeted about the games bio to determine which team that user supported and then create a chart of statistics displaying
which team had more supporters/support through Twitter. While I haven't fully tested this idea I will explain what I have come up with so far. Please note that this is work in progress and
will probably end up changing numerous times going forward.

## Parsing Twitter user description

In order to parse through a Twitter user's description we would first need a twitter user. An easy way to do this is to use `Tweepy`'s built in streaming module to stream tweets for a certain topic.
We can create a `MyStreamListener()` and create a function to call when the Listener receives data:

```
class MyStreamListener(tweepy.StreamListener):
    def on_status(self, status):
        if status.user.description:
            find_social_accounts(no_emoji(status.user.description))

    def on_error(self, status_code):
        print(status_code)
```


Since it is possible for some users on Twitter to not have a bio, we want to just go ahead and skip over those accounts, we can do that by checking if the user has a description or not.

**Side Note**: Make sure to print out the `status_code` on error so you can be aware if you are hitting rate limiting or not.


Another thing that we will needed before we go ahead and process a user's bio is we need to remove all emojis or other non-printable characters. This will make handling the text easier. I have found that the following function works for me in getting rid of those unwanted characters:

```
def no_emoji(tweet):
    tweet = list(filter(lambda x: x in string.printable, tweet))
    new_string = ''.join(tweet)
    return new_string.strip()
```

Now that we have removed all the extra garbage we can go ahead and move onto processing the Twitter User Description. Like I mentioned above I did not use the example I had stated above, instead I chose to see if I could extract social media accounts from user's descriptions. Obviously, these accounts would have to be from a social media platform other than Twitter. I would argue that the two other most listed social media profiles in Twitter User's Descriptions are Snapchat and Instagram. So now we need a way to parse through the user's description and return their account names. First, let's take a minute to think of the ways that one could display their account names in their descriptions. There are two main ways that jumped out to me which I went ahead and implemented. The first way would be simply to say something along the lines of:
```
Snap: Username
Insta: Username
```

Similarly the user could also also abbreviate the other two platforms as "SC" or "IG", so we need to take that into account as well. A quick google search shows a useful python package that would could import to help make this easier for us. `fnmatch` can be used to match a certain pattern. For instance I can use `if fnmatch.fnmatch(str(word), 'ig?')` to match any case that has 'ig' as the first two letters and it doesn't matter what the third letter is. This is useful for those who chose to display their accounts as either `SC:` or `IG:` or even `SC-` or `IG-`. Again, we can also apply this to those who chose to display their accounts as `Snap` or `Insta` - like so - `if fnmatch.fnmatch(str(word), 'snap?')` or `if fnmatch.fnmatch(str(word), 'insta?')`

Combining that with logic to write the user names to a file and we are left with:
```
def find_social_accounts(bio):
    with open('accounts.txt', 'a') as account_file:
        words = bio.split(' ')
        for word in words:
            if fnmatch.fnmatch(str(word), 'ig?'):
                print("Wrote an Instagram username to the file")
                the_index = int(words.index(word))
                if the_index + 1 < len(words):
                    # need a better way to handle cases with values found at the end of bio
                    if len(words[the_index + 1]) > 0:
                        account_file.write('Instagram account - ' + words[the_index + 1] + '\n')
            if fnmatch.fnmatch(str(word), 'insta?'):
                print("Wrote an Instagram username to the file")
                the_index = int(words.index(word))
                if the_index + 1 < len(words):
                    # need a better way to handle cases with values found at the end of bio
                    if len(words[the_index + 1]) > 0:
                        account_file.write('Instagram account - ' + words[the_index + 1] + '\n')
            if fnmatch.fnmatch(str(word), 'sc?'):
                print("Wrote a snapchat username to the file")
                the_index = int(words.index(word))
                if the_index + 1 < len(words):
                    # need a better way to handle cases with values found at the end of bio
                    if len(words[the_index + 1]) > 0:
                        account_file.write('Snapchat account  -  ' + words[the_index + 1] + '\n')
            if fnmatch.fnmatch(str(word), 'snap?'):
                print("Wrote a snapchat username to the file")
                the_index = int(words.index(word))
                if the_index + 1 < len(words):
                    # need a better way to handle cases with values found at the end of bio
                    if len(words[the_index + 1]) > 0:
                        account_file.write('Snapchat account  -  ' + words[the_index + 1] + '\n')
            else:
                continue
```

As of writing this article, the code is still pretty _hackish_ and needs to be refactored, but for now the code does run the way I intend for it to.


# Issues to address in the future

There are some issues that still need to be dealt with due to the _hackish_ nature of the code. For example, if a user spaces out their media accounts in their bio as such:
`Snap : Username` that extra space after snap causes the program to only write `:` as the media account name, which of course can not be the case. Also, sometimes the program fails to write anything and just writes a blank line. Issues like these along with refactoring the code are things that I plan on taking a look at in future revisions. To see the entire program, visit my [TwitterCode](https://github.com/B-Weyl/TwitterCode) section on my [Github](https://github.com/B-Weyl).