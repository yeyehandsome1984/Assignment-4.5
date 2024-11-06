## Assignment

### Brief

Continue this assignment from where the lesson is left off. You are to configure a job to publish docker image to the docker hub registry only when the test job passed.

Hint:
- Use Docker Orbs
- Setup environmental variables that contain username and password that login to docker hub

### Answer

Setup Environment Variable:
- DOCKER_LOGIN
- DOCKER_PASSWORD

```yml
version: 2.1
orbs:
  node: circleci/node@5.0.1
  docker: circleci/docker@2.1.4
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
  
  build-and-push:
    executor: docker/docker
    steps:
      - setup_remote_docker
      - checkout
      - docker/check
      - docker/build:
          image: edisonzsq/education-space
      - docker/push:
          image: edisonzsq/education-space

workflows:
  simple_workflow:
    jobs:
      - build
      - test:
          requires:
            - build
      - build-and-push:
          requires:
            - test

```

### Submission 

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL. 

### References

_Example of Referencing Classmate_

Referenced the code block below from Terence.
```js
    function printMe(){
        console.log("I am a reference example");
    }
```

_Example of Referencing Online Resources_

- https://developer.mozilla.org/en-US/
- https://www.w3schools.com/html/
- https://stackoverflow.com/questions/14494747/how-to-add-images-to-readme-md-on-github

