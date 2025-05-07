# V-Server Setup Documentation

## Introduction

This document outlines the steps that were followed to set up a V-Server for project deployment. It covered configuring SSH access, installing and configuring a web server (Nginx), and setting up GitHub integration.

---

## SSH Configuration

### 1. Generated SSH Key Pair

To securely connect to the server, an SSH key pair was generated.

On the local machine, the following command was run to generate the SSH key pair (if it was not generated already):

```bash
ssh-keygen -t ed25519

```

This generated an SSH key pair (public and private keys) in the ~/.ssh/ directory.

### 2. Copied the Public Key to the Server
After generating the SSH key pair, the public key was copied to the server. This allowed authentication using the SSH key rather than a password.

The ssh-copy-id command was used to transfer the public key:
```bash
ssh-copy-id -i /Users/abdul/.ssh/id_ed25519.pub aalibrahim@128.140.8.32
```
### 3. Disabled Password Authentication
To enhance security, password-based login was disabled, and only SSH key authentication was allowed.

The SSH configuration file was edited as follows:

```bash
sudo nano /etc/ssh/sshd_config
```

The following line was modified:

```bash
PasswordAuthentication no
```
The changes were saved, and the editor was closed.

Then, the SSH service was restarted to apply the changes:

```bash
sudo systemctl restart sshd
```

## Webserver installation and configuration 

### 1. Updated and Upgraded the Packages
Before installing the web server, the system was updated by running the following commands:

```bash
sudo apt update
sudo apt upgrade
```

### 2. Installed Nginx
Next, Nginx was installed, which was used as the web server:

```bash
sudo apt install nginx
```
### 3. Created an alternative  HTML File
A simple HTML file was created to be served by Nginx. 

The following directory structure and HTML file were created:

```bash
sudo mkdir /var/www/alternatives
sudo touch /var/www/alternatives/alternative-index.html
```
The HTML file was edited with the following content:

```bash
sudo nano /var/www/alternatives/alternative-index.html

```
The content was as follows:

```html 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hello Nginx</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      text-align: center;
      padding: 100px;
    }
    h1 {
      color: #2c3e50;
    }
    p {
      font-size: 1.2em;
      color: #555;
    }
  </style>
</head>
<body>
  <h1>Hello Nginx</h1>
  <p>I just set up my server on the cloud and I am ready for the next pro>
</body>
</html>
```
The file was saved and the editor was exited.

### 4. Configured Nginx Server Block

A new Nginx configuration file was created for the "alternatives" website. This specified the server's root directory and the default index file.

The following configuration was added:

```bash
sudo nano /etc/nginx/sites-enabled/alternatives
```

The following server block was added:

```bash
server {
    listen 8081;
    listen [::]:8081;

    root /var/www/alternatives;
    index alternative-index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
The file was saved and the editor was exited.


### 5. Restarted Nginx

After updating the configuration, Nginx was restarted to apply the changes:

```bash
sudo systemctl restart nginx.service
```


## Github configuration 

### 1. Configured Git Globally

To ensure that commits made on the server were associated with the GitHub account, Git was configured with the user name and email.

The following commands were run to set the GitHub credentials globally:
```bash
git config --global user.email "e1pmiS@github.com"
git config --global user.name "e1pmiS"
```
### 2. Generated SSH Key for GitHub

An SSH key for GitHub was generated on the server.
Then, the public key was added to the GitHub account.

The key was added to the GitHub account by navigating to Settings > SSH and GPG keys and clicking New SSH key. The key was pasted into the field and saved.
