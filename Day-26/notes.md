# Day 26 - Create an EC2 Instance and Install Nginx

## Objective
Create an Amazon EC2 instance that acts as a web server by installing and running the Nginx web server.

---

## Services Used

- Amazon EC2
- Security Groups
- Amazon Machine Image (AMI)
- SSH
- Nginx

---

## Steps Performed

### 1. Launch EC2 Instance
- Open AWS Management Console.
- Navigate to **EC2**.
- Click **Launch Instance**.
- Enter the required instance name.
- Select the required Amazon Machine Image (AMI).
- Choose the specified instance type.
- Select or create a key pair.
- Configure network settings.
- Attach the required security group.
- Ensure the security group allows:
  - SSH (Port 22)
  - HTTP (Port 80)
- Launch the instance.

---

### 2. Connect to EC2

```bash
ssh -i <key.pem> ubuntu@<Public-IP>
```

or

```bash
ssh -i <key.pem> ec2-user@<Public-IP>
```

(depending on the AMI)

---

### 3. Update the Server

Ubuntu

```bash
sudo apt update
```

Amazon Linux

```bash
sudo yum update -y
```

---

### 4. Install Nginx

Ubuntu

```bash
sudo apt install nginx -y
```

Amazon Linux

```bash
sudo amazon-linux-extras install nginx1 -y
```

or

```bash
sudo yum install nginx -y
```

---

### 5. Start Nginx

```bash
sudo systemctl start nginx
```

Enable it on boot

```bash
sudo systemctl enable nginx
```

---

### 6. Verify Status

```bash
sudo systemctl status nginx
```

Expected state:

```
active (running)
```

---

### 7. Test the Web Server

Open the browser:

```
http://<Public-IP>
```

The default Nginx welcome page should appear.

---

## Important Commands

Check instance connectivity

```bash
ping <Public-IP>
```

Check listening port

```bash
sudo ss -tulpn
```

Restart Nginx

```bash
sudo systemctl restart nginx
```

Stop Nginx

```bash
sudo systemctl stop nginx
```

Reload configuration

```bash
sudo systemctl reload nginx
```

---

## Learning Outcome

- Learned how to launch an EC2 instance.
- Connected to the instance using SSH.
- Installed the Nginx web server.
- Managed services using systemctl.
- Hosted a basic web server on AWS.
- Verified web accessibility through the public IP.