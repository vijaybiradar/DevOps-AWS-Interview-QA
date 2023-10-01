 # step-by-step guide for setting up a monitoring solution using Prometheus and Grafana on Amazon EC2 instances, including configuring security groups, Docker containers, NGINX as a reverse proxy, Route 53 for DNS


# Prerequisites:
- Amazon EC2 instances with Docker installed.
- AWS Security Groups configured for the following ports:
- Port 9090 for Prometheus Server.
- Port 9100 for Prometheus Node Exporter.
- Port 3000 for Grafana.
- A Route 53 hosted zone configured for your domain, with proper domain registration and DNS configuration.

# Step 1: Configure Security Groups for EC2 Instances:

Log in to the AWS Management Console.

Navigate to the EC2 Dashboard.

Select "Security Groups" from the left sidebar.

Click "Create Security Group."

Fill in the details:

Name: Enter a descriptive name (e.g., "Monitoring-SG").
Description: Provide a meaningful description for reference.
In the "Inbound Rules" section, click "Edit inbound rules."

Click "Add Rule."

Configure the rules as follows:

For Prometheus (Port 9090):

Type: Select "HTTP."
Port Range: Enter "9090."
Source: Choose "Anywhere" or specify the IP range that should have access.
For Prometheus Node Exporter (Port 9100):

Type: Select "HTTP."
Port Range: Enter "9100."
Source: Choose "Anywhere" or specify the IP range that should have access.
For Grafana (Port 3000):

Type: Select "HTTP."
Port Range: Enter "3000."
Source: Choose "Anywhere" or specify the IP range that should have access.
Click "Save rules."

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/553f9178-446a-4c49-8212-59b9e434b950)


# Step 2: Create Docker Containers for Prometheus and Grafana:

SSH into your EC2 instances.
Create a docker-compose.yml file in the monitoring directory with the following content:


```
version: '3.8'
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```
Run the Docker Compose stack:

```
docker-compose up -d
```
Step 3: Configure NGINX as a Reverse Proxy:

Install NGINX on your EC2 instance if not already installed:

```
sudo apt update
sudo apt install nginx -y
```
Create NGINX configuration files for Grafana and Prometheus:

Create a file named grafana.conf:
```
server {
    listen 80;
    server_name grafana.example.com;  # Replace with your desired domain

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Create a file named prometheus.conf:
```
server {
    listen 80;
    server_name prometheus.example.com;  # Replace with your desired domain

    location / {
        proxy_pass http://localhost:9090;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
Create symbolic links to enable the NGINX server blocks:

```
sudo ln -s /etc/nginx/sites-available/grafana.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/prometheus.conf /etc/nginx/sites-enabled/
```
Test the NGINX configuration:

```
sudo nginx -t
```
If the test is successful, reload NGINX to apply the configuration changes:

```
sudo systemctl reload nginx
```
**Step 4: Configure Route 53 for External URLs:**

Log in to the AWS Management Console.

Navigate to the Route 53 Dashboard.

Create DNS A records for grafana.example.com and prometheus.example.com, pointing to the public IP address of your EC2 instance.

**Step 5: Access Grafana and Prometheus:**

Open your web browser and navigate to http://grafana.example.com to access Grafana.

Open your web browser and navigate to http://prometheus.example.com to access Prometheus.

Now you have a complete monitoring solution with Prometheus and Grafana running on your EC2 instances, secured by security groups, and accessible via NGINX reverse proxy with Route 53 DNS records. This setup allows for effective monitoring of your AWS resources.




