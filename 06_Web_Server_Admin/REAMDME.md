# 🚀 Setting Up an NGINX Web Server on AWS EC2

This README documents how I set up an NGINX web server on an EC2 instance from scratch. It includes every command I used and why each step was necessary. I kept it personal and straightforward so I can repeat the process confidently in the future.

---

## ☁️ Step 1: Connect to My EC2 Instance

I used SSH to log into my server:

```bash
ssh -i my-key.pem ubuntu@<my-ec2-public-ip>
```

This gave me terminal access to my EC2 instance where I’d install and configure everything.

---

## 🔧 Step 2: Update My Server’s Package List

Before installing anything, I updated the system to get the latest package versions:

```bash
sudo apt update && sudo apt upgrade -y
```

This ensures all software is up-to-date and reduces the chance of bugs or missing dependencies.

---

## 🌐 Step 3: Install NGINX

I installed the NGINX web server using the package manager:

```bash
sudo apt install nginx -y
```

This downloaded and installed NGINX, which will be used to serve websites and manage traffic.

---

## ▶️ Step 4: Start and Enable NGINX

After installation, I started the NGINX service and enabled it to launch on boot:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

This ensures the server is running and will stay running after a restart.

To check if it’s active:

```bash
sudo systemctl status nginx
```

I looked for “active (running)” to confirm.

---

## 🔓 Step 5: Allow HTTP Traffic in the Firewall (if UFW is enabled)

On Ubuntu, I allowed traffic on HTTP and HTTPS ports:

```bash
sudo ufw allow 'Nginx Full'
```

This command opened port 80 (HTTP) and 443 (HTTPS) so users can access the server.

---

## 🔐 Step 6: Open Ports in AWS Security Group

I went to the AWS Console → EC2 → My Instance → Security → Security Groups  
Then I:
1. Clicked **Edit inbound rules**
2. Added:
   - Type: HTTP
   - Protocol: TCP
   - Port: 80
   - Source: Anywhere (or just my IP for testing)

This step is critical — even if NGINX is working, the server won’t be accessible without this.

---

## 🧪 Step 7: Test NGINX in My Browser

I opened a browser and visited:

```
http://<my-ec2-public-ip>
```

I saw the default NGINX welcome page, which confirmed the setup was successful.

---

## 🎨 Step 8: Customize My Web Page

I replaced the default web page with my own by editing the index.html file:

```bash
sudo nano /var/www/html/index.html
```

I added a clean, styled page that looks modern and friendly. For example:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Welcome</title>
  <style>
    body {
      font-family: sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f8fafc;
      margin: 0;
    }
    .card {
      text-align: center;
      padding: 2rem;
      background: white;
      border-radius: 12px;
      box-shadow: 0 10px 15px rgba(0,0,0,0.1);
    }
    h1 {
      color: #4f46e5;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>Welcome to My NGINX Web Server</h1>
    <p>This page is being served from an EC2 instance!</p>
  </div>
</body>
</html>
```

---

## ✅ Success

At this point:
- NGINX is installed and running
- Port 80 is open and accessible
- My custom page is live and served from the EC2 instance

This setup is now ready for further configuration like HTTPS, domains, or hosting multiple sites.
