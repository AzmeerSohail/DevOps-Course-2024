# Streamlining DevOps Workflows: A Practical Guide

In the modern software development landscape, DevOps practices are essential for seamless integration, efficient development, and faster delivery. This blog outlines the steps I took to implement DevOps practices for a Python-based web application using Docker and GitHub Actions, along with lessons learned along the way.

---

## Step 1: Setting Up the Environment

Every journey starts with setting up the right environment. For this project, I began by forking and cloning the **GitHub Actions Starter Workflows** repository into my local development environment.

### Creating a Virtual Environment

To manage Python dependencies in isolation, I set up a virtual environment using `venv`. This ensures that dependencies for this project do not interfere with other projects.

```bash
python -m venv venv
.\venv\Scripts\activate
```

---

## Step 2: Building the Sample Application
With the environment ready, I created a basic Flask application:

# The Application Code
The app simply responds with "Hello, World!" when accessed:
```bash
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

I saved this as app.py in a new sample-app directory. Then, I defined dependencies in a requirements.txt file:
```bash
flask==2.2.5
```

Installing the dependencies was as simple as running:
```bash
pip install -r requirements.txt
```

---


## Step 3: Dockerizing the Application
# Creating the Dockerfile
Dockerizing the application made it platform-independent. Here’s the Dockerfile I created:

```bash
FROM python:3.9-slim
WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["python", "app.py"]
```

# Building and Running the Docker Image
Next, I built the Docker image:
```bash
docker build -t sample-app .
```

And ran the container, mapping it to a port on my machine:
```bash
docker run -p 5002:5000 sample-app
```

Visiting **http://localhost:5002** in my browser confirmed the app was running successfully.

---

## Step 4: Integrating CI/CD with GitHub Actions
DevOps isn’t just about creating applications—it’s about automating the workflows that build, test, and deploy them. I used GitHub Actions to set up a CI/CD pipeline.

# The Workflow Configuration
Here’s the .github/workflows/ci-cd.yml file I created:

```bash
name: CI/CD Pipeline for Sample App

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.9'

    - name: Install dependencies
      run: |
        pip install -r sample-app/requirements.txt

    - name: Run tests (if any)
      run: |
        echo "No tests implemented yet"

    - name: Build Docker Image
      run: |
        docker build -t sample-app ./sample-app

    - name: Push Docker Image (optional)
      run: |
        echo "Add steps for DockerHub/GitHub Container Registry if required"
```

Every push to the main branch now triggers this workflow, which:

- Installs dependencies.
- Builds the Docker image.
- Provides placeholders for running tests and pushing the image to a registry.

---

## Step 5: Handling Git Submodules
To keep the project modular and avoid duplicating code, I added the starter-workflows repository as a Git submodule:
```bash
git rm --cached starter-workflows
git submodule add https://github.com/actions/starter-workflows.git starter-workflows
git commit -m "Added starter-workflows as a submodule"
```
Submodules allowed me to link another repository without directly embedding it, simplifying project organization and future updates.

