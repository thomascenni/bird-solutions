---
title: 'Data analysis with Pandas, WebDataRocks and Datapane'
date: '2019-11-19'
draft: false
layout: page-anfavea
slug: 'data-analysis-with-pandas-webdatarocks-and-datapane'
tags: ["pandas", "webdatarocks", "python", "datapane"]
categories: ["Data Analysis"]
excerpt: Pandas is a great tool to clean and manipulate data. This post shows an example of data analysis and visualization, applied to the Brazilian automotive market.
image: '/images/anfavea.jpg'
---

[Anfavea](http://www.anfavea.com.br/), the National Association of Motor Vehicle Manufacturers in Brazil, discloses in its website some monthly statistics about the production and licensing of vehicles.
People in the automotive market who use these [data](http://www.anfavea.com.br/estatisticas) for analysis and forecast, certainly encountered various limitations in the Excel file available for download: the report looks like a PowerPoint presentation, with indented cells, values referring to subtotals in the middle of the lines and other small details that make it very difficult to use.

![](/images/anfavea-excel.png)

To make these data useful, I developed a Python script with a powerful analysis tool called [Pandas](https://pandas.pydata.org/), in order to be able to clean the original data and create a new Excel or CSV file.
Here's the code:

![](/images/anfavea-python-code.png)

OK, so now we have a new Excel file; but what about the possibility to create a Pivot Table and render it on this webpage to be able to filter the data, for example by segments (cars or trucks) ?

No problem, we can use the Free Web Pivot Table Tool from [WebDataRocks](https://www.webdatarocks.com/) to visualize it.

The code is very simple: as explained in their [quick start guide](https://www.webdatarocks.com/doc/how-to-start-online-reporting/), we define some Javascript to parse the data from the CSV file that we generated with our Python code and we render it.

```
<link href="https://cdn.webdatarocks.com/latest/webdatarocks.min.css" rel="stylesheet"/>
<script src="https://cdn.webdatarocks.com/latest/webdatarocks.toolbar.min.js"></script>
<script src="https://cdn.webdatarocks.com/latest/webdatarocks.js"></script>
<script>
var pivot = new WebDataRocks({
	container: "#wdr-component",
	toolbar: true,
    report: "/images/anfavea-report.json"
});
</script>
```

This is the result:

<div id="wdr-component"></div>
<br/>
And what about the possibility to execute the script, obtain some reports and be able to interact ?
For example setting the year of the analysis.

I discovered that there is a tool that allows you to do that: [Datapane](https://datapane.com/)

The report is available here:

<iframe src="https://datapane.com/thomas7/reports/anfavea-data-analysis_862f27f7/embed/" width="100%" height="540px" frameBorder="0">Iframe not supported.</iframe>

If you want to play with the script, it is public and available [here](https://datapane.com/thomas7/scripts/anfavea_data_analysis/).

Enjoy!

Thomas Cenni