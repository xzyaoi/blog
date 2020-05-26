---
layout: post
title: "Analysing Sentiment Changes during the COVID-19 Pandemic"
author: "Xiaozhe Yao"
mathjax: true
header-style: text
catalog: true
tags:
    - Machine Learning
    - Systems
    - Natural Language Processing
---

> United we stand, Divided we fall.

Since last December, the coronavirus has been spreaded across the globe. Besides of the physical harm to more than 5 million innocent people, there comes a lot of harmful speech that hurts people mentally. 

When I was browsing Twitter, I've seen the emotional change. At the start, I was also angry, as many people do, and thought that the governments were doing something weird. But soon the things changed, and a LOT of hateful words about a specific race, a specific person arose. I was pretty curious on how things changed during the whole pandemic and decided to learn it from the Twitter data. 

So my fellows and I built a crawler based on a great project [Twint](https://github.com/twintproject/twint). The project, as well as the crawler that we built, is pretty fast and can crawl more than hundred thousands tweets in two to three hours. Afterwards, we processed it with pretty standard way: analyse if they are either positive or negative by using [TextBlob](https://textblob.readthedocs.io/en/dev/) and draw [wordcloud](https://github.com/amueller/word_cloud) based on the tweets.

One different thing is that we also try to analyse the sentiment in more detail, i.e. we want to know if a tweet is either happy, discouraged, sad, loving, joy or depressed. To achieve this goal, we build a neural network and trained it on a previously collected twitter dataset, and then build the neural network into an [AID](https://aid.autoai.org) package. We deploy the package into a 3-node cluster under [HAProxy](http://www.haproxy.org/). By doing so, we can easily get the emotion of each tweet.

The visualization of the project is deployed at [Sentiment Analysis During COVID-19 Pandemic](https://covid19.yaonotes.org/).

When I was reviewing the wordcloud of each day, I've seen lots of warm speech and that indeed encouraged me alot. For example, I've seen several most popular words are **Hope** and **Believe**, **Family** and **Friends**. I hope you will also be encouraged by the wordcloud, and see that people are becoming more positive as things are getting better.

If you'd like to contribute your part to tackling the global pandemic, please wear the mask, keep social distancing, and consider donating to [UZH Pandemic Fund](https://www.uzhfoundation.ch/de/pandemiefonds).

Stay Safe.