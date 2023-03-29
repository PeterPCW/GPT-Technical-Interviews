<details>
  <summary>Describe the use of CircleCI in a Continuous Integration workflow. How does CircleCI differ from Jenkins, and what are some advantages of using CircleCI? Explain how to set up a CircleCI pipeline to build and test a Node.js application stored in a GitHub repository. Provide an example of a configuration file for the CircleCI pipeline.</summary>
  
  Compared to Jenkins, CircleCI is a more lightweight and cloud-native solution that requires less maintenance and setup time. CircleCI provides a user-friendly interface and a simple YAML configuration file that can be used to define and customize the build process. It also offers a free tier for open-source projects, making it a popular choice for small and mid-sized teams.

  To set up a CircleCI pipeline to build and test a Node.js application stored in a GitHub repository, follow these steps:

  1. Sign up for CircleCI: Sign up for a CircleCI account and connect it to your GitHub account.
  2. Create a configuration file: Create a .circleci/config.yml file in your Node.js application repository. This file will define the build process and any necessary dependencies.
  3. Define the build steps: In the configuration file, define the steps required to build, test, and deploy your Node.js application. This may include installing dependencies, running tests, and creating a production-ready build.
  4. Add CircleCI to your GitHub repository: Add a CircleCI configuration to your GitHub repository by clicking on the "Add a configuration" button in the CircleCI dashboard.
  5. Trigger the CircleCI build: Once the configuration is added to your repository, any changes pushed to the repository will automatically trigger a build on CircleCI. The build status and results can be viewed in the CircleCI dashboard or in the GitHub pull request.

  Here's an example of a CircleCI configuration file for a Node.js application:

  ```yaml
  version: 2
  jobs:
    build:
      docker:
        - image: circleci/node:latest
      steps:
        - checkout
        - run: npm install
        - run: npm test
        - run: npm run build
    deploy:
      docker:
        - image: dockerhub/my-app:latest
      steps:
        - checkout
        - run: docker build -t my-app .
        - run: docker push dockerhub/my-app:latest
  workflows:
    version: 2
    build-and-deploy:
      jobs:
        - build:
            filters:
              branches:
                only: master
        - deploy:
            requires:
              - build
            filters:
              branches:
                only: master
  ```

  This configuration file defines two jobs: build and deploy. The build job uses the official CircleCI Node.js Docker image to install dependencies, run tests, and build the application. The deploy job builds a Docker image of the application and pushes it to a Docker Hub repository. The workflows section defines a build-and-deploy workflow that runs the build job on the master branch and triggers the deploy job if the build succeeds.
</details>