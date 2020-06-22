# Meeting Eight (Babies First Pipeline)

## Questions That Will be Answered
* What is a pipeline?
* How do we build one?

## What is CI/CD
Continuous integration is a coding philosophy and set of practices that drive development teams to implement small changes and check in code to version control repositories frequently. Because most modern applications require developing code in different platforms and tools, the team needs a mechanism to integrate and validate its changes.

Continuous delivery is the automation that pushes applications to delivery environments. Most development teams typically have one or more development and testing environments where application changes are staged for testing and review. A CI/CD tool such as Jenkins, CircleCI, AWS CodeBuild, Azure DevOps, Atlassian Bamboo, or Travis CI is used to automate the steps and provide reporting.

## Set Up and GitLab Runner

GitLab Runner is the open source project that is used to run your jobs and send the results back to GitLab. It is used in conjunction with GitLab CI/CD, the open-source continuous integration service included with GitLab that coordinates the jobs.

GitLab Runner is written in Go and can be run as a single binary, no language specific requirements are needed.

It is designed to run on the GNU/Linux, macOS, and Windows operating systems. Other operating systems will probably work as long as you can compile a Go binary on them.

### Installation
Wherever this is installed is where your pipeline will run from. So make sure this server can access anything that it needs to to run the pipeline

    curl -LJO https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_<arch>.deb

    dpkg -i gitlab-runner_<arch>.deb


### Registration
Now we have to register the gitlab runner with the gitlab server

    sudo gitlab-runner register

    https://gitlab.com      Enter URL

    Please enter the gitlab-ci token for this runner <This can be found in settings under pipelines>

    Skip Tags for now

    Choose Shell

    sudo gitlab-runner restart

Now you are registered and ready to run your first pipeline

## Gitlab .gitlab-ci.yml
GitLab CI/CD pipelines are configured using a YAML file called .gitlab-ci.yml within each project.

The .gitlab-ci.yml file defines the structure and order of the pipelines and determines:


* What to execute using GitLab Runner.
* What decisions to make when specific conditions are encountered. For example, when a process succeeds or fails.

Examples:

```
# see https://docs.gitlab.com/ce/ci/yaml/README.html for all available options

# you can delete this line if you're not using Docker
image: busybox:latest

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build1:
  stage: build
  script:
    - echo "Do your build here"

test1:
  stage: test
  script:
    - echo "Do a test here"
    - echo "For example run a test suite"

test2:
  stage: test
  script:
    - echo "Do another parallel test here"
    - echo "For example run a lint test"

deploy1:
  stage: deploy
  script:
    - echo "Do your deploy here"
```
---
```
# Official language image. Look for the different tagged releases at:
# https://hub.docker.com/r/library/python/tags/
image: python:latest

# Change pip's cache directory to be inside the project directory since we can
# only cache local items.
variables:
  PIP_CACHE_DIR: "$CI_PROJECT_DIR/.cache/pip"

# Pip's cache doesn't store the python packages
# https://pip.pypa.io/en/stable/reference/pip_install/#caching
#
# If you want to also cache the installed packages, you have to install
# them in a virtualenv and cache it as well.
cache:
  paths:
    - .cache/pip
    - venv/

before_script:
  - python -V  # Print out python version for debugging
  - pip install virtualenv
  - virtualenv venv
  - source venv/bin/activate

test:
  script:
    - python setup.py test
    - pip install tox flake8  # you can also use tox
    - tox -e py36,flake8

run:
  script:
    - python setup.py bdist_wheel
    # an alternative approach is to install and run:
    - pip install dist/*
    # run the command here
  artifacts:
    paths:
      - dist/*.whl

pages:
  script:
    - pip install sphinx sphinx-rtd-theme
    - cd doc ; make html
    - mv build/html/ ../public/
  artifacts:
    paths:
      - public
  only:
    - master
```

## Exercise 1
* Create a pipeline 
* Make it have a before script that runs whoami, starts ssh agent, and echos ready
* Create 2 stages, start and cleanup
* Make it start a docker container
* Have it kill the container 

## Exercise 2 (Friday)
* Create a deployment process for your website
* This can be done in many of ways but think of all we learned with Ansible and Docker and use that to logically create something that Deploys you application








