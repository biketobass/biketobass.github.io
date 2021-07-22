---
layout: post
title: Updates to my Python Code For Strava
date: 2021-07-22 5:30:00 -0400
categories: computer python software data strava fitness
tag: strava
---

# Introduction

In [an earlier post](https://biketobass.github.io/computer/python/software/data/strava/fitness/2021/07/13/using-python-to-analyze-strava-data.html), I discuss my Python code [found in my GitHub repository](https://github.com/biketobass/strava-analysis) that downloads a user's Strava data and allows the user to ask questions of the data that can't be asked on Strava's website or app (as useful as those are). If you'd like to use that code, please read that earlier post to get started, especially the section on OAuth 2.0 authorization which is a necessary, one-time step to be able to access Strava's API.

In this entry, I briefly review how to use the code and discuss two new features.

# Review

Once you've gone through the OAuth 2.0 process, to use the code create a `StravaAnalyzer` instance like this:

`sa = strava.StravaAnalyzer()`

The constructor will download all of your Strava data and store it locally in a few different CSV files (see my earlier post).

# New Features

The `predict_avg_speed()` method now supports activity types other than bike rides. To predict the average speed of a 10 mile hike with 364 feet of elevation gain, for example, run the following:

`sa.predict_avg_speed(elev_gain=364, distance=10, activity_type="Hike")`

Make sure that the `activity_type` matches Strava's name for that activity.

As before, this method uses three different models to make three
predictions about the average speed of a proposed route. Two use
regression. The third uses the average speed of similar activities.
Use the `dist_fudge` and `elev_fudge` factors to indicate what it
means to be similar. The defaults are each 0.1 which means that an
activity is similar if the distance and elevation are within plus or
minus 10% of those given.

`suggest_similar_activities()` is a new method that, given an elevation
gain and a distance, returns a list of URLs of your past activities
that have similar elevation and distance profiles. As with
`predict_avg_speed()`, use the fudge factors to indicate what "similar" means. For example, if you want to see past bike rides that have an elevation gain of around 2000 feet and a distance of around 50 miles run the following:

`sa.suggest_similar_activities(elev_gain=2000, distance=50, elev_fudge=0.1, dist_fudge=0.1, activity_type="Ride")`

If you want greater variation, increase the fudge factors. You can also leave either distance or elevation gain out (but not both). For example, this call `sa.suggest_similar_activities(distance=5, activity_type="Walk")` will return all of your past walks on Strava that were around 5 miles long.

I find `suggest_similar_activities()` useful in planning my bike
rides. I might know that I want to aim for a particular length ride
with a certain amount of elevation gain. Given that information, I can
look at what I've done in the past and decide if I want to ride any of
those routes again. I can also use them as starting out points for new
routes.
