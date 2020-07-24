---
title: 'ParkJA'
date: '2020-07'
draft: false
tags: ["Docker", "Django", "CouchDB", "Framework7", "Cordova"]
categories: ["Mobile Application", "Web Application"]
excerpt: A web dashboard and a mobile application to manage Brazilian parkings.
image: '/images/download_parkja.png'
website: https://parkja.com
---

PArkJA is a web and mobile application to manage parking operations in Brazil.

Python is used for serving the web application using the [Django framework](https://www.djangoproject.com/): the simple and effective dashboard allows the parking manager to setup the pricing tables of the parking, the additional services provided (car wash for example) and the users that are allowed to manage the customers vehicles.

Users interact with the platform through their mobile phone: [CouchDB](https://couchdb.apache.org/) seamlessly synchronizes in real time the data between the parking employee phone and the server, in background, without affecting the operation, even if for some reasons there is no Internet connectivity. This offline feature is very important in a country where the mobile network coverage depends a lot from region to region, even in the same state or city.

The mobile application is developed in Javascript using [Framework7](https://framework7.io/), a free and open source framework to develop mobile, desktop or web apps with native look and feel.
