Got it ✅ You want me to convert this step-by-step guide into a **proper `README.md` file** with comments/documentation style so that anyone can follow it.
Here’s a clean, interview/project-ready `README.md`:

---

````markdown
# AWS Lightsail & Elastic Beanstalk Deployment Guide

This guide provides **step-by-step instructions** for deploying:
1. A WordPress application using **AWS Lightsail**.  
2. A Node.js application using **AWS Elastic Beanstalk**.  

Both include setup commands, IAM configuration, and deployment steps.  

---

## 🚀 1. AWS Lightsail (WordPress Hosting)

### Steps:
1. **Search Lightsail Service**  
   - Open AWS Console → Search **Lightsail**.

2. **Create an Instance**  
   - Choose **Create Instance**.  
   - Select: **Linux/Unix + WordPress**.  
   - Name instance → `wordpress`.

3. **Connect to Instance**  
   ```bash
   # Connect via browser-based SSH in Lightsail
   sudo cat bitnami_application_password
````

* Copy the password shown (example: `/K88PyTxEtd1`).
* Default **username** = `user`.

4. **Access WordPress Admin Panel**

   * Open:

     ```
     http://<instance-ip>/wp-login.php
     ```

     Example: `http://13.233.132.45/wp-login.php`

   * Login with:

     * **Username:** `user`
     * **Password:** (copied from previous step)

5. **Check Website**

   * Visit:

     ```
     http://<instance-ip>
     ```

     Example: `http://13.233.132.45`

---

## 🌱 2. AWS Elastic Beanstalk (Node.js Hosting)

### Prerequisites

* IAM user with **Administrator Access**.
* AWS CLI configured.
* Node.js & Git installed.

---

### Step 1: IAM Setup

1. Go to **IAM** → Create user: `atul`.
2. Add to group: `admin`.
3. Attach policy: **AdministratorAccess**.
4. Create **Access Key** → Download credentials.

---

### Step 2: Launch EC2 Instance

* Choose:

  * Instance Type: `t3.medium`
  * OS: **Amazon Linux**
  * Key Pair: `key.pem`
  * Security Group: allow `SSH-22`, `Node.js-3000`.

```bash
cd Downloads
chmod 400 key.pem
ssh -i "key.pem" ec2-user@<public-dns>
```

---

### Step 3: Configure AWS CLI

```bash
aws --version
aws configure
# Enter:
# Access Key, Secret Key, Region (us-east-1), Output format (json)
```

---

### Step 4: Install Node.js & NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
. "$HOME/.nvm/nvm.sh"
nvm install 22

# Verify
node -v   # v22.18.0
npm -v    # 10.9.3
```

---

### Step 5: Install Git & Dependencies

```bash
sudo yum update -y
sudo yum install git -y
sudo yum install python3 -y
sudo yum install pip -y

pip install awsebcli
npm install express
```

---

### Step 6: Clone Node.js Application

```bash
git clone https://github.com/atulkamble/elastic-beanstalk-nodejs.git
cd elastic-beanstalk-nodejs
node app.js
```

* Test app:

  ```
  http://<instance-ip>:3000
  ```

---

### Step 7: Deploy with Elastic Beanstalk

```bash
# Initialize Beanstalk project
eb init -p node.js -r us-east-1

# Create environment configuration
mkdir .ebextensions
nano .ebextensions/01_environment.config
```

Add the following inside file:

```yaml
option_settings:
  aws:elasticbeanstalk:application:environment:
    NODE_ENV: production
```

```bash
# Create environment
eb create eb-node-env

# Open deployed app
eb open

# Deploy latest changes
eb deploy
```

---

## ✅ Verification

* WordPress → `http://<lightsail-ip>/wp-login.php`
* Node.js → Beanstalk URL provided by `eb open`

---

## 📝 Notes

* Lightsail = Beginner-friendly VPS (best for WordPress/simple apps).
* Elastic Beanstalk = Managed PaaS (best for Node.js & scalable apps).

```
