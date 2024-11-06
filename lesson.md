## Brief

### Preparation

Installations:
- [Node (LTS)](https://nodejs.org/en/)

### Lesson Overview

We will be covering only the CI part of the entire CICD Pipeline in this lesson. This lesson, learners will learn how to run CI jobs (build, test, publish) in containerized environment within CircleCI (the platform for CICD Pipeline). This lesson sets up a basic platform for the remaining lessons: CD, Secret Management, and Vulnerability Scan. Learners must go through this lesson before progressing to the next.

---

## Part 1 - Intro Continuous Integration

### What is Continuous Integration (CI)

> Continuous Integration is a software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible. Many teams find that this approach leads to significantly reduced integration problems and allows a team to develop cohesive software more rapidly. - *Excerpt from [Martin Fowler](https://martinfowler.com/articles/continuousIntegration.html)*

There are typically three jobs in the CI Pipeline (can be more based on the organizations' needs):
1. Build / compile
1. Run tests
1. Upon tests passed, publish container image to registry and proceed to the CD Pipeline

> Clone this [repository](https://github.com/su-ntu-ctp/6m-software-m4-node-app-for-devops) as we will be using this code base for the remaining lessons in this module.

Follow the command below to:
1. Install dependencies / build / compile
2. Run tests

### Install Dependencies

On your Terminal, change directory to the node application. Run the following command to install dependencies.

```sh
npm install
```

In this case, JavaScript do not need to be compiled. Hence, there is no compilation step. Installing the dependencies is considered part of preparing the application to run.

Examples of programming languages that require compilation:
- Java
- C++
- C#
- Golang

### Run Tests

Tests that are executed in the CI Pipeline must be self contained. For example, if you are tests depend on a running database, the tests might fail if the database is down. If your tests depends on a set of database, making it self contained can possibly means that you might need to setup a database container to go along with the tests.

There are different types of tests that can be executed at this stage, such as:
- **unit tests** - test against small unit of code
- **end to end tests (e2e)** - test if a user stories is functioning properly
- **integration tests** - test a module against another 
- **performance tests** 
- **security tests** - there are multiple layers of security to tests for

Only a simple unit test is included for this repository. Run the follow command:

```sh
npm test
```

To fail the test, you can make changes to this [file](https://github.com/su-ntu-ctp/6m-software-m4-node-app-for-devops/blob/main/controller.test.js) and run `npm test` again.

### Run the Application

To see the actual output of the application, you may run the following command and go to `http://localhost:3000` on your browser.

```sh
npm start
```

> We have run the application directly using the node cli for now. We will attempt to run these commands on CircleCI using docker.

---

## Part 2 - Prepare accounts and setup

You are required to setup the following items before proceeding to Part 3.

- Ensure you already have an account with Docker Hub
- Ensure you have installed Docker locally
- Create a new account at [circleci.com](https://circleci.com/) using github

---

## Part 3 - CI Configuration

We will break down what needs to be done in the three jobs listed in Part 1.

### The Build Job

In this job, we would:
1. Define a node image container (use node v16)
1. Check out code 
1. Install the `npm` 
1. Run `npm install`


### The Test Job

In this job, we would continue from the previous `build job`. 
1. Ensure the build job is successful and then,
1. Define a node image container (use node v16)
1. Check out code 
1. Install the `npm` 
1. Run `npm test`


### The Publish Job

In this job, we would continue from the previous `test job`.
1. Ensure the test job is successful, then,
1. Define a node image container (use node v16)
1. Check out code 
1. Run `docker build`
1. Run `docker push`

### Setup Circle CI

Step 1: Fork [this repository](https://github.com/su-ntu-ctp/6m-software-m4-node-app-for-devops) to your personal github account

Step 2: Sign in / sign up CircleCI.com using your github account and link the forked repository to CircleCI

Step 3: In the forked repository, create `./.circleci/config.yml` and fill in the file with the following:

```yml
version: 2.1
orbs:
  node: circleci/node@5.0.1
jobs:
  build:
    docker:
      - image: cimg/node:16.10
    steps:
      - checkout
      - node/install-packages:
          pkg-manager: npm
      - run: |
          echo Installing dependencies...”
          npm install
  test:
    docker:
      - image: cimg/node:16.10
    steps:
      - checkout
      - node/install-packages:
          pkg-manager: npm
      - run: |
          echo “Running tests...”
          npm run test

workflows:
  simple_workflow:
    jobs:
      - build
      - test:
          requires:
            - build
```
Step 4: Make any changes to the repository to re-trigger the `simple_workflow`. 

### Assignment

This CI Pipeline config file will execute build and test job. In your assignment, you are to work on the `publish` job where you will push a docker image to docker hub repository. Refer to [Lesson 4.3](https://github.com/su-ntu-ctp/6m-software-4.3-container-2) on the commands used to push docker image to registry.

END
