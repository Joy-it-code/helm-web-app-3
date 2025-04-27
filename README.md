# 🚀 Integrating Helm with CI/CD using Jenkins

This project demonstrates how to deploy a Kubernetes application using a customized Helm chart, and then automate the deployment using a Jenkins pipeline set up on an AWS EC2 instance.

The goal is to practice Kubernetes packaging (Helm) and CI/CD automation (Jenkins) in real-world scenarios.
---


## 📦 Project Overview

 This project helps to package a Kubernetes application using Helm and automate its deployment with Jenkins CI/CD pipelines on an AWS EC2 instance.

I will:

- Customize a Helm chart to deploy an Nginx-based application.

- Manually deploy using Helm and Kubernetes.

- Set up Jenkins as a CI/CD server on an AWS EC2 machine.

- Automate the Helm deployments via a Jenkins pipeline triggered by Git pushes.
---

## 🧰 Before starting Jenkins, make sure:

- You have an AWS EC2 instance (Ubuntu preferred) ready.

- You already installed kubectl, helm, and connected to your Kubernetes cluster.

- Your Helm chart is working locally.

--- 

## 📁 Project Structure
```
helm-web-app/
├── webapp/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       └── deployment.yaml
```

---


## 🏗️ Step 1: Install Jenkins on AWS EC2 Instance

### 1.1 Connect to Your EC2
```
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

### 1.2 Install Java 
```
sudo apt update
sudo apt install openjdk-17-jdk -y
```

### 1.3 Install Jenkins
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

1.4 Start Jenkins
```
sudo systemctl start jenkins
sudo systemctl enable jenkins
```


### 1.5 Open Jenkins on your Browser

+ Go to http://your-ec2-public-ip:8080

+ You will see a Jenkins unlock screen.

+ Get the password from EC2:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste the password into the browser.

Install recommended plugins.

Create Admin User (username and password).



## ✅ Conclusion

This project shows how Helm can simplify Kubernetes application deployments through templated configuration and version-controlled charts. Helm does not only helps maintain consistency across environments but also empowers teams to deploy and manage applications at scale with confidence.


