# Lightweight Webserver Setup on Raspberry Pi Zero 2 W

This guide walks you through setting up an ultra-lightweight webserver on your Raspberry Pi Zero 2 W. You’ll install Nginx, PHP-FPM (with PHP-MySQL support), and SQLite to build a reliable server for personal projects, demos, or small websites. Check out my [website](http://comprobrain.site) hosted on the same device (work in progress) and subscribe to my [YouTube Channel](https://www.youtube.com/@comprobrain).

---

## Prerequisites

- **Hardware:** Raspberry Pi Zero 2 W.
- **Operating System:** Raspberry Pi OS (Lite version recommended).
- **Network:** Stable internet connection.
- **Access:** Sudo privileges.
- **Experience:** Basic command line usage.

---

## Step-by-Step Installation

### 1. Update Your System

Update package lists and upgrade existing packages:

```bash
sudo apt update
sudo apt upgrade -y
```

*Keeping your system updated minimizes security risks and compatibility issues.*

---

### 2. Install Nginx

Install Nginx, a high-performance web server that uses minimal resources:

```bash
sudo apt install nginx -y
```

After installation, visit your Raspberry Pi’s IP address in a browser to see the default Nginx welcome page.

---

### 3. Install PHP-FPM and PHP-MySQL

Install PHP-FPM for efficient PHP processing and PHP-MySQL to connect PHP with MySQL:

```bash
sudo apt install php-fpm php-mysql -y
```

*This setup is essential if you plan to use a MySQL database in the future.*

---

### 4. Install SQLite

For a lightweight, file-based database solution, install SQLite:

```bash
sudo apt install sqlite3 -y
```

*SQLite is ideal for smaller projects or development environments.*

---

## Configuration

### A. Configure Nginx for PHP

1. **Edit the default server block:**

   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```

2. **Replace or update the file with the following content:**

   ```nginx
   server {
       listen 80 default_server;
       listen [::]:80 default_server;

       root /var/www/html;
       index index.php index.html index.htm;

       server_name _;

       location / {
           try_files $uri $uri/ =404;
       }

       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/run/php/php-fpm.sock;
       }

       location ~ /\.ht {
           deny all;
       }
   }
   ```

3. **Save and exit:** Press `Ctrl+O` to save, then `Ctrl+X` to exit.

---

### B. Restart Services

Restart Nginx and PHP-FPM to apply the changes:

```bash
sudo systemctl restart nginx
sudo systemctl restart php7.4-fpm  # Adjust the version if you're using a different PHP version.
```

*For troubleshooting, review logs:*
- Nginx: `sudo tail -f /var/log/nginx/error.log`
- PHP-FPM: `sudo tail -f /var/log/php7.4-fpm.log`

---

## Testing Your Webserver

1. **Create a test PHP file:**

   ```bash
   echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/index.php
   ```

2. **Access your Raspberry Pi’s IP address in a browser:**  
   You should see the PHP information page confirming that PHP is working correctly.

*Remember to remove or secure this file after testing, as it displays sensitive information.*

---

## Extra Tips

- **Performance:** Consider caching strategies and fine-tuning Nginx settings.
- **Security:** Set up a firewall (e.g., UFW) and consider SSL/TLS certificates from Let’s Encrypt.
- **Maintenance:** Regularly back up your configuration files and databases.
- **Additional Resources:**  
  Visit [comprobrain.site](http://comprobrain.site) or check out my [YouTube Channel](https://www.youtube.com/@comprobrain) for more tutorials and tips.

---
Enjoy setting up your webserver and happy hosting!
