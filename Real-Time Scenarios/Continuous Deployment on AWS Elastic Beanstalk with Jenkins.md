## Continuous Deployment on AWS Elastic Beanstalk with Jenkins
![AWS Elastic Beanstalk](https://user-images.githubusercontent.com/119833411/248327973-83b54959-f715-4b04-acce-e25619f799a8.gif)

### what is AWS Elastic Beanstalk?
* **AWS Elastic Beanstalk is a fully managed platform as a service (PaaS) offered by AWS for deploying and managing applications.** 
* **It simplifies the process of deploying applications by abstracting away the underlying infrastructure details and providing an easy-to-use interface.**   
* **With Elastic Beanstalk, developers can focus on writing their application code while AWS takes care of the deployment, capacity provisioning, load balancing, and automatic scaling.**
* **It supports a variety of programming languages and frameworks, including Java, .NET, PHP, Node.js, Python, Ruby, and Go.**

### Prerequisites
* **Jenkins server up and running.**
* **Basic knowledge of Elastic Beanstalk.**

### Reference 
* **Jenkins Installation on Amazon-Linux-2.** https://gist.github.com/sampathshivakumar/54449ea95540ad0fd0f0cf44beb54ff9
* **GitHub Repo** https://github.com/sampathshivakumar/Java-Web-Apps.git

### Open Amazon Elastic Beanstalk in AWS console.
![1](https://user-images.githubusercontent.com/119833411/247903469-0e49b6d9-0fbb-4836-b465-6013a88fd970.jpg)

### Create application

* **Amazon Elastic Beanstalk has two types of environment tiers to support different types of web applications.**
* **For now we are going with Web server environment**
![2](https://user-images.githubusercontent.com/119833411/247904182-e6788037-5c2e-4d99-bd7e-1464ade2bbc6.jpg)

* **I have selected Platform type as Tomcat as i want to deploy war file to Amazon Elastic Beanstalk**

![3](https://user-images.githubusercontent.com/119833411/247905586-ae33ea10-ba7f-4cdf-b8fc-2ee51e14da11.jpg)

* **For now we will go with Sample application, later we will deploy our code using jenkins.**
* **Lets go with High availability, and click on save.**

![4](https://user-images.githubusercontent.com/119833411/247905976-019f5651-1bf8-4ab7-b78b-1b7f15cbed58.jpg)

* **Select appropriate Service role and EC2 instance profile**
* **For more info you can go through this link** https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.iam.html

![5](https://user-images.githubusercontent.com/119833411/247907068-b1eee2d3-ecab-40ff-9928-3a0664572f7a.jpg)

* **EC2 instance profile**
![6](https://user-images.githubusercontent.com/119833411/247909844-bf992958-a9a1-4c8c-8882-8969e961e491.jpg)

* **Service role**
![7](https://user-images.githubusercontent.com/119833411/247910079-43b16c9f-cdf8-462f-8b1c-2d751422b5b0.jpg)

* **As i am deploying a simple stateless application, there is no need of Database so click on Skip to review.**

![8](https://user-images.githubusercontent.com/119833411/247911002-217c058b-0216-4df4-96d8-1bc3e446bfa8.jpg)

* **Review and click submit**

![9](https://user-images.githubusercontent.com/119833411/247913024-57bef371-4010-4b00-b359-529decb9ef18.jpg)

* **It will take few minutes**

![10](https://user-images.githubusercontent.com/119833411/247913796-635dcd6a-cc8d-4034-9659-7e767423182b.jpg)

**AWS Elastic Beanstalk in back groud will Create Environment,S3 Bucket, security group, target groups, Auto Scaling group, launch EC2 instances, CloudWatch alarm, load balancer, once done also checks health of application.**

**Environment successfully launched, you can check by clicking on the Domain link.**

![11](https://user-images.githubusercontent.com/119833411/247915780-2be2c464-89fd-4306-8d7e-ad3f5617c0e3.jpg)

![12](https://user-images.githubusercontent.com/119833411/247916071-d7ecda9a-0ffd-4adf-bfd3-9c4f479a432a.jpg)

### Now lets setup jenkins Pipeline to deploy our code using AWS Elastic Beanstalk.

![13](https://user-images.githubusercontent.com/119833411/247921975-ae32701a-cfba-4a2b-a039-88fbade0233c.jpg)

* **Install a plugin AWSEB Deployment**

![14](https://user-images.githubusercontent.com/119833411/247922310-47e30f63-6089-4119-ac50-d8181d9b429e.jpg)

* **Add your AWS Credentials in Jenkins Global credentials**

![15](https://user-images.githubusercontent.com/119833411/247925847-9ffd8459-9966-4f9f-b424-2ff7192f3d4c.jpg)

* **Configure Java and Maven inside Jenkins.**

![16](https://user-images.githubusercontent.com/119833411/247927393-dcd9faf4-f8ea-4e6a-b67e-bd0554539834.jpg)

### Create a Freestyle Project.

![17](https://user-images.githubusercontent.com/119833411/247927899-8ed97ebf-3e67-4217-af60-98ea9acb599a.jpg)

### Make the following configurations.

* **Source Code Management**
![18](https://user-images.githubusercontent.com/119833411/247928343-dc8ebfe2-acac-4467-ab37-7bf3322cf56e.jpg)

* **Build Triggers**
![19](https://user-images.githubusercontent.com/119833411/247928671-243383e5-95a8-42db-af81-8546a3f5aad6.jpg)

![21](https://user-images.githubusercontent.com/119833411/247929278-7dde6862-00b4-4da7-9daa-4eb5be35ed9d.jpg)

* **Build Steps**
* **Invoke top-level Maven targets**

![22](https://user-images.githubusercontent.com/119833411/247929640-5950934c-4263-4267-8c8f-333115063d00.jpg)

* **AWS Elastic Beanstalk**
![23](https://user-images.githubusercontent.com/119833411/247930405-58f739f8-0810-4bdf-8db5-9cc1d9a73cfb.jpg)

![24](https://user-images.githubusercontent.com/119833411/247934009-4fdd2a43-1390-49bf-93c3-84f168ea211d.jpg)

**You can get Bucket name from here**
![25](https://user-images.githubusercontent.com/119833411/247934305-6ca57af2-6c6d-46d2-95b0-749fffc2fa66.jpg)

### Now update index.jsp in your GitHub Project Repo, GitHub Webhook will push new code to Jenkins and start the job.

![26](https://user-images.githubusercontent.com/119833411/247935722-aa6a1bb6-561a-4840-8f5b-10b56a166716.jpg)

* **New Code is successfuly deployed**

![27](https://user-images.githubusercontent.com/119833411/247936044-e69ec787-6734-41cb-b939-010fa418135d.jpg)

### Lets see the output

![28](https://user-images.githubusercontent.com/119833411/247936583-1ba560cf-7de6-40af-a70f-0881d8d3f63d.jpg)

**we can see the previous output of Sample Application is replaced by our Application.**

### Hence we have Achived Continuous Deployment on AWS Elastic Beanstalk with Jenkins successfully.

**Thank you for reading this post! I hope you found it helpful. If you have any feedback or questions,Please connect with me on LinkedIn at https://www.linkedin.com/in/sampathsivakumar-boddeti-1666b810b/. Your feedback is valuable to me. Thank you!**








⚛
