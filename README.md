````markdown
# 🚀 Jenkins + Docker Setup on AWS EC2  
*By Sumeet Khandale*  

This guide helps you set up **Jenkins on AWS EC2 (Ubuntu 22.04)** and integrate it with **Docker** — all steps are copy-paste ready 💪  

---

## 🧩 Step 1: Launch EC2  
(If you don’t have an AWS account, create one first)

**What to do:**
1. In **AWS Console → EC2 → Launch Instance**
2. Choose **Ubuntu 22.04 LTS**
3. **Instance type:** `t2.medium` *(recommended for Jenkins + Docker)*
4. **Create/Select** a Key Pair
5. Under **Security Group**, allow:
   - **SSH (TCP 22)** from your IP  
   - **Custom TCP 8080** (for Jenkins UI)
6. Then connect to your instance:

```bash
ssh -i my-key.pem ubuntu@<EC2-Public-IP>
````

---

## ⚙️ Step 2: Install Java & Jenkins

Run these commands one by one 👇

```bash
sudo apt update -y
sudo apt install -y openjdk-17-jre
```

Add the Jenkins repo key and source list:

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | \
sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Then install Jenkins:

```bash
sudo apt update -y
sudo apt install -y jenkins
sudo systemctl enable --now jenkins
```

---

## 🔒 Step 3: Open Port 8080 for Jenkins

By default, Jenkins is not accessible externally.
Follow these steps to open it:

1. Go to **EC2 → Instances → Your Running Instance**
2. In the bottom panel → Click **Security tab**
3. Under **Security groups**, click on the group name
4. Click **Edit inbound rules → Add rule**
5. Type: **Custom TCP**
6. Port range: **8080**
7. Source: **My IP** *(recommended)* or **Anywhere (0.0.0.0/0)** *(for testing)*
8. Click **Save rules**

---

### 🌐 Access Jenkins

Now open your browser:

```
http://<EC2-Public-IP>:8080
```

To unlock Jenkins, copy the initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

👉 Copy the password from the terminal
👉 Paste it into the Jenkins unlock page
👉 Click **Continue**

Then:

* Choose **Install suggested plugins**
* Wait for installation to complete
* Create your first admin user *(recommended)*
* Click **Start using Jenkins**

✅ Jenkins is now installed and running!

---

## 🐳 Step 4: Install Docker & Configure Jenkins

Run these commands:

```bash
sudo apt install -y docker.io
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl enable --now docker
sudo systemctl restart jenkins
```

**Explanation:**

* Installs Docker
* Adds Jenkins and Ubuntu users to Docker group
* Enables Docker on boot and restarts Jenkins

Test Docker:

```bash
docker run hello-world
```

✅ If you see “Hello from Docker!”, installation is successful.

---

## 🔌 Step 5: Install Docker Pipeline Plugin in Jenkins

In Jenkins UI:

1. Go to **Manage Jenkins → Manage Plugins → Available**
2. Search for **Docker Pipeline**
3. Click **Install without restart**
4. Restart Jenkins

🧠 This plugin allows Jenkins to run builds inside Docker containers — perfect for CI/CD pipelines.

---

## 🧱 Step 6: Test Docker Pipeline

Create a **New Pipeline Job** → Add this Jenkinsfile 👇

```groovy
pipeline {
  agent {
    docker {
      image 'maven:3.8.7-openjdk-17'
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -v'
        sh 'echo "Build Successful 🎉"'
      }
    }
  }
}
```

Run the pipeline. Jenkins will:

* Pull the Maven Docker image
* Run build steps inside the container
* Print **“Build Successful 🎉”**

✅ Jenkins + Docker integration verified!

---

## 🧾 Summary

| Component      | Status                   |
| -------------- | ------------------------ |
| EC2 Instance   | ✅ Launched               |
| Java & Jenkins | ✅ Installed              |
| Port 8080      | ✅ Open                   |
| Docker         | ✅ Installed & Configured |
| Docker Plugin  | ✅ Added                  |
| Pipeline       | ✅ Tested Successfully    |

---

### 🎉 Congratulations — Jenkins + Docker setup is complete!

Now you can start building CI/CD pipelines for your projects 🚀

```
