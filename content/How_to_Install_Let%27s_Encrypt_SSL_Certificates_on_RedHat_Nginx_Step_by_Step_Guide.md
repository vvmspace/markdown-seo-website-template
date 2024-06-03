---
tags: ["SSL", "Letsencrypt", "Nginx", "Certbot", "RedHat", "DevOps", "SEO"]
description: "Learn how to install Let's Encrypt SSL certificates on your RedHat Nginx server using Certbot. This step-by-step guide ensures your site is secure with HTTPS."
title: "How to Install Let's Encrypt SSL Certificates on RedHat Nginx: Step-by-Step Guide"
---

## How to Install Let's Encrypt SSL Certificates on RedHat Nginx: Step-by-Step Guide

Securing your website with SSL certificates is crucial in today's digital landscape. Let's Encrypt offers a free and automated way to obtain these certificates. This guide will walk you through the process of installing Let's Encrypt SSL certificates on your RedHat server running Nginx, using Certbot.

### What is Let's Encrypt?

Let's Encrypt is a free, automated, and open certificate authority that provides SSL/TLS certificates. Its goal is to make encrypted connections ubiquitous across the web. The certificates are trusted by all major browsers and offer the same level of security as paid certificates.

### Prerequisites

Before we dive into the installation process, ensure you have the following:

- A registered domain name.
- A RedHat server running Nginx.
- Shell access to your server.
- A user with sudo privileges.

### Installing Certbot on RedHat

Certbot is a free, open-source software tool for automatically using Let’s Encrypt certificates on manually-administrated websites to enable HTTPS.

1. **Enable EPEL Repository**

   First, enable the EPEL repository:

   ```bash
   sudo yum install epel-release
   ```

2. **Install Certbot**

   Install Certbot and the necessary plugins for Nginx:

   ```bash
   sudo yum install certbot python3-certbot-nginx
   ```

### Obtaining an SSL Certificate for Nginx

1. **Run Certbot**

   To obtain and install a certificate for Nginx, run:

   ```bash
   sudo certbot --nginx
   ```

2. **Follow the Prompts**

   Certbot will prompt you to enter your email address and agree to the terms of service. It will then automatically configure SSL for your domain. Follow these steps:
   
   - Enter your email address.
   - Agree to the terms of service.
   - Select the domain name you want to secure.

3. **Verify Installation**

   After installation, verify that the SSL certificate is working by visiting your site using `https://`. You can also use an online SSL checker.

### Automating SSL Renewal

Let's Encrypt certificates are valid for 90 days. It's essential to automate the renewal process.

1. **Test Automatic Renewal**

   You can test the renewal process with:

   ```bash
   sudo certbot renew --dry-run
   ```

2. **Set Up Cron Job**

   Certbot automatically installs a cron job to handle renewals. You can verify it by checking `/etc/cron.d`:

   ```bash
   sudo cat /etc/cron.d/certbot
   ```

### Troubleshooting Common Issues

1. **Firewall Configuration**

   Ensure that your firewall allows traffic on ports 80 (HTTP) and 443 (HTTPS). For firewall-cmd, run:

   ```bash
   sudo firewall-cmd --permanent --add-service=http
   sudo firewall-cmd --permanent --add-service=https
   sudo firewall-cmd --reload
   ```

2. **SELinux Configuration**

   If you use SELinux, you may need to adjust its policies. Check your server’s SELinux settings if you encounter issues.

### Conclusion

By following this guide, you can secure your website using Let's Encrypt SSL certificates with minimal effort. Whether you're running Nginx on RedHat, Certbot simplifies the process and ensures your site is protected with HTTPS. Regularly check and renew your certificates to maintain security. With Let's Encrypt, SSL encryption is more accessible than ever, making it a vital tool for any webmaster.

For more detailed guides and troubleshooting tips, keep an eye on our blog and don't hesitate to reach out with your questions.
