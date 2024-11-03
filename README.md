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

### Setup Instructions

### 1 create EC2 Instance & security Groups 

### 1. Sign in to AWS

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












---

## Contact

For further assistance, please contact:

- **LinkedIn**: [Anil Rupnar](https://www.linkedin.com/in/anil-rupnar/)
- **Email**: anilrupnar2003@gmail.com
  
---






