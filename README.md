This is branch of repo miguelgrinberg/microblog for DevOps Exercise [CI/D].

Here you can find short description of CI/D flow of my progect.

I've installed Jenkins on ec2 in aws cloud, you can have access to this node by:

http://34.205.45.100:8080
Credentials in email.

Jobs in Jenkins:

1. Build

Project Triggers:

Poll SCM with schedule (*****) each second from repo url: https://github.com/beylistan/microblog.git on branch master.

Project Build:

If changes has been commited to master branch of this repo then Poll SCM is pulling new master branch and 
execute shell steps of build project.
In 1st shell step generating Docker file in workspace directory and in next execute shell step building image based on Dockerfile in current directory, runing unit tests on instance of built image.
If tests passed (based on exit code of tests.py script) then create tag latest for current build.
Login to docker hub, push images to docker hub, remove images from workspace.

2. CI

Project Triggers:

I've installed plug-in:
Monitor Docker Hub/Registry for image changes
Any referenced Docker image can trigger this job
Specified repositories will trigger this job: beylistan/microblog_rc.
In Docker Hub specified web hook: 
microblog_rc_webhook
http://34.200.250.184:8080/dockerhub-webhook/notify

Project Build:

Build execute shell:
Login to docker hub, pull and run last succesfully built container on localhost with port 5000.
After simple check that continer is giving response to web request (curl localhost:5000)
Change tag from beylistan/microblog_rc to beylistan/microblog and push to dockerhub. 
Cleanup: stop running container, remove stoped container, remove image of stoped container 

Post-build Actions:
Projects to build: configure-opsworks(CD)

3. configure-opsworks(CD)

#aws opsworks provide ability to use CM: Cheif-solo on deploy step from scratch. 

Get stack id and check it on existance if not exist create and run stack.
On this step in json specifying nessesary data for creating stack note that here you can find url to github:
https://github.com/beylistan/chef_repo.git
From where opsworks is getting cheif recipies and template for creating service on deploy step. 

Get layer id and check it on existance if not exist create.
Get INSTANCEID and check it on existance if not exist create.

Post-build Actions:
Projects to build: microblog_deploy.

4. microblog_deploy

Build Environment:
SSH Agent: Specific credentials based on Specific credentials: Jenkins Credentials Provider: Jenkins

Get stackid, destenation deploy instance ip, deployment id.
Wait untill opsworks complete deployment.
Run chef-solo with recipe microblog on destenation deploy host

Thats it.

Following descreption from master branch of miguelgrinberg/microblog regarding application what we are running.

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

