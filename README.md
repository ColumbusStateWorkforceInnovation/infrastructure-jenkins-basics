# Infrastructure Automation Continuous Integration Lab

## Introduction: 

In this lab, you will learn to use an industry-standard pipeline tool, Jenkins, to build a simple pipeline job.  

## Objective

For this lab, you will gain hands-on experience using a dockerized Jenkins instance to pull down source code and execute jobs.

## Getting Started:

1. Copy the starter code from here into a new, private repository in your personal GitHub account. When adding a collaborator, be sure to add me ("cscc-luke-rouker").

## Understanding the Starter Code
This repository contains all you need to have a fully dockerized Jenkins and 
configurable plugins.  Note that you shouldn't have to change the plugins that are 
provided.  However, it's helpful to walk through what is provided, to better understand what you need to do.

#### Building a Custom Jenkins Image
You are provided with a [build-jenkins-image.sh](build-jenkins-image.sh) script that will build the `edu.cscc.special-topics/jenkins:latest` Jenkins image.  You can look under [docker/](docker/) to see how the image is built:
1. There is, of course, a [Dockerfile](docker/Dockerfile).  Note that it uses jenkins/jenkins:lts as its base image. 
1. The `Dockerfile` references [init.groovy.d](docker/init.groovy.d).  Scripts in this directory in Jenkins home will be automatically executed during Jenkins startup.  There are 2 scripts in here; two of them install Tools that Jenkins can use (Java and Maven). 
1. The `Dockerfile` references [plugins.txt](docker/plugins.txt).  This text file simply contains a list of Jenkins plugins we wish to be installed as part of the image build.  Note that there are more plugins listed than necessary, but it gives you a flavor of the rich plugin environment that Jenkins supports.

#### The Provided Source Code
The source code for this project in ex03 is incredibly simple.  It's a simple shell script that you will invoke from your freshly built Jenkins job.  We will see more about how Jenkins can be configured as code and how we can build more complex projects in next week's lab.

## Completing the Assignment
See [ex01/Readme.md](ex01/Readme.md) and [ex02/Readme.md](ex02/Readme.md) for details on completing the assignment.  In each repo, there will be activities for you to perform in Jenkins and a worksheet to complete to demonstrate that you have performed the appropriate activities.  There will be no code changes for this lab since we will be working primarily with the UI to gain basic understanding of Jenkins, but you should still submit your worksheet answers as a pull request in GitHub.

## Hints
1. We've created a [run-jenkins.sh](run-jenkins.sh) script that will start up jenkins for you if you'd like.  You can view your Jenkins by opening Firefox within your workspace to [http://localhost:8080](http://localhost:8080).
1. If your repository is marked private, you might get an error when running your `build-pipeline` job.  To fix this, you can [add your credentials](https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials) and select them in the SCM section of the  [build-pipeline job configuration](http://localhost:8080/job/build-pipeline-job/configure).

## Submitting Your Work

1. Publish your repository as a private repo, and ensure that you have pushed the latest version
1. Submit the assignment in Blackboard with the link to your repo
