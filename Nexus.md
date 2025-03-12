# Nexus Installation

📦 **Automated Nexus Installation & Configuration**\
📍 **For Ubuntu EC2 Instances** | 🌐 **Self-hosted Repository Manager**

---

### 😎 About This Guide

This guide walks you through setting up ***Nexus Repository Manager*** on an Ubuntu-based system, automating the installation, user setup, and service configuration.

🔹 **Purpose**: Efficiently manage and store software artifacts using **Sonatype Nexus**.\
🔹 **Tech Stack**: **Ubuntu | Java | Nexus | Systemd | Automation**

---

## 📌 Infrastructure Setup

### 🔹 EC2 Instances

- **Amazon Linux 2, t2.micro** → Install Jenkins, Git, Java.
- **Ubuntu, t2.medium, 30GB** → Install Sonatype Nexus.

---

## 📌 Installation Steps

### 🔹 Step 1: Update the System

```sh
sudo apt update -y && sudo apt upgrade -y
```

### 🔹 Step 2: Install Java 11

```sh
sudo apt install openjdk-11-jdk -y
```

### 🔹 Step 3: Download Nexus Repository

🔗 [Sonatype Nexus Official Website](https://help.sonatype.com/en/download.html)

```sh
wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.78.1-02.tar.gz
```

### 🔹 Step 4: Extract the Downloaded File

```sh
tar -xvzf nexus-unix-x86-64-3.78.1-02.tar.gz
```

### 🔹 Step 5: Create a New User for Nexus

```sh
sudo groupadd nexus
sudo useradd -m -s /bin/bash -g nexus nexus
sudo usermod -aG nexus nexus
```

### 🔹 Step 6: Move Nexus to a Proper Location

```sh
sudo cp -r nexus-3.78.1-02 /home/nexus/nexus
sudo cp -r sonatype-work/ /home/nexus
```

### 🔹 Step 7: Change Ownership

```sh
sudo chown -R nexus:nexus /home/nexus/nexus/ /home/nexus/sonatype-work/
ls -l /home/nexus
```

### 🔹 Step 8: Configure Nexus

```sh
echo 'run_as_user="nexus"' | sudo tee /home/nexus/nexus/bin/nexus.rc
chmod +x /home/nexus/nexus/bin/nexus
ls -l /home/nexus/nexus/bin
```

### 🔹 Step 9: Do Not Sign in to the Frontend Yet

### 🔹 Step 10: Create a Systemd Service File

```sh
sudo vi /etc/systemd/system/nexus.service
```

Paste the following content:

```ini
[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
Group=nexus
ExecStart=/home/nexus/nexus/bin/nexus start
ExecStop=/home/nexus/nexus/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

### 🔹 Step 11: Reload and Start Nexus Service

```sh
sudo systemctl daemon-reload
sudo systemctl start nexus
sudo systemctl enable nexus
```

### 🔹 Step 12: Verify Nexus Service Status

```sh
sudo systemctl status nexus
```

📌 **Nexus will be running at:**

```
http://<ip-address-of-ubuntu(nexus)-ec2-instance>:8081
```

---

## 📌 Nexus Configuration

### 🔹 Step 13: Login to Nexus Frontend

- **Username**: `admin`
- **Password**: Located in the path till root directory.

### 🔹 Step 14: Change Admin Password

- Set a new password for the admin user.

### 🔹 Step 15: Configure Anonymous Access

```sh
Enable anonymous access in Nexus settings.
```

### 🔹 Step 16: Modify Repository Settings

```sh
Edit the maven-releases repository.
```

### 🔹 Step 17: Create a New Repository

- Navigate to **Repositories**.
- Create a new repository.
- Open repository settings.

### 🔹 Step 18: Open CodeSpace and Setup Maven Project

```sh
mvn archetype:generate
```

- Create a sample Maven WebApp project.
- Move source files and `pom.xml`.
- Create a `Jenkinsfile`.

### 🔹 Step 19: Setup Jenkins Pipeline

- Open **Jenkins** → **New Item** → **Pipeline** → **Create**.
- Add **Maven** as a build tool.
- Set **SCM Repository URL**.
- Click **Build Now**.

### 🔹 Step 20: Configure GitHub Repository

- In the GitHub repository, create a **settings.xml** file.
- Paste the required configuration template.
- In `pom.xml`, add the following:
  - Paste **repositories** between **dependencies** and **build**.
  - Paste **distributionManagement** after **build**.
  - Replace `localhost` with **EC2 Nexus IP Address**.

### 🔹 Step 21: Edit and Trigger the Pipeline

```sh
Edit Jenkins pipeline and trigger the build.
```

### 🔹 Step 22: Verify Artifacts in Nexus

- Navigate to **Browse** → **maven-snapshots** or **maven-releases**.
- Ensure the uploaded artifacts are available.

### 🔹 Step 23: Start Nexus Manually (If Needed)

```sh
sudo ./nexus run
```

📌 **Access Nexus at:**

```
http://<ip-address-of-ubuntu(nexus)-ec2-instance>:8081
```

---

### 📜 License

🆓 This setup guide is open-source and free to use.

