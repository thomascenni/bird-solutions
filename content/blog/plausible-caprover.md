---
title: 'Self hosting Plausible with Caprover'
date: '2020-10-30'
draft: false
slug: 'self-hosting-plausible-with-caprover'
tags: ["Plausible", "Caprover", "Docker"]
categories: ["DevOps"]
excerpt: Plausible Analytics is a simple, open-source, lightweight and privacy-friendly web analytics alternative to Google Analytics.
image: '/images/plausible-caprover.png'
---
<a href="https://plausible.io/" target="_blank">Plausible</a> Analytics is a simple and privacy-friendly alternative to Google Analytics.

They offer a free Plausible Analytics Self-Hosted, that it’s exactly the same code as their Cloud solution, but you have to install on your server.

As a fan of Caprover, that I use everyday to manage my web applications, and 
after looking at the official Caprover repository for one-click apps, I found that Plausible was missing, so I decided to write the template.

On their [offical repository](https://github.com/plausible/hosting) there is a docker-compose.yml file that I used as starting point to transform in a one-click Caprover app, but the GeoLite2 database created by MaxMind for enriching analytics data with visitor countries was not present.

So, looking at the [self hosted guides](https://docs.plausible.io/plausible-analytics-self-hosted-guides) available in their documentation, I came up with a plausible.yml template:

<script src="https://gist.github.com/thomascenni/cd25a5df48935f2d8567b01e6ae6f2ba.js"></script>

Simply copy and paste this template in Caprover, and you'll have a Plausible.io self hosted analytics application up and running!

Then follow their guide to add your first website, and copy the automatically generated snippet in the `<head>` tag of your website.

<strong>EDIT</strong>: as if today, 31/10/2020, the Plausible template is already available in the official repository.
Simply search for the app inside Caprover installation:

![](/images/caprover-plausible-select.png)

Select an app name - can be plausible 😀 - and deploy!

![](/images/caprover-plausible-deploy.png)
