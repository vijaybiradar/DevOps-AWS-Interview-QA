# Automatic Deploying with Jenkins on an AWS EC2 instance (Includes GitHub + Docker + Git)
<p> <br/ > <p>

To deploying model we try to best model accuracy. And after tuning some of the parameters and adding some more data, you expect had better accuracy than the previous model built. That' s mean you plan to deploy this model and you have to go through the trouble of building, testing and deploying the model to production again which is a lot of work.
  
You can tedious to manually run tests weekly, or even daily, which is where a CI/CD or Continuous Integration/ Continuous Deployment Pipeline like Jenkins comes in. Jenkins allows us to automatically run tests weekly, daily, hourly, or even on commit to a repository. 
In this article, I will show you how we can use a powerful tool called Jenkins to automate this process.

![Jenkins_DataSci](https://user-images.githubusercontent.com/91700155/171888179-281f8d18-7585-40fb-820c-19a2598a8524.png)

  
## What is Jenkins?
In addition to automating other routine development tasks, Jenkins provides a simple way to set up a continuous integration or continuous delivery (CI/CD) environment for virtually any language and source code repository combination that uses pipelines. While Jenkins doesn't eliminate the need to create scripts for individual steps, it does offer a faster and more robust way to integrate your entire chain of build, test, and deployment tools than you can easily build yourself.

For example, you can set up Jenkins to automatically detect code commit in a repository and automatically trigger commands either creating a Flask application using Docker building a Docker image from a Dockerfile, running unit tests or push an image to a container registry or deploy it to the production server without manually doing anything.
Let's look basic concept we need to know in order to perform some automation in our project. 


## Jenkins features
Jenkins has some features that really sell it as a CI/CD tool. These are some of them:

- Plug-ins
- Easy to set up
- Supports most environments
- Open-source
- Easy distribution
  
## Before the practical side, there are some important terms need to know.
  
## - Jenkins Job  
Jobs are the heart of Jenkins' build process. In Jenkins, a job can be thought of as a specific task to achieve a necessary purpose. We can also create and build these jobs to test our app or project. Jenkins provides the following types of build jobs that a user can create based on need.
Creating Job is very easy in Jenkins but in a software environment, you may not build a single job but instead, you’ll be doing what is referred to as a pipeline.
  
  
## - Jenkins Pipeline
In simple words, a pipeline is a set of interconnected tasks executed in a specific order. Additionally, Jenkins Pipeline is a plugin package that helps users implement and integrate continuous delivery pipelines into Jenkins. You can also use Pipeline to create complex or simple deployment pipelines as code through the Pipeline domain-specific language (DSL) syntax. Then, the following states represent a continuous delivery Line.

Types of Jenkins Pipeline:

- Declarative pipeline: This is a feature that supports the pipeline as a code concept. It makes the pipeline code easier to read and write. This code is written in a Jenkinsfile which can be checked into a source control management system such as Git.
  
- Scripted pipeline: This is one of the old ways of writing the code. Using this method, the pipeline code is written on the Jenkins User Interface instance instead of writing it in a file. Though both these pipelines perform the same function and they use the same scripting language(Groovy).
  
![1 Jenkins Continuous Deliver Pipeline 2](https://user-images.githubusercontent.com/91700155/171913858-2d5a00ab-dfe9-4055-9848-b9c4fd73b4ee.png)
 
In this example, we create a trained Machine Learning model that assisting the doctor's decision-making process on disease prediction by detecting disease symptoms who coming from the patient, I deployed as an API using Flask .
I structured my Jenkins pipeline to:

Pull changes from the repository when a commit is made >>> Build Docker Image >>> Built the model. 
  
## Steps
### Installation 
Install Jenkins, Git and Docker in your instance. Startup a Jenkins server and install Git, Docker, Pipeline and build plugins.
  
This is part 2 of my series on deploying Jenkins to create an efficient CI/CD Pipeline. 
In part one, I covered installing and launching Jenkins,Git,Docker,Plugins on a AWS EC2 instance. You can find part one link below:
  - https://github.com/sedaatalay/How-to-Install-Jenkins-Git-Docker-on-Amazon-EC2-Instance
  
Push the code to a repository, in this article I used Github. You can find the code for this article here.
  - https://github.com/sedaatalay/jenk_deneme
  
My working directory:
.
├── app.py
├── Dockerfile
├── Jenkinsfile
├── ML_model
│   └── first_model.sav
└── requirements.txt

<img width="203" alt="Ekran Resmi 2022-06-03 19 19 53" src="https://user-images.githubusercontent.com/91700155/171905592-f2ea6017-d0d1-4da9-b48b-413b66369b07.png">
  
- Next, we have to tell Jenkins to start building the pipeline whenever a change is made to the code repository. To do this, you need to add the Jenkins webhook to the GitHub repository for Github to contact Jenkins if there is a change in the code. 
Click on settings in your code repository:
  
<img width="984" alt="Ekran Resmi 2022-06-03 19 22 17" src="https://user-images.githubusercontent.com/91700155/171906611-1af366fe-1f29-444d-8a8a-d62bbbeb18b3.png">

- Click the webhooks panel and click on Add Webhook
- Then use the public DNS or public IP of your Jenkins server and add “/github-webhook/” at the end, then select application/json as the content type. In the “Which events would you like to trigger this webhook?”, select the individual events and select pushes only and click on add webhook.
  
<img width="1188" alt="Ekran Resmi 2022-06-02 19 24 48" src="https://user-images.githubusercontent.com/91700155/171907453-d8a5d626-5589-48f1-a27e-1c306e982a32.png">
  
<img width="1044" alt="Ekran Resmi 2022-06-02 19 26 27" src="https://user-images.githubusercontent.com/91700155/171908219-88cc96e9-366d-41d1-b291-75cdeee90bf9.png">

<img width="1097" alt="Ekran Resmi 2022-06-02 19 26 34" src="https://user-images.githubusercontent.com/91700155/171908103-2d39c7c2-337b-42ec-ab6c-97a5e176c39f.png">
 
- Now, head over to Jenkins and click on New Item
  
<img width="1326" alt="Ekran Resmi 2022-06-02 19 31 25" src="https://user-images.githubusercontent.com/91700155/171910100-6ce5c49b-67a5-477b-8754-cc587e1ba016.png">

- Enter an Item name, Select Pipeline and click OK 
  
<img width="1079" alt="Ekran Resmi 2022-06-02 19 32 04" src="https://user-images.githubusercontent.com/91700155/171909033-89d08108-82cd-4f60-a965-ca35b2f78dc8.png">

- Scroll down from General to Build Triggers and select Github hook trigger for GitScm polling. What that simply does is that helps us build our pipeline whenever there’s a commit in our code.

<img width="915" alt="Ekran Resmi 2022-06-02 19 42 02" src="https://user-images.githubusercontent.com/91700155/171909203-b0f2a55c-5b5a-4b62-98bc-8d328d10d43d.png">

- Then Scroll to Pipeline and in the Definition select Pipeline script from SCM from the dropdown. In the SCM(Source Code Management) select Git and put in the Repository URL, if your repo is private you need to create a credential using your username and password. If it is not private yo do not need to.
In the Branch Specifier, you can select master or any branch your code repository is in and the Script Path select Jenkinsfile, the Jenkinsfile is where I wrote my pipeline for the project. Then select Save.

<img width="1067" alt="Ekran Resmi 2022-06-02 19 46 23" src="https://user-images.githubusercontent.com/91700155/171909434-d2b268f5-2d8c-45e6-9fcf-070581ae2684.png">

## Building Docker Image using Jenkins Pipeline 
Add docker plugin to Jenkins
- Go to the Jenkins Dashboard >> Manage Jenkins >> Manage plugin then tap on the available and type Docker, you need to install docker as well as docker pipeline plugin. Also you can install the requirements for your project.
  
![1_hKm3slIPamulZFO6_ZduWA](https://user-images.githubusercontent.com/91700155/171923914-db0e2834-1d1c-4fcc-b85d-c7e83bf13cdb.png)

## Jenkins Building Docker Image and Sending to Registry
You can build this image to github jenkins_file
- Pipeline explanation
  - The job will have one step. It will run the docker build and use the jenkins build number in docker tag. With build number turn easeful to deploy or rollback based in jenkins.
  
<img width="780" alt="Ekran Resmi 2022-06-03 21 33 22" src="https://user-images.githubusercontent.com/91700155/171925336-209e83ed-85aa-4968-94d7-abe284ddd472.png">

- Now let’s build the pipeline…
<img width="858" alt="Ekran Resmi 2022-06-02 19 47 11" src="https://user-images.githubusercontent.com/91700155/171910232-9b8cfade-de31-48c9-a831-9d574d038c47.png">

- It ran through all the stages and you should see a successful run of the job.
  
If you get an error in this "Build Docker Image" section, don't forget to add the following to the command line.
  
 ```console
sudo chmod 666 /var/run/docker.sock
```
<img width="1225" alt="Ekran Resmi 2022-06-03 20 11 43" src="https://user-images.githubusercontent.com/91700155/171913186-edc790ff-8910-490d-8710-135e6a94c169.png">
  
When you do little change in your code repository and push. If you go back to Jenkins you’ll see that it has already detected the changes made already and it automatically triggers another build.

And that's how you can use Jenkins to automate these processes. 
  
  
---- 
  
  
  
  

#### Thank you :) 
<p>  <br /><br />
</p>


⚛
