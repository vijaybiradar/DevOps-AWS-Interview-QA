<details>
<summary>
 Can you explain the CICD process in your current project ? or Can you talk about any CICD process that you have implemented ?</summary><br><b>

A: In the current project we use the following tools orchestrated with Jenkins to achieve CICD.
   - Maven, Sonar, AppScan, ArgoCD, and Kubernetes
   
   Coming to the implementation, the entire process takes place in 8 steps
    
    1. Code Commit: Developers commit code changes to a Git repository hosted on GitHub.
    2. Jenkins Build: Jenkins is triggered to build the code using Maven. Maven builds the code and runs unit tests.
    3. Code Analysis: Sonar is used to perform static code analysis to identify any code quality issues, security vulnerabilities, and bugs.
    4. Security Scan: AppScan is used to perform a security scan on the application to identify any security vulnerabilities.
    5. Deploy to Dev Environment: If the build and scans pass, Jenkins deploys the code to a development environment managed by Kubernetes.
    6. Continuous Deployment: ArgoCD is used to manage continuous deployment. ArgoCD watches the Git repository and automatically deploys new changes to the development environment as soon as they are committed.
    7. Promote to Production: When the code is ready for production, it is manually promoted using ArgoCD to the production environment.
    8. Monitoring: The application is monitored for performance and availability using Kubernetes tools and other monitoring tools.
</b></details>   


<details>
<summary>
Q: What are the different ways to trigger jenkins pipelines ?</summary><br><b>

A: This can be done in multiple ways,
   To briefly explain about the different options,
   ```
     - Poll SCM: Jenkins can periodically check the repository for changes and automatically build if changes are detected. 
                 This can be configured in the "Build Triggers" section of a job.
                 
     - Build Triggers: Jenkins can be configured to use the Git plugin, which allows you to specify a Git repository and branch to build. 
                 The plugin can be configured to automatically build when changes are pushed to the repository.
                 
     - Webhooks: A webhook can be created in GitHub to notify Jenkins when changes are pushed to the repository. 
                 Jenkins can then automatically build the updated code. This can be set up in the "Build Triggers" section of a job and in the GitHub repository settings.
   ```
</b></details>
<details>
<summary>
Q: How to backup Jenkins ?</summary><br><b>

A: Backing up Jenkins is a very easy process, there are multiple default and configured files and folders in Jenkins that you might want to backup.
```  
  - Configuration: The `~/.jenkins` folder. You can use a tool like rsync to backup the entire directory to another location.
  
    - Plugins: Backup the plugins installed in Jenkins by copying the plugins directory located in JENKINS_HOME/plugins to another location.
    
    - Jobs: Backup the Jenkins jobs by copying the jobs directory located in JENKINS_HOME/jobs to another location.
    
    - User Content: If you have added any custom content, such as build artifacts, scripts, or job configurations, to the Jenkins environment, make sure to backup those as well.
    
    - Database Backup: If you are using a database to store information such as build results, you will need to backup the database separately. This typically involves using a database backup tool, such as mysqldump for MySQL, to export the data to another location.
```
One can schedule the backups to occur regularly, such as daily or weekly, to ensure that you always have a recent copy of your Jenkins environment available. You can use tools such as cron or Windows Task Scheduler to automate the backup process.

</b></details>

<details>
<summary>
Q: How do you store/secure/handle secrets in Jenkins ?</summary><br><b>

A: Again, there are multiple ways to achieve this, 
   Let me give you a brief explanation of all the posible options.
```  
   - Credentials Plugin: Jenkins provides a credentials plugin that can be used to store secrets such as passwords, API keys, and certificates. The secrets are encrypted and stored securely within Jenkins, and can be easily retrieved in build scripts or used in other plugins.
   
   - Environment Variables: Secrets can be stored as environment variables in Jenkins and referenced in build scripts. However, this method is less secure because environment variables are visible in the build logs.
   
   - Hashicorp Vault: Jenkins can be integrated with Hashicorp Vault, which is a secure secrets management tool. Vault can be used to store and manage sensitive information, and Jenkins can retrieve the secrets as needed for builds.
   
   - Third-party Secret Management Tools: Jenkins can also be integrated with third-party secret management tools such as AWS Secrets Manager, Google Cloud Key Management Service, and Azure Key Vault.
```
</b></details>
<details>

<summary>
Q: What is latest version of Jenkins or which version of Jenkins are you using ?</summary><br><b>

A: This is a very simple question interviewers ask to understand if you are actually using Jenkins day-to-day, so always be prepared for this.

</b></details>

<details>
<summary>
Q: What is shared modules in Jenkins ?</summary><br><b>

A: Shared modules in Jenkins refer to a collection of reusable code and resources that can be shared across multiple Jenkins jobs. This allows for easier maintenance, reduced duplication, and improved consistency across multiple build processes.
   For example, shared modules can be used in cases like:
```
        - Libraries: Custom Java libraries, shell scripts, and other resources that can be reused across multiple jobs.
        
        - Jenkinsfile: A shared Jenkinsfile can be used to define the build process for multiple jobs, reducing duplication and making it easier to manage the build process for multiple projects.
        
        - Plugins: Common plugins can be installed once as a shared module and reused across multiple jobs, reducing the overhead of managing plugins on individual jobs.
        
        - Global Variables: Shared global variables can be defined and used across multiple jobs, making it easier to manage common build parameters such as version numbers, artifact repositories, and environment variables.
```
</b></details>

<details>
<summary>
Q: can you use Jenkins to build applications with multiple programming languages using different agents in different stages ?</summary><br><b>

A: Yes, Jenkins can be used to build applications with multiple programming languages by using different build agents in different stages of the build process.

Jenkins supports multiple build agents, which can be used to run build jobs on different platforms and with different configurations. By using different agents in different stages of the build process, you can build applications with multiple programming languages and ensure that the appropriate tools and libraries are available for each language.

For example, you can use one agent for compiling Java code and another agent for building a Node.js application. The agents can be configured to use different operating systems, different versions of programming languages, and different libraries and tools.

Jenkins also provides a wide range of plugins that can be used to support multiple programming languages and build tools, making it easy to integrate different parts of the build process and manage the dependencies required for each stage.

Overall, Jenkins is a flexible and powerful tool that can be used to build applications with multiple programming languages and support different stages of the build process.

</b></details>

<details>
<summary>

Q: How to setup auto-scaling group for Jenkins in AWS ?</summary><br><b>

A: Here is a high-level overview of how to set up an autoscaling group for Jenkins in Amazon Web Services (AWS):
```
    - Launch EC2 instances: Create an Amazon Elastic Compute Cloud (EC2) instance with the desired configuration and install Jenkins on it. This instance will be used as the base image for the autoscaling group.
    
    - Create Launch Configuration: Create a launch configuration in AWS Auto Scaling that specifies the EC2 instance type, the base image (created in step 1), and any additional configuration settings such as storage, security groups, and key pairs.
    
    - Create Autoscaling Group: Create an autoscaling group in AWS Auto Scaling and specify the launch configuration created in step 2. Also, specify the desired number of instances, the minimum number of instances, and the maximum number of instances for the autoscaling group.
    
    - Configure Scaling Policy: Configure a scaling policy for the autoscaling group to determine when new instances should be added or removed from the group. This can be based on the average CPU utilization of the instances or other performance metrics.
    
    - Load Balancer: Create a load balancer in Amazon Elastic Load Balancer (ELB) and configure it to forward traffic to the autoscaling group.
    
    - Connect to Jenkins: Connect to the Jenkins instance using the load balancer endpoint or the public IP address of one of the instances in the autoscaling group.
    
    - Monitoring: Monitor the instances in the autoscaling group using Amazon CloudWatch to ensure that they are healthy and that the autoscaling policy is functioning as expected.

 By using an autoscaling group for Jenkins, you can ensure that you have the appropriate number of instances available to handle the load on your build processes, and that new instances can be added or removed automatically as needed. This helps to ensure the reliability and scalability of your Jenkins environment.
```
</b></details>

<details>
<summary>

Q: How to add a new worker node in Jenkins ?</summary><br><b>

A: Log into the Jenkins master and navigate to Manage Jenkins > Manage Nodes > New Node. Enter a name for the new node and select Permanent Agent. Configure SSH and click on Launch.

</b></details>

<details>
<summary>
Q: How to add a new plugin in Jenkins ?</summary><br><b>

A: Using the CLI, 
   `java -jar jenkins-cli.jar install-plugin <PLUGIN_NAME>`
  
  Using the UI,

   1. Click on the "Manage Jenkins" link in the left-side menu.
   2. Click on the "Manage Plugins" link.

</b></details>

<details>
<summary>

Q: What is JNLP and why is it used in Jenkins ?</summary><br><b>

A: In Jenkins, JNLP is used to allow agents (also known as "slave nodes") to be launched and managed remotely by the Jenkins master instance. This allows Jenkins to distribute build tasks to multiple agents, providing scalability and improving performance.

   When a Jenkins agent is launched using JNLP, it connects to the Jenkins master and receives build tasks, which it then executes. The results of the build are then sent back to the master and displayed in the Jenkins user interface.

</b></details>
Q: What are some of the common plugins that you use in Jenkins ?

A: Be prepared for answer, you need to have atleast 3-4 on top of your head, so that interview feels you use jenkins on a day-to-day basis.

</b></details>

<details>
<summary>

# Q: Explain CI/CD flow</summary><br><b>

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/66e58971-db48-4435-a65d-8e0f26c4b3b8)
![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/3f3f15a4-2641-4abd-8301-e33889582fc1)



**1. Version Control System (VCS) Integration:**
 - Developers use Git as the underlying VCS and host their repositories on Bitbucket.
 - They create a new branch in Bitbucket for each feature or bug fix.


**2. Local Development Workflow:**
 - Developers run unit tests and conduct code reviews locally before committing changes.


**3. Continuous Integration (CI):**
 - Jenkins is configured with a Git webhook that listens for changes in the Bitbucket repository.
 - When developers push changes to the repository, the webhook notifies Jenkins.


**4. Automated Build:**
- Jenkins fetches the latest code from the repository using the Git plugin.
- Jenkins performs automated builds of the application using build tools like Maven, NPM, or ANT, based on the project's requirements.
- If the build fails/successful, then the concerned team will be notified.
- If the build is successful, then Jenkins deploys the build in the test server.


**5. Automated Testing:**
- Automated testing, including unit tests, integration tests, and any other relevant tests, is executed as part of the CI pipeline.
- After testing, Jenkins generates feedback and then notifies the developers about the build and test results.
- It will continue to check the source code repository for changes made in the source code and the whole process keeps on repeating.
- Code quality checks are performed using static analysis tools.


**6. Artifact Management:**
- Jenkins interacts with an Artifactory repository using the JFrog plugin.
- Binary files, including packaged artifacts and dependencies, are stored in the Artifactory repository for versioning and easy retrieval.


**7. Containerization:**
- Jenkins uses Docker to create a container image of the application.
- A Dockerfile defines the application's runtime environment, dependencies, and configuration.


**8. Docker Image Security Scanning:**
 - Before proceeding, Docker images undergo security scanning using tools like Twistlock  to identify vulnerabilities.


**9. Docker Image Publishing:**
- The Docker image is then pushed to a Docker registry.
- This can be a private registry like DTR or a Nexus repository in the case of AWS ECR.
- This action makes the image available for deployment in various environments.


**10. Infrastructure as Code (IaC):**
- Infrastructure provisioning is managed as code using tools like Terraform or AWS CloudFormation.
- Infrastructure changes are versioned alongside application code.


**11. Container Orchestration with Kubernetes:**
- Jenkins interacts with Kubernetes using the Kubernetes plugin.
- Kubernetes manages the deployment, scaling, and container management across a cluster of nodes.
- Kubernetes ensures high availability and reliability by distributing containers across nodes and monitoring their health.

**12. Blue-Green Deployment:**
- Set up two identical environments, often referred to as "Blue" and "Green."
- Deploy the new version (Green) alongside the existing version (Blue).
- Route a portion of traffic to the Green environment for testing and validation.
- Gradually shift more traffic to the Green environment based on testing results.
- If any issues are detected, easily switch back to the Blue environment.
- Once the Green environment is stable and validated, it becomes the new production (Blue) environment.

**13. Kubernetes Ingress for External URL Access:**
- Create Kubernetes Ingress resources to allow external access to your services.
- Configure Ingress rules to route incoming traffic based on hostnames and paths to specific services and ports.
- Use a cloud-based load balancer or an on-premises solution to route external traffic to the Kubernetes cluster.

**14. Continuous Deployment (CD) with ArgoCD:**
- ArgoCD actively monitors the Git repository for changes.
- When changes are committed, ArgoCD automatically deploys them to lower environment environments (Dev, SIT, UAT, Perf, Pre-prod).
- Necessary approvals and tests are carried out in lower environments.

**15. Production Deployment with ArgoCD:**
- After successful testing in lower environments, ArgoCD deploys the new changes to the production environment with all necessary approvals and checks.

**16. Helm Chart Management:**
- Helm is utilized for defining, managing, and versioning Kubernetes application deployments.
- Specific Helm values.yaml files are used for each environment, such as SIT values.yaml, UAT values.yaml,Perf values.yaml and prod values.yaml, to customize configurations for different 
  stages of deployment.

**17. Monitoring and Observability:**
- The deployed application is monitored using Prometheus for metrics collection.
- Grafana is used for visualization and alerting.
- Loki or a centralized logging solution like ELK is employed for log management.

**18. User Accessibility:**
- The application is now deployed and accessible to users.
- Users can access the application via exposed services and Ingress endpoints managed by Kubernetes.

**19. Documentation and Knowledge Sharing:**
- Thorough documentation of the entire CI/CD pipeline and its processes is maintained and updated.
- Knowledge sharing among team members ensures everyone understands and can effectively use the pipeline.

**20. Disaster Recovery Planning:**
- The pipeline includes disaster recovery planning, such as data backups and automated rollback procedures in case of deployment failures.

**21. Compliance and Security Checks:**
- Depending on the application and industry, compliance checks (e.g., PCI DSS, HIPAA) and security scanning (e.g., OWASP ZAP) are integrated into the pipeline.

**22. Notification and Alerting:**
- Notifications and alerting mechanisms (e.g., Slack or email notifications) are implemented for pipeline status updates and critical issues to keep the team informed.

</b></details>


<details>
<summary>

# Q: Explain CI/CD flow</summary><br><b>

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/66e58971-db48-4435-a65d-8e0f26c4b3b8)
![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/3f3f15a4-2641-4abd-8301-e33889582fc1)



**1. Code Versioning and Triggering:**
- Developers commit their code changes to a Git repository.
- A Git webhook or Jenkins polling detects changes and triggers the pipeline.

**2. Build and Notification:**
- Jenkins uses the Maven plugin to build the code, creating deployable artifacts.
- After the build, an email notification is sent to the concerned team with the build status (success or failure).
- CI/CD server builds the code and executes the unit tests in pom.xml.
- If the unit tests pass, the code is deployed to the lower (SIT,UAT,PERF) environment.

**3. Code Quality Analysis:**
- The SonarQube plugin is integrated into the pipeline for code quality analysis.
- Jenkins monitors SonarQube for analysis status (success or failure).
- If code issues are identified, a notification is sent, but the pipeline continues.

**4. Containerization:**
- Docker is used to create a container image of the application based on a Dockerfile.

**5. Docker Image Security Scanning:**
- Docker images undergo security scanning using tools like Twistlock to identify vulnerabilities.

**6. Docker Image Publishing:**
- The Docker image is pushed to a Docker registry, which can be private or public.

**7. Testing:**
- Various tests are executed, including System Integration Tests (SIT), User Acceptance Tests (UAT), and Performance Tests (Perf).
- Tests are executed using different Helm values.yaml configuration files.
- If all tests pass, the pipeline proceeds to the next stage.

**8. Artifact Management:**
- Jenkins interacts with an Artifactory repository to store binary files, including packaged artifacts and dependencies.

**9. Infrastructure as Code (IaC):**
- Infrastructure provisioning is managed as code using tools like Terraform or AWS CloudFormation.
- Infrastructure changes are versioned alongside application code.
- Kubernetes manifests are stored in a Git repository.

**10. Deployment to Kubernetes:**
- The application is deployed to a Kubernetes cluster and accessible to users.
- ArgoCD is used for GitOps-based deployments, ensuring consistency and traceability.
- Outside users can access the application through exposed URLs via Ingress controllers.
- Prometheus scrapes Kubernetes endpoints for monitoring.

**11. Deployment Strategies:**
- For production deployments, a blue-green deployment strategy is employed.
- Helm is used to manage the deployment process, allowing easy rollbacks.

**12. User Interface (UI) Testing:**
- Automated UI testing tools like Selenium may be used to ensure the functionality and usability of the application's user interfaces.

**13. Observability and Monitoring:**
- Prometheus is used for monitoring application performance, including Kubernetes resources.
- Grafana provides a dashboard for visualizing metrics from Prometheus.
- Loki is used for log aggregation and monitoring.

**14. Disaster Recovery Planning:**
- Disaster recovery plans are in place to ensure data and application recovery in case of failures.

**15. Notification and Alerting:**
- Notification and alerting mechanisms are set up to inform relevant teams or individuals about pipeline status and critical incidents. Grafana can be configured to send alerts based on predefined thresholds.

</b></details>

<details>
<summary>

# Q: complete CI/CD pipeline for java application without using jenkins ? or
**What is the workflow for deploying an JAVA application to multiple environments (SIT, UAT, Perf, and Prod) without utilizing Jenkins, using a manual process? Can you describe how popular DevOps tools such as Git, Maven, Docker, Kubernetes, Artifactory, Helm, Prometheus/Grafana/Loki, Blue-Green Deployment works**</summary><br><b>

**Step 1: Version Control with Git:**
Set up a Git repository to manage your application code. Initialize the repository:
```
git init
```
Create and switch to a feature branch for development:
```
git checkout -b feature/my-feature
```
Make changes, add files, and commit your code:

# Make changes, add files
```
git add .
git commit -m "Feature: Added new feature"
```
When the feature is ready, merge it back into the main branch:
```
git checkout main
```
git merge feature/my-feature

**Step 2: Generate Maven Archetype (if needed):**

Generate a Maven archetype for your project (if needed):
```
mvn archetype:generate -DgroupId=com.example -DartifactId=your-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
This will create a new project directory with the following structure:

````````````````````````
my-application
├── pom.xml
└── src
    └── main
        └── java
            └── com
                └── example
                    └── myapplication
                        └── App.java
`````````````````````````````

Customize the generated project structure and files as necessary.

** Maven Project Setup (pom.xml):**


Configure your Maven project's pom.xml file. Include GAV (Group, Artifact, Version) information and other project details:
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-application</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>

```
Specify project dependencies, plugins, profiles, and other settings according to your project requirements.
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.3.8</version>
    </dependency>
    <!-- Add more dependencies as needed -->
</dependencies>
```


**Step 3: Code Analysis with SonarQube:**

Integrate SonarQube for code analysis. You'll need to have a SonarQube server set up.

Add the SonarQube plugin to your pom.xml:

```
<build>
    <plugins>
        <plugin>
            <groupId>org.sonarsource.scanner.maven</groupId>
            <artifactId>sonar-maven-plugin</artifactId>
            <version>3.7.0.1746</version>
        </plugin>
    </plugins>
</build>
```

Run SonarQube analysis in your CI/CD pipeline:
```
mvn clean install sonar:sonar -Dsonar.projectKey=my-app -Dsonar.host.url=http://sonarqube-server:9000 -Dsonar.login=your-sonar-token
```
Step 5: Artifact Repository with JFrog Artifactory

Set up JFrog Artifactory as your artifact repository manager.

Configure your Maven project to publish artifacts to Artifactory by updating your settings.xml file and pom.xml file. Here's an example of a settings.xml configuration:

```
<settings>
  <servers>
    <server>
      <id>artifactory-server</id>
      <username>your-username</username>
      <password>your-password</password>
    </server>
  </servers>
</settings>
```
Update your pom.xml to specify the deployment repository:
```
<distributionManagement>
  <repository>
    <id>artifactory-server</id>
    <url>https://artifactory.your-domain.com/artifactory/your-repo</url>
  </repository>
</distributionManagement>
```
Deploy your Maven artifacts to Artifactory:
```
mvn deploy
```


**Maven Build and Build Outputs:**
- Understand the Maven build lifecycle and phases (e.g., clean, validate, compile, test, package, deploy).

**Build the application:**

```
mvn clean install
```

Capture build outputs (JAR file, GAV, test reports, etc.):

You can find the JAR file and other build outputs in the target directory

After the build process completes successfully, you'll find your application's JAR file in the target directory within your project folder.

For example, if your project is named my-application, you'll find the JAR file at ```my-application/target/my-application-1.0-SNAPSHOT.jar.```

 **Run Your Java Application**
  - You can run your Java application by executing the JAR file. For example:

```
java -jar my-application/target/my-application-1.0-SNAPSHOT.jar
```

**Step 4: Create a Docker Image of the Application:**

Create a Dockerfile for the application to package it into an image. For example, create a Dockerfile with the following content:
Dockerfile
```
# Use a base image
FROM openjdk:11-jre-slim

# Copy application JAR file
COPY target/your-app-1.0.0.jar /app/your-app.jar

# Set environment variable
ENV ENVIRONMENT=production

# Expose port
EXPOSE 8080

# Run the application
CMD ["java", "-jar", "/app/your-app.jar"]
```

Build the Docker image:
```
docker build -t your-app:1.0.0 .
```
Step 7: Push the Docker Image to a Registry:

Push the Docker image to a container registry like Docker Hub or a private registry:
```
docker push your-docker-registry/your-app:1.0.0
```
**Step 5: Create Kubernetes Manifests for the Application:**

Create Kubernetes Deployment and Service manifests for the application. Define how the application should be deployed to the Kubernetes cluster.
deployment.yaml:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-app
        image: your-docker-registry/your-app:1.0.0
        ports:
        - containerPort: 8080
```
service.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: your-app-service
spec:
  selector:
    app: your-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
**Step 6: Deploy the Application to Kubernetes:**

Deploy the application to the Kubernetes cluster using the Kubernetes manifests:
```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
**Step 7: Helm Charts for Different Environments:**
Create Helm charts for deploying your application to different environments (SIT, UAT, Perf, Prod).

Set up values.yaml files for each environment to configure Helm deployments:

values-sit.yaml:

```
image:
  repository: your-docker-registry/your-app
  tag: 1.0.0
environment: sit
```
# Add other environment-specific configuration
values-uat.yaml:

```
image:
  repository: your-docker-registry/your-app
  tag: 1.0.0
environment: uat
```
# Add other environment-specific configuration
values-perf.yaml:

```
image:
  repository: your-docker-registry/your-app
  tag: 1.0.0
environment: perf
```
# Add other environment-specific configuration
values-prod.yaml:

```
image:
  repository: your-docker-registry/your-app
  tag: 1.0.0
environment: prod
```
# Add other environment-specific configuration
**Step 8: Blue/Green Deployment with Helm:**

Implement a blue/green deployment strategy using Helm. Create separate Helm releases for blue and green deployments.

Configure Helm charts and values.yaml files for both blue and green environments with the appropriate image tags and configurations.

Deploy the blue release:

```
helm install my-app-blue -f values-blue.yaml ./my-app-chart
```
Test the blue environment.

If the blue environment passes testing, deploy the green release:

```
helm install my-app-green -f values-green.yaml ./my-app-chart
```
Gradually switch traffic to the green environment if it passes testing.

Monitor both blue and green environments to ensure the new version is stable.


</b></details>

<details>
<summary>
# Q: complete CI/CD pipeline for a Node.js application without using jenkins ? or
**What is the workflow for deploying an JAVA application to multiple environments (SIT, UAT, Perf, and Prod) without utilizing Jenkins, using a manual process? Can you describe how popular DevOps tools such as Git, Maven, Docker, Kubernetes, Artifactory, Helm, Prometheus/Grafana/Loki, Blue-Green Deployment works**</summary><br><b>

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/b186502f-e788-4b6f-b4bf-080839f3f153)


complete steps for setting up a CI/CD pipeline for a Node.js application using npm, Docker, Kubernetes, Helm, and implementing a blue/green deployment strategy:

Step 1: Version Control with Git

Initialize a Git repository to manage your Node.js application code:
```
git init
```
Create and switch to a feature branch for development:
```
git checkout -b feature/my-feature
```
Make changes, add files, and commit your code:

# Make changes and add files
```
git add .
git commit -m "Feature: Added new feature"
```
When the feature is ready, merge it back into the main branch:
```
git checkout main
git merge feature/my-feature
```
Step 2: Create a Node.js Application

Create a new Node.js application or use an existing one.

Initialize a Node.js project with npm:

```
npm init -y
```
Step 3: Develop and Test Your Application

Develop your Node.js application and use npm to install dependencies:
```
npm install
```
Start your application for local testing:
```
npm start
```
Sample package.json:

Here's a sample package.json file with dependencies and scripts:

```
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "A Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "mocha"
  },
  "author": "Your Name",
  "license": "MIT",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "mocha": "^8.4.0",
    "chai": "^4.2.0"
  }
}
```
Step 4: Code Analysis and Testing

Set up code analysis and testing tools like ESLint, Mocha, Chai, or Jest as needed. Configure them in your package.json and create test scripts.

For example, you can use Mocha and Chai for testing. Install them as development dependencies:

```
npm install --save-dev mocha chai
```
Configure your package.json to include test scripts:
```
"scripts": {
  "test": "mocha"
}
```
Write your tests using Mocha and Chai and run them:
```
npm test
```
Step 5: Dockerize Your Node.js Application

Create a Dockerfile in your project's root directory to package your Node.js application into a Docker image. Here's a sample Dockerfile:
```
# Use a base Node.js image
FROM node:14

# Create and set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install application dependencies
RUN npm install

# Copy application source code
COPY . .

# Expose a port (if your application uses one)
EXPOSE 3000

# Start the application
CMD [ "npm", "start" ]
```
Step 6: Build and Push the Docker Image

Build the Docker image using the Dockerfile:
```
docker build -t your-app:1.0.0 .
```
Push the Docker image to a container registry like Docker Hub or a private registry:
```
docker push your-docker-registry/your-app:1.0.0
```
Step 7: Kubernetes Deployment and Service

Create Kubernetes Deployment and Service manifests for your Node.js application. Define how the application should be deployed to the Kubernetes cluster.
Sample Deployment.yaml:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-app
        image: your-docker-registry/your-app:1.0.0
        ports:
        - containerPort: 3000
```
Sample Service.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: your-app-service
spec:
  selector:
    app: your-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```
Step 8: Deploy to Kubernetes

Deploy the application to the Kubernetes cluster using the Kubernetes manifests:
```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
Step 9: Helm Charts for Different Environments

Create Helm charts for deploying your Node.js application to different environments (SIT, UAT, Perf, Prod).
Step 10: Blue/Green Deployment with Helm

Implement a blue/green deployment strategy using Helm. Create separate Helm releases for blue and green deployments. Configure Helm charts and values.yaml files for both blue and green environments with the appropriate image tags and configurations.

Deploy the blue release:

```
helm install my-app-blue -f values-blue.yaml ./my-app-chart
```
Test the blue environment.

If the blue environment passes testing, deploy the green release:

```
helm install my-app-green -f values-green.yaml ./my-app-chart
```
Gradually switch traffic to the green environment if it passes testing.

Monitor both blue and green environments to ensure the new version is stable.

This comprehensive guide covers setting up a CI/CD pipeline for a Node

</b></details>

<details>
<summary>What is Continuous Integration?</summary><br><b>

A development practice where developers integrate code into a shared repository frequently. It can range from a couple of changes every day or a week to a couple of changes in one hour in larger scales.

Each piece of code (change/patch) is verified, to make the change is safe to merge. Today, it's a common practice to test the change using an automated build that makes sure the code can be integrated. It can be one build which runs several tests in different levels (unit, functional, etc.) or several separate builds that all or some has to pass in order for the change to be merged into the repository.
</b></details>

<details>
<summary>What is Continuous Deployment?</summary><br><b>

A development strategy used by developers to release software automatically into production where any code commit must pass through an automated testing phase. Only when this is successful is the release considered production worthy. This eliminates any human interaction and should be implemented only after production-ready pipelines have been set with real-time monitoring and reporting of deployed assets. If any issues are detected in production it should be easy to rollback to previous working state.

For more info please read [here](https://www.atlassian.com/continuous-delivery/continuous-deployment)
</b></details>

<details>
<summary>Can you describe an example of a CI (and/or CD) process starting the moment a developer submitted a change/PR to a repository?</summary><br><b>

There are many answers for such a question, as CI processes vary, depending on the technologies used and the type of the project to where the change was submitted.
Such processes can include one or more of the following stages:

* Compile 
* Build
* Install
* Configure
* Update
* Test

An example of one possible answer:

A developer submitted a pull request to a project. The PR (pull request) triggered two jobs (or one combined job). One job for running lint test on the change and the second job for building a package which includes the submitted change, and running multiple api/scenario tests using that package. Once all tests passed and the change was approved by a maintainer/core, it's merged/pushed to the repository. If some of the tests failed, the change will not be allowed to merged/pushed to the repository.

A complete different answer or CI process, can describe how a developer pushes code to a repository, a workflow then triggered to build a container image and push it to the registry. Once in the registry, the k8s cluster is applied with the new changes.
</b></details>

<details>
<summary>What is Continuous Delivery?</summary><br><b>

A development strategy used to frequently deliver code to QA and Ops for testing. This entails having a staging area that has production like features where changes can only be accepted for production after a manual review. Because of this human entanglement there is usually a time lag between release and review making it slower and error prone as compared to continuous deployment.

For more info please read [here](https://www.atlassian.com/continuous-delivery/continuous-deployment)
</b></details>

<details>
<summary>What is difference between Continuous Delivery and Continuous Deployment?</summary><br><b>

Both encapsulate the same process of deploying the changes which were compiled and/or tested in the CI pipelines.<br>
The difference between the two is that Continuous Delivery isn't fully automated process as opposed to Continuous Deployment where every change that is tested in the process is eventually deployed to production. In continuous delivery someone is either approving the deployment process or the deployment process is based on constraints and conditions (like time constraint of deploying every week/month/...)
</b></details>

<details>
<summary>What CI/CD best practices are you familiar with? Or what do you consider as CI/CD best practice?</summary><br><b>

* Commit and test often.
* Testing/Staging environment should be a clone of production environment.
* Clean up your environments (e.g. your CI/CD pipelines may create a lot of resources. They should also take care of cleaning up everything they create)
* The CI/CD pipelines should provide the same results when executed locally or remotely
* Treat CI/CD as another application in your organization. Not as a glue code.
* On demand environments instead of pre-allocated resources for CI/CD purposes
* Stages/Steps/Tasks of pipelines should be shared between applications or microservices (don't re-invent common tasks like "cloning a project")
</b></details>

<details>
<summary>You are given a pipeline and a pool with 3 workers: virtual machine, baremetal and a container. How will you decide on which one of them to run the pipeline?</summary><br><b>

The decision on which type of worker (virtual machine, bare-metal, or container) to use for running a pipeline would depend on several factors, including the nature of the pipeline, the requirements of the software being built, the available resources, and the specific goals and constraints of the development and deployment process. Here are some considerations that can help in making the decision:

1. Pipeline requirements
2. Resource availability
3. Scalability and flexibility
4. Deployment and isolation requirements
5. Security considerations
6. Development and operational workflows
7. Cost considerations

Based on these considerations, the appropriate choice of worker (virtual machine, bare-metal, or container) for running the pipeline would be determined by weighing the pros and cons of each option and aligning with the specific requirements, resources, and goals of the development and deployment process. It may also be useful to consult with relevant stakeholders, such as developers, operations, and infrastructure teams, to gather input and make an informed decision.
</b></details>

<details>
<summary>Where do you store CI/CD pipelines? Why?</summary><br><b>

There are multiple approaches as to where to store the CI/CD pipeline definitions:

1. App Repository - store them in the same repository of the application they are building or testing (perhaps the most popular one)
2. Central Repository - store all organization's/project's CI/CD pipelines in one separate repository (perhaps the best approach when multiple teams test the same set of projects and they end up having many pipelines)
3. CI repo for every app repo - you separate CI related code from app code but you don't put everything in one place (perhaps the worst option due to the maintenance)
4. The platform where the CI/CD pipelines are being executed (e.g. Kubernetes Cluster in case of Tekton/OpenShift Pipelines).
</b></details>

<details>
<summary>How do you perform plan capacity for your CI/CD resources? (e.g. servers, storage, etc.)</summary><br><b>

Capacity planning for CI/CD resources involves estimating the resources required to support the CI/CD pipeline and ensuring that the infrastructure has enough capacity to meet the demands of the pipeline. Here are some steps to perform capacity planning for CI/CD resources:

1. Analyze workload
2. Monitor current usage
3. Identify resource bottlenecks
4. Forecast future demand
5. Plan for growth
6. Consider scalability and elasticity
7. Evaluate cost and budget
8. Continuously monitor and adjust

By following these steps, you can effectively plan the capacity for your CI/CD resources, ensuring that your pipeline has sufficient resources to operate efficiently and meet the demands of your development process.
</b></details>

<details>
<summary>How would you structure/implement CD for an application which depends on several other applications?</summary><br><b>

Implementing Continuous Deployment (CD) for an application that depends on several other applications requires careful planning and coordination to ensure smooth and efficient deployment of changes across the entire ecosystem. Here are some general steps to structure/implement CD for an application with dependencies:

1. Define the deployment pipeline
2. Automate the deployment process
3. Version control and dependency management
4. Continuous integration and testing
5. Rolling deployments
6. Monitor and manage dependencies
7. Testing across the ecosystem
8. Rollback and recovery strategies
9. Security and compliance
10. Documentation and communication

Implementing CD for an application with dependencies requires careful planning, coordination, and automation to ensure efficient and reliable deployments. By following best practices such as automation, version control, testing, monitoring, rollback strategies, and effective communication, you can ensure a smooth and successful CD process for your application ecosystem.
</b></details>

<details>
<summary>How do you measure your CI/CD quality? Are there any metrics or KPIs you are using for measuring the quality?</summary><br><b>

Measuring the quality of CI/CD processes is crucial to identify areas for improvement, ensure efficient and reliable software delivery, and achieve continuous improvement. Here are some commonly used metrics and KPIs (Key Performance Indicators) to measure CI/CD quality:

1. Build Success Rate: This metric measures the percentage of successful builds compared to the total number of builds. A high build success rate indicates that the majority of builds are successful and the CI/CD pipeline is stable.
2. Build and Deployment Time: This metric measures the time it takes to build and deploy changes from code commit to production. Faster build and deployment times indicate shorter feedback loops and faster time to market.
3. Deployment Frequency: This metric measures the frequency of deployments to production within a given time period. Higher deployment frequency indicates faster release cycles and more frequent updates to production.
4. Mean Time to Detect (MTTD): This metric measures the average time it takes to detect issues or defects in the CI/CD pipeline or production environment. Lower MTTD indicates faster detection and resolution of issues, leading to higher quality and more reliable deployments.
5. Mean Time to Recover (MTTR): This metric measures the average time it takes to recover from issues or incidents in the CI/CD pipeline or production environment. Lower MTTR indicates faster recovery and reduced downtime, leading to higher availability and reliability.
6. Feedback Loop Time: This metric measures the time it takes to receive feedback on code changes, including code reviews, test results, and other feedback mechanisms. Faster feedback loop times enable quicker iterations and faster improvements in the CI/CD process.
7. Customer Satisfaction: This metric measures the satisfaction of end-users or customers with the quality and reliability of the deployed software. Higher customer satisfaction indicates that the CI/CD process is delivering high-quality software that meets customer expectations.

These are just some examples of metrics and KPIs that can be used to measure the quality of CI/CD processes. It's important to choose metrics that align with the goals and objectives of your organization and regularly track and analyze them to continuously improve the CI/CD process and ensure high-quality software delivery.
</b></details>

#### CI/CD - Jenkins

<details>
<summary>What is Jenkins? What have you used it for?</summary><br><b>

Jenkins is an open source automation tool written in Java with plugins built for Continuous Integration purpose. Jenkins is used to build and test your software projects continuously making it easier for developers to integrate changes to the project, and making it easier for users to obtain a fresh build. It also allows you to continuously deliver your software by integrating with a large number of testing and deployment technologies.

Jenkins integrates development life-cycle processes of all kinds, including build, document, test, package, stage, deploy, static analysis and much more.

</b></details>

<details>
<summary>What are the advantages of Jenkins over its competitors? Can you compare it to one of the following systems?

  * Travis
  * Bamboo
  * Teamcity
  * CircleCI</summary><br><b>

  Jenkins has several advantages over its competitors, including Travis, Bamboo, TeamCity, and CircleCI. Here are some of the key advantages:

1. Open-source and free
2. Customizable and flexible
3. Wide range of integrations and Plugins
4. Active and supportive community

When comparing Jenkins to its competitors, there are some key differences in terms of features and capabilities. For example:

- Travis: Travis is a cloud-based CI/CD platform that is known for its ease of use and fast setup. However, it has fewer customization options and integrations compared to Jenkins.
- Bamboo: Bamboo is a CI/CD tool from Atlassian, the makers of JIRA and Confluence. It provides a range of features for building, testing, and deploying software, but it can be more expensive and complex to set up compared to Jenkins.
- TeamCity: TeamCity is a CI/CD tool from JetBrains, the makers of IntelliJ IDEA. It provides a range of features for building, testing, and deploying software, but it can be more complex and resource-intensive compared to Jenkins.
- CircleCI: CircleCI is a cloud-based CI/CD platform that is known for its fast build times and easy integration with GitHub. However, it can be more expensive compared to Jenkins, especially for larger projects.
</b></details>

<details>
<summary>What are the limitations or disadvantages of Jenkins?</summary><br><b>

This might be considered to be an opinionated answer:

* Old fashioned dashboards with not many options to customize it
* Containers readiness (this has improved with Jenkins X)
* By itself, it doesn't have many features. On the other hand, there many plugins created by the community to expand its abilities
* Managing Jenkins and its pipelines as a code can be one hell of a nightmare
</b></details>

<details>
<summary>Explain the following:

- Job
- Build
- Plugin
- Node or Worker
- Executor</summary><br><b>
- Job is an automation definition = what and where to execute once the user clicks on "build" 
- Build is a running instance of a job. You can have one or more builds at any given point of time (unless limited by configuration)
- A worker is the machine/instance on which the build is running. When a build starts, it "acquires" a worker out of a pool to run on it.
- An executor is variable of the worker, defining how many builds can run on that worker in parallel. An executor value of 3 means, that 3 builds can run at any point on that executor (not necessarily of the same job. Any builds)
</b></details>

<details>
<summary>What plugins have you used in Jenkins?</summary><br><b>

Jenkins has a vast library of plugins, and the most commonly used plugins depend on the specific needs and requirements of each organization. However, here are some of the most popular and widely used plugins in Jenkins:

    Pipeline: This plugin allows users to create and manage complex, multi-stage pipelines using a simple and easy-to-use scripting language. It provides a powerful and flexible way to automate the entire software delivery process, from code commit to deployment.

    Git: This plugin provides integration with Git, one of the most popular version control systems used today. It allows users to pull code from Git repositories, trigger builds based on code changes, and push code changes back to Git.

    Docker: This plugin provides integration with Docker, a popular platform for building, shipping, and running distributed applications. It allows users to build and run Docker containers as part of their build process, enabling easy and repeatable deployment of applications.

    JUnit: This plugin provides integration with JUnit, a popular unit testing framework for Java applications. It allows users to run JUnit tests as part of their build process and generates reports and statistics on test results.

    Cobertura: This plugin provides code coverage reporting for Java applications. It allows users to measure the code coverage of their tests and generate reports on which parts of the code are covered by tests.

    Email Extension: This plugin provides advanced email notification capabilities for Jenkins. It allows users to customize the content and format of email notifications, including attachments, and send notifications to specific users or groups based on build results.

    Artifactory: This plugin provides integration with Artifactory, a popular artifact repository for storing and managing binaries and dependencies. It allows users to publish and retrieve artifacts from Artifactory as part of their build process.

    SonarQube: This plugin provides integration with SonarQube, a popular code quality analysis tool. It allows users to run code quality checks and generate reports on code quality metrics such as code complexity, code duplication, and code coverage.
</b></details>

<details>
<summary>Have you used Jenkins for CI or CD processes? Can you describe them?</summary><br><b>

Let's assume we have a web application built using Node.js, and we want to automate its build and deployment process using Jenkins. Here is how we can set up a simple CI/CD pipeline using Jenkins:

1. Install Jenkins: We can install Jenkins on a dedicated server or on a cloud platform such as AWS or Google Cloud.
2. Install necessary plugins: Depending on the specific requirements of the project, we may need to install plugins such as NodeJS, Git, Docker, and any other plugins required by the project.
3. Create a new job: In Jenkins, a job is a defined set of instructions for automating a particular task. We can create a new job and configure it to build our Node.js application.
4. Configure the job: We can configure the job to pull the latest code from the Git repository, install any necessary dependencies using Node.js, run unit tests, and build the application using a build script.
5. Set up a deployment environment: We can set up a separate environment for deploying the application, such as a staging or production environment. We can use Docker to create a container image of the application and deploy it to the environment.
6. Set up continuous deployment: We can configure the job to automatically deploy the application to the deployment environment if the build and tests pass.
7. Monitor and troubleshoot: We can monitor the pipeline for errors or failures and troubleshoot any issues that arise.

This is just a simple example of a CI/CD pipeline using Jenkins, and the specific implementation details may vary depending on the requirements of the project.
</b></details>

<details>
<summary>What type of jobs are there? Which types have you used?</summary><br><b>

In Jenkins, there are various types of jobs, including:

1. Freestyle job: This is the most common type of job in Jenkins, which allows users to define custom build steps and configure various options, including build triggers, SCM polling, and post-build actions.
2. Pipeline job: Pipeline job is a newer feature in Jenkins that allows users to define a pipeline of jobs that can be executed in a specific order. The pipeline can be defined using a Jenkinsfile, which provides a script-like syntax for defining the pipeline stages, steps, and conditions.
3. Multi-configuration job: This type of job allows users to execute the same job with multiple configurations, such as different operating systems, browsers, or devices. Jenkins will execute the job for each configuration specified, providing a matrix of results.
4. Maven job: This type of job is specifically designed for building Java applications using the Maven build tool. Jenkins will execute the Maven build process, including compiling, testing, and packaging the application.
5. Parameterized job: This type of job allows users to define parameters that can be passed into the build process at runtime. Parameters can be used to customize the build process, such as specifying the version number or target environment.
</b></details>

<details>
<summary>How did you report build results to users? What ways are there to report the results?</summary><br><b>

You can report via:
  * Emails
  * Messaging apps
  * Dashboards

Each has its own disadvantages and advantages. Emails for example, if sent too often, can be eventually disregarded or ignored.
</b></details>

<details>
<summary>You need to run unit tests every time a change submitted to a given project. Describe in details how your pipeline would look like and what will be executed in each stage</summary><br><b>

The pipelines will have multiple stages:

  * Clone the project
  * Install test dependencies (for example, if I need tox package to run the tests, I will install it in this stage)
  * Run unit tests
  * (Optional) report results (For example an email to the users)
  * Archive the relevant logs/files
</b></details>

<details>
<summary>How to secure Jenkins?</summary><br><b>

 [Jenkins documentation](https://www.jenkins.io/doc/book/security/securing-jenkins/) provides some basic intro for securing your Jenkins server.
</b></details>

<details>
<summary>Describe how do you add new nodes (agents) to Jenkins</summary><br><b>

You can describe the UI way to add new nodes but better to explain how to do in a way that scales like a script or using dynamic source for nodes like one of the existing clouds.
</b></details>

<details>
<summary>How to acquire multiple nodes for one specific build?</summary><br><b>

To acquire multiple nodes for a specific build in Jenkins, you can use the "Parallel" feature in the pipeline script. The "Parallel" feature allows you to run multiple stages in parallel, and each stage can run on a different node.

Here is an example pipeline script that demonstrates how to acquire multiple nodes for a specific build:

```tsx
pipeline {
    agent any
    stages {
        stage('Build') {
            parallel {
                stage('Node 1') {
                    agent { label 'node1' }
                    steps {
                        // Run build commands on Node 1
                    }
                }
                stage('Node 2') {
                    agent { label 'node2' }
                    steps {
                        // Run build commands on Node 2
                    }
                }
                stage('Node 3') {
                    agent { label 'node3' }
                    steps {
                        // Run build commands on Node 3
                    }
                }
            }
        }
        stage('Deploy') {
            agent any
            steps {
                // Deploy the built artifacts
            }
        }
    }
}
```

In this example, the "Build" stage has three parallel stages, each running on a different node labeled as "node1", "node2", and "node3". The "Deploy" stage runs after the build is complete and runs on any available node.

To use this pipeline script, you will need to have the three nodes (node1, node2, and node3) configured in Jenkins. You will also need to ensure that the necessary build commands and dependencies are installed on each node.
</b></details>

<details>
<summary>Whenever a build fails, you would like to notify the team owning the job regarding the failure and provide failure reason. How would you do that?</summary><br><b>

In Jenkins, you can use the "Email Notification" plugin to notify a team when a build fails. Here are the steps to set up email notifications for failed builds:

1. Install the "Email Notification" plugin if it's not already installed in Jenkins.
2. Go to the Jenkins job configuration page and click on "Configure".
3. Scroll down to the "Post-build Actions" section and click on "Add post-build action".
4. Select "Editable Email Notification" from the list of options.
5. Fill out the required fields, such as the recipient email addresses, subject line, and email content. You can use Jenkins environment variables, such as ${BUILD_URL} and ${BUILD_LOG}, to include build-specific information in the email content.
6. In the "Advanced Settings" section, select the "Send to recipients" option and choose "Only on failure" from the dropdown menu.
7. Click "Save" to save the job configuration.

With this setup, Jenkins will send an email notification to the specified recipients whenever a build fails, providing them with the failure reason and any other relevant information.
</b></details>

<details>
<summary>There are four teams in your organization. How to prioritize the builds of each team? So the jobs of team x will always run before team y for example</summary><br><b>

In Jenkins, you can prioritize the builds of each team by using the "Priority Sorter" plugin. Here are the steps to set up build prioritization:

1. Install the "Priority Sorter" plugin if it's not already installed in Jenkins.
2. Go to the Jenkins system configuration page and click on "Configure Global Security". Scroll down to the "Access Control" section and click on "Per-project basis".
3. In the "Project default actions" section, select "Configure build triggers and execution" from the dropdown menu. Click on "Add user or group" and add the groups that represent each team in your organization.
4. Go to each Jenkins job configuration page and click on "Configure". Scroll down to the "Build Environment" section and click on "Add build step". Select "Set build priority with Priority Sorter" from the list of options.
5. Set the priority of the job based on the team that owns it. For example, if Team X owns the job, set the priority to a higher value than the jobs owned by Team Y. Click "Save" to save the job configuration.

With this setup, Jenkins will prioritize the builds of each team based on the priority value set in the job configuration. Jobs owned by Team X will have a higher priority than jobs owned by Team Y, ensuring that they are executed first.
</b></details>

<details>
<summary>If you are managing a dozen of jobs, you can probably use the Jenkins UI. But how do you manage the creation and deletion of hundreds of jobs every week/month?</summary><br><b>

Managing the creation and deletion of hundreds of jobs every week/month in Jenkins can be a daunting task if done manually through the UI. Here are some approaches to manage large numbers of jobs efficiently:

1. Use job templates
2. Use Job DSL
3. Use Jenkins REST API
4. Use a configuration management tool
5. Use a Jenkins job management tool
</b></details>

<details>
<summary>What are some of Jenkins limitations?</summary><br><b>

  * Testing cross-dependencies (changes from multiple projects together)
  * Starting builds from any stage (although Cloudbees implemented something called checkpoints)
</b></details>

<details>
<summary>What is the different between a scripted pipeline to declarative pipeline? Which type are you using?</summary><br><b>

Jenkins supports two types of pipelines: Scripted pipelines and Declarative pipelines.

Scripted pipelines use Groovy syntax and provide a high degree of flexibility and control over the build process. Scripted pipelines allow developers to write custom code to handle complex scenarios, but can be complex and hard to maintain.

Declarative pipelines are a newer feature and provide a simpler way to define pipelines using YAML syntax. Declarative pipelines provide a more structured and opinionated way to define builds, making it easier to get started with pipelines and reducing the risk of errors.

Some key differences between the two types of pipelines are:

1. Syntax: Scripted pipelines use Groovy syntax while declarative pipelines use YAML syntax.
2. Structure: Declarative pipelines have a more structured format and define specific stages, while scripted pipelines provide more flexibility in defining build stages and steps.
3. Error handling: Declarative pipelines provide a more comprehensive error handling system with built-in conditions and actions, while scripted pipelines require more manual error handling.
4. Ease of use: Declarative pipelines are easier to use for beginners and provide a simpler syntax, while scripted pipelines require more expertise in Groovy and can be more complex.
5. Maintenance: Declarative pipelines are easier to maintain and can be modified with less effort compared to scripted pipelines, which can be more difficult to modify and extend over time.

I am familiar with both types of pipelines, but generally prefer declarative pipelines for their ease of use and simplicity.
</b></details>

<details>
<summary>How would you implement an option of a starting a build from a certain stage and not from the beginning?</summary><br><b>

To implement an option of starting a build from a certain stage and not from the beginning in a Jenkins pipeline, we can use the `when` directive along with a custom parameter to determine the starting stage. Here are the steps to implement this:

1. Add a custom parameter to the pipeline. This parameter can be a simple string or a more complex data type like a map.
    
    ```tsx
    parameters {
        string(name: 'START_STAGE', defaultValue: '', description: 'The name of the stage to start the build from')
    }
    ```
    
2. Use the `when` directive to conditionally execute stages based on the value of the `START_STAGE` parameter.
    
    ```tsx
    stage('Build') {
        when {
            expression {
                params.START_STAGE == '' || currentStage.name == params.START_STAGE
            }
        }
        // Build steps go here
    }
    
    stage('Test') {
        when {
            expression {
                params.START_STAGE == '' || currentStage.name == params.START_STAGE || previousStage.result == 'SUCCESS'
            }
        }
        // Test steps go here
    }
    
    stage('Deploy') {
        when {
            expression {
                params.START_STAGE == '' || currentStage.name == params.START_STAGE || previousStage.result == 'SUCCESS'
            }
        }
        // Deploy steps go here
    }
    ```
    

  In this example, we use the `when` directive to execute each stage only if the `START_STAGE` parameter is empty or matches the current stage's name. Additionally, for the `Test` and `Deploy` stages, we also check if the previous stage executed successfully before running.

3. Trigger the pipeline and pass the `START_STAGE` parameter as needed.
    
    ```tsx
    pipeline {
        agent any
        parameters {
            string(name: 'START_STAGE', defaultValue: '', description: 'The name of the stage to start the build from')
        }
        stages {
            stage('Build') {
                // Build steps go here
            }
            stage('Test') {
                // Test steps go here
            }
            stage('Deploy') {
                // Deploy steps go here
            }
        }
    }
    ```
    

When triggering the pipeline, you can pass the `START_STAGE` parameter to start the build from a specific stage.

For example, if you want to start the build from the `Test` stage, you can trigger the pipeline with the `START_STAGE` parameter set to `'Test'`:

```tsx
pipeline?START_STAGE=Test
```

This will cause the pipeline to skip the `Build` stage and start directly from the `Test` stage.
</b></details>

<details>
<summary>Do you have experience with developing a Jenkins plugin? Can you describe this experience?</summary><br><b>

Developing a Jenkins plugin requires knowledge of Java and familiarity with Jenkins API. The process typically involves setting up a development environment, creating a new plugin project, defining the plugin's extension points, and implementing the desired functionality using Java code. Once the plugin is developed, it can be packaged and deployed to Jenkins.

The Jenkins plugin ecosystem is extensive, and there are many resources available to assist with plugin development, including documentation, forums, and online communities. Additionally, Jenkins provides tools such as Jenkins Plugin POM Generator and Jenkins Plugin Manager to help with plugin development and management.
</b></details>

<details>
<summary>Have you written Jenkins scripts? If yes, what for and how they work?</summary><br><b>
</b></details>
