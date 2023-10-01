## step-by-step guide on how to set up Prometheus, Grafana, NGINX Ingress Controller, and Route 53 on Amazon EKS using Helm

**Prerequisites:**
- An Amazon EKS cluster.
- Helm 3 installed on your local machine.
- A Route 53 hosted zone configured for your domain.

**Step-by-Step Guide:**

**Step 1: Create a Security Group for Prometheus Grafana and Prometheus Node Exporter :**

1.1. Navigate to the Amazon EC2 console.

1.2. In the left navigation pane, select "Security Groups."

1.3. Click "Create Security Group."

1.4. Fill in the details:

Name: Enter a descriptive name for the security group.
Description: Provide a meaningful description for reference.
1.5. In the "Inbound Rules" section, click "Add Rule."

1.6. Configure the rule as follows:

Type: Select "Custom TCP."
Port Range: Enter the port numbers for Prometheus (9090),Grafana (3000) and Prometheus Node Exporter (9100)
Source: Choose "Anywhere" to allow traffic from any source.
1.7. Click "Save."

![image](https://github.com/vijaybiradar/DevOps-AWS-Interview-QA/assets/38376802/afef79d5-69d9-49ce-b719-92deeec86072)


**Step 2: Deploy Prometheus and Grafana Using Helm Charts:**

2.1. Install the Helm CLI on your local machine if not already installed.

2.2. Clone the Prometheus Helm chart repository:
```
git clone https://github.com/prometheus-community/helm-charts.git
```
2.3. Clone the Grafana Helm chart repository:

```
git clone https://github.com/grafana/helm-charts.git
```
2.5. Install the Prometheus Helm chart:

```
helm install prometheus prometheus-community/prometheus -n <namespace_of_your_choice> -f prometheus-values.yaml
```
2.6. Install the Grafana Helm chart:

```
helm install grafana grafana/grafana -n <namespace_of_your_choice> -f grafana-values.yaml
```
**Step 3: Create an Ingress Controller Using NGINX Ingress Controller Helm Chart:**

3.1. Install the NGINX Ingress Controller Helm chart:

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

helm install nginx-ingress ingress-nginx/ingress-nginx -n <namespace_of_your_choice>


**Step 4: Configure Ingress Resources:**


4.1. Retrieve the NGINX Ingress Controller load balancer IP address:

```
kubectl get svc -n <namespace_of_nginx_controller> nginx-ingress-ingress-nginx-controller
```
4.2. Update the Prometheus Ingress resource (prometheus-ingress.yaml) to point to the NGINX Ingress Controller load balancer IP address:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: prometheus-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
    - host: prometheus.example.com  # Replace with your domain
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: prometheus-server
                port:
                  number: 9090
```
---
Set prometheus.example.com to the Load Balancer IP address obtained in step 4.1.

4.3. Update the Grafana Ingress resource (grafana-ingress.yaml) to point to the NGINX Ingress Controller load balancer IP address:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: grafana-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
    - host: grafana.example.com  # Replace with your domain
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: grafana
                port:
                  number: 80
```
---
Set grafana.example.com to the Load Balancer IP address obtained in step 4.1.

**Step 5: Integrate Route 53 with Ingress Resources:**

5.1. In your Route 53 hosted zone, create A records for the Prometheus and Grafana hostnames. These A records should point to the NGINX Ingress Controller Load Balancer IP address obtained in step 4.1.

**Step 6: Configure Prometheus and Grafana Data Sources:**

6.1. Access Grafana using the domain you set up (e.g., grafana.example.com) and the provided login credentials.

6.2. Add Prometheus as a data source in Grafana, using the URL of your Prometheus service (e.g., http://prometheus-server.<namespace_of_your_choice>.svc.cluster.local:9090).

6.3. Create dashboards and set up alerts as needed in Grafana for monitoring your EKS cluster.
