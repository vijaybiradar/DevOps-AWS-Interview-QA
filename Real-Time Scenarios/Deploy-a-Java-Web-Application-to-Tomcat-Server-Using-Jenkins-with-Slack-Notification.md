## Deploy-a-Java-Web-Application-to-Tomcat-Server-Using-Jenkins-with-Slack-Notification.
### **DevOps Pipeline Diagram:-**
![java web-app](https://user-images.githubusercontent.com/119833411/241620850-56e50196-4555-473c-9c28-9f45625dbea5.jpg)

### Prerequisites:-
* **Tomcat Server up and running.**
* **Jenkins Server up and running.**
* **Jenkins Integration with Slack Notification.**
* **A Java Project.**

### Reference:-

* **Tomcat Setup** https://github.com/sampathshivakumar/Installations/blob/main/Tomcat%20Installation%20on%20Amazon-Linux-2.md
* **Jenkins Setup** https://github.com/sampathshivakumar/Installations/blob/main/Jenkins%20Installation%20on%20Amazon-Linux-2.md
* **Jenkins Integration with Slack Notification** https://github.com/sampathshivakumar/Integrating-Jenkins-with-Slack-Notifications
* **Java Project** https://github.com/sampathshivakumar/Java-Web-Apps.git

### Install Git in Jenkins Server.
```
# Become a root 
sudo su -

# Install git.
yum install git -y

# Check Version of git.
git version
```
### Open Jenkins in web-browser, click on manage jenkins then select Global Tool Configuration.
![1](https://user-images.githubusercontent.com/119833411/241621062-d2cefe8c-660e-40be-8c30-99137a08356e.jpg)

![2](https://user-images.githubusercontent.com/119833411/241621114-39e07247-8c0d-4b11-9948-c907ea2a133a.jpg)

### In Git installations click Add git then select git.
![3](https://user-images.githubusercontent.com/119833411/241621155-b1810c0a-b603-476e-a6cc-3abc554e8c27.jpg)

![4](https://user-images.githubusercontent.com/119833411/241621182-b0fe0008-c576-4ea7-89aa-01238b0041c0.jpg)

### Enter Name and Path to Git executable as below then click apply and save.
![5](https://user-images.githubusercontent.com/119833411/241621236-5d289510-7b21-4601-964e-294102b66744.jpg)

**We have Successfully integrated Git with Jenkins.**

### Now go to Jenkins Dashboard select Manage Jenkins then click on Global Tool Configuration.
![2](https://user-images.githubusercontent.com/119833411/241621295-c89e6bec-9f91-4434-9da1-5fec167cb823.jpg)

### In Maven installations click Add Maven and give name and select Install automatically then Appy and Save.
![6](https://user-images.githubusercontent.com/119833411/241621319-f1fa80a5-b2da-4682-b2bb-fb2e779618a3.jpg)

**Maven is also Successfully integrated with Jenkins.**

### Install Deploy to container Plugin.
![10](https://user-images.githubusercontent.com/119833411/241621349-9dce420f-edb8-443b-aeed-7572d4df1937.jpg)

![11](https://user-images.githubusercontent.com/119833411/241621433-323f6ef1-78e4-4cd1-a141-c57a8a088098.jpg)

### Add Tomcat user credentials in Jenkins Secrets.

**You can check it in tomcat-user.xml**
![12](https://user-images.githubusercontent.com/119833411/241621570-cfe7dd3f-2001-4fad-bb17-f5d12ea3de89.jpg)

### Go to Manage Jenkins and Click on Credentials.
![13](https://user-images.githubusercontent.com/119833411/241621645-d884ba46-3667-4ec0-8850-ff138562ad19.jpg)

### Select System.
![14](https://user-images.githubusercontent.com/119833411/241621674-b4a92551-16d1-4c6c-8282-6bcb6cda0f85.jpg)

### Select Global credentials (unrestricted).
![15](https://user-images.githubusercontent.com/119833411/241621690-0423eb07-9436-4086-916f-1054736b5e38.jpg)

### Select Add Credentials.
![16](https://user-images.githubusercontent.com/119833411/241621716-bbfbb6e6-be17-4774-8a4d-a4bb6acb7625.jpg)

### Enter username and password.
![17](https://user-images.githubusercontent.com/119833411/241621744-e2004c8c-ab1b-4cbc-9d47-a59fe4d1d8a5.jpg)

### Once done you should see like this.
![18](https://user-images.githubusercontent.com/119833411/241621768-141b91dc-92a8-4b6f-8dc0-b5c98947e38d.jpg)

### Now lets create a Jenkins job.
![7](https://user-images.githubusercontent.com/119833411/241621809-61a721b2-aa53-43b7-9ccb-5338a6fc1d6e.jpg)

![9](https://user-images.githubusercontent.com/119833411/241621867-9cade719-d908-42f6-bb7b-8f615945df81.jpg)

![19](https://user-images.githubusercontent.com/119833411/241621890-636ca131-ede0-467d-a974-072906f7bafb.jpg)

![20](https://user-images.githubusercontent.com/119833411/241621914-4e616b79-00f5-4f1c-88dd-c587fbca2ae7.jpg)

### Provide Build details.
![21](https://user-images.githubusercontent.com/119833411/241621999-458b2ddd-3818-41bc-b5a8-4f22e3b90655.jpg)

### In Post-build Actions click Add Post-build Actions and Select Slack Notification and select following and click apply and save.
![22](https://user-images.githubusercontent.com/119833411/241622018-1ad74371-8800-41e5-8a64-20f82ce381f5.jpg)

### In Post-build Actions click Add Post-build Actions and Select Deploy war/ear to container.
![26](https://user-images.githubusercontent.com/119833411/241622162-0b67dcee-b579-42a6-9366-3d3c96cde1a8.jpg)

### Enter the details as following then click on apply and save.
![27](https://user-images.githubusercontent.com/119833411/241622189-406715a7-c28b-4d56-8254-73e742423066.jpg)

### Now Click Build Now.
![23](https://user-images.githubusercontent.com/119833411/241622269-b16c695a-a558-48f6-a8d6-ad893ccd16bd.jpg)

### Build Successfull.
![24](https://user-images.githubusercontent.com/119833411/241622328-ae80c3cf-8b0a-45d6-b441-345b342c267a.jpg)

### Slack Notifications.
![25](https://user-images.githubusercontent.com/119833411/241622388-8d533474-572c-4026-aada-0d57a62c7e41.jpg)

### Output.
![28](https://user-images.githubusercontent.com/119833411/241622446-c1d09f6d-4c13-4f7a-a18c-6510fe7f783b.jpg)

## Project is Successfully Completed.

**Thank you for reading this post! I hope you found it helpful. If you have any feedback or questions,Please connect with me on LinkedIn at https://www.linkedin.com/in/sampathsivakumar-boddeti-1666b810b/. Your feedback is valuable to me. Thank you!**
⚛
