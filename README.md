# jenkins-pipeline-deploy
Set up a basic Jenkins pipeline to automate the process of building and deploying an application.
# Prequisites
1. GitHub repository with source code and configuration files
2. Jenkinsfile configured with steps in repository
3. Dockerfile to build docker image
4. .dockerignore file to avoid unnecessary file upload
5. EC2 Jenkins server to manage and run commands on server
# Create an EC2 Instance and Install Jenkins
1. ssh -i your-key.pem ubuntu@ec2-ip
2. sudo apt update && sudo apt upgrade -y
3. sudo apt install -y fontconfig openjdk-21-jre
4. java -version
# Add the Jenkins repository
1. sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
   https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
2. echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] \
   https://pkg.jenkins.io/debian-stable binary/" | \
   sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
3. sudo apt update && sudo apt install -y jenkins
# Install Docker on the Jenkins server
1. Add jenkins user to run Docker commands
2. sudo usermod -aG docker ubuntu jenkins
3. newgrp docker
# Start Jenkins & Check the status
1. sudo systemctl enable --now jenkins
2. sudo systemctl status jenkins
# Allow port in EC2 Security groups
1. Add jenkins port in inbound rules in SG
2. Add application host port in inbound rules in SG
# Open Jenkins on the browser
1. http://ec2-ip:jenkins-port
# Get the initial administrator password with:
1. sudo cat /var/lib/jenkins/secrets/initialAdminPassword
2. Copy that password into the Jenkins setup screen
# Create Jenkinsfile in the GitHub repository
1. nano Jenkinsfile and add configure steps
# Clone the GitHub repository
1. git clone github-url
# Create Jenkins pipeline
1. In Jenkins, create a Pipeline job
2. Select Pipeline script from SCM
3. Select Git and enter your repository URL
4. Set the script path to Jenkinsfile
5. Configure a webhook or SCM polling so Jenkins detects new commits
# Trigger the pipeline on every commit
1. Add triggers poll scm step in Jenkinfile to continuously polling the repository
# Test the pipeline
1. git add .
2. git commit -m "Test Jenkins CI/CD pipeline"
2. git push origin main
# verify the pipeline
1. Then open the Jenkins dashboard and verify that the pipeline runs through build, test, Docker build, deploy
2. if all stages are successful, Jenkins will built the Docker image and started the application container.
3. docker ps 
4. docker image ls
# Access application locally
1. curl http://ec2-ip:8080
# Access application browser
1. http://ec2-ip:8080
# Workflow of Jenkins pipeline
1. Developer pushes code
2. Jenkins detects commit
3. Performs Build, Test, Docker image, Deploy
4. check jenkins logs to verify.
# END
