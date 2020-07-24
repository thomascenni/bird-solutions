---
title: 'Industrial Dashboard'
date: '2017-09'
draft: false
tags: ["InfluxDB", "Plotly", "Grafana", "Modbus"]
categories: ["Web Application"]
excerpt: A web dashboard for monitoring realtime data in an industrial plant.
image: '/images/gzr.png'
---

GZR is a dashboard for monitoring realtime data in an industrial plant. 

Scheduled background tasks, managed by the [Celery](http://www.celeryproject.org/) distributed task queue, collect data available in a [Modbus](https://en.wikipedia.org/wiki/Modbus) network and store the information on [InfluxDB](https://www.influxdata.com/), a high performance time-series database. 

Data are plotted using [Plotly](https://plot.ly/), Bokeh or directly inside a [Grafana](https://grafana.com/) dashboard.
