# Host-a-website-on-ec2-by-keeping-it's-img-files-on-s3-bucket
## Description
Hi guys,
Here, im going to mention that how to host a static website on amazone ec2 instance by keeping it's image files on an s3 bucket.
We can use amazone s3 service to store our backup files. It provides unlimited storage for various use cases at a very low cost.
# Steps
1. Create instance 
2. Install httpd, PHP and insert files to document root
3. Create S3 bucket
4. Create IAM policy to use the S3 bucket
5. Create an IAM user and attach the policy
6. Configure the IAM user and copy the IMG files to S3.
7. Add rewrite rules on configuration file of apache.
8. Add bucket policy to the S3 for enabling public Access.


Now, let’s get into it!

## Step 1 : Create instance
1. Login to aws console
2. Go to EC2 dashboard
3. Select "instances" from left side menu.
4. Click "Launch instance"
5. Select the AMI then click next. [I am selecting "Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type"]
6. Choose "instance type" then click next
7.  Configure Instance Details then click next.
8.  Add the storage needed then click next.
9.  Add tags, click next
10.  Select security group for instance, click next.
11.  Click "launch".
12.  Select the key file need to use while logging in to the server.

  ![image](https://user-images.githubusercontent.com/100774483/158379207-0619b564-9ab7-43f0-a523-794d2f9d9604.png)

## Step 2 : Install httpd, PHP and insert files to document root
1. install httpd and php 

   ~~~
   yum install httpd -y
   amazone-linux-extras install php7.4 -y
   ~~~
   
2. Upload website files to default document root "/var/www/html"  

## Step 3 : Create S3 bucket
1. Go to "Amazone S3 console"
2. Click on "create bucket"

 ![image](https://user-images.githubusercontent.com/100774483/158382362-91ae353b-6454-493d-b542-0e1b69a13dfa.png)

3. Choose the "bucket name", "AWS region"

 ![image](https://user-images.githubusercontent.com/100774483/158382812-35b58534-4915-4365-96ed-27a2eb8bac6f.png)

4. Untick the "Block all public access" , So that the filec would be publically accessible.

 ![image](https://user-images.githubusercontent.com/100774483/158383364-39f2dd02-e8bf-4b39-8e53-3622ae4ede41.png)

5. Provide the tag and create bucket.

 ![image](https://user-images.githubusercontent.com/100774483/158383556-60036dd5-0605-439d-8402-94002e3ece83.png)
 ![image](https://user-images.githubusercontent.com/100774483/158383671-974f895c-b442-4021-a540-edb55534db4b.png)

## Step 4 : Create IAM policy to use the S3 bucket

1. Go to "Identity and Access Management (IAM)"
2. Select "Policys" on left side menu
3. Click "Create policy"

![image](https://user-images.githubusercontent.com/100774483/158384740-b9bce9e9-19af-411b-b9c8-9642ce978e69.png)

4.Click "json" and add below policy.

~~~
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::webserver-jibin.online",
                "arn:aws:s3:::webserver-jibin.online/*"
            ]
        }
    ]
}
~~~

![image](https://user-images.githubusercontent.com/100774483/158386962-c1210be2-040d-4011-bb07-8ca5ad105724.png)

Under resouces, we need to mention the "arn" of the created bucket. So that we would be able to copy the image files from instance to bucket.
5. Add tags
6. Provide name and description then create policy

## Step 5 Create an IAM user and attach the policy

1. Select "users" from "Identity and Access Management (IAM)"
2. Click "Add user"

![image](https://user-images.githubusercontent.com/100774483/158387976-3b863429-0aaf-4b5a-b0d7-b7dd09208f31.png)

3.Provide username and select "Access key - Programmatic access"

![image](https://user-images.githubusercontent.com/100774483/158388352-7124ce71-8951-4793-b519-0ae5a8458c0c.png)

4. On the next page, assign the created policy for the user.

![image](https://user-images.githubusercontent.com/100774483/158388667-04cf76fe-c3b0-4298-9f37-3be69c7533ef.png)

5. On next, add tag and create user.
6. Once the user is created, copy Access key ID and  Secret access key

![image](https://user-images.githubusercontent.com/100774483/158389197-51736c38-fd89-474b-95c5-9b79b3603338.png)

Now, the user has been created. 

## Step 6 : Configure the IAM user and copy the IMG files to S3.

1. Configuring the created IAM user on instance.

~~~
aws configure
AWS Access Key ID [None]: AKIAUGMQMUKSD6BTXW6W
AWS Secret Access Key [None]: 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
Default region name [None]: ap-south-1
Default output format [None]: json
~~~

2. Copy image files from /var/www/html/img/ to created s3 

~~~
aws s3 sync /var/www/html/img/ s3://webserver-jibin.online
~~~

Once all copied, we can confirm by listng the objects in s3 bucket.

~~~
 aws s3 ls s3://webserver-jibin.online
~~~

Please refer to the screenshot

![configuring IAM user and copyng image files](https://user-images.githubusercontent.com/100774483/158391605-513528b9-41d7-46f6-9332-de05b802b293.JPG)


## Step 7 : Add rewrite rules on configuration file of apache.

1. Edit configuration file of apache

~~~
vi /etc/httpd/conf/httpd.conf
~~~

2. Add the below rewrite rule

~~~
RewriteEngine On
RewriteRule ^/img/(.*)$  https://s3.ap-south-1.amazonaws.com/webserver-jibin.online/$1 [L]
~~~

This will make the redirection to s3 bucket for images.

3. Restart httpd

~~~
systemctl restart httpd.service
~~~


 ## Step 8 : Add bucket policy to the S3 for enabling public Access.
 
 
 1. Go to Amazone S3 dashboard
 2. Select the created S3 bucket
 
 ![image](https://user-images.githubusercontent.com/100774483/158427418-f34baab9-e57e-4043-b351-4a115937ee43.png)
 
 3.Select permissions
 
 4. Go to bucket policy and click on "edit".
 
 ![image](https://user-images.githubusercontent.com/100774483/158427690-07266872-e21a-46d8-aa42-4dd1dd04e0a4.png)

5. Change the current bucket policy with below.

~~~
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::webserver-jibin.online/*"
		}
	]
}
~~~

This policy is used to use the objects under the S3 bucket publically. 

![image](https://user-images.githubusercontent.com/100774483/158427978-b02583e8-f876-4ffb-b325-d32b8edb49f3.png)

6.Click "Save changes"

## Now the Web files would be loading from EC2 instance and the images would be from S3 bucket. We can confirm the same by checking url of any image.

![image](https://user-images.githubusercontent.com/100774483/158430970-eb1dfa7f-55f7-4b59-a1fc-dc72b7421811.png)


# Conclusion

In this tutorial we discussed how to host a static website on EC2 by keeping it's image files on an S3 bucket. The goal is to get you started on using S3 to keep image files of a simple static website as it is cheap and easy to do.

⚛
