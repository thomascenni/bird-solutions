---
title: 'Deploy a Django application with Caprover'
date: '2019-12-12'
draft: false
slug: 'deploy-a-django-application-with-caprover'
tags: ["django", "caprover", "docker"]
categories: ["DevOps"]
excerpt: Caprover is an open source tool which allows you to build your own PaaS server to host any type of applications and databases (NodeJS, Python, PHP, ASP.NET, Ruby, MySQL, MongoDB, Postgres, etc).
image: '/images/django-caprover.png'
---
<a href="https://caprover.com/" target="_blank">Caprover</a> is an open source tool which allows you to build your own PaaS server to host any type of applications and databases (NodeJS, Python, PHP, ASP.NET, Ruby, MySQL, MongoDB, Postgres, etc). Compared to commercial applications, like Heroku, it is a great and cheaper alternative.

Today I’ll show you how to create a Django application and deploy to Caprover step by step. I assume that:

* you have a Python virtual environment with Django installed (pip install django).

* you have a Caprover server already installed. You can follow their <a href="https://caprover.com/docs/get-started.html" target="_blank">getting started</a> guide, the installation is very easy.

OK so let’s start. We create a project called **djcap**; you can use the name of the project that you want, but for this tutorial is important to remember this name because it will be used later.

    cd ~/Desktop
    django-admin startproject djcap
    cd djcap
    python manage.py runserver

This is the basic starting point of creating a Django project. If everything works fine, pointing the browser to <a href="http://localhost:8000/" target="_blank">localhost:8000</a> you’ll see a nice webpage:

![](/images/django-caprover-localhost.png)

Nothing special until now. Remember to create a requirements.txt file to contain all your modules:

    pip freeze > requirements.txt

It’s time to prepare the application for Caprover. According to the documentation, Caprover needs a Captain Definition File at the root of your project.

Create a file called captain-definition with the following contents:

<script src="https://gist.github.com/thomascenni/d4be3dd2597639c59b124b90e630ef36.js"></script>

This basically tells Caprover to look for a Dockerfile in the root of your project. Now we will use a Docker image with python and nginx to run our web application. There are a lot of alternatives, but I came across this image

<a href="https://github.com/tiangolo/meinheld-gunicorn-docker" target="_blank">tiangolo/meinheld-gunicorn-docker</a>

which totally satisfies my needs and is very limited in size because it uses Alpine Linux, a security-oriented and lightweight Linux distribution.

So these will be the contents of our Dockerfile:

<script src="https://gist.github.com/thomascenni/e9180cd64b157907bd07292948baa712.js"></script>

The Docker engine will pull the image, create some directories for the application path and some other static files, set the correctly working directory inside the container, copy all your Django project inside it and then install all the python modules needed by your application to run correctly.

Following the documentation found in the [repository of the custom docker image](https://github.com/tiangolo/meinheld-gunicorn-docker#custom-appprestartsh), we can create a prestart.sh file that will be called before the starting of the application:

<script src="https://gist.github.com/thomascenni/c6897a198839d506e5610d405079f8a4.js"></script>

With this shell script, we are running the Django [collectstatic](https://docs.djangoproject.com/en/3.0/ref/contrib/staticfiles/#collectstatic) command to copy the static files to our STATIC_ROOT and then the [migrate](https://docs.djangoproject.com/en/3.0/ref/django-admin/#django-admin-migrate) command to make database migrations. For everything working fine, our settings.py file in the djcap project needs to contain the following lines:

    STATIC_ROOT = '/usr/src/static'
    STATIC_URL = '/static/'

OK, now we will create a new application within our Caprover installation and deploy the djcap project.

From the “Apps” menu, click on the button “One-Click Apps/Databases”:

![](/images/django-caprover-new-app.png)


Then from the “One-Click Apps List”, select **>> TEMPLATE <<**. 
This list is used if you want to install some already tested and available apps, or, like in our case, if we want to install a custom application. A text box will open, and inside it copy and paste the contents of this file:

<script src="https://gist.github.com/thomascenni/8cfcea7977c7e1a0e4fff188c40525a8.js"></script>

Click “Next”.
In the following page, the only field you need to fill is the “App Name”; chose the name you want, but remember that the field “MODULE_NAME” needs to be coherent with the name of the project you choose when you created the Django project (in my case, “djcap.wsgi”).

![](/images/django-caprover-new-app-config.png)

Click the button Deploy. Basically an empty application based on Alpine 3.8 is created with the correct Environmental Variables needed to run it.

![](/images/django-caprover-new-app-environment.png)


Good. At this time, no application is running. It is time to deploy the real Django application.

Following the [Caprover deployment guide](https://caprover.com/docs/deployment-methods.html), the simplest and best way is to deploy via command line. Follow the instructions to install the CLI:

    npm install -g caprover

Then, always in the root directory of the Django project, run the command:

    caprover deploy

You’ll be asked to enter the URL of the Caprover server, the name of the application and a password. Once the image is built, you can follow the application logs and see that gunicorn is running and serving your Django app.

![](/images/django-caprover-new-app-deployed.png)

You’ll find the project in my Github repository: <a href="https://github.com/thomascenni/djcap" target="_blank">thomascenni/djcap</a>.

N.B.: for production, remember to set Debug=False and SECRET_KEY as an additional environment variable. For correct static files serving, you should use <a href="http://whitenoise.evans.io/en/stable/django.html" target="_blank">WhiteNoise</a>.
