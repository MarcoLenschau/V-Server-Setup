# V-Server Setup

This page documents how I configured my very first cloud server instance in the Developer Akademie DevSecOps Course.

## ✅ Steps

## 🔐 SSH Key Authentication

### 1. Generate an SSH key pair

```
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### 2. Login into a server with password

```
ssh <username>@<host>
```

### 3. Add your public SSH-Key

``` 
ssh-copy-id -i ~/.ssh/your-public-key.pub <username>@<host>
```

### 4. Login into a server with public key

``` 
ssh -i <path/to/key> user@host
```

### 5. Disable password login

Edit the SSH daemon config on the server:

``` 
sudo nano /etc/ssh/sshd_config
```

Find and change this line from:

``` 
#PasswordAuthentication yes
```

To:

```
PasswordAuthentication no
```

Then restart the SSH service:

``` 
sudo systemctl restart sshd
``` 

### 6. Disable Root-Login

Edit the SSH daemon config on the server:
``` 
sudo nano /etc/ssh/sshd_config
```

Find and update this line from:

``` 
PermitRootLogin yes
```

To:

``` 
PermitRootLogin no
```

Then restart the SSH service:

``` 
sudo systemctl restart sshd
``` 


## 🖥️ Configure Webserver

### 1. Update Server System

```
sudo apt update
sudo apt dist-upgrade -y
```

### 2. Install Dependencies

```
sudo apt install git nginx -y
```

### 3. Create new webspace directory

```
sudo mkdir /var/www/alternatives
```

### 4. Create and Open new Nginx Configure File

```
sudo touch /etc/nginx/sites-enabled/<your-config-name>
sudo nano /etc/nginx/sites-enabled/<your-config-name>
```

### 5. Add Nginx Configuration

```
server {
    listen 8081;
    listen [::]8081;
    
    root /var/www/alternatives;
    index index.html index.htm index.nginx-debian.html;
    
    location / {
            try_files $uri $uri/ =404;
    }
}
```

### 6. Test NGINX Config 

```
sudo /usr/sbin/nginx -t
```

### 7. Create and open new index.html file 

```
sudo touch /var/www/alternatives/index.html
sudo nano /var/www/alternatives/index.html
```

### 8. Add Test Site in the new index.html

```
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>V-Server-Setup</title>
</head>
<body>
    <h2>Your V-Server-Setup is ready</h2>
</body>
</html>
```

## Configure GitHub

### 1. Generate an SSH key pair 

```
ssh-keygen -t ed25519 -C "your-email@example.com"
```

### 2. Show and Copy the Public Key 

```
cat ~/.ssh/id_ed25519.pub
```

### 3. Configure Git Username and Email

Set your global Git username and email (replace with your actual name and email):

```
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### 4. Add Public Key on Github 

Open the following URL in your browser:

```
https://github.com/settings/keys
```

Click the "New SSH key" button.  
Enter a descriptive title (e.g., "V-Server") and paste your copied public key into the "Key" field.  
Click "Add SSH key" to save.

Now your server can authenticate with GitHub using your SSH key.

### 5. Test SSH Key by Cloning a Repository

Try cloning a private or public repository from GitHub to verify that your SSH key authentication works:

```
git clone git@github.com:<your-username>/<your-repo>.git
```

If the clone succeeds without asking for a password, your SSH key setup is correct.