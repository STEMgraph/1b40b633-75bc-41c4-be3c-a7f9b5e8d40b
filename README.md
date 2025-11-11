<!---
{
  "id": "1b40b633-75bc-41c4-be3c-a7f9b5e8d40b",
  "teaches": "Packaging a Python Application with Docker",
  "depends_on": [
    "24b25804-5bb5-443f-a78a-1bd485bebed8",
    "2d1d315d-bb92-48c0-b19f-19529a45e5ff"
  ],
  "author": "Stephan Bökelmann",
  "first_used": "2025-05-13",
  "keywords": ["docker", "python", "packaging", "requirements.txt"]
}
--->

# Packaging a Python Application with Docker

> In this exercise you will learn how to containerize a Python application using Docker. Furthermore we will explore how to manage dependencies using `requirements.txt` inside a Docker context.

## Introduction

Packaging applications in Docker containers has become a standard practice in software development and deployment. Containers provide a consistent environment, making it easier to deploy applications across different platforms. For Python developers, Docker not only ensures consistency in runtime environments but also simplifies the handling of dependencies and system libraries.

In this exercise, you'll start by creating a basic "Hello, World!" Python script and learn how to package it into a Docker image. You'll then extend the setup to include Python dependencies managed via a `requirements.txt` file. This represents the most widely supported and straightforward method in Python dependency management and serves as a reliable baseline for containerized Python projects.

Understanding this method will help you grasp the essential practices of Python packaging and how Docker integrates seamlessly into development workflows.

### Further Readings and Other Sources

* [Docker Official Docs on Python](https://docs.docker.com/language/python/)
* [Python Packaging Authority Guide](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
* [YouTube: Python Docker Tutorial](https://www.youtube.com/watch?v=GW0rj4sNH2w)

## Tasks

### Task 1: Create a Hello World and Package it in an Image

In this task, you will create a minimal Python application and package it inside a Docker image. This is your first step in containerization and helps ensure your script can run reliably on any system with Docker installed.

1. Create a new directory structure to organize your files:

   ```bash
   mkdir -p hello-docker/app
   cd hello-docker
   ```

2. Write a simple Python script in the `app` folder:

   ```python
   # ./app/hello.py
   print("Hello, World!")
   ```

   This script simply prints a greeting message and serves as the application entry point.

3. Create a `Dockerfile` in the root of the project directory:

   ```Dockerfile
   # ./Dockerfile
   FROM python:3.11-slim
   WORKDIR /app
   COPY ./app /app
   CMD ["python", "hello.py"]
   ```

   * `FROM python:3.11-slim` selects a minimal Python base image.
   * `WORKDIR /app` sets the working directory inside the container.
   * `COPY ./app /app` transfers your local Python code into the container.
   * `CMD [...]` defines the command to run when the container starts.

4. Build your Docker image using the Docker CLI:

   ```bash
   docker build -t hello-python .
   ```

   This command tells Docker to create an image named `hello-python` using the current directory (where the Dockerfile resides).

5. Run the container:

   ```bash
   docker run --rm hello-python
   ```

   This will execute your script inside a container and output:

   ```
   Hello, World!
   ```

   The `--rm` flag ensures the container is removed after execution, keeping your system clean.

### Task 2: Add Dependencies with requirements.txt

In this task, you will enhance your application by including external Python dependencies, and learn how to install them in your Docker container using a `requirements.txt` file — the most widely used format for listing dependencies in Python projects.

1. Modify your Python script to use a third-party library. We'll use the `requests` library:

   ```python
   # ./app/hello.py
   import requests
   print("Hello, World!", requests.__version__)
   ```

   This version of the script prints the installed version of `requests`, demonstrating that the package is available inside the container.

2. Create a `requirements.txt` file in your root project directory with the following content:

   ```txt
   requests
   ```

   This file specifies the packages to be installed via `pip`. Each line should list one package, optionally with a version specifier.

3. Update your Dockerfile to include this dependency installation step:

   ```Dockerfile
   FROM python:3.11-slim
   WORKDIR /app
   COPY ./app /app
   COPY requirements.txt ./
   RUN pip install --no-cache-dir -r requirements.txt
   CMD ["python", "hello.py"]
   ```

   Here's what changed:

   * The `COPY requirements.txt ./` step adds the requirements file to the Docker image.
   * The `RUN pip install ...` command installs the packages inside the container.

4. Rebuild your image and run the container:

   ```bash
   docker build -t hello-python-reqs .
   docker run --rm hello-python-reqs
   ```

   If everything is configured correctly, you should see output like:

   ```
   Hello, World! 2.31.0
   ```

   (Version number may vary depending on the latest version of `requests`).

This setup illustrates the basic workflow of containerizing a Python app with external dependencies using the `requirements.txt` method.

## Questions

1. What are the advantages of using Docker over virtual environments for Python?
2. What are the limitations of using `requirements.txt` compared to more modern tools?
3. How does Docker help ensure that dependencies remain consistent across different environments?

## Advice

Mastering Docker for Python projects can significantly enhance your productivity and confidence in deploying reliable software. Begin with simple setups and focus on mastering `requirements.txt`-based workflows. This foundation will serve you well before exploring advanced tools and standards. Don’t hesitate to consult the official Docker and Python packaging documentation—these are essential references. For related exercises, see [Virtual Environments and pip](https://github.com/STEMgraph/missing) and [Docker Networks and Volumes](https://github.com/STEMgraph/missing).
