# Django, uWSGI and Nginx in a container, using Supervisord

# Docker config for your app

Copy `Dockerfile` and `Docker` directory to you django project

change: Docker/uwsgi.ini #29 to your application project name 



Forked: https://github.com/dockerfiles/django-uwsgi-nginx

This Dockerfile shows you *how* to build a Docker container with a fairly standard
and speedy setup for Django with uWSGI and Nginx.

uWSGI from a number of benchmarks has shown to be the fastest server 
for python applications and allows lots of flexibility. But note that we have
not done any form of optimalization on this package. Modify it to your needs.

Nginx has become the standard for serving up web applications and has the 
additional benefit that it can talk to uWSGI using the uWSGI protocol, further
eliminating overhead. 

Most of this setup comes from the excellent tutorial on 
https://uwsgi.readthedocs.org/en/latest/tutorials/Django_and_nginx.html

The best way to use this repository is as an example. Clone the repository to 
a location of your liking, and start adding your files / change the configuration 
as needed. Once you're really into making your project you'll notice you've 
touched most files here.

### Build and run
#### Build with python3
* `docker build -t webapp .`
* `docker run -d -p 80:80 webapp`
* go to 127.0.0.1 to see if works

#### Build with python2
* `docker build -f Dockerfile-py2 -t webapp .`
* `docker run -d -p 80:80 webapp`
* go to 127.0.0.1 to see if works

### How to insert your application

In /app currently a django project is created with startproject. You will
probably want to replace the content of /app with the root of your django
project. Then also remove the line of django-app startproject from the 
Dockerfile

uWSGI chdirs to /app so in uwsgi.ini you will need to make sure the python path
to the wsgi.py file is relative to that.
