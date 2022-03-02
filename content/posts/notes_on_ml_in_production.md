---
title: "Notes on ML in production"
date: 2022-01-22
categories:
  - blog
  - machine learning
  - ML
  - production
  - data
  - heuristic
  - baseline 
  - hyperparameter
---
Today, I find myself working on another Machine Learning (ML) model with the goal of putting it in production. I thought a good first step would be to look back on my previous experiences and identify what (not) to do!

I won’t explain what ML is in this article, or which algorithm(s) to pick according to different cases; I’ll go through good practices like: how to start, what not to forget about and little tricks to make your life easier.

# First things first: Set your goal(s) in advance 🥅

Before playing around with your favourite ML or Deep Learning (DL) library, set the goal(s)of your work. You need to define one (or more) metric(s) for your model, as well as the threshold(s) you need to reach for the model to be a success.  
You want your metric(s) to be validated by the business side. For example, with the product manager or the interested party (might be your CEO, the product’s final user, ...). In the case of Google Maps [1], they had a goal to make image recognition of street addresses as good - or better than - humans! Measuring the accuracy of humans gave them their goal: 98% accuracy in correctly identifying street addresses.  
When starting your next task, take the time to define your goal(s) first! It will help you get started more quickly, stay on task for effectively, report your progress to stakeholders, and recognize when you’ve achieved your goal(s)!  
Now that you know what you want to do, get your dataset!  

# Data, data, data, data 💽

More often than not, when working on ML tasks, you won’t have readily accessible data. Do not underestimate the importance of gathering clean, well labelled datasets. As the ML community gravitates from Model Centric to Data Centric [2], it has become increasing clear that “data is the most under-valued and de-glamorised aspect of AI”, and you shouldn’t make this mistake!  
If you’re working on a supervised learning task, you want the most well and consistently classified labels. To make sure that every sample your model will be fed or measured with is as good as possible, try finding a good source, an expert or by labelling yourself carefully.  
In the past, I‘ve underestimated the impact of clean data. I believed that manually labeling data was boring and useless. So, we hired a few interns to label training and testing data for our ML project. The result was an inconsistent and sometimes wrong dataset that greatly impacted our model’s performance. We were able to correct our mistake by going over the labels and fixing most issues, but a lot of time and energy was wasted.  
When exchanging ideas about this subject with a new colleague [3], he shared a few good practices that they implemented at his previous company. Define precise and exhaustive label rules: anyone with the documentation should be able to correctly label a sample. Use multiple people labels, humans make mistakes and by having multiple people label each sample, most of them should be identified and corrected. Finally, have a “I don’t know” class! If too many of the samples are classified as such, maybe ML is not a correct approach. It’s possible that the necessary information to solve your classification task isn’t available.  
At this point, you have a consistent and polished dataset. Next, start simple and establish a baseline!

# Start with a baseline!

Do not jump the gun and start by implementing the most complex DL model you’ve recently read about. Start simple - as simple as possible - with a baseline for your task.  
What’s that? It’s a very simple model, heuristic or method that answers your problem!  
Building it first will give you an idea of the difficulty of the problem at hand. By measuring your baseline’s performance with your metric(s), you will know how far you are from completing your project!  
In the case of Natural Language Processing (NLP) and text classification, it can be a simple bag-of-word for instance. For image recognition between oranges and apples, it would be as “orange pixels” ⇒ orange and the rest are apples, etc.  
You have an up and running baseline for your problem, your data is ready, now go ahead and try out multiple models. Have fun!  

# Simplicity is queen 👑

If the goal is to have a production-ready model, cut out the complex and reach for simplicity instead! You and your team need to understand the model, how it works, its limits and advantages.  
The more complex the model is, the trickier it will be to monitor it, and more errors might be revealed!  
The first ever ML model that I put into production was during my internship at Oui.sncf. As the only data scientist assigned to this task, I used the web extensively to figure out the best way to do… well everything! I stumbled on a post (it might have been on stackoverflow or datascience.stackexchange) advocating for the Occam’s razor - or the principle of parcimony [4], and I believe it’s a principle that everyone should try to implement in their work. It states that, when presented with competing hypotheses about the same prediction, one should select the solution with the fewest assumptions. After a few months of work, we ended up with a simple Random Forest Classifier in production... and it did the job!  

# Little trick: hyper-parameter tuning

A little trick I learnt while browsing Deep Learning [1] is, when tuning your model’s hyperparameters, do not define long grid-searches with tons of hyper-parameters. Lean towards iterating over multiple shorter grid or random-searches.  
If possible, choose random-search over grid-search, it will save you time. Instead of picking the values from all possible parameters values stated in a methodical manner, they are picked at random. Good parameters will be identified without having to try every single combination.   
For instance, on the 1st iteration define your search space for parameter C as [0.01, 0.1, 1]. Take the best solution as C = 0.1. So for your next iteration, define a new search space “converging” over your previous solution: [0.05, 0.1, 0.15]! You can then repeat the operation, converging more and more, until there is barely any increase in your model’s performances.  

# Happy with the model? Think of what happens next

You’ve done it, you have your dataset, your model, its hyperparameters, and it achieves more than your goal... What now?  
Take the time to think about what happens next for your model! You’re not done with it yet. How do you want to monitor it? You should consider how to best monitor your model, even if you’re not able to implement monitoring right this minute - right now is probably the best time as you know your model in and out. You might want to look into decision or concept drift when manual labelling is required for identifying the correct target. If you can, define when you should retrain your model next (unless it is made automatic somehow 👍).  
Determine, and share with the stakeholders the limits and downfalls of the model. It’s important to know ahead of time what can happen, so you know what steps to take preemptively.  
Finally, when doing experiments or deploying your model, don’t hesitate to check out ML Devops tools such as MLFlow [5].  

---

There you have it! In this short article, I described various insights I gathered when previously working on ML tasks for my company. I hope you learnt interesting tips and tricks.

And I will see you in the next article 👋

# Sources

[1] Deep Learning. Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016.

[2] Andrew Ng Launches a campagin for Data-Centric AI. Andrew Ng. [https://www.forbes.com/sites/gilpress/2021/06/16/andrew-ng-launches-a-campaign-for-data-centric-ai/?sh=10f1a27574f5](https://www.forbes.com/sites/gilpress/2021/06/16/andrew-ng-launches-a-campaign-for-data-centric-ai/?sh=10f1a27574f5)

[3] Colleague @ Smartway.

[4] [https://en.wikipedia.org/wiki/Occam's_razor](https://en.wikipedia.org/wiki/Occam's_razor).

[5] [https://mlflow.org/](https://mlflow.org/).