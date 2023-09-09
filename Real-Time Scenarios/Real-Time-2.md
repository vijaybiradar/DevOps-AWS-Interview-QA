# step-by-step process for setting up Prometheus and Grafana on Amazon Linux 2 instances, including all necessary commands and configurations, and also guide me on configuring Security Groups and Amazon Route 53 for access?


**Step 1: Launch Amazon Linux 2 Instances**

1.1. Prometheus Instance:

Launch an Amazon Linux 2 instance (EC2) in a public subnet with an associated security group.
Ensure that port 9090 is open in the security group.
1.2. Grafana Instance:

Launch another Amazon Linux 2 instance (EC2) in a public subnet with an associated security group.
Ensure that port 3000 is open in the security group.
Step 2: Set Up Prometheus

2.1. SSH into Prometheus Instance:

```
ssh -i your-key.pem ec2-user@prometheus-instance-public-ip
```
2.2. Update the Server:

```
sudo yum update -y
```
2.3. Download and Install Prometheus:

```
# Download the latest Prometheus release
```
wget https://github.com/prometheus/prometheus/releases/download/v2.30.0/prometheus-2.30.0.linux-amd64.tar.gz
```
```
# Extract Prometheus binaries
tar -xvf prometheus-2.30.0.linux-amd64.tar.gz
```
```
# Rename the extracted folder
mv prometheus-2.30.0.linux-amd64 prometheus-files
```
2.4. Create Prometheus User and Directories:

```
# Create Prometheus user
sudo useradd --no-create-home --shell /bin/false prometheus
```
```
# Create required directories and set ownership
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

2.5. Copy Prometheus Binaries:

```
# Copy Prometheus and Promtool binaries to /usr/local/bin
sudo cp prometheus-files/prometheus /usr/local/bin/
sudo cp prometheus-files/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
```
2.6. Move Consoles and Console Libraries:

```
# Move consoles and console_libraries directories
sudo cp -r prometheus-files/consoles /etc/prometheus
sudo cp -r prometheus-files/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
2.7. Set Up Prometheus Configuration:

```
# Create and edit the Prometheus configuration file
sudo vi /etc/prometheus/prometheus.yml
```
Edit the configuration file to define your scraping targets, scrape intervals, etc. Example configuration:

```
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['node-exporter-ip:9100']
```
2.8. Create a Prometheus systemd Service:

```
# Create a Prometheus service file
sudo vi /etc/systemd/system/prometheus.service
```
Add the following content:

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
2.9. Start and Enable the Prometheus Service:

```
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```
Step 3: Set Up Node Exporter on Target Servers

3.1. Launch Amazon Linux 2 Instances:

Launch additional Amazon Linux 2 instances (target servers) in the same VPC and subnet as Prometheus.
3.2. SSH into Each Target Server:

SSH into each target server and securely manage SSH keys for them.
3.3. Download and Install Node Exporter:

Download and install Node Exporter on each target server following Node Exporter's official documentation.
Step 4: Configure Prometheus to Scrape Node Exporter Metrics

4.1. Modify Prometheus Configuration:

SSH into the Prometheus instance.
```
ssh -i your-key.pem ec2-user@prometheus-instance-public-ip
```
Edit the Prometheus configuration (/etc/prometheus/prometheus.yml) to include scraping targets for Node Exporter on target servers.
```
scrape_configs:
  - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['node-exporter-ip-1:9100', 'node-exporter-ip-2:9100', ...]
```
Step 5: Install and Configure Grafana

5.1. SSH into Grafana Instance:

```
ssh -i your-key.pem ec2-user@grafana-instance-public-ip
```
5.2. Update the Server:

```
sudo yum update -y
```
5.3. Download and Install Grafana:

```
# Download and install Grafana
sudo yum install -y https://dl.grafana.com/oss/release/grafana-8.1.5-1.x86_64.rpm
```
5.4. Start the Grafana Service:

```
sudo systemctl start grafana-server
```
**Step 6: Configure Security Groups**

6.1. Prometheus Server Security Group:

In the AWS Console, configure the Prometheus instance's Security Group to allow incoming traffic on port 9090.
6.2. Target Instances Security Group (with Node Exporter):

Configure the Security Group of target instances (with Node Exporter) to allow incoming traffic on port 9100.
6.3. Grafana Server Security Group:

Configure the Grafana instance's Security Group to allow incoming traffic on port 3000.

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/4e49ba14-8a7c-4e62-a91d-b5bca921fccf)



**Step 7: Amazon Route 53 Configuration**

7.1. Go to the Amazon Route 53 Console:

In the AWS Management Console, navigate to Route 53.
7.2. Create a Hosted Zone:

Create a hosted zone for your domain (e.g., example.com).
7.3. Create A Records for Grafana and Prometheus:

Create A records in your hosted zone for Grafana and Prometheus instances:
Grafana: Create an A record (e.g., grafana.example.com) pointing to the Grafana instance's public IP.
Prometheus: Create an A record (e.g., prometheus.example.com) pointing to the Prometheus instance's public IP.

**Step 8: Access Prometheus and Grafana UIs**

8.1. Access Prometheus UI:

Open a web browser and access the Prometheus UI using the Prometheus instance's public IP and port 9090:
```
http://prometheus-instance-public-ip:9090
```
8.2. Access Grafana UI:

Open a web browser and access the Grafana UI using the Grafana instance's public IP and port 3000:
```
http://grafana-instance-public-ip:3000
```
With these steps, you'll have Prometheus and Grafana set up, including Security Groups and Amazon Route 53 configurations. You can access both UIs using the provided URLs.
