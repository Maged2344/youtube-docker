# YouTube Playlist Length App

This repository contains a Python application that calculates and displays the total length of a YouTube playlist. The application is containerized using Docker and can be deployed on an AWS EC2 instance.

## Prerequisites

- AWS account
- Docker installed on your local machine
- Basic knowledge of Docker, Python, and AWS

## Getting Started

### 1. Clone the Repository


### 2. Install Dependencies
Make sure you have the necessary Python dependencies listed in requirements.txt:

```
pip install -r requirements.txt
```

### 3. Create a Dockerfile
Here's a sample Dockerfile to containerize your application:
```
# Use the official Python image from the Docker Hub
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy the application code and requirements file
COPY . /app

# Install the Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Expose port 5000
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

### 4. Build and Run the Docker Container
Build the Docker image:

```
docker build -t youtube-playlist-app .
```
Run the Docker container, mapping port 5000:
```
docker run -d -p 5000:5000 youtube-playlist-app
```

### 5. Deploy to AWS EC2
Create an EC2 Instance
Log in to the AWS Management Console.
Navigate to the EC2 Dashboard.
Click on "Launch Instance."
Choose an Amazon Machine Image (AMI). For this setup, an Amazon Linux 2 AMI is recommended.
Choose an Instance Type (e.g., t2.micro for testing).
Configure Instance Details and Add Storage as needed.
Add Tags (optional).
Configure Security Group:
Add a rule to allow inbound traffic on port 5000.
Review and Launch the instance.
Generate or use an existing key pair to access your instance.
Connect to Your EC2 Instance
Use SSH to connect:

```
ssh -i <path-to-key.pem> ec2-user@<ec2-public-ip>
```
Install Docker on EC2
Update and install Docker:
```
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
Log out and log back in to ensure Docker permissions are updated.
```
Upload and Run Docker Container on EC2
Transfer your Docker image to the EC2 instance using scp or push it to a Docker registry.

For example, if you use Docker Hub:

```
docker tag youtube-playlist-app <your-dockerhub-username>/youtube-playlist-app
docker push <your-dockerhub-username>/youtube-playlist-app
```
On EC2, pull the image:
```
docker pull <your-dockerhub-username>/youtube-playlist-app
```
Run the Docker container on EC2:
```
docker run -d -p 5000:5000 <your-dockerhub-username>/youtube-playlist-app
```
### 6. Access the Application
Open a web browser and navigate to:


http://<ec2-public-ip>:5000
Replace <ec2-public-ip> with the public IP address of your EC2 instance.

Troubleshooting
Ensure that Docker is running on the EC2 instance.

Verify that the security group rules for port 5000 are correctly configured.

Check the logs of the Docker container if the application isn't working:

```
docker logs <container-id>
```
