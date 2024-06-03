---
title: "How to Proxy Node.js Virtual Hosts in Nginx with HTTPS Using Let's Encrypt and Certbot"
description: "Learn how to set up Nginx to proxy Node.js applications with HTTPS using Let's Encrypt and Certbot. Follow this step-by-step guide for a secure and efficient configuration."
tags: ["Node.js", "Nginx", "HTTPS", "Let's Encrypt", "Certbot"]
---

# How to Proxy Node.js Virtual Hosts in Nginx with HTTPS Using Let's Encrypt and Certbot

Setting up a secure web server that proxies Node.js applications with Nginx is a common requirement for modern web development. By using Let's Encrypt and Certbot, you can easily obtain and manage SSL/TLS certificates for your virtual hosts. In this article, we'll guide you through the process step-by-step.

## Prerequisites

Before we begin, make sure you have the following:

1. A server with a public IP address.
2. Root or sudo access to the server.
3. Nginx and Node.js installed.
4. Domain names pointed to your server's IP address.

## Step 1: Install Certbot

Certbot is a free, open-source tool for obtaining and renewing Let's Encrypt SSL certificates. To install Certbot on your server, run the following commands:

\`\`\`bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
\`\`\`

## Step 2: Configure Nginx for Your Node.js Applications

Next, you'll need to configure Nginx to proxy requests to your Node.js applications. Create a new configuration file for each virtual host in the \`/etc/nginx/sites-available/\` directory. For example, if you have two applications, \`app1\` and \`app2\`, create the following files:

### Example Configuration for \`app1\`

\`\`\`bash
sudo nano /etc/nginx/sites-available/app1.example.com
\`\`\`

Add the following content to the file:

\`\`\`nginx
server {
    listen 80;
    server_name app1.example.com;

    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
\`\`\`

### Example Configuration for \`app2\`

\`\`\`bash
sudo nano /etc/nginx/sites-available/app2.example.com
\`\`\`

Add the following content to the file:

\`\`\`nginx
server {
    listen 80;
    server_name app2.example.com;

    location / {
        proxy_pass http://localhost:3002;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
\`\`\`

Enable these configurations by creating symbolic links in the \`/etc/nginx/sites-enabled/\` directory:

\`\`\`bash
sudo ln -s /etc/nginx/sites-available/app1.example.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/app2.example.com /etc/nginx/sites-enabled/
\`\`\`

Test the Nginx configuration for syntax errors:

\`\`\`bash
sudo nginx -t
\`\`\`

If there are no errors, reload Nginx to apply the changes:

\`\`\`bash
sudo systemctl reload nginx
\`\`\`

## Step 3: Obtain SSL Certificates with Certbot

Run the following command to obtain SSL certificates for your domains:

\`\`\`bash
sudo certbot --nginx -d app1.example.com -d app2.example.com
\`\`\`

Follow the on-screen instructions to complete the certificate issuance process. Certbot will automatically configure Nginx to use the obtained certificates.

## Step 4: Verify HTTPS Configuration

After Certbot has successfully obtained and configured the SSL certificates, verify that your virtual hosts are accessible over HTTPS. Open a web browser and navigate to:

- \`https://app1.example.com\`
- \`https://app2.example.com\`

You should see your Node.js applications running securely over HTTPS.

## Step 5: Set Up Automatic Certificate Renewal

Let's Encrypt certificates are valid for 90 days, but Certbot can automatically renew them for you. To set up automatic renewal, add a cron job that runs the renewal command regularly. Open the crontab configuration:

\`\`\`bash
sudo crontab -e
\`\`\`

Add the following line to the crontab file:

\`\`\`bash
0 3 * * * /usr/bin/certbot renew --quiet
\`\`\`

This cron job will run the Certbot renewal command every day at 3 AM. Certbot will check if the certificates are due for renewal and renew them if necessary.

## Conclusion

By following this guide, you have successfully set up Nginx to proxy your Node.js virtual hosts with HTTPS using Let's Encrypt and Certbot. Your applications are now secure and accessible over HTTPS, with automatic certificate renewal ensuring continued security. If you encounter any issues, refer to the documentation for Nginx, Certbot, and Let's Encrypt for additional help.
