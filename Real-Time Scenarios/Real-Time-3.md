# step-by-step guide that includes NodePort, ClusterIP, and Port Forwarding steps to access Prometheus and Grafana on Amazon EKS using Kubernetes manifests without using Helm:

Prerequisites:

An Amazon EKS cluster.
kubectl installed and configured to connect to your EKS cluster.
A Route 53 hosted zone configured for your domain.
AWS CLI installed and configured for Security Groups setup.
Step-by-Step Guide:

Step 1: Create Security Groups:

1.1. Open the AWS Management Console.

1.2. In the navigation pane, select "Security Groups" under the "EC2" section.

1.3. Click "Create Security Group."

1.4. Fill in the details:

Name: Enter a descriptive name for the security group.
Description: Provide a meaningful description for reference.
1.5. In the "Inbound Rules" section, click "Add Rule."

1.6. Configure the rule as follows:

Type: Select "Custom TCP."
Port Range: Enter the port numbers for Prometheus (9090), Prometheus Node Exporter (9100), and Grafana (3000).
Source: Choose "Anywhere" to allow traffic from any source.
1.7. Click "Create."

Step 2: Deploy Prometheus:

2.1. Apply the Prometheus configuration (prometheus-config.yaml) using kubectl apply -f prometheus-config.yaml:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
  namespace: <namespace_of_your_choice>
data:
  prometheus.yml: |
    global:
      scrape_interval: 10s

    scrape_configs:
      - job_name: 'prometheus'
        scrape_interval: 5s
        static_configs:
          - targets: ['localhost:9090']
```
2.2. Create a Prometheus deployment (prometheus-deployment.yaml) using kubectl apply -f prometheus-deployment.yaml:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
  namespace: <namespace_of_your_choice>
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
        - name: prometheus
          image: prom/prometheus:latest
          ports:
            - containerPort: 9090
          volumeMounts:
            - name: prometheus-config
              mountPath: /etc/prometheus/prometheus.yml
              subPath: prometheus.yml
      volumes:
        - name: prometheus-config
          configMap:
            name: prometheus-config
```
2.3. Create a Prometheus ClusterIP service (prometheus-service.yaml) using kubectl apply -f prometheus-service.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: prometheus-clusterip
  namespace: <namespace_of_your_choice>
spec:
  selector:
    app: prometheus
  ports:
    - protocol: TCP
      port: 9090
      targetPort: 9090
  type: ClusterIP
```
2.4. Create a Prometheus NodePort service (prometheus-nodeport.yaml) using kubectl apply -f prometheus-nodeport.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: prometheus-nodeport
  namespace: <namespace_of_your_choice>
spec:
  selector:
    app: prometheus
  ports:
    - protocol: TCP
      port: 9090
      targetPort: 9090
      nodePort: 30090
  type: NodePort
```
2.5. Create a Prometheus Ingress resource (prometheus-ingress.yaml) using kubectl apply -f prometheus-ingress.yaml to route external traffic:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: prometheus-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: prometheus.example.com  # Replace with your domain
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: prometheus-clusterip
                port:
                  number: 9090
```
Step 3: Deploy Grafana:

3.1. Create a Grafana deployment (grafana-deployment.yaml) using kubectl apply -f grafana-deployment.yaml:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
  namespace: <namespace_of_your_choice>
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana
          image: grafana/grafana:latest
          ports:
            - containerPort: 3000
```
3.2. Create a Grafana ClusterIP service (grafana-service.yaml) using kubectl apply -f grafana-service.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: grafana-clusterip
  namespace: <namespace_of_your_choice>
spec:
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: ClusterIP
```
3.3. Create a Grafana NodePort service (grafana-nodeport.yaml) using kubectl apply -f grafana-nodeport.yaml:

```
apiVersion: v1
kind: Service
metadata:
  name: grafana-nodeport
  namespace: <namespace_of_your_choice>
spec:
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30080
  type: NodePort
```
3.4. Create a Grafana Ingress resource (grafana-ingress.yaml) using kubectl apply -f grafana-ingress.yaml to route external traffic:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: grafana-ingress
spec:
  rules:
    - host: grafana.example.com  # Replace with your domain
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: grafana-clusterip
                port:
                  number: 3000
```
Step 4: Deploy NGINX Ingress Controller:

4.1. Apply the NGINX Ingress Controller configuration using kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.0/deploy/static/provider/aws/deploy.yaml.

Step 5: Configure Ingress Resources:

5.1. Update the Prometheus Ingress resource (prometheus-ingress.yaml) to point to the NGINX Ingress Controller Load Balancer IP address.

Step 6: Integrate Route 53 with Ingress Resources:

6.1. In your Route 53 hosted zone, create A records for the Prometheus and Grafana hostnames. These A records should point to the NGINX Ingress Controller Load Balancer IP address.

Step 7: Access Prometheus and Grafana:

7.1. To access Prometheus via NodePort, use the Node's public IP or EKS worker node IP and the assigned NodePort (e.g., http://<node_ip>:30090).

7.2. To access Grafana via NodePort, use the Node's public IP or EKS worker node IP and the assigned NodePort (e.g., http://<node_ip>:30080).

7.3. To access Prometheus and Grafana via Port Forwarding, you can use the following commands:

For Prometheus Port Forwarding:
```
kubectl port-forward svc/prometheus-clusterip 9090:9090 -n <namespace_of_your_choice>
```
Access Prometheus at http://localhost:9090.

For Grafana Port Forwarding:

```
kubectl port-forward svc/grafana-clusterip 3000:3000 -n <namespace_of_your_choice>
```
Access Grafana at http://localhost:3000.

This detailed guide ensures that you have Prometheus, Grafana, NGINX Ingress Controller, and Route 53 set up on Amazon EKS using Kubernetes manifests directly, with options to access them via NodePort, ClusterIP, or Port Forwarding, along with the specified Security Group configurations and ports.
