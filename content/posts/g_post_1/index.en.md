---
weight: 1
title: "Web Scrapping"
date: 2020-09-07T15:32:23+00:00
lastmod: 2020-09-07T15:32:23+00:00
draft: true
author: "Shayan Ahmed Khan"
authorLink: "https://www.linkedin.com/in/shayan-ahmed-504a52169/"
description: "Shayan writes about his experience with Web Scrapping he had"
resources:
  - name: "featured-image"
    src: "Featured.jpg"

tags: ["Python", "Technical Articles", "Web Scrapping"]
categories: ["Tech"]

lightgallery: true
---

Did you know that web scraping is integral to the data analysis process? It is because it allows quick and efficient extraction of data from different sources, which can then be processed to glean insights as required. Data Analysis is a huge market today, for some of the software giants it's the source of every decision making, even for small businesses, they get to keep track of the brand value and reputation of their company. You never know when a small automation script to dig in data from various sources might find someplace in engineering life or workspace :smile: (ignore the emoticon, just trying to get your attention).

In this post, I will be using python, it's a diverse programming language and when you pick the right tools, libraries, you are a superhero. Starting to learn any programming language takes curiosity, will, time, and a couple of energy drinks :wink:. I would suggest you get a little basic concept of Python handy if you are not already using it.

### Hang on with me for 15 mins and scrap your first website today.

### The Basics of web scrapping

**The crawler**

A web crawler, which we generally call a “spider,” is an artificial intelligence program that browses the internet to index and searches for content by following links and exploring layers of pages, like a person with too much time on their hands. It's the machine version of an internet stalker.

**The Scrapper**

A web scraper is a specialized tool designed to accurately and quickly extract data from a web page. Web scrapers vary widely in design and complexity, depending on the project.

### Prerequisite
- An IDE, I prefer Spyder
- Python3, comes pre-installed on Ubuntu, and for another OS, just have it setup. and Test if you can view the Python INteractive Shell by running `python3`
- Basic knowledge of html tags and python

### OK!!! Let's start
- **Step 1**: Import the required libraries we need
  - BeautifulSoup from bs4 (It is a simple library that makes your scrapping easily.)
  - Pandas
  - Request and urlopen from urllib

- **Step 2:** Get the Url you want to scrap data from.
- **Step 3**: making request to access resources using request module
req = Request(url+"-"+str(a), headers={'User-Agent': 'Mozilla/5.0'})


note
Take care of the user agent.Most of the time when you try to scrap website some of these webisites because of thier
safetly protocol does not allow to scrap.For obviously that their are many automated scrapper scripts continuously running to scrap website.
Robots.txt is a text file webmasters create to instruct web robots (typically search engine robots) how to crawl pages on their website. The robots.txt file is part of the the robots exclusion protocol (REP), a group of web standards that regulate how robots crawl the web, access and index content, and serve that content up to users.

step 4: creating soup object to parser html script
html_doc = urlopen(req).read()
soup=BeautifulSoup(html_doc,'html.parser')

*you can see the beautifulsoup object
print(soup.prettify).
which represents the document as a nested data structure

step 5: use predefined function to extract
your desired information.
findAll()
find()


Here are some simple ways to navigate that data structure
1.soup.title
o/p: The Dormouse's story
2.soup.title.name
o/p: u'title'
3.soup.title.string
o/p: u'The Dormouse's story'
4.soup.title.parent.name
o/p:u'head'
5.soup.p
o/p:The Dormouse's story
6.soup.p['class']
o/p: u'title'
7.soup.a
o/p:Elsie
8.soup.find_all('a')
o/p: [Elsie,
Lacie,
Tillie]
9.soup.find(id="link3")
o/p: Tillie
One common task is extracting all the URLs found within a page’s  tags:

for link in soup.find_all('a'):
print(link.get('href'))

o/p:  http://example.com/elsie
http://example.com/lacie
http://example.com/tillie

Another common task is extracting all the text from a page:
1.print(soup.get_text())
o/p:
The Dormouse's story
The Dormouse's story
Once upon a time there were three little sisters; and their names were
Elsie,
Lacie and
Tillie;
and they lived at the bottom of a well.

conclusion
It is very easy to extract data using beautifulSoup.After you need to use data structures such as dataframe in pandas library very famous one to store the extracted data in well structured way.Nice and simple application of python that you can learn if your are a college fresher this also give you some hold python programming.
here is the link to my github repository
Hope you enjoyed the post!!!


