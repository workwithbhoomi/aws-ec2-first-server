# AWS EC2 Setup and Remote Server Configuration using WSL

## Project Overview

This project documents my hands-on learning journey while transitioning into DevOps and Cloud Engineering.

The goal of this project was to:

* Create and secure an AWS account
* Configure IAM users and groups
* Launch an EC2 Ubuntu instance
* Connect securely using SSH from local WSL (Windows Subsystem for Linux)
* Perform Linux operations remotely
* Configure and host an nginx web server

This project helped me understand the practical workflow of cloud infrastructure provisioning and remote Linux server management.

---

# Technologies Used

* AWS EC2
* AWS IAM
* Ubuntu Linux
* WSL (Windows Subsystem for Linux)
* SSH
* nginx

---

# Steps Performed

## 1. AWS Account Setup

* Created AWS Free Tier account
* Enabled MFA for root user
* Avoided using root account for daily operations

---

## 2. IAM Configuration

* Created IAM User Group
* Attached AdministratorAccess policy
* Created IAM User
* Logged in using IAM credentials instead of root account

### Key Learning

Understanding secure access management and AWS IAM best practices.

---

## 3. Launching EC2 Instance

* Launched Ubuntu EC2 instance
* Selected t2.micro (Free Tier)
* Created SSH key pair (.pem)
* Configured Security Group rules:

  * SSH (22)
  * HTTP (80)

### Key Learning

Understanding cloud infrastructure provisioning and networking basics.

---

## 4. SSH Connection using WSL

Connected to EC2 remotely from local WSL terminal using:

```bash
ssh -i devops-key.pem ubuntu@<public-ip>
```

### Issue Faced

SSH failed because the private key permissions were too open.

Error:

```bash
WARNING: UNPROTECTED PRIVATE KEY FILE!
```

### Root Cause

The `.pem` key was stored inside the Windows-mounted filesystem (`/mnt/c/...`) where Linux permissions were not applied correctly.

### Resolution

Moved the key into native Linux SSH directory:

```bash
~/.ssh
```

Applied secure permissions:

```bash
chmod 400 devops-key.pem
```

### Key Learning

* Linux file permissions
* WSL filesystem differences
* Secure SSH authentication practices

---

## 5. Linux Commands Executed

```bash
pwd
ls
mkdir devopsProjects
touch hello.txt
cat hello.txt
```

### Key Learning

Basic Linux navigation and file management.

---

## 6. nginx Installation

Installed nginx on EC2:

```bash
sudo apt update
sudo apt install nginx -y
```

Verified service status:

```bash
systemctl status nginx
```

Accessed nginx webpage using EC2 public IP.

### Issue Faced

Browser initially showed:

```text
ERR_CONNECTION_TIMED_OUT
```

### Resolution

Added HTTP (Port 80) rule in EC2 Security Group inbound rules.

### Key Learning

Understanding AWS Security Groups and network access configuration.

---

# Final Outcome

Successfully:

* Provisioned cloud infrastructure on AWS
* Configured secure IAM access
* Connected to remote Linux server using SSH
* Managed Linux environment remotely
* Installed and hosted nginx web server

### Issue Faced

Website was unreachable despite nginx running successfully.

### Root Cause

Browser was attempting HTTPS connection while nginx was configured only for HTTP.

### Resolution

Accessed the application using:
http://<public-ip>
instead of https://

---

# Skills Practiced

* AWS IAM
* EC2
* Linux
* WSL
* SSH
* Networking Basics
* Security Groups
* nginx Deployment
* Troubleshooting

---

# Future Improvements

* Dockerize application deployment
* Configure CI/CD using GitHub Actions
* Automate provisioning using Infrastructure as Code
* Explore Kubernetes deployment

---

# Screenshots to Add

* IAM User Configuration
* EC2 Instance Dashboard
* SSH Connection from WSL
* nginx Running in Browser
* Linux Commands Execution

