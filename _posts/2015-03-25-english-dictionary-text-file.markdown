---
layout: post
title: English dictionary text file
date: '2015-03-25 06:45:02'
tags:
- mac
- linux
---

Couple of days before, I was making a search engine for my two projects. One project was using Elasticsearch and another one was using Apache Solr. For them both I needed a file containing all the words from an English dictionary.

I search here and there and finally found that every unix like system has this kind of file with it to predict text. I was using and iMac. I found the text file in the following location:

`/usr/share/dict/words`

My work is done, I used the file and rebuild the Apache Solr dictionary.