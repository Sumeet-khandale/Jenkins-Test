
 Jenkins + Docker Setup on AWS EC2
By Sumeet Khandale

This guide helps you set up Jenkins on AWS EC2 (Ubuntu 22.04) and integrate it with Docker 


 Step 1: Launch EC2
(If you don’t have an AWS account, create one first)

What to do
In AWS Console → EC2 → Launch Instance
Choose Ubuntu 22.04 LTS
Instance type: t2.medium (recommended for Jenkins + Docker)
Create or Select a Key Pair
Under Security Group, allow the following rules:
SSH (TCP 22) → Your IP
Custom TCP 8080 → For Jenkins UI
Connect to your instance:

## 🖥️ Step 1: EC2 Instance Details
![EC2 Instance](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/1.png)

## SSh by using .pem (for Linux/Mac/AWS/CLI) or .ppk (PuTTY Private Key (for Windows PuTTY))
```bash
ssh -i my-key.pem ubuntu@<EC2-Public-IP>
````
##  Step 2: SSH Connection
![SSH Connection](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/2.png)

---

##  Step 2: Install Java & Jenkins

Continue in your **EC2 terminal** and run:

```bash
sudo apt update -y
sudo apt install -y openjdk-17-jre
```
commands to check if Java installed on your system 
```
java --version
```

# Add Jenkins repo key and source list
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | \
sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt update -y
sudo apt install -y jenkins
sudo systemctl enable --now jenkins
```

---

##  Step 3: Open Port 8080 for Jenkins

1. Go to **EC2 → Instances** in the AWS Console
2. Select your **running instance**
3. In the **Security tab**, open the **Security Group**
4. Click **Edit inbound rules → Add rule**

   * **Type:** Custom TCP
   * **Port range:** 8080
   * **Source:** My IP (recommended) or Anywhere (0.0.0.0/0)
5. Click **Save rules**

Now open Jenkins in your browser:

```
http://<EC2-Public-IP>:8080
```

##  Step 4: Unlock Jenkins
![Unlock Jenkins](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/4.png)

To unlock Jenkins, run:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password from the terminal → paste it into the browser → click **Continue**.

Then:

* Choose **Install suggested plugins**

##  Step 5: Customize Jenkins
![Customize Jenkins](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/5.png)

* Wait for installation to complete

##  Step 6: Jenkins Plugins Installing
![Plugins Installing](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/6.png)

* Create your first admin user (or Skip)

##  Step 7: Create New Admin User
![Create Admin User](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/7.png)

* Click **Start using Jenkins**

 Jenkins is now installed and running.

---

##  Step 4: Install Docker & Configure Jenkins

Run these commands:

```bash
sudo apt install -y docker.io
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl enable --now docker
sudo systemctl restart jenkins
```

###  Explanation

* Installs Docker to build images and run containers
* Adds `jenkins` and `ubuntu` to the Docker group
* Enables Docker on startup and restarts Jenkins

### Test Docker

```bash
docker run hello-world
```

 If you see “Hello from Docker!”, installation is successful.

---

##  Step 5: Install Docker Plugin in Jenkins

In the Jenkins UI:

1. Go to **Manage Jenkins → Manage Plugins → Available**
2. Search for **Docker Pipeline**
3. Install it and restart Jenkins

##  Step 8: Manage Jenkins & Plugin Configuration
![Manage Jenkins Plugins](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/8.png)

 The plugin lets Jenkins run build stages inside Docker containers — ideal for CI/CD pipelines.

---

##  Step 6: Test a Docker Pipeline

Create a new **Pipeline Job** in Jenkins → Add this Jenkinsfile:

```groovy
pipeline {
  agent {
    docker { image 'maven:3.8.7-openjdk-17' }
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -v'
        sh 'echo "Build Successful "'
      }
    }
  }
}
```

Run the pipeline.

 Jenkins will:

* Pull the Maven Docker image
* Run the build steps inside the container
* Print "Build Successful "

##  Step 9: Build Successful
![Build Successful](https://raw.githubusercontent.com/Sumeet-khandale/Microservices-mini-project/main/9.png)

 This confirms that Jenkins + Docker integration works properly.

---

##  Summary

| Component      | Status                   |
| -------------- | ------------------------ |
| EC2 Instance   |  Launched               |
| Java & Jenkins |  Installed              |
| Port 8080      |  Open                   |
| Docker         |  Installed & Configured |
| Docker Plugin  |  Added                  |
| Pipeline       |  Tested Successfully    |

---

 **Congratulations — Your Jenkins + Docker Setup is Complete!**
You can now start building CI/CD pipelines for your projects 


---
