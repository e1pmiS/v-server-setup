# V-Server Setup Documentation

## Introduction

This document outlines the steps to set up a V-Server for project deployment. It covers configuring SSH access, installing and configuring a web server (Nginx), and setting up GitHub integration.

## SSH Configuration

### 1. Generate SSH Key Pair

Generate an SSH key pair to securely connect to the server.

On the local machine, run the following command to generate the SSH key pair:

```bash
ssh-keygen -t ed25519
```

This generates an SSH key pair (public and private keys) in the ~/.ssh/ directory.

### 2. Copy the Public Key to the Server

After generating the SSH key pair, copy the public key to the server. This enables authentication using the SSH key instead of a password.

Use the ssh-copy-id command to transfer the public key:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@server_ip
```

### 3. Disable Password Authentication

To enhance security, disable password-based login and allow only SSH key authentication.

Edit the SSH configuration file as follows:

```bash
sudo nano /etc/ssh/sshd_config
```

Modify the following line:

```bash
# change password authentication to no in order to disable passwords for logins
#PasswordAuthentication yes
PasswordAuthentication no
```
Save the changes and close the editor.

Restart the SSH service to apply the changes:

```bash
sudo systemctl restart sshd
```

## Webserver installation and configuration 

### 1. Update and Upgrade the Packages

Before installing the web server, update the system by running the following commands:

```bash
sudo apt update
sudo apt upgrade
```

### 2. Install Nginx

Next, install Nginx to use it as the web server:

```bash
sudo apt install nginx
```

### 3. Create an alternative  HTML File

Create a simple HTML file to be served by Nginx.

Create the following directory structure and HTML file:

```bash
sudo mkdir /var/www/alternatives
sudo touch /var/www/alternatives/alternative-index.html
```

Edit the HTML file with the following content:

```bash
sudo nano /var/www/alternatives/alternative-index.html
```

The content was as follows:

[index.html](Docs/index.html)

Save the file and exit the editor.

### 4. Configure Nginx Server Block

Create a new Nginx configuration file for the website (e.g., “alternatives”). Specify the server’s root directory and the default index file.

Open the configuration file with:

```bash
sudo nano /etc/nginx/sites-enabled/alternatives
```

Add the following server block:

[Nginx_Config](Docs/Nginx_Config.txt)

Save the file and exit the editor.

### 5. Restart Nginx

Restart Nginx to apply the configuration changes:

```bash
sudo systemctl restart nginx.service
```

## Github configuration 

### 1. Configure Git Globally

Set Git user name and email globally on the server to associate commits with the GitHub account:

```bash
git config --global user.email "e1pmiS@github.com"
git config --global user.name "e1pmiS"
```

### 2. Generate SSH Key for GitHub

Generate an SSH key on the server for GitHub access.

Add the public key to the GitHub account by navigating to Settings > SSH and GPG keys, clicking New SSH key, pasting the key into the field, and saving it.
