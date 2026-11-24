# CAML (Capital Allowances and Machine Learning)

## Background
Within tax there's a concept of tax depreciation, where you get tax relief for the assets that a business buys. In the UK this is called capital allowances, and it works by looking at the type of asset bought and then putting it in a category or "pool". The pool that it's in then determines how much tax relief an asset gets, ranging from a full up front deduction, down to 6% per year. 

Because of this range, and the benefit to accelerating this tax relief, reviewing capital allowance categorisations can be a valuable part of a tax advisor's work for a client - but clients can buy hundreds or even thousands of assets in a year which each need to individually categorised. For a lot of tax advisers they'll use some excel filters and maybe a couple of if/then formulae to churn through some of the similar stuff (e.g. all of the iphones or laptops) but then still need to work line by line reading the description and assigning it a category based on their knowledge of the rules and their experience. Various 3rd party rule based tools are on the market which look for keywords, and EY even trained a machine learning model about 10 years ago to do much of the heavy lifting. 

As a novice to machine learning, with an existing knowledge of capital allowances and an awareness that ML could be a solution, this seemed a great way to get started. 

## The problem
Take an input of a list of fixed asset addition names/descriptions e.g. "Conveyor belt motor", "new fire door for warehouse", "Ford Transit" - including realistic unhelpful descriptions like "new asset" and "invoice 001234" and categorise into the correct capital allowances treatment or identify those where additional information is required.   

## The data
My first problem was data. This is a personal experiment so using any kind of data that my employer might have was immediately off the table - never mind how painful extracting it all may have been. 

Searches of a few different dataset libraries were also fruitless. I briefly thought that UK local authorities might publish the data, but this was a single council that gave enough detail over a few hundred with the others aggregating at the level of "cars", "computers", etc - not the sort of data you practically see in the real world - and a few hundred lines of data wasn't going to be enough.

I could have done my best to individually create a fictional set of data but would have taken a lot of time and effort to find full model names and specs of different items and then present them in different levels of accuracy. 

The solution I concluded on was to use a small-ish LLM (GPT 5.4 mini) to produce a lot (c.35,000) of different asset names

## Risks in the data
Within the data, and capital allowances more generally, for most businesses a lot of their spend is "plant and machinery" that would all fall into the same category of "main pool"- think of things like computers, production line and packaging machinery, vans and trucks etc. This means that there's a good chance that if the model guessed "main pool" all the time, it would probably have a good accuracy rate. 
