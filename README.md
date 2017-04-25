This is branch of repo miguelgrinberg/microblog for DevOps Exercise [CI/D].

Here you can find short description of CI/D flow of my progect.

I've installed Jenkins on ec2 in aws cloud, you can have access to this node by:

http://34.200.250.184:8080
Credentials in email.

Projects in Jenkins:

1. Build
Project trigger:
Poll SCM with schedule (*****) each second from repo url: https://github.com/beylistan/microblog.git on branch master.
If changes has been commited to master branch of this repo then Poll SCM is pulling new master branch and 
execute shell steps of build project.
In 1st shell step generating Docker file in workspace directory and in next execute shell step building image based on Dockerfile in current directory, runing unit tests on instance of built image.
If tests passed (based on exit code of tests.py script) then create tag latest for current build.
Login to docker hub, push images to docker hub, remove images from workspace.

2. CI
Project trigger:


microblog
=========

A decently featured microblogging web application written in Python and Flask that I'm developing in my Flask Mega-Tutorial series that begins [here](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).

Installation
------------

The tutorial referenced above explains how to setup a virtual environment with all the required modules.
 
The sqlite database must also be created before the application can run, and the `db_create.py` script takes care of that. See the [Database tutorial](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database) for the details.

Running
-------

To run the application in the development web server just execute `run.py` with the Python interpreter from the flask virtual environment.

