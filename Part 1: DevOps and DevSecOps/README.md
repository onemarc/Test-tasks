### Created a simple Flask web application using CI/CD techniques, namely using GitHub Actions, which check each git push and then deploy the application to the Google Cloud Platform cloud service using Cloud Run.

Step-by-step, the application works as follows:

1. We have a simple Flask application that displays the text `"Hello👋, Application!"`.
2. <p>The source files are placed in the <a href="https://github.com/onemarc/flask-web-app" target="_blank">GitHub repository</a>.</p>
3. The GitHub repository contains three `YAML` files for further configuration of the pipelines. One of them is responsible for automatic dependency testing and test execution, and the other is responsible for automatic deployment of the web application to Google Cloud services, the latter performs static code analysis using the SonarQube service. The deployment `YAML` file uses GitHub secrets that have been set in advance for secure authentication and interaction with Google Cloud. These secrets include sensitive information, such as Google Cloud access credentials, which must be kept confidential for security reasons.
4. Configured 3 CI/CD pipelines using GitHub Actions. In more detail, the first pipeline checks whether the submission contains the required requirements, which are specified in `requirements.txt`. The second pipeline builds a Docker image based on a `Dockerfile` that describes how to set up the environment for the application, and then uploads the Docker image to the artifact registry. All of this is then hosted on Google Cloud's serverless service, i.e. Cloud Run. The third pipeline performs a static code analysis for bugs, vulnerabilities, code smells and security hotspots. Given that it works with the help of the SonarQube service, it is possible to go to the administrator account and view in detail all the necessary information about the analysis.
5. Cloud Run automatically deploys the docker image as a managed service and configures it for further work.
6. After the deployment is complete, Cloud Run automatically provides an `HTTPS` URL to access the application.

Such an architectural solution will ensure automatic, scalable and secure web application development.

### Diagram of Flask Web Application
![diagram-for-flask-web-app](https://github.com/user-attachments/assets/458e89d2-7ae3-4f7d-8a2d-61d9533d133f)
