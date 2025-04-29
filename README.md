# 🚀 Integrating Helm with CI/CD using Jenkins

## 📚 Project Overview

This project demonstrates how to set up a CI/CD pipeline using Jenkins to automate the deployment of a Kubernetes application managed through Helm charts.
The goal is to automatically update and deploy the application every time new changes are pushed to a Git repository.

---



## 🛠️ Prerequisites

+ A computer system

+ Basic knowledge of Git, Helm, Kubernetes, and Jenkins

+ Helm installed on your system

+ Kubernetes cluster ready (e.g., Minikube)

+ Jenkins installed on your system

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



## 📥 Step-by-Step Setup

### 1: Install Java

Jenkins requires Java to run.
Install Java JDK 21 and verify with:
```
java -version
```
![](./img/1a.java.version.png)



### 2: Install Jenkins On Windows

+ Download Jenkins for Windows from https://www.jenkins.io/download/.

+ Install it using the default settings.

+ Open Jenkins in your browser at http://localhost:8080.

+ Unlock Jenkins with the admin password found here:

```
type C:\Program Files\Jenkins\secrets\initialAdminPassword
```

Install Suggested Plugins.

Create your admin user.


### 3:Install Helm
Install Helm from https://helm.sh/docs/intro/install/.

Confirm Helm is installed by running:
```
helm version
```
![](./img/1b.helm.version.png)

---


### 4: Find the Helm Binary Path
Open PowerShell and run:
```
Get-Command helm | Select-Object -ExpandProperty Source
```
**Note: Copy this path for Jenkins pipeline.**


### 5: Create Jenkins Pipeline Job

#### 1: GitHub Credentials:

+ In Jenkins, go to Manage Jenkins → Credentials.

+ Add your GitHub username/password or token.


#### 2. Create a New Pipeline:

+ In Jenkins, click New Item → Pipeline.

+ Name it **helm-webapp-deploy** and click OK.


#### 3: Set Up Pipeline:

+ Scroll to the Pipeline Script section.

+ Add this pipeline script (update the Helm path to your system's path):


```
pipeline {
    agent any

    stages {
        stage('Deploy with Helm') {
            steps {
                script {
                    bat 'C:\\ProgramData\\chocolatey\\bin\helm.exe upgrade --install my-webapp ./webapp --namespace default'
                }
            }
        }
    }
}
```


### **Step 4: Set Pipeline Source to Git**
1. **Select "Pipeline script from SCM"**:
   - In the **Definition** dropdown, choose **"Pipeline script from SCM"**. This tells Jenkins to pull the pipeline script directly from Git repository.


2. **Choose SCM Type**:
   - Select **Git** as the Source Code Management (SCM) system.

3. **Enter Your Repository URL**:
   - Paste GitHub repository URL (e.g., `https://github.com/username/repository.git`) into the **Repository URL** field.


#### Note: you add Crendentials (4) if repo is private

4. **Credentials**:
   - If your repository is private:
     - Click **Add** next to the **Credentials** field.
     - Choose **Username and Password**, and provide your GitHub username and password (or personal access token if using two-factor authentication).
   - Select the credentials you just added.


5. **Branch to Build**:
   - Under **Branches to build**, specify the branch you want Jenkins to use (`main`).


### **Step 4: Save and Trigger the Pipeline**
1. Click **Save** at the bottom of the configuration page.
2. pipeline is now connected to Git!
3. To test it:
   - Make a code commit in your Git repository.
   - Jenkins will detect the commit and automatically trigger the pipeline (if set up for automatic builds).




---

## 6: Update Helm Chart
Edit values.yaml:

Change the number of replicas:
```
replicaCount: 3
```

## 7: Edit templates/deployment.yaml:
Update the resources section:
```
resources:
  requests:
    memory: "180Mi"
    cpu: "120m"
```


## 8: Commit and Push Changes
Run the following commands:

```
git add .
git commit -m "Updated replicas, memory, and CPU requests"
git push origin main
```

This will push the changes to GitHub.




## ✅ Conclusion

This project shows how Helm can simplify Kubernetes application deployments through templated configuration and version-controlled charts. Helm does not only helps maintain consistency across environments but also empowers teams to deploy and manage applications at scale with confidence.