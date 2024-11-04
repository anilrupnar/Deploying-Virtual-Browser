# Neko Virtual Browser Project

## Overview
Neko is an innovative, self-hosted virtual browser leveraging Docker and WebRTC technology. This project provides a fully functional, secure, and private browser in a virtual environment, enabling users to browse the internet safely and collaborate remotely. With its multi-user access feature, Neko is ideal for both personal and professional use cases, including secure browsing, team collaboration, and virtual events like watch parties.

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

## 1. Sign in to AWS

1. Access the [AWS Management Console](https://aws.amazon.com/console/) and log in with your credentials.

## 2. Navigate to EC2 Dashboard

1. In the AWS Console, select **Services** from the top menu.
2. Under **Compute**, choose **EC2** to open the EC2 Dashboard.

## 3. Launch an EC2 Instance

1. In the EC2 Dashboard, click **Launch Instances**.
2. **Choose Amazon Machine Image (AMI)**:
   - Select **Ubuntu Server 22.04** as the operating system for your instance.
3. **Choose Instance Type**:
   - For this setup, select **t2.large**.

## 4. Configure Storage

1. Set **Storage** to 25 GB.
2. Choose the **GP2 volume type** for optimal performance.
3. Click **Next** to proceed with the instance details configuration.

## 5. Select Key Pair

1. In the **Key Pair** section, select **Choose an existing key pair**.
2. Choose the desired key pair from the dropdown list.
3. Ensure you have access to the selected private key file.
4. Click **Launch Instances** to create the instance.

## 6. Configure Security Group

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
   -
## 7. Connect to the Instance Using MobaXterm and `.pem` Key File

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

# Step 2: Install Jenkins

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

To retrieve the password:
- Access the terminal where your EC2 instance is open and run the command to display the initial admin password needed to proceed with the Jenkins setup.
- Copy the code and paste it into the Jenkins Getting Started screen.
- Follow the on-screen instructions to complete the setup. Once done, you’ll be directed to the Jenkins dashboard.

# Step 3: Install Plugins in Jenkins

Now, let’s install some plugins in Jenkins:
- SonarQube Scanner
- OWASP Dependency-Check
- Docker
- Docker Pipeline
- Docker Build Step
- Cloud Bees Docker Build and Publish

# Step 4: Install Docker and Set Up SonarQube

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

# Step 5: Configure Docker, Dependency-Check, and SonarQube Scanner in Jenkins

Now that Docker and SonarQube are installed on our EC2 instance, let’s configure them through Jenkins.

1. **Configure Docker**:
   - Navigate to the Jenkins Dashboard.
   - Select `Manage Jenkins` -> `Global Tool Configuration`.
   - Scroll down to **Docker** and configure it as follows:
     - **Name**: `docker`
     - Check “Install automatically”
     - Select “Download from docker.com”

2. **Configure Dependency-Check**:
   - In the same **Global Tool Configuration** section, configure **Dependency-Check** as follows:
     - **Name**: `DC`
     - Check “Install automatically”
     - Select “Install from GitHub”
     - **Version**: `6.5.1`

3. **Configure SonarQube Scanner**:
   - Still in the **Global Tool Configuration** section, configure **SonarQube Scanner** as follows:
     - **Name**: `sonar`
     - Check “Install automatically”
     - Select “Install from Maven Central”
     - **Version**: `5.0.1.3006`

4. **Apply the Configuration**:
   - Once all configurations are set, click on **Apply** to save the changes.

5. **Verify SonarQube is Running**:
   - To confirm that SonarQube is running on the EC2 instance, execute the following command:
     ```bash
     docker ps  # Check if the SonarQube container is running
     ```
This completes the configuration of Docker, Dependency-Check, and SonarQube Scanner in Jenkins. You can now use these tools within Jenkins for your CI/CD workflows.











---

## Contact

For further assistance, please contact:

- **LinkedIn**: [Anil Rupnar](https://www.linkedin.com/in/anil-rupnar/)
- **Email**: anilrupnar2003@gmail.com
  
---






