# Infrastructure Automation Continuous Integration Lab

## Introduction: 

In this lab, you will learn to use two industry-standard pipeline tools, GitHub and Jenkins, to build a simple CI/CD pipeline.  Up to this point, you’ve probably encountered GitHub, but now we will practice using two different industry-standard workflows.  In Part A of the lab, you will demonstrate your knowledge of trunk-based development, and in Part B of the lab, you will demonstrate your knowledge of Git Flow. 

Your Git History will be graded.  Be sure to use the workflows as they are described, using your own workflow will cause point loss. 

## Objective

For this lab, you will gain hands-on experience using Jenkins for continuous integration and managing your Jenkins configuration
as code.  You will write pipeline jobs using the Groovy DSL and create those jobs in a "dockerized" Jenkins managed by a DSL seed job.

## Getting Started:

1. Copy the starter code from here into a new, private repository in your personal GitHub account. When adding a collaborator, be sure to add me ("cscc-luke-rouker").

## Understanding the Starter Code
This repository contains all you need to have a fully dockerized Jenkins that supports a DSL seed job and 
configurable plugins.  Note that you shouldn't have to change either the seed job provided or the plugins that are 
provided.  However, it's helpful to walk through what is provided, to better understand what you need to do.

#### Building a Custom Jenkins Image
You are provided with a [build-jenkins-image.sh](build-jenkins-image.sh) script that will build the `edu.cscc.special-topics/jenkins:latest` Jenkins image.  You can look under [docker/](docker/) to see how the image is built:
1. There is, of course, a [Dockerfile](docker/Dockerfile).  The `Dockerfile` has had lines removed from it. Note that it uses jenkins/jenkins:lts as its base image.  You need to follow the comments in the Dockerfile to fill it in.
1. The `Dockerfile` references [init.groovy.d](docker/init.groovy.d).  Scripts in this directory in Jenkins home will be automatically executed during Jenkins startup.  There are 3 scripts in here; two of them install Tools that Jenkins can use (Java and Maven).  The third is more important.  [startup.groovy](docker/init.groovy.d/startup.groovy) creates the [seed](docker/jobs/seed.groovy) job and runs it at startup.  This is great because you can go from a "naked" Jenkins to one with all of your jobs defined and ready to go without any manual intervention.  For more on seed jobs, see [the Jenkins Job DSL Tutorial](https://github.com/jenkinsci/job-dsl-plugin/wiki/Tutorial---Using-the-Jenkins-Job-DSL).
1. The `Dockerfile` references [plugins.txt](docker/plugins.txt).  This text file simply contains a list of Jenkins plugins we wish to be installed as part of the image build.  Note that there are more plugins listed than necessary, but it gives you a flavor of the rich plugin environment that Jenkins supports.
1. The [seed job](docker/jobs/seed.groovy) is set up to create jobs based on the files under the [dsl](docker/dsl) directory.  For your setup there is only one job, [build_pipeline.groovy](docker/dsl/build_pipeline.groovy) so this whole setup seems like overkill.  However, it is common for a given Jenkins instance to host 10s or 100s of jobs, and managing that many jobs through the Jenkins GUI is cumbersome and difficult.  Study the structure of `build_pipeline.groovy`.  Note that it creates a Pipeline job based on the instructions provided in the [Jenkinsfile](Jenkinsfile) from this repo.

#### The Maven Project
The source code for this project is quite simple.  It's pretty much the unit test example from the quality lab.  You won't need to change any of its code.  Note that if you want you can build it from the command line with a simple `mvn clean install`

## Completing the Assignment

### Part A:  Create a Dockerized Jenkins | Working with Trunk-Based Development: 

For this part of the lab, you will implement one “feature” - a Dockerfile for the repository you have checked out.  The repository already includes the majority of code you’ll need for this lab, but the Dockerfile is a skeleton.  

Fill in the Dockerfile as guided by the comments in the file.  When you have completed it, commit it to the repository as if you are working in a trunk-based development workflow.  (If you create a PR, be sure to merge it before starting part B). 

Tag the repository as 1.1 and push the tag before moving on to part B 

### Part B – Building a Jenkins job | Working with Git Flow (kinda): 

Start with the repository from Part A, with any outstanding branches merged to master.   

Create a develop branch from master and push it to your remote repository. 

Now, you’ll work in Git Flow to implement your next features (be sure to branch as appropriate for Git flow and merge your branches accordingly)

1. For this part you will modify the existing Jenkins configuration to have the pipeline job do 3 things:
    1. Checkout _your_ repository (push tag 1.2 when finished)
    1. Build the source code using a maven stage (push tag 1.3 when finished)
        1. To use maven in a pipeline you need to have access to the `mvn` command, which is provided through a Tool Configuration in Jenkins.  See [the snippet](#maven-pipeline-build-snippet) below for a sample snippet.
    1. Report the unit test results.  (push tag 1.4 when finished)
        1. See the [Jenkins tutorial](https://jenkins.io/doc/pipeline/tour/tests-and-artifacts/) on how to do this.  Note that the tutorial shows two versions of the pipeline syntax.  By default it shows the Declarative pipeline syntax (which uses `stages` and `post` syntax).  However we're using the scripted syntax.  There is a link to show the scripted syntax.  Use that syntax (i.e. no `stages` or `post` keywords).
1. To do so, you will need to:
    1. Change the [build_pipeline.groovy](docker/dsl/build_pipeline.groovy) to use your repository
    1. Modify your [Jenkinsfile](Jenkinsfile) appropriately. 
    1. To test your changes, you can rebuild the docker image running `./build-jenkins-image.sh` and then start your jenkins instance running `./run-jenkins-images.sh`.  Run your `build-pipeline-job` and verify it meets the requirements.
    
Note:  You WILL need to merge your changes into master before testing so that you don't need to change the branch configuration locally.

## Hints
1. We've created a [run-jenkins.sh](run-jenkins.sh) script that will start up jenkins for you if you'd like.  You can view your Jenkins by opening Firefox within your workspace to [http://localhost:8080](http://localhost:8080).
1. If your repository is marked private, you might get an error when running your `build-pipeline` job.  To fix this, you can [add your credentials](https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials) and select them in the SCM section of the  [build-pipeline job configuration](http://localhost:8080/job/build-pipeline-job/configure).
1. The [DSL API](https://jenkinsci.github.io/job-dsl-plugin/) is available online.
1. There is a [Pipeline Syntax](http://localhost:8080/job/build-pipeline-job/pipeline-syntax/) page to help you out too!.
1. Note the branch specification in [docker/dsl/build_pipeline.groovy](docker/dsl/build_pipeline.groovy).  You will likely not be working on the master branch.
1. To test your changes locally before pushing them to GitHub, you can use the path to your repo as your git URLs in your [docker/dsl/build_pipeline.groovy](docker/dsl/build_pipeline.groovy) and [Jenkinsfile](Jenkinsfile).  So instead of saying:
```
   def repo = 'https://github.com/jschmersal-cscc/special-topics-labs-ci'
```
you can say:
```
   def repo = '/path/to/your/copy/of/special-topics-labs-ci'
```
Note that you should change this to be your github repo url before finally pushing it to github for testing. 

## Submitting Your Work

1. Publish your repository as a private repo, and ensure that you have pushed the latest version (and the latest tag!)
1. Submit the assignment in Blackboard with the link to your repo, please be sure your repo is tagged
__NOTE: I will provide feedback via. comments in Blackboard.__
If you need to amend your work after you issue your initial pull request:

1. Commit your updates
1. Push your changes to gitHub
1. Update the tag to the next minor version (1.5, 1.6, etc)
1. Resubmit in Blackboard

## Maven Pipeline Build Snippet
Here is a snippet that will be useful to include in your pipeline.  The "build" stage should include something similar to this: 
```
        withMaven (maven: 'maven3') {
          sh "mvn package"
        }
```
The `withMaven` clause does the necessary setup to make the `mvn` executable that corresponds to the `maven3` tool installation
available to the `sh` command.  Without it the `sh` would complain that it couldn't find `mvn`.
