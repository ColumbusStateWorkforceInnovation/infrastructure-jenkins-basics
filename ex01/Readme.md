For this exercise, we will set up your dockerized Jenkins and explore the Jenkins UI.

* To begin, clone your private repo to your VM if you haven't already.
* From the root of that repository, run `./build-jenkins-image.sh`.  This will create an image built on the base cscc Jenkins docker image which contains the plugins you will need for this lab and the next lab.

**Question 1 (Provide your answers in ex01Questions.txt)**:

Review the Dockerfile in your repo at docker/Dockerfile and related files in the docker directory.  In particular, review the plugins we have installed into this image.  What plugins / applications do you recognize?  Which ones do you think would be most useful when setting up a CI/CD pipeline?

**Question 2**:

What do you think are some advantages of running a dockerized Jenkins?  How would it differ to install Jenkins directly on a host?

* Now, run `./run-jenkins.sh`.  
* Leave your terminal running and open a browser.
* Navigate to `localhost:8080` to view Jenkins.  This is the screen you would use to manage Jenkins at a platform level.  We will explore this area briefly before building our first job.
* Clicking "People" will show you all users on the Jenkins system. 
* Clicking "Build History" will show all the jobs that have been run on this system (since you haven't run any jobs yet, this will be empty)
* Click on "Manage Jenkins".  You'll likely see deprecation or vulnerability warnings, don't worry about those in the educational environment.
* Here, you can browse the administrator-level settings and configurations available in Jenkins.
* You can navigate back to the Jenkins home screen by navigating back to `localhost:8080` or by clicking the Jenkins logo in the upper left corner.
* One more thing before we get to building, click on the ? icon in the search bar in the upper left of the screen.  This will take you to Jenkins documentation (jenkins.io).  This is a convenient way to access the documentation as you are using the tool

**Question 3**:

Based on the behavior you have seen exploring Jenkins' administrative console and your experience setting up Jenkins on your VM, what are two ways to add a plugin to your Jenkins instance?
