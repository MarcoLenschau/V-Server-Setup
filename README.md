# V-Server Setup

This page documents how I configured my very first cloud server instance in the Developer Akademie DevSecOps Course.

## 🔐 SSH Key Authentication
This guide explains how to securely access a remote V-Server using SSH keys, disable password login, and block root login for improved server security.

## ✅ Steps

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
