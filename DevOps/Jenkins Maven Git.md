# What is Jenkins and how is it used for Continuous Integration? Describe the architecture of a Jenkins system, including the roles of the Jenkins Master and Jenkins Slave nodes. Explain how to configure Jenkins to build, test, and deploy a simple Java application using Maven and Git.

Jenkins is an open-source automation server used for Continuous Integration and Continuous Delivery. It allows developers to build, test, and deploy code changes automatically and continuously, making it easier to catch and fix bugs early in the development process.

Jenkins architecture consists of a master node and one or more slave nodes. The Jenkins master node is responsible for managing the automation pipeline and distributing tasks to the slave nodes for execution. The Jenkins slave nodes are worker machines that execute tasks assigned by the master node.

To configure Jenkins for building, testing, and deploying a simple Java application using Maven and Git, follow these steps:

1. Install and configure Jenkins: Download and install Jenkins on a server or cloud instance. Once installed, configure Jenkins with the necessary plugins, security settings, and integrations.

2. Create a new Jenkins job: Create a new Jenkins job that will handle building, testing, and deploying your Java application. In the Jenkins job configuration, specify the source code repository (e.g., Git), the Maven build settings, and the target environment for deployment.

3. Configure Git: Connect your Git repository to Jenkins so that Jenkins can automatically trigger builds when changes are pushed to the repository.

4. Configure Maven: Configure Jenkins to use Maven for building your Java application. Specify the Maven build settings in the Jenkins job configuration, including the location of the pom.xml file and any necessary build arguments.

5. Configure testing: Configure Jenkins to run automated tests as part of the build process. Specify the location of the test suites and any necessary testing frameworks in the Jenkins job configuration.

6. Configure deployment: Once the build and test process is complete, configure Jenkins to deploy the Java application to the target environment. Specify the deployment target and any necessary deployment scripts or configuration files in the Jenkins job configuration.

7. Run the Jenkins job: Once the Jenkins job is configured, trigger a build to test the configuration. If the build is successful, the Java application will be deployed to the target environment.