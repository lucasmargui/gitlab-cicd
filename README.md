
# GITLAB CICD

This repository contains a sample application in Python using Flask, intended to demonstrate the structure and development flow using Docker, Azure Web App and unit tests. The project uses a **Makefile** to automate common tasks such as testing, Docker image creation and cloud deployment.

## Cloning the Repository

To clone the project, run the following command in the terminal:

<details>
<summary>Click to show details about </summary>

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

### Custom Docker Image

By default, GitLab Managed Runners use a Ruby image to launch containers. However, you can override the default image by specifying a custom Docker image for specific jobs. This can be done by adding the `image` attribute within the job definition in the `.gitlab-ci.yml` file.

To use a custom Python image, go to the following website to find a suitable image:
[Docker Hub - Python Images](https://hub.docker.com/_/python)

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

In GitLab Pipelines, you can view the jobs that have been executed in each stage of the pipeline. To do this:

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


## Screenshot

![screen](https://user-images.githubusercontent.com/14982936/30533171-db17fccc-9c4f-11e7-8862-eb8c148fedea.png)
