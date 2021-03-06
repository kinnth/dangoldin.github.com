---
layout: post
title: "Eighteen months of Django"
description: "Some best practices I've picked up after working on Django products over the past 18 months"
keywords: "Django, hacking, coding, web development, startups"
category:
tags: []
---
{% include JB/setup %}
I’ve discovered that every new project lets me correct mistakes from my earlier attempts by allowing me to start from scratch. This is especially true with a web framework such as Django that has a ton of little nooks and crannies that take a while to explore and understand. It’s usually not worth it to go back and fix something that’s not broken on a functional product but starting a new project lets me do it right from the beginning. Now that I’ve developed and launched (with <a href="http://www.sandylin.com/" target="_blank">Sandy</a> and <a href="http://marcschaffnergurney.com/" target="_blank">Marc</a>) two serious Django-based products as well as bunch of smaller ones, I wanted to document some personal best practices I’ve picked up. Obviously, I'm still learning and I may be completely wrong with them so let me know if you disagree. If you’re interested in a deeper look at some of the topics let me know and I can write up another post going into detail about a particular topic.

<ul class="bulleted">
    <li>Use <a href="https://pypi.python.org/pypi/virtualenv" target="_blank">virtualenv</a>: Virtualenv lets you create a virtual environment for each project you’re working on with its own version of Python and its own libraries. I’ve also created alias commands for my major projects that make moving to and activating the virtualenv of that project a single command. Note that using a virtualenv does make a few things more difficult (such as installing <a href="http://mdshaonimran.wordpress.com/2012/01/14/virtualenv-environmenterror-mysql_config-not-found/" target="_blank">MySQL-python</a>, setting up nginx, configuring <a href="http://stackoverflow.com/questions/1180411/activate-a-virtualenv-via-fabric-as-deploy-user/5359988#5359988" target="_blank">fabric</a>, getting supervisor running) but they’re all surmountable via Stackoverflow and Google.</li>

    <li>Use <a href="http://south.aeracode.org/" target="_blank">South</a>: A simpler way of handling database migrations in Django. It’s natural to be updating your database models as the app grows and South makes the migration a little bit easier. It’s not perfect and every once in a while I’ll need to revert some of the migrations and craft them by hand but it’s still better than the alternative.</li>

    <li>Use <a href="http://docs.fabfile.org/en/1.6/" target="_blank">Fabric</a>: Fabric gives you the ability to set up your own set of commands that can interact with a remote server. This lets you do git pulls, deployments, and run any other command on a server without needing to manually SSH. This becomes especially useful when you have your app served by multiple machines with each one having a different role.</li>

    <li>Use <a href="http://supervisord.org/" target="_blank">Supervisor</a>: Supervisor monitors the running processes and can restart any that go down.</li>

    <li>Nginx/Gunicorn vs Apache: I’ve used both and don’t have strong feelings about either one. I think there’s more information online about getting Apache running but I’ve found Nginx/Gunicorn a bit easier to configure and debug. The other benefits I’ve gotten from Nginx/Gunciron is that it’s less memory intensive out of the box than Apache and I was able to get it to play nicely with Supervisor. In full disclosure, I haven’t really tried to do the same with Apache and it may very well be possible.</li>

    <li>Use S3 for static files: Hosting your static files as well as user-uploaded files on S3 is a nice win. You don’t have to worry about serving static content and you can also move the static elements away from your web server. Another benefit I’ve found is that once you move to multiple web servers, it’s nice having all static content on a 3rd party on S3 since that allows all web servers to remain stateless and insync. Otherwise you have to worry about a user uploading a file to one web server and then having to copy it over to the other one to make it accessible.</li>

    <li>MySQL/PostgreSQL vs RDS: Unless you plan on monetizing immediately, I suggest using MySQL/PostgreSQL. RDS ends up getting pretty expensive and configuring it isn’t as straightforward as modifying a local installation of MySQL or PostgreSQL. If you end up running into scaling issues you can make the move to RDS relatively easily (especially with MySQL) by dumping and reimporting your database and updating your production settings file.</li>

    <li>On Django packages: Install new packages using pip instead of just downloading them into your project folder unless you know you’ll be modifying them. Even then, the well written packages let you customize their behavior by writing your own views, templates, and middleware that can exist outside the installed package. This will keep your project much simpler and better organized, and will force you focus on your app rather than trying to hack someone else’s.</li>
</ul>

After writing this, I realize I need do another post about the Django packages I’ve found to be useful. I'll put that together in a future post.

Edit: Here's the <a href="http://dangoldin.com/2013/05/10/eighteen-months-of-django-part-2/">follow-up post</a> where I cover useful packages.