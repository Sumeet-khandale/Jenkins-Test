Are you looking forward to learn Jenkins right from Zero(installation) to Hero(Build end to end pipelines)? then you are at the right place.

 Step 1: Launch EC2 ( If you don’t have an aws account create a aws account )
What to do:

•	In AWS Console, go to EC2 → Launch Instance
•	Choose Ubuntu 22.04 LTS
•	Instance type: t2.medium (recommended for Jenkins + Docker)
•	Create/Select a Key Pair
•	Under Security Group → allow:
•	SSH (TCP 22) from your IP
•	Custom TCP 8080 (for Jenkins UI)
•	Then connect to your instance:

	ssh -i my-key.pem ubuntu@<EC2-Public-IP>


Step – 2 continue in cmd prompt with cmds
-	sudo apt update -y
-	sudo apt install -y openjdk-17-jre
-	# Add Jenkins repo key and source list
-	curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | \
-	sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
-	echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
-	https://pkg.jenkins.io/debian binary/" | \
-	sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
-	sudo apt update -y
-	sudo apt install -y jenkins
-	sudo systemctl enable --now Jenkins
	

Step – 3

o	Steps to open port 8080:
o	Go to EC2 > Instances in the AWS Console
o	Click on your running instance
o	In the bottom panel, click on the Security tab
o	Under Security groups, click on the security group name
o	Click Edit inbound rules → Add rule
o	Type: Custom TCP or All traffic
o	Port range: 8080 or Anywhere
o	Source: My IP (recommended) or Anywhere (0.0.0.0/0) for open access
o	Click Save rules

Login to Jenkins using the below URL:

o	http://<EC2-Public-IP>:8080
o	(You can find the EC2 public IP on your AWS EC2 console page.)
o	If you don’t want to allow all traffic:
o	Delete the inbound traffic rule for All traffic
o	Edit the inbound rules to allow only Custom TCP port 8080
o	After you log in to Jenkins:
o	Run the command below to get the Jenkins Administrator password:

 

sudo cat /var/lib/jenkins/secrets/initialAdminPassword  (in command prompt) 

After you run the command above, copy the password shown in the terminal.
Then:
Go to your browser and open
http://<EC2-Public-IP>:8080
Paste the copied password into the “Administrator password” field on the Jenkins unlock page.
Click Continue
On the next screen, choose Install suggested plugins
 

Wait for all plugins to finish installing
 

Create your first admin user (recommended) — or click Skip to use the default admin
 

Click Start using Jenkins
✅ Jenkins is now installed and ready to use!
Next, proceed to install Docker on the same EC2 instance.

🐳 Step 4: Install Docker & Configure Jenkins to Use It
🪄 Commands:
-	sudo apt install -y docker.io
-	sudo usermod -aG docker jenkins
-	sudo usermod -aG docker ubuntu
-	sudo systemctl enable --now docker
-	sudo systemctl restart jenkins
🧠 Explanation:
Installs Docker to build images and run containers
Adds jenkins and ubuntu users to Docker group (so they can run Docker commands without sudo)
Restarts services to apply permissions
Test Docker:
docker run hello-world
✅ If you see “Hello from Docker!”, installation is successful.

🔌 Step 5 : Jenkins Plugin Setup
In Jenkins UI:
Go to Manage Jenkins → Manage Plugins → Available
Search for Docker Pipeline
Install it
 
Restart Jenkins
🧠 This plugin lets Jenkins run build stages inside Docker containers — perfect for CI/CD pipelines.

🧱 Step 5: Test Docker Pipeline
Create a new Pipeline job in Jenkins → Add this Jenkinsfile:
groovy
Copy if u need 

pipeline {
  agent {
    docker { image 'maven:3.8.7-openjdk-17' }
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
Run the pipeline

✅ Jenkins will:
Pull the Maven Docker image
Run the build steps inside the container
Print “Build Successful 🎉”
🧠 This confirms Jenkins + Docker integration works properly.
 

