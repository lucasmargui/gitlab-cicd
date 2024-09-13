
# GITLAB CICD

This repository contains a sample application in Python using Flask, intended to demonstrate the structure and development flow using Docker, Digital Ocean Deployment Server and unit tests. The project uses a **Makefile** to automate common tasks such as testing, Docker image creation and cloud deployment.

![image](https://github.com/user-attachments/assets/d2b18e26-9cdc-463e-8d03-b8dbccfee207)


## Commit code

A fundamental part of the workflow in version control systems, allowing developers to securely and organizedly save their changes to the project history.

<details>
<summary>Click to show details about </summary>

### Cloning the Repository

To clone the project, run the following command in the terminal:


```
git clone https://github.com/benc-uk/python-demoapp
```

### Cloned project structure

```
.
├── src/                             # Application source code directory
│   ├── app/                         # Contains the Flask application and its modules
│   ├── requirements.txt             # List of Python dependencies
│   └── run.py                       # Main file to start the Flask server
├── build/                           # Dockerfile and scripts related to building the Docker image
├── deploy/                          # Configuration files for deploying to Azure
├── tests/                           # Directory containing API integration tests
└── Makefile                         # File that automates development and deployment tasks
```

### Unit tests

The application's unit tests are located in the `src/app/tests` directory. These tests are responsible for validating the behavior of the code locally, before any deployment or continuous integration.

![image](https://github.com/user-attachments/assets/3e37d839-b2ff-4164-96fe-9ec0f3c6cf2a)

![image](https://github.com/user-attachments/assets/eccbc530-286b-479d-a717-5ff591a63a80)

### makefile

Makefile is used to automate development and deployment tasks of a Python application, through the definition of specific rules and commands, allowing the execution of predefined workflows, such as installing dependencies, running tests and deploying

To run the project's unit tests, the `make test` command will perform the following functions defined in the makefile:

1. **Creation of the Virtual Environment**:
   - Will create a Python virtual environment in the `src/.venv` directory.

2. **Installation of Dependencies**:
   - Will install the dependencies listed in the `src/requirements.txt` file within the created virtual environment.

3. **Test Execution**:
   - Will run the project's unit tests using `pytest`. Tests must be located in `src/app/tests`.

![image](https://github.com/user-attachments/assets/600b840f-994f-4fe0-812b-3aa69e114262)

</details>

## Code push to gitlab

GitLab is a complete DevOps platform that offers integrated tools for code versioning, CI/CD (Continuous Integration and Continuous Delivery), and collaboration between development teams. It allows you to manage the software lifecycle from planning to implementation and monitoring.

<details>
<summary>Click to show details about </summary>

### Create a new project:

After logging in, click on the "New project" or "Create a project" button.

![image](https://github.com/user-attachments/assets/3e26d4b5-8899-46b0-940c-b393c13e3d09)

Choose between "Create blank project", "Import project" (import from another platform), or "Create from template" (use a pre-defined template).

Fill in the project name, optional description, and set permissions (public, internal, or private).

![image](https://github.com/user-attachments/assets/d0a6eacd-ab9e-44e1-a20f-91dbdefacb25)

Click on "Create project".

![image](https://github.com/user-attachments/assets/49380de9-e96f-447b-9b2d-b78413772f6b)

</details>

## Create CI/CD Pipeline 

Pipeline is a set of automated steps that help you build, test, and deploy your code. Instead of performing these steps manually, you write a pipeline that describes these steps as code.

<details>
<summary>Click to show details about </summary>

### create gitlab-ci.yml file

This is the default pipeline configuration file name for GitLab CI/CD. GitLab CI/CD is a tool that enables continuous integration and continuous delivery of software. The .gitlab-ci.yml file is where you write instructions for how GitLab should build, test, and deploy your code.

![image](https://github.com/user-attachments/assets/5797b5d8-930b-4bea-bbc0-7d7f1cfe627f)

![image](https://github.com/user-attachments/assets/997d52f6-21d8-4d65-a490-dcb3bc618bb2)

</details>


## Create JOB: run_tests

In the context of CI/CD (Continuous Integration/Continuous Delivery), jobs in a pipeline represent individual steps and automated tasks that are executed sequentially or in parallel

The requirements for the `run_tests` job to be executed are:

- **Python must be available**: With Python installed, the project's unit tests will be run using `pytest` in a virtual environment, created using the `make test` command.
- **Pip must be available**: The code will install dependencies based on the `requirements.txt` file, therefore, `pip` must be installed in the environment.
- **Make command must be available**: Just as done locally, we will use `make test` to perform the unit tests. For it to work correctly, the machine where the job will be executed must have Make installed.


<details>
<summary>Click to show details about </summary>

### Python / Pip must be available:

By default, GitLab Managed Runners use a Ruby image to launch containers. However, you can override the default image by specifying a custom Docker image for specific jobs. This can be done by adding the `image` attribute within the job definition in the `.gitlab-ci.yml` file.

![image](https://github.com/user-attachments/assets/74a9456f-76ce-4282-bf69-6a0185f47729)

To use a custom Python image, go to the following website to find a suitable image:
[Docker Hub - Python Images](https://hub.docker.com/_/python)

### Make command must be available:

This command installs the make package, a tool used to automate project compilation. If the project or CI pipeline depends on make to compile code or run tests, it needs to be installed in the environment.

![image](https://github.com/user-attachments/assets/a1b363ab-65f8-4d2e-a4bb-61a9d09c2884)


### Explanation of Components:

 ![image](https://github.com/user-attachments/assets/db799ebf-2ba2-4b77-a954-983fe2cf926f)


1. **`run_tests`**:
   - This is the name of the job. It defines a specific task to be performed within the pipeline, in this case, running the project's tests.

2. **`stage: test`**:
   - Defines the stage of the pipeline to which this job belongs. The "test" stage is common in CI/CD pipelines and is where quality checks, such as unit tests, are performed.

3. **`image: python:3.8-slim-bullseye`**:
   - Defines the Docker image to be used for the job. Here, it uses the official Python image version 3.8, in its minimalistic form based on Debian Bullseye. Using a slim image helps reduce execution time and resource usage.
   ![image](https://github.com/user-attachments/assets/84dd37fe-8274-461d-8392-31bde4bedf86)

4. **`before_script`**:
   - Commands that will be executed before the main script.
   - `apt-get update && apt-get install make`: Updates the system's package list and installs the `make` tool. This is necessary because the job uses the `make` command to run tests, and the slim image does not include this tool by default.

5. **`script`**:
   - Defines the set of commands that will be executed during the job.
   - `make test`: The `make test` command is used to run the tests, assuming that the `Makefile` has a rule defined for `test`. The `Makefile` should contain instructions on how the tests should be executed (e.g., using `pytest`, `unittest`, or another testing framework).


When you commit the file, GitLab will automatically trigger the execution of the pipeline defined in `.gitlab-ci.yml`, starting the jobs as specified in the pipeline stages.


### Execution of the run_tests Job in the Pipeline:

In GitLab Pipelines, you can view the jobs that have been executed in each stage of the pipeline.

- Go to CI / CD > Pipelines in your project.
- Click on the pipeline you want to inspect.
- In the detailed view of the pipeline, you will see the stages and, within each stage, the individual jobs that were executed.
- To check the status and logs of a specific job, simply click on the job name. This will display details such as log output, errors, and job results.

### Job Execution Steps: run_tests

1. **Job Passed**

   The `run_tests` job has successfully passed the initial checks, allowing the pipeline to proceed.

   ![image](https://github.com/user-attachments/assets/056c4ac1-b2dc-4ba9-b5b3-d56f214cf75d)

2. **Environment Setup: Ruby to Python Transition**

   The default Docker image for Ruby is replaced with a Python-based image. This switch is necessary to ensure that `pip` and the Python development environment are available. This allows the job to initialize the Python environment, where dependencies can be installed using the `pip` tool, based on the `requirements.txt` file.

   ![image](https://github.com/user-attachments/assets/7a7ccaf6-3e9e-4a12-9590-32cd70e54441)

3. **Environment Setup: System Package Updates**

   Before executing the test scripts, the job performs a system update and installs `make`, a crucial tool required for building and running the project. These commands are necessary for ensuring that the environment is fully equipped to handle the execution of Makefile targets.

   ![image](https://github.com/user-attachments/assets/4a1d2235-0acd-4f3a-9b95-0efc648f3197)

4. **Running Test: make test**

   The `make test` command is executed to invoke the `test` target defined within the Makefile. This target, in turn, triggers the `pip install` command, installing all required dependencies specified in the `src/requirements.txt` file.

   This ensures that the environment is ready for running the tests with all dependencies correctly installed.

   ![image](https://github.com/user-attachments/assets/da34032c-fae2-457b-a9b1-6bb4d07798a0)

   With all dependencies installed and the Python virtual environment set up, the job proceeds to run unit tests using `pytest`. This step verifies that the codebase passes all unit tests and that the application functions as expected.

   ![image](https://github.com/user-attachments/assets/3b49218a-eff9-4d27-9d4e-cc3bd45b3d0c)

</details>


## Create JOB: build_image

The requirements for the `build_image` job to be executed are:

- **Docker Image must be available**: With Docker installed, we can generate an image of the app using a build. 
- **Docker Service Daemon must be available**: The Docker Daemon is a background service responsible for managing all Docker operations, allowing the job to execute Docker commands within the container.
- **Definition of the directory for certificates**: To ensure secure communication between the Docker client and the Docker daemon, TLS (Transport Layer Security) certificates are used. 
- **Login in Docker Hub**: Authenticate to Docker Hub using credentials with the docker login command.
- **Build Docker Image**: Create a Docker image from a Dockerfile using the docker build command.
- **Push Docker Image in Docker Hub**: Push the created image to the repository in Docker Hub with the docker push command.

<details>
<summary>Click to show details about </summary>

### Docker Image must be available:
This defines the environment necessary for the application to run, including the base system, dependencies and build instructions.

![image](https://github.com/user-attachments/assets/ac22cfcf-3afb-4619-acc5-4e5d900f36c2)


### Docker Service Daemon must be available
Deals with creating, running, and managing containers, images, volumes, and networks. The Docker Daemon listens to Docker API requests and can also communicate with other daemons to manage Docker services across clusters, ensuring the efficient functioning of all processes involved in running containers.

![image](https://github.com/user-attachments/assets/bbe638d0-f251-43b1-ae25-3fd41f51a17d)



### Definition of the directory for certificates
These certificates authenticate and encrypt the connection, protecting against unauthorized access.

![image](https://github.com/user-attachments/assets/586c3347-1632-422d-a856-9ebefe8062e5)

   
### Login in Docker Hub

To authenticate to Docker Hub, use the docker login command. You will need your Docker Hub credentials (username and password).

We will use environment variables to set up the credentials and utilize them within the job, similarly to how a .env file operates.

![image](https://github.com/user-attachments/assets/5ee55199-23ec-437b-b487-c2c5fc6930df)

We need to configure these credentials  `$REGISTRY_USER` and `$REGISTRY_PASS` in GitLab. Navigate to the settings menu and select CI/CD.

![image](https://github.com/user-attachments/assets/12ee8802-f5d2-43bd-b4fd-1d4c09ce3006)

![image](https://github.com/user-attachments/assets/8cdc613f-7b45-484c-a874-29eb22353a49)

![image](https://github.com/user-attachments/assets/f9f40636-96dd-4fe4-844f-85721865737c)


This way, Gitlab Runner will be able to log into your Docker repository before you perform the push.


If you do not have a repository created, follow the steps below

#### 1. Log In to Docker Hub

1. **Open Docker Hub**: Visit [hub.docker.com](https://hub.docker.com/).
2. **Sign In**: Click the "Sign In" button at the top-right corner and enter your Docker Hub credentials.

#### 2. Create a Private Repository

1. **Navigate to Repositories**: Go to your Docker Hub dashboard after logging in.
2. **Create Repository**:
   - Click on the “Create Repository” button.
   - Fill in the repository details:
     - **Name**: Enter the name for your repository.
     - **Visibility**: Select "Private" to keep the repository accessible only to you and users you grant access.
     - **Description**: (Optional) Provide a description for your repository.
   - Click the “Create” button to finalize the creation.
  

![image](https://github.com/user-attachments/assets/456a6021-839f-4c32-b23f-e7ed6c285db7)



### Build Docker Image

To build a Docker image from a Dockerfile, use the docker build command. Make sure you are in the directory where your Dockerfile is located.

![image](https://github.com/user-attachments/assets/8a722939-db8f-4bb3-9d5e-ed130a128e3c)


The pipeline uses the following dockerfile located in `build/Dockerfile` to create the docker image (the python version must be the same as that used in  Create JOB: run_tests )

![image](https://github.com/user-attachments/assets/9abb031c-7a46-40c1-8792-3c5b6eb56db4)



### Push Docker Image

Once your Docker image is built, you can push it to Docker Hub using the docker push command.

Using a Docker container, the process will involve interacting with the Docker Daemon to authenticate to the Docker Registry, a Docker image repository similar to GitHub. The image will be built with the `docker build` command, specifying the Dockerfile located in the `build` directory, and the resulting image will be pushed to the repository using the `docker push` command


![image](https://github.com/user-attachments/assets/48c2cc36-0e4a-4b3d-927e-7e2c6555a989)


### Explanation of Components:

![image](https://github.com/user-attachments/assets/8200fca5-748d-48d8-a353-59037236ff47)


### `stage: build`
Defines the stage in which the job will be executed. In this case, the job is in the "build" stage, which is responsible for building the Docker image.

### `image: docker:27.2.1-cli`
Specifies the Docker image to be used for executing commands. The `docker:27.2.1-cli` image provides the Docker CLI (command-line interface) version 27.2.1.

### `services`
Defines the services that will be used by the job. The `docker:27.2.1-dind` service is Docker-in-Docker, which allows Docker to be run inside a Docker container.

### `variables`
Defines environment variables that will be used during the execution of the job.
- `DOCKER_TLS_CERTDIR: "/certs"`: Sets the directory where Docker TLS certificates will be stored. This is necessary for secure communication with Docker-in-Docker.

### `before_script`
Commands that are executed before the main script. In this case:
- `docker login -u $REGISTRY_USER -p $REGISTRY_PASS`: Logs into the Docker registry using the `$REGISTRY_USER` and `$REGISTRY_PASS` environment variables for authentication. This is necessary to allow access to the registry where the image will be pushed.

### `script`
The main commands executed by the job:
- `docker build --build-arg srcDir=src -t $IMAGE_NAME:$IMAGE_TAG -f build/Dockerfile .`: Builds the Docker image. Here, `--build-arg srcDir=src` sets a build argument (`srcDir`) for the Dockerfile, `-t $IMAGE_NAME:$IMAGE_TAG` sets the image tag (name and tag), and `-f build/Dockerfile` specifies the path to the Dockerfile.
- `docker push $IMAGE_NAME:$IMAGE_TAG`: Pushes the built Docker image to the registry, using the previously defined tag.

## Considerations
- **Security**: Ensure that the `$REGISTRY_USER` and `$REGISTRY_PASS` variables are correctly configured and kept secure.
- **Docker-in-Docker**: Using Docker-in-Docker may have security and performance implications. Evaluate if it is the best approach for your use case.

### Job Execution Steps: build_image

</details>

## Create JOB: deploy

The requirements for the `deploy` job to be executed are:

- **Create a deployment server**: Create a virtual server (droplet) in the cloud that will be used to host and run your application.
- **Create an ssh key**: Generate an SSH key to enable secure remote connection to the server to remotely connect to the server.
- **Connect to server created**: Establish a connection to the newly created server using the SSH key.
- **Install docker on the server**: Install Docker on the server to manage containers and images.
- **Add ssh private key in gitlab**: Add the SSH key to GitLab to allow the GitLab pipeline to connect to the server for executing tasks.
- **Connect job to the server droplet**: Ensure that the GitLab Runner container has SSH access configured.
- **With the job connected to the server droplet**: run the docker command to log in and pull image.
- **With the job connected to the server droplet**: stop and remove any container using port 5000.
- **With the job connected to the server droplet**: run image in port 5000.

<details>
<summary>Click to show details about </summary>


### Create a deployment server:

Set up a virtual server (droplet) in the cloud to host and run your application.

![image](https://github.com/user-attachments/assets/c2cd5a7f-3876-43c5-9aff-8d07b8c8a2b6)


  
### Create an ssh key:

Generate an SSH key to securely connect to the server remotely.

![image](https://github.com/user-attachments/assets/4788b9fd-a369-4c82-91c0-b4724ffbe21c)

![image](https://github.com/user-attachments/assets/11da7145-b200-4054-a411-039bcbcecf4c)


### Connect to server created: 

When setting up a remote server on DigitalOcean, an SSH key is required for access. Use a previously generated public `id_rsa.pub` ssh key

![image](https://github.com/user-attachments/assets/e5674a2a-35ef-4118-85b1-7a6fa39ea1a9)

![image](https://github.com/user-attachments/assets/7e371a7d-ee41-4d34-91e9-b3077c4a0793)

![image](https://github.com/user-attachments/assets/eb74ce11-d6b4-4b9a-8c97-55a7f06b9a63)

Now, all servers created can be accessed via this public key,
  
### Install docker on the server 
  
To establish the connection, we will use SSH and authenticate with the private ssh key  `id_rsa`.

```
ssh -i C:/Users/YourUser/.ssh/id_rsa root@"ip_droplet_created"
```

Once connected, you need to install Docker on the remote machine.

![image](https://github.com/user-attachments/assets/2459d0d2-b602-4281-aa05-b37c80eec6c1)

The remote server now has Docker installed.

### Add ssh private key in gitlab:

GitLab Runner requires SSH access within a container to connect to the remote server.

To facilitate this, we need the private key available in GitLab to establish the connection to the server.

Similarly to how we set environment variables for Docker Hub, we can create secret variables in GitLab for the private key.

![image](https://github.com/user-attachments/assets/e0405c25-8e7c-4b93-a00a-c47431a7ea18)


### Connect job to the server droplet

  ![image](https://github.com/user-attachments/assets/5586c198-9871-413e-98e4-d89bb70ab555)

Stricthostkeychecking is to avoid manual check by pressing enter that appears when connecting to the server
  
### With the job connected to the server droplet**: run the docker command to log in and pull image.

![image](https://github.com/user-attachments/assets/5e1bd6f2-5024-4015-b8aa-1a888db98706)

So in the same way that previously we needed to be logged in to perform a push image to registry, in the deploy stage we need to be logged in to perform a pull image


### With the job connected to the server droplet**: stop and remove any container using port 5000.

![image](https://github.com/user-attachments/assets/f858c571-7894-4c5b-a60f-431d177b80e1)


Each time we need to establish a connection, we must stop and remove the container on port 5000, as multiple processes cannot share the same port. This ensures that a new container can be created on that port without conflicts.
  
### With the job connected to the server droplet**: run image in port 5000.

![image](https://github.com/user-attachments/assets/9650a09e-3b47-4114-b9d3-5ab6bd9d7e9a)


To verify the application, access the IP address of the droplet on port 5000

### Explanation of Content: 

![image](https://github.com/user-attachments/assets/b7b79e2e-3559-4a8e-8e21-ebe19967ff6a)


### 1. Step: Deploy
This is the name of the deployment step in the pipeline. It is used to identify and organize different phases of the CI/CD process in your configuration file.

### 2. Stage: Deploy
Specifies the stage of the pipeline at which this step should be executed. In the context of CI/CD, pipelines are divided into various stages, such as build, test, and deploy. Here, "deploy" indicates that this step is part of the deployment stage.

### 3. before_script
This section defines commands that will be executed before the main script of the deployment step. It is used to prepare the environment for deployment.

### 4. Command: `chmod 400 $SSH_KEY`
This command changes the permissions of the SSH key file (`$SSH_KEY`) so that only the owner can read it. This is important for securing the key and preventing unauthorized access by other users.

### 5. script
Defines the commands that will be executed as part of the deployment step. These commands are responsible for carrying out the actual deployment.

### 6. Command: `ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@ip_do_droplet " ... "`
This command establishes an SSH connection to the remote server (`root@ip_do_droplet `) using the provided SSH key (`$SSH_KEY`). The `-o StrictHostKeyChecking=no` parameter disables host key verification to prevent deployment failures if the remote host is not already in the known hosts list.

### 7. Command: `docker login -u $REGISTRY_USER -p $REGISTRY_PASS`
Logs in to the Docker registry using the provided credentials (`$REGISTRY_USER` and `$REGISTRY_PASS`). This step is necessary to authenticate and access the Docker image registry.

### 8. Command: `docker ps -aq | xargs docker stop | xargs docker rm`
These commands are used to stop and remove all running Docker containers on the server. The process is as follows:

- `docker ps -aq`: Lists all container IDs.
- `xargs docker stop`: Stops all listed containers.
- `xargs docker rm`: Removes all stopped containers.

### 9. Command: `docker run -d -p 5000:5000 $IMAGE_NAME:$IMAGE_TAG`
Runs a new Docker container with the specified image (`$IMAGE_NAME:$IMAGE_TAG`). The parameters are:

- `-d`: Runs the container in detached mode (in the background).
- `-p 5000:5000`: Maps port 5000 of the container to port 5000 on the host, allowing access to the service running in the container.

</details>

## Screenshot

![screen](https://user-images.githubusercontent.com/14982936/30533171-db17fccc-9c4f-11e7-8862-eb8c148fedea.png)
