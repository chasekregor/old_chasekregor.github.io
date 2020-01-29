---
layout: post
title: Making Quantified Chase
comments: true
redirect_from: "/2018-6-11-making-quantified-chase/"
permalink: making-quantified-chase
categories: 
    - "biohacker"
    - "quantified-self"
    - "DataScience"
---
---

[Click here to see Quantified Chase](https://quantified-chase.herokuapp.com)

On May 4th, 2018 I acquired an Apple Watch as a graduation present. I immediately started to realize its potential as a device that could genuinely help improve my life.

Over the next month, I found the information gain and habit encouragement the Apple Watch provided to be phenomenal. More importantly, though, I noticed two things. One, that I now had a constant heart rate sensor on me 24/7, calculating my active and resting calories. And two, that paired with the Health app on IOS has a lot of potential, especially as a central repository for bio tracking. In the health app, I started to track the various food, sleep, and activity metrics I wanted to know about on a daily basis.

Over the next month, I experimented with various platforms that had HealthKit integrations that would allow me to automatically feed my Health app higher quality data. Ultimately I settled on myfitnesspal for food, my phone for sleep, Strava and my Apple Watch for activity and fitness metrics.

I also have always been a fan of the quantified self-movement, and thought "wouldn't it be cool if I could create a dashboard of my life?". I also have always loved the way Strava and the Apple Watch enable you to share data about yourself to family and friends. I think it really creates a sense of accountability and information gain for fitness.

And so the idea was born to somehow take all of my personal Health app data and put it on display for all the world to see.

The first step in this project was figuring out how to export my data out of the Health app. Apple makes it pretty hard to export data out of HealthKit for privacy reasons. Eventually I stumbled upon [QS Access](http://quantifiedself.com/qs-access-app/), it is a quantified self-application made specifically for this sort of thing. It helps you export your HealthKit data into a CSV.

Next, I built the flask app that would display the various metrics I wanted to track on a daily, weekly, and monthly basis. I used an awesome visualization package called [C3PYO](https://benalexkeen.github.io/C3PyO/) to make D3 style graphs out of matplotlib style syntax.

Once I built the site out into a somewhat well-designed format, I deployed the site to the internet using [heroku](www.heroku.com). Sometimes, deploying sites on Heroku is easy, other times it's not. This was one of those times when it wasn't. After a couple of days and a good night's rest, I finally successfully deployed the site.

Since then, I have improved the about page and started uploading my fitness, energy, and sleep data on a daily basis. Data after June 1 is considered accurate, that's when I started tracking and uploading all my data diligently.

I have had a really great time creating this project and think it displays some real-world data science proficiency on my part. In the future I would love to add to the site and track more metrics, possibly using machine learning to predict various aspects of my life. I will be uploading all my data for the month of June 2018. You can follow along on [Quantified Chase](https://quantified-chase.herokuapp.com) and see how my various metrics change throughout the month. I am currently training for a half marathon so expect my walking and running mileage to go up. Feel free to email or contact me for any other metrics or motivation you may have for me!

