---
layout: post
title: Capstone Ideas - Fire Engines or Fine Schools?
---

In a few days time I'll need to pithc my ideas for my Capstone project to my classmates. It's been a headache trying to find potentially useful, interesting ideas that I have
some hope of finding the right data for.

To cut a long story short, I've settled on 2 possible ideas - one generated from a conversation I had with an ex-copper a few years ago ('One of the best predictors for how
busy we would be was the weather - people don't riot when it's raining') and one from my own experience ('When we move, what school should we choose?').

First Idea: How Many Fire Engines Will Be Called Out Tomorrow?

Why, Who, What, How Much and Where From? 

For me, the questions above summarise the requirements for the information needed in a pitch (Problem Statement, Potential Audience, Goals, Success Metrics, Data Source(s))
and so are a useful way to keep track of what's needed to persuade my classmates that I might be doing something worthwhile (and keep them interested enought that they 
give me some feedback to help me improve my project).

In this case, the 'Why?' is trying to help the London Fire Brigade to plan their resources by helping to predict the number of callouts on a day in the near future, based on
the weather forecast. The idea was built on the conversation mentioned above - if the Police can adjust their plans based on the weather (as well as the ambulance service - 
a web search revealed that ambulance calls based on respiratory problems increase with temperature), then I wanted to explore if we could do the same for the LFB.
It's important to note that in 2020, the LFB had 100,000 calls (half of which were false alarms - but that might be for a future project...).

'Who?' would be the LFB themselves, the people who pay for the LFB (and local authorities generally) as well as fire engine enthusiasts.

'What?' is a live prediction of the callouts for the next week based on trained models (with a stretch goal of more granular predictions by fire station, based on more local
weather data).

'How Much?' is a beetter than baseline model (much better for a truly successful project) and an idea of other factors to explore for the remaining variation.

'Where From?' is the source of the data - csv files for all the LFB callouts (from the London datastore - well worth a look), csv files from the Met office/CEDA for the
wether data up to the end of 2019 and other web sources for the 2020 data and finally a webscrape at the time of the prediction to gather the live weather forecast.

Second Idea: Which Secondary School Should we Choose?

Why, Who, What, How Much and Where From? 

'Why?' Based on my own experience, it's a pain to choose a new school when you move house. Any smart tools that can help narrow down the choice would be helpful. There
are a number of websites and guides available - but very little in the way of consistent data (and consistent rating) to help make an informed choice.

'Who?' would be parents, schools, local authorities, possibly employers and dare I say it children as well.

'What?' is creating a reliable predictor of the rating of a school (ie a high score for the question ‘Would I recommend this school to another parent? - a Net Promoter Score
question) based on not just academic factors, with a stretch goal of returning a list of top 5 schools near a given location, with important factors input.

'How Much?' is creating a model that predicts school ratings better than baseline  - the higher the accuracy, the better. 

'Where From?' .gov.uk - csv tables of all schools, including academic performance indicators.
Ofsted - Parent-view csv records, including aggregate parent answers to ratings type questions
Wikipedia - webscrape pages for a large number of English secondary schools with additional info.

Thanks for reading.

Andrew


