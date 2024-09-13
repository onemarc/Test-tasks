Created a simple Flask web application using CI/CD techniques, namely using GitHub Actions, which check each git push and then deploy the application to the Google Cloud Platform cloud service using Cloud Run.

Step-by-step, the application works as follows:

1. We have a simple Flask application that displays the text `"Hello👋, Application!"`.
2. <p>The source files are placed in the <a href="https://github.com/onemarc/flask-web-app" target="_blank">GitHub repository</a>.</p>
3. The GitHub repository contains two `YAML` files for further configuration of pipelines. One of them is responsible for automatic dependency testing and test execution, and the other is responsible for automated deployment of a web application to Google Cloud services. The deployment `YAML` file uses GitHub Secrets, which have been installed beforehand, for secure authentication and interaction with Google Cloud. These Secrets include sensitive information, such as Google Cloud access credentials, which must be kept private for security reasons.
4. Configured 2 CI/CD pipelines using GitHub Actions. In more detail, the first pipeline checks whether the push contains the required requirements, which are specified in `requirements.txt`. The second pipeline creates a Docker image based on a `Dockerfile` that describes how to configure the environment for the application, and then uploads the docker image to the Artifacts Registry. Then all this is placed on a serverless service from Google Cloud, i.e. Cloud Run.
5. Cloud Run automatically deploys the docker image as a managed service and configures it for further work.
6. After the deployment is complete, Cloud Run automatically provides an `HTTPS` URL to access the application.

Such an architectural solution will ensure automatic, scalable and secure web application development.

Diagram of Flask Web Application
![flask-diagram](https://github.com/user-attachments/assets/53d76c97-ffb9-460e-aaf3-e4fc122a8ec0)
