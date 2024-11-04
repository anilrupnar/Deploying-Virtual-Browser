# Deploying Neko Virtual Browser in aws

## Overview
Neko is an innovative, self-hosted virtual browser leveraging Docker and WebRTC technology. This project provides a fully functional, secure, and private browser in a virtual environment, enabling users to browse the internet safely and collaborate remotely. With its multi-user access feature, Neko is ideal for both personal and professional use cases, including secure browsing, team collaboration, and virtual events like watch parties.

## Project Architecture Diagram

   ![Project Architecture Diagram ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/Project%20Architecture%20Diagram.gif)

## Key Features
- **Secure and Private Browsing**: Experience enhanced privacy and security with a dedicated virtual browser environment.
- **WebRTC Technology**: Smooth streaming and interactivity for real-time collaboration.
- **Multi-User Access**: Supports multiple users with simultaneous sessions.
- **Easy Sharing and Collaboration**: Simple to share sessions for collaborative browsing.
- **Ideal for Privacy-Conscious Users**: Great for those who value data privacy.
- **Supports Watch Parties and Presentations**: Perfect for virtual events and shared experiences.

## Benefits
- **Enhanced Security and Privacy**: Isolated browsing environment helps maintain data security.
- **Convenient and Flexible**: Use from anywhere without compromising on browsing features.
- **Scalable and Accessible**: Suitable for both personal and professional needs.

## Use Cases
- **Secure Online Transactions**: Ideal for secure browsing and handling of sensitive data.
- **Web Application Testing**: Perfect for testing and development in a controlled environment.
- **Team Collaboration**: Share resources and work together in real-time.
- **Virtual Events**: Use for watch parties, presentations, and interactive demonstrations.
- **Personal Projects**: An excellent tool for privacy-focused individuals.

## Technologies Used
- **GitHub**: Source code management.
- **Jenkins**: Continuous integration and deployment.
- **SonarQube**: Code quality and security analysis.
- **Dependency Checker**: Ensures secure dependencies.
- **Docker**: Containerized environment for the virtual browser.
- **Trivy**: Vulnerability scanning for Docker images.
- **Docker Compose**: Multi-container Docker applications.

# Setup Instructions

# 1 create EC2 Instance & security Groups 

### 1. Sign in to AWS

1. Access the [AWS Management Console](https://aws.amazon.com/console/) and log in with your credentials.

### 2. Navigate to EC2 Dashboard

1. In the AWS Console, select **Services** from the top menu.
2. Under **Compute**, choose **EC2** to open the EC2 Dashboard.

### 3. Launch an EC2 Instance

1. In the EC2 Dashboard, click **Launch Instances**.
2. **Choose Amazon Machine Image (AMI)**:
   - Select **Ubuntu Server 22.04** as the operating system for your instance.
3. **Choose Instance Type**:
   - For this setup, select **t2.large**.

#### 4. Configure Storage

1. Set **Storage** to 25 GB.
2. Choose the **GP2 volume type** for optimal performance.
3. Click **Next** to proceed with the instance details configuration.

### 5. Select Key Pair

1. In the **Key Pair** section, select **Choose an existing key pair**.
2. Choose the desired key pair from the dropdown list.
3. Ensure you have access to the selected private key file.
4. Click **Launch Instances** to create the instance.

### 6. Configure Security Group

1. **Select or Create a Security Group**:
   - Choose an existing security group or create a new one that meets your requirements.
   - Ensure it allows necessary inbound and outbound traffic for your application.
   
2. **Configure Inbound Rules**:
   - In the AWS Console, navigate to the **EC2 Dashboard**.
   - From the left-hand menu, select **Security Groups**.
   - Choose the security group associated with your EC2 instance.
   - Click on the **Inbound Rules** tab.
   - Click on **Edit inbound rules**.
   - Add the necessary rules for your application, such as:
   ![security groups  ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/security%20groups%20.png )  
   
### 7. Connect to the Instance Using MobaXterm and `.pem` Key File

To connect to the EC2 instance using MobaXterm and the `.pem` key file, follow these steps:

1. **Open MobaXterm**: Launch MobaXterm on your local device.
2. **Start a New SSH Session**:
   - Click **Session** > **SSH**.
   - Enter the **Public IPv4 address** of your EC2 instance in the **Remote host** field.
   - Use `ubuntu` as the **username** (for Ubuntu instances).
3. **Add the `.pem` Key for Authentication**:
   - Check **Use private key**.
   - Click **Browse** to locate your `.pem` file (e.g., `project_access_key.pem`).
4. **Connect to the Instance**:
   - Click **OK** to start the connection.
   - If prompted with a security warning, confirm to proceed.
5. **Successful Connection**:
   - You should now have access to your EC2 instance's terminal.

Click “Save rules” to apply the changes.

## Step 2: Install Jenkins

Before installing Jenkins, ensure that the Java Development Kit (JDK) is installed on the instance. For this purpose, we’ll install JDK 17 headless.

1. **Install JDK 17 Headless**:
   - Connect to the EC2 instance using SSH or any preferred method.
   - Install JDK 17:
     ```bash
     sudo apt update  # Update the package index
     sudo apt install openjdk-17-jdk  # Install JDK 17
     ```
   - Verify the installation by checking the Java version:
     ```bash
     java -version  # Check Java version
     ```

2. **Install Java Runtime Environment (JRE)**:
   - Install the Java Runtime Environment:
     ```bash
     sudo apt install openjdk-17-jre  # Install JRE
     ```

3. **Download and Run Jenkins**:
   - Download the Jenkins WAR file:
     ```bash
     wget https://get.jenkins.io/war-stable/2.426.2/jenkins.war  # Download Jenkins WAR file
     ```
   - Run Jenkins using Java:
     ```bash
     java -jar jenkins.war --httpPort=8081  # Start Jenkins on port 8081
     ```

4. **Access Jenkins**:
   - Open Jenkins in your browser by copying the public IP address of the EC2 instance and pasting it into the address bar of your browser, followed by `:8081`. 
   - Example: `publicIP:8081`
   
   If you are unable to see the Jenkins login page, check whether Jenkins is running in the terminal.
   
   After entering the IP address followed by `:8081` in the browser, the login page will appear, prompting for a password.
   ![login page ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/login%20page.png )
   
To retrieve the password:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
- Access the terminal where your EC2 instance is open and run the command to display the initial admin password needed to proceed with the Jenkins setup.
- Copy the code and paste it into the Jenkins Getting Started screen.
- Follow the on-screen instructions to complete the setup. Once done, you’ll be directed to the Jenkins dashboard.

## Step 3: Install Plugins in Jenkins

Now, let’s install some plugins in Jenkins:
- SonarQube Scanner
- OWASP Dependency-Check
- Docker
- Docker Pipeline
- Docker Build Step
- Cloud Bees Docker Build and Publish

![Install Plugins in Jenkins ]( )

## Step 4: Install Docker and Set Up SonarQube

Since Docker isn’t installed on our EC2 instance yet, let’s install Docker and Docker Compose, then set up SonarQube for code analysis.

1. **Install Docker**:
   - Run the command to install Docker:
     ```bash
     sudo apt install docker  # Install Docker
     ```

2. **Install Docker Compose**:
   - Next, install Docker Compose by running the command:
     ```bash
     sudo apt install docker-compose  # Install Docker Compose
     ```

3. **Adjust Docker Permissions**:
   - After installing Docker Compose, execute the following command to adjust permissions for other users to access the Docker socket:
     ```bash
     sudo chmod 666 /var/run/docker.sock  # Grant access to Docker socket
     ```

4. **Install SonarQube**:
   - Now, set up SonarQube on our EC2 instance using Docker with the following command:
     ```bash
     docker run -d -p 9000:9000 sonarqube:lts-community  # Run SonarQube container
     ```
   - This command starts a SonarQube server in detached mode, mapping port 9000 on the host to port 9000 on the container. This setup enables us to perform code analysis and quality checks.

5. **Access SonarQube**:
   - You can access SonarQube’s web interface at `http://localhost:9000` or by replacing `localhost` with your EC2 instance's public IP address if accessing it remotely.
   
   Example: `http://publicIP:9000`

   SonarQube will be available on port 9000, allowing you to perform code quality analysis on your projects.

## Step 5: Configure Docker, Dependency-Check, and SonarQube Scanner in Jenkins

Now that Docker and SonarQube are installed on our EC2 instance, let’s configure them through Jenkins.

1. **Configure Docker**:
   - Navigate to the Jenkins Dashboard.
   - Select `Manage Jenkins` -> `Global Tool Configuration`.
   - Scroll down to **Docker** and configure it as follows:
     - **Name**: `docker`
     - Check “Install automatically”
     - Select “Download from docker.com”
       
     ![Configure Docker ](  )
     
2. **Configure Dependency-Check**:
   - In the same **Global Tool Configuration** section, configure **Dependency-Check** as follows:
     - **Name**: `DC`
     - Check “Install automatically”
     - Select “Install from GitHub”
     - **Version**: `6.5.1`

     ![Dependency-Check ]( )
       
3. **Configure SonarQube Scanner**:
   - Still in the **Global Tool Configuration** section, configure **SonarQube Scanner** as follows:
     - **Name**: `sonar`
     - Check “Install automatically”
     - Select “Install from Maven Central”
     - **Version**: `5.0.1.3006`

     ![sonar Build ]( )

4. **Apply the Configuration**:
   - Once all configurations are set, click on **Apply** to save the changes.

5. **Verify SonarQube is Running**:
   - To confirm that SonarQube is running on the EC2 instance, execute the following command:
     ```bash
     docker ps  # Check if the SonarQube container is running
     ```
This completes the configuration of Docker, Dependency-Check, and SonarQube Scanner in Jenkins. You can now use these tools within Jenkins for your CI/CD workflows.

## Step 6: Configure SonarQube Server in Jenkins

Now we will configure the SonarQube server in Jenkins.

1. **Access SonarQube**:
   - Copy the Public IP Address of your EC2 Instance.
   - Since SonarQube operates on Port 9000, open your web browser and navigate to `<Public IP>:9000` to access the SonarQube server.

2. **Login to SonarQube**:
   - The default SonarQube username and password are:
     - **Username**: `admin`
     - **Password**: `admin`
 
    ![Sonarqube login ]( )
   
4. **Generate a SonarQube Token**:
   - Once logged in, follow these steps:
     - Go to **Administration**.
     - Select **Security**.
     - Click on **Users**.
     - Navigate to **Tokens** and select **Update Token**.
     - Assign a name to the token.
     - Click on **Generate Token** to create it.

      ![sonarqube token genrate ]( )
   
5. **Copy the Token**:
   - After generating the token, copy it. This token will be needed for integrating SonarQube with Jenkins in the next steps.

     ![sonarqube token copy ]( )

This token allows Jenkins to authenticate with your SonarQube server and perform code quality checks on your projects.

## Step 7: Add SonarQube and Docker Credentials in Jenkins

Now, we need to add the SonarQube token as a credential in Jenkins, as well as configure Docker credentials for pushing images to a Docker repository.

1. **Add SonarQube Token in Jenkins**:
   - Go to the Jenkins Dashboard.
   - Navigate to `Manage Jenkins` -> `Credentials` -> `Global credentials` -> `Add credentials`.
   - Configure the following fields:
     - **Kind**: `Secret text`
     - **Secret**: Paste the token copied from SonarQube.
     - **ID**: `sonar-token`
     - **Description**: `SonarQube token`
   - Click **Create** to finalize. Your SonarQube token is now added as a credential in Jenkins.

      ![Sonar token ]( )

2. **Add Docker Credentials in Jenkins**:
   - Add credentials for Docker to enable Jenkins to push Docker images to a repository:
     - **Username**: `anyname`
     - **Password**: `anypassword`
     - **ID**: `docker-cred`
     - **Description**: `docker-cred`
   - Save these credentials for use in your Jenkins pipelines.

3. **Integrate SonarQube Server with Jenkins**:
   - To configure the SonarQube server in Jenkins:
     - Go to the Jenkins Dashboard.
     - Navigate to `Manage Jenkins` -> `Configure System`.
     - Under **System**, locate **SonarQube servers** -> **SonarQube installations** -> **Add SonarQube**.
     - Configure the following fields:
       - **Name**: `sonar`
       - **URL**: `<Public IP>:9000` (replace `<Public IP>` with your EC2 instance’s public IP address)
       - **Server authentication token**: `sonar-token` (select the SonarQube token created earlier)
     - Click **Apply** to save the changes.
   
   ![Jenkins server ]( )


Your SonarQube and Docker credentials are now configured in Jenkins, and SonarQube is integrated with Jenkins to enable code quality analysis in your pipelines.

## Step 8: Setting Up the Jenkins Pipeline

To set up the Jenkins pipeline for the **Virtual Browser** project, follow these steps:

1. **Open Jenkins Dashboard:**
   - Go to your Jenkins dashboard.

2. **Create a New Pipeline Job:**
   - Click on **"New Item"** in the upper-left corner of the dashboard.

3. **Configure Pipeline Job:**
   - In the **"Enter an item name"** field, type **"Virtual Browser"** as the project name.
   - Select **"Pipeline"** as the project type.
   - Click **"OK"** to proceed.
   
This will create a new Pipeline job in Jenkins for the **Virtual Browser** project. Continue with your pipeline script setup and stage configurations in subsequent steps.


## Step 9: Add Docker Build, Trivy Scan, and Docker Push Stages to the Pipeline

Now that we have configured Git, OWASP Dependency Check, and SonarQube in the pipeline, we will extend the pipeline to include Docker build, Trivy scan, and Docker push stages.
```groovy
pipeline {
    agent any


   environment {
        SCANNER_HOME = tool 'sonar-scanner'
        
    }


   stages {
        stage('git-Checkout') {
            steps {
               
                    git branch: 'main', url: 'https://github.com/anilrupnar/Virtual-Browser01.git'
               
            }
        }


       stage('Owasp Dependency Check') {
            steps {
                script {
                    dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }

       stage('SonarQube') {
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=VirtualBrowser -Dsonar.projectName=VirtualBrowser"
                    }
                }
            }
        }
   }
}

```
### Pipeline Declaration
- `pipeline { … }`: Defines the Jenkins Pipeline.

### Agent Directive
- `agent any`: Specifies the pipeline can run on any available agent.

### Environment Variables
- `environment { … }`: Defines environment variables for the pipeline.
- `SCANNER_HOME = tool 'sonar'`: Sets the `SCANNER_HOME` variable to the location of the SonarQube scanner tool installed on the Jenkins server.

### Stages
- `stages { … }`: Contains the different stages of the pipeline.

### Stage: Git-checkout
- `stage('Git-checkout') { … }`: Checks out the code from a Git repository.
- `git branch: 'main', url: 'https://github.com/harimohan886/virtual-browser.git'`: Uses the Git plugin to pull code from the specified repository URL and branch.

### Stage: Owasp Dependency Check
- `stage('Owasp Dependency Check') { … }`: Runs an OWASP Dependency-Check on the project.
- `dependencyCheck additionalarguments: ' --scan ./--disableNodeAudit', odcInstallation: 'DC'`: Executes the Dependency-Check plugin with additional arguments for scanning and disabling the Node.js audit.
- `dependencyCheckPublisher pattern: '**/dependency-check-report.xml'`: Specifies the pattern for the Dependency-Check report.

### Stage: SonarQube
- `stage("SonarQube") { … }`: Runs SonarQube analysis on the project.
- `withSonarQubeEnv('sonar') { … }`: Configures the SonarQube environment using the server setup named `sonar`.
- `sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectkey=VirtualBrowser -Dsonar.projectName=VirtualBrowser'''`: Runs the SonarQube scanner with the specified project key and name.

## Stage 10: Docker Build & Tag Image

- `stage("Docker Build & Tag image") { … }`: Defines a new stage in the pipeline to build and tag a Docker image.
  
- `steps { … }`: Contains the steps to be executed within this stage.

- `script { … }`: Allows execution of Groovy script within the pipeline.

- `withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') { … }`: Configures the Docker registry credentials to be used for pushing the Docker image. It specifies:
  - `credentialsId`: The ID of the Docker registry credentials (`docker-cred`).
  - `toolName`: Specifies the Docker tool (`docker`).

- `dir('/home/ubuntu/.jenkins/workspace/virtual_browser/.docker/tor-browser') { … }`: Changes the working directory to the location containing the Dockerfile. This directory path might vary depending on the Jenkins workspace setup.

- `sh "docker build -t harimohan8/virtual:latest ."`: Executes the Docker build command to create the Docker image. It uses the `-t` flag to specify the image tag as `harimohan8/virtual:latest`. The `.` at the end denotes that the Dockerfile is in the current directory.

### Updated Pipeline Script with Docker Build Step

```groovy
pipeline {
    agent any


   environment {
        SCANNER_HOME = tool 'sonar-scanner'
        
    }


   stages {
        stage('git-Checkout') {
            steps {
               
                    git branch: 'main', url: 'https://github.com/anilrupnar/Virtual-Browser01.git'
               
            }
        }


       stage('Owasp Dependency Check') {
            steps {
                script {
                    dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }

       stage('SonarQube') {
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=VirtualBrowser -Dsonar.projectName=VirtualBrowser"
                    }
                }
            }
        }
        
       stage('Docker Build and Tag') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'Docker') {
                        dir('/var/lib/jenkins/workspace/Virtual-Browser/.docker/tor-browser')  {
                            sh "docker build -t anilrupnar/vb:latest ."
                        }
                    }
                }
            }
        }
   }       
}
```
## Stage 11: Installing Trivy on EC2 and Adding Scanning Stages to Jenkins Pipeline

To enable Docker image scanning with Trivy, we’ll first install Trivy on the EC2 instance.

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```
### Adding Trivy and Docker Push to Jenkins Pipeline
```bash 
        stage("Trivy Docker Scan"){
            steps{
                sh "trivy image anilrupnar/vb:latest > trivy.txt" 
            }
        }  
          
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'Docker') {
                        sh "docker push anilrupnar/vb:latest"
                    }
                }
            }
        }

```
## Stage 11: Now build Pipline

![Build stage optput](link_to_screenshot)

## Stage 12: Final Deployment Steps

To finalize the deployment and use the latest Docker image, update the `docker-compose.yml` file in your GitHub repository.

1. **Edit `docker-compose.yml`**: Update the `image` section to reference the latest image.

```yaml
version: "3"
services:
  neko:
    image: "anilrupnar/vb:latest"
    restart: "unless-stopped"
    shm_size: "2gb"
    ports:
      - "8082:8080"
      - "52000-52100:52000-52100/udp"
    environment:
      NEKO_SCREEN: 1920x1080@30
      NEKO_PASSWORD: neko
      NEKO_PASSWORD_ADMIN: admin
      NEKO_EPR: 52000-52100
      NEKO_ICELITE: 1
```
## Add the Deploy Stage to the Jenkins Pipeline Script

Add the `Deploy` stage in the Jenkins Pipeline script to use the updated `docker-compose.yml` file for deployment:

```groovy
stage("Deploy") {
    steps {
        sh "docker-compose up -d"
    }
}
```
## Stage 13: Final stage 

Now for the last and final test: whether our browser is working or not.

1. Copy the public IP address with port `8082` of the EC2 instance.
2. Paste it into your local browser’s address bar.

Once logged in, it should look like this:

![Final Output 3](link_to_screenshot)

## Login Credentials

- **Username**: neko
- **Password**: admin

   
![Final Output 1](link_to_screenshot)

![Final Output 2](link_to_screenshot)

Additionally, check SonarQube to see if there is something new.
![SonarQube Report](link_to_screenshot)
Finally, the project is done!



---
**Thank you for reading my README file! 😊**

**Feel free to connect with me:**

- **LinkedIn**: [Anil Rupnar](https://www.linkedin.com/in/anilrupnar/)
- **Email**: anilrupnar2003@gmail.com







