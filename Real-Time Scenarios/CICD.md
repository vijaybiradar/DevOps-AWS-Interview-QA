# Complete Jenkins CI/CD  Project and its Documentation.

Let's make a beautiful **CI/CD Pipeline** for your Node JS Application.

## What is CI/CD Pipeline?
- **`CI`** or **`Continuous Integration`** is the practice of automating the integration of code changes from multiple developers into a **single codebase**. It is a software development practice where the developers commit their work frequently into the central code repository (Github or Stash). Then there are automated tools that build the newly committed code and do a code review, etc as required upon integration.
- The `key goals of Continuous Integration` are to find and `address bugs quicker`, `make the process of integrating code across a team of developers easier`, `improve software quality` and `reduce the time` it takes to release new feature updates. 

- **`CD`** or **`Continuous Delivery`** is carried out after **Continuous Integration** to make sure that we can release new changes to our customers quickly in an `error-free way`. This includes running integration and regression tests in the staging area (similar to the production environment) so that the final release is not broken in production. It ensures to automate the release process so that we have a release-ready product at all times and we can deploy our application at any point in time. 

---
### Note: Before Heading to the Project first fork this repository from this link.

Fork from G itHub [Repository](https://github.com/LondheShubham153/node-todo-cicd.git) 

---

# Task-01

### Create a connection to your Jenkins job and your GitHub Repository via GitHub Integration. 

- Let's create a **`freestyle project`** for forked repository in which we will connect **Jenkins** and **GitHub repository** via **GitHub Integration**.

**`Forked into my Personal Account`**

![Screenshot from 2023-04-07 19-09-54](https://user-images.githubusercontent.com/76991475/230708894-2c91d3e9-4196-4424-9311-3381c7cf184d.png)

**`Creation of freestyle project on Jenkins`**

![Screenshot from 2023-04-07 19-11-24](https://user-images.githubusercontent.com/76991475/230708897-d8e78678-5ec3-447f-9d3e-e8ba4455b888.png)

---
## What is GitHub Webhooks ?
- A GitHub webhook is a mechanism that allows GitHub to send a notification to an external service (such as Jenkins) when certain events occur in a GitHub repository, such as when a new commit is pushed, a pull request is opened or closed, or a new release is published.

### Steps:
- In your GitHub repository, go to "Settings" and then click on "Webhooks".
- Click on "Add webhook" and specify the URL that GitHub should send the webhook to.
- Select the events that should trigger the webhook.
Optionally, you can add a secret key to the webhook for added security.
- Click on "Add webhook" to save your webhook configuration.

![Screenshot from 2023-04-08 13-10-21](https://user-images.githubusercontent.com/76991475/230709823-50dda19f-4f73-40fe-8fab-43591ed24f85.png)

### Now Since this our private repository we will create SSH Key Pair.

**`Public Key`** -> GitHub Profile.

**`Private Key`** -> Jenkins Profile.

So, **Open Terminal**:
- `ssh-keygen`

![Screenshot from 2023-04-08 13-18-30](https://user-images.githubusercontent.com/76991475/230710196-39a81f0d-491e-4f3c-b597-ae932a931756.png)

- To see what is Public Key :- **`cat ~/.ssh/id_rsa.pub`**

![Screenshot from 2023-04-08 13-21-16 (1)](https://user-images.githubusercontent.com/76991475/230710555-e129124b-121c-4fc4-9a5d-f0d592de30cc.png)

- Now paste your **Public Key** in  **`"Settings > SSH and GPG Keys > Create New Key > Give any name > Paste Public Key"`**.

![Screenshot from 2023-04-08 13-30-31](https://user-images.githubusercontent.com/76991475/230710662-93babcd3-31c6-47f0-bd53-ef7709d1bd2a.png)

- To see what is Private Key :- **`cat ~/.ssh/id_rsa`**

![Screenshot from 2023-04-08 13-35-48](https://user-images.githubusercontent.com/76991475/230710937-d1cdc737-3ba2-4f7a-9782-aa0e8a13a56a.png)

Now, we will paste the **Private Key** in Jenkins **`"Created_Project > Configuration > Source Code Management > Credentials > Add Jenkins > Choose SSH Private Key > Paste it"`**.

![Screenshot from 2023-04-08 13-39-18](https://user-images.githubusercontent.com/76991475/230711032-34ee245b-40f0-49e9-9fc4-ba1c71634711.png)

![Screenshot from 2023-04-08 13-39-47](https://user-images.githubusercontent.com/76991475/230711038-d93997e5-c801-41ec-bf49-18d93b2cf68b.png)

**Now we have connected everything between GitHub and Jenkins**.

## Let's start Project Testing On Jenkins.

**`Creation of freestyle project on Jenkins`**

![Screenshot from 2023-04-07 19-11-24](https://user-images.githubusercontent.com/76991475/230708897-d8e78678-5ec3-447f-9d3e-e8ba4455b888.png)

- **`Did the Basic Configurations in Jenkins`**.

![Screenshot from 2023-04-07 20-26-36](https://user-images.githubusercontent.com/76991475/230708915-e8280a17-2eff-4457-8e4a-4e931103d569.png)

![Screenshot from 2023-04-07 20-26-42](https://user-images.githubusercontent.com/76991475/230708919-97d02d86-6ed6-44a5-8472-35e9695790a9.png)

- **`Final Output`**

![Screenshot from 2023-04-07 20-22-24](https://user-images.githubusercontent.com/76991475/230708905-b18d7708-967c-413f-909d-bd29601531af.png)

---
# Task-02

### Let's first build Docker Image and run on port 8000.
```
docker build . -t todo-node-app
docker run -d --name node-todo-app -p 8000:8000 todo-node-app
```
![Screenshot from 2023-04-08 00-33-10](https://user-images.githubusercontent.com/76991475/230708921-5f0d4a3a-5c10-4958-bfd3-7509528847b8.png)

**`Final Output`**

![Screenshot from 2023-04-08 00-55-33](https://user-images.githubusercontent.com/76991475/230708924-aa3df7ad-3ed2-4fa1-800f-8a7671e32300.png)

**`On localhost 8000`**

![Screenshot from 2023-04-08 12-39-42](https://user-images.githubusercontent.com/76991475/230708957-1f58713c-6136-4c55-8dfa-b7d73decd966.png)

---
### In the Execute shell run the application using Docker Compose Down

` docker-compose down`

![Screenshot from 2023-04-08 12-37-57](https://user-images.githubusercontent.com/76991475/230708945-46d9431c-f041-47d6-b038-7acf40925a50.png)

**`Final Output`**

![Screenshot from 2023-04-08 12-37-42](https://user-images.githubusercontent.com/76991475/230708943-9385df26-2fc6-4c43-8a98-d9fcb5969113.png)

**Since docker downed website so there is no connection**

![Screenshot from 2023-04-08 12-37-34](https://user-images.githubusercontent.com/76991475/230708940-8c01e983-507d-4b0a-ac1f-267fd29fdeee.png)

---
### In the Execute shell run the application using Docker Compose Up.

`docker-compose up -d`

![Screenshot from 2023-04-08 12-39-57](https://user-images.githubusercontent.com/76991475/230708959-b3e08be1-1558-4e1d-9dcb-27e4c67064c8.png)

**`Final Output`**

![Screenshot from 2023-04-08 12-39-35](https://user-images.githubusercontent.com/76991475/230708950-7ed155f2-f156-42a2-bcfb-438f332138b7.png)

**Since docker is Up we have website connected to CI/CD Pipeline**

![Screenshot from 2023-04-08 12-39-42](https://user-images.githubusercontent.com/76991475/230708957-1f58713c-6136-4c55-8dfa-b7d73decd966.png)

---
⚛
