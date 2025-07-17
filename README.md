# 🚀 Deploy Node.js App on AWS EC2 with NGINX 

Yo! 👋 This guide will walk you through hosting your Node.js app on an AWS EC2 instance using NGINX + SSL like a total dev savage 😤. Whether you're pushing a portfolio or a startup idea, this setup got your back.

---

## 🔧 What You’ll Need
- AWS Account (Free Tier)
- Domain name (Hostinger or whatever)
- Node.js app
- A little patience and some terminal magic 💻✨

---

## 🆓 1. Create an AWS Account
Head over to [aws.amazon.com](https://aws.amazon.com/) and sign up. Free tier is all you need.

---

## 🌍 2. Buy a Domain
If you don’t already have a domain, get one from [Hostinger](https://hostinger.in) or Namecheap or Google Domains. Keep it clean and short.

---

## ⚙️ 3. Launch an EC2 Instance
- Go to EC2 Dashboard → Launch Instance
- Choose **Ubuntu**
- Create a new **key pair** → Download `.pem` file
- ✅ Enable HTTP, HTTPS, SSH in Network settings
- Set storage to **30 GB**
- Hit **Launch**

---

## 🖥️ 4. SSH into Your Instance

### Windows:
```bash
cd Downloads
ssh -i "your-key.pem" ubuntu@your-public-ip
```

### Mac/Linux:
```bash
cd Downloads
ssh -i "your-key.pem" ubuntu@your-public-ip
```

### 🧩5. Install Node.js + NPM
```bash
sudo apt update
sudo apt install curl
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v
```

### 🔄 6. Clone Your Project & Setup
```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
npm install
```
If you’ve got environment variables:
```bash
nano .env
# paste stuff → Ctrl + X → Y → Enter
```
### 💪 7. Keep App Alive with PM2
```bash
sudo npm install pm2 -g
pm2 start index.js     # change if different entry file
pm2 startup ubuntu
```
💡 PM2 Commands:
```bash
pm2 status
pm2 restart appname
pm2 stop appname
pm2 logs
pm2 flush
```
### 🔐 8. Setup Firewall (UFW)
```bash
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw status
```
### 🌐 9. Install + Configure NGINX
```bash
sudo apt install nginx
sudo nano /etc/nginx/sites-available/your-app
```
Paste this inside:

```nginx
server {
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
Then:
```bash
sudo ln -s /etc/nginx/sites-available/your-app /etc/nginx/sites-enabled
sudo nginx -t
sudo nginx -s reload
```
### 🔒 10. Add SSL with Let’s Encrypt
```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt update
sudo apt install python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
SSL renews every 90 days. Run this to simulate renewal:
```bash
sudo certbot renew --dry-run
```
🥳 You're Live!

Your Node.js app is:
- ✅ Hosted  
- ✅ Secure (HTTPS flex)  
- ✅ Auto-restarts  
- ✅ Firewall protected  
- ✅ GenZ certified 😎

