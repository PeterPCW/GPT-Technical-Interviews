<details>
  <summary>Explain how Docker containers work and how they differ from virtual machines. Provide an example of a Dockerfile and explain the different sections.</summary>
  
  Docker containers are a lightweight alternative to traditional virtual machines. They allow developers to package an application and all its dependencies into a single portable container that can be run on any system with Docker installed. Containers share the host operating system kernel, but are isolated from each other and from the host system through a combination of namespaces and cgroups.
  
  Compared to virtual machines, which emulate an entire operating system, Docker containers only include the application and its dependencies, resulting in a smaller footprint and faster startup times. Containers also offer greater flexibility and scalability, as they can be easily moved between different environments and can be scaled up or down to meet changing demand.

  A Dockerfile is a script used to build a Docker container image. It contains a set of instructions that tell Docker how to create the image, including the base image to use, any software packages to install, and how to configure the container. Here is an example of a Dockerfile:

  ```bash
  # Use an official Python runtime as a parent image
  FROM python:3.9-slim-buster

  # Set the working directory to /app
  WORKDIR /app

  # Copy the current directory contents into the container at /app
  COPY . /app

  # Install any needed packages specified in requirements.txt
  RUN pip install --trusted-host pypi.python.org -r requirements.txt

  # Make port 80 available to the world outside this container
  EXPOSE 80

  # Define environment variable
  ENV NAME World

  # Run app.py when the container launches
  CMD ["python", "app.py"]
  ```
  
  The different sections of the Dockerfile are:

  * `FROM`: Specifies the base image to use for the container.
  * `WORKDIR`: Sets the working directory for subsequent instructions.
  * `COPY`: Copies files from the local host to the container.
  * `RUN`: Runs a command to install software packages or perform other setup tasks.
  * `EXPOSE`: Specifies which ports to expose from the container to the host system.
  * `ENV`: Defines an environment variable.
  * `CMD`: Specifies the command to run when the container starts.
  
  By running `docker build` command, Docker reads the Dockerfile and builds an image. Once the image is built, it can be used to create containers that run the application.
</details>