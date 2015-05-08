---
title: "Data Hygiene"
layout: post
category: Clean your Data
tags: [wrangling, data janitor]
---

####**Pre-requisites** Intro to Spatial Data, Data Collection

####**Objectives**
  - Understand how to clean datasets
  - Recognize issues in our datasets

----

{% include JB/setup %}
# Data Hygiene

In tutorials, and exercises it is common to receive prepared data sets which will easily fit into a workflow. Unfortunately, in real life, data comes in all shapes and sizes, and more often than not, the data we need has quality, consistency or format issues. These can be products of data entry, sensor calibrations, or legacy formats. 

----

## Data Janitor: Someone has to do it

It is important to know how to clean up the data sets we download, if we hope to make our workflows reproducible.

----

## Keep a clean workspace

Its hard to keep anything clean in a dirty kitchen. The same principle applies to our workspaces. If we are testing pieces of a workflow, it will be difficult to prevent ourself from creating an abundance of files, but the difference between a clean workspace and a messy one can be as simple as using standards with our naming conventions. 

If we find that our workspace starts to look like:

{% highlight bash %}
data.csv
data_1.csv
data-1.csv
data1.csv
data3.csv
data_REAL.csv
dataREAL_1.csv
data_processed.csv
data_FINAL.csv
USE_THIS_data.csv
{% endhighlight %}

We might be guilty of data negligence. 

If we are doing testing, it is good practice to create a scratch(temp) workspace to hold all of the intermediate files being created.

----

## Naming Standards

It is also good practice to keep with a consistent naming structure to prevent from confusing ourself when we go looking for the right file 6 months from now.

There are a variety of ways to name our files, and the one use is our own choice as long as we use it consitently

Here are a few examples:

**CamelCase**
{% highlight bash %}
myCoolFile.csv
{% endhighlight %}

**snake_case**
{% highlight bash %}
my_cool_file.csv
{% endhighlight %}

**spinal-case**
{% highlight bash %}
my-cool-file.csv
{% endhighlight %}

**Train-Case**
{% highlight bash %}
My-Cool-File.csv
{% endhighlight %}

**SCREAMING_SNAKE_CASE**
{% highlight bash %}
MY_COOL_FILE.csv
{% endhighlight %}

---

## Keep it simple

If we have a lot of similar data, it can be easy for us to stuff everything into a single table that we can scan through. This can be good when datasets are still relatively small, but once we are dealing with 30+ columns it can become a huge pain to understand our datasets. 

Once our datasets start growing, we should check to make sure we actually need all the data we have. If we don't, it can be easy to become a data hoarder. 

**Just because we can fit everything doesn't mean we should.**

These extra columns can not only make our data hard to read, but it can also make our data less reliable, more redundant, and slower to process.

---

## Data Redundancy

Keeping redundant data in your dataset can seem like a good idea in case we mess up one column or row, then we can copy/paste the original data. This safeguard can introduce inconsistency and corruption into our data if we are not careful.

| id || temp || tempCel |
|:---:|-|:-------:|-|:--------:|
| 1  || 67      || 19.4    |
| 2  || 80      || 26.7    |
| 3  || 89.6    || 32      |
| 4  || *45*      || *45*      |
| 5  || 33.8        || 1      |

In the table above, there is a temp column and a tempCel column. At first glance it could be easy to tell that the first temp column is Fahrenheit and the second is Celsius. An issue with redundancy comes when one column is updated and the other is not. If we don't have information on which column is more reliable, we can only guess. These guesses become harder as datasets become larger and the relationships between the redundant data is more complex.








