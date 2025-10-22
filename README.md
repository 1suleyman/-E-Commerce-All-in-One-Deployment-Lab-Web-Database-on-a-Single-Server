# 🛍️ E-Commerce All-in-One Deployment Lab – Web + Database on a Single Server

In this lab, I deployed the complete **KodeKloud E-Commerce (LAMP)** application — both **MariaDB database** and **Apache + PHP web server** — on a **single CentOS server**.
I configured the database, loaded sample data, cloned the app from GitHub, and verified end-to-end connectivity via the browser.

---

## 📋 Lab Overview

**Goal:**

* Install and configure MariaDB and Apache (HTTPD) on one server
* Set up the `EcomDB` database and `ecomuser` credentials
* Load sample data from SQL script
* Deploy the PHP application and configure it to use localhost
* Verify the application runs correctly on port 80

**Learning Outcomes:**

* Manage services with `systemctl` (start, enable, verify)
* Create and grant privileges to a MySQL user
* Load data using SQL scripts
* Modify Apache’s default configuration to use `index.php`
* Clone application code from GitHub and update database connection settings

---

## 🛠 Step-by-Step Journey

### Step 1 – Install MariaDB

```bash
sudo yum install -y mariadb-server
```

✅ Installs the MariaDB package.

---

### Step 2 – Start and Enable the Database Service

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

✅ Confirms MariaDB is running and will start automatically on reboot.

---

### Step 3 – Create Database, User, and Grant Privileges

```bash
sudo mysql
```

Inside the MySQL shell:

```sql
CREATE DATABASE ecomdb;
CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

✅ Database `ecomdb` and user `ecomuser` created with full privileges.

---

### Step 4 – Load the Sample Data

```bash
sudo mysql < /opt/db-load-script.sql
```

✅ Populates the `ecomdb` database with product inventory information.

---

### Step 5 – Install Web Server and PHP

```bash
sudo yum install -y httpd php php-mysqlnd
```

✅ Installs Apache and PHP modules for database connectivity.

---

### Step 6 – Set PHP as Default Homepage

Replace `index.html` with `index.php` in Apache configuration:

```bash
sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf
```

✅ Ensures the app’s PHP index page loads first.

---

### Step 7 – Start and Enable Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

✅ Apache web server is active and persistent across reboots.

---

### Step 8 – Clone Application Code

```bash
sudo git clone https://github.com/kodekloudhub/learning-app-ecommerce.git /var/www/html/
```

✅ Application files downloaded to Apache’s web directory.

---

### Step 9 – Update Application Configuration

Update database host to localhost:

```bash
sudo sed -i 's/172.20.1.101/localhost/g' /var/www/html/index.php
```

✅ Connects the web app to the local MariaDB instance since both run on the same server.

---

### Step 10 – Verify Deployment

* Open the **E-Commerce Application** tab in the terminal (Apache default port 80).
* ✅ The website loads correctly and displays product data from the database.

---

## 🏁 End of Lab

### ✅ Key Commands Summary

| Task                  | Command                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------ |
| Install MariaDB       | `sudo yum install -y mariadb-server`                                                       |
| Start & enable DB     | `sudo systemctl start mariadb && sudo systemctl enable mariadb`                            |
| Enter MySQL shell     | `sudo mysql`                                                                               |
| Load SQL file         | `sudo mysql < /opt/db-load-script.sql`                                                     |
| Install Web + PHP     | `sudo yum install -y httpd php php-mysqlnd`                                                |
| Change default page   | `sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf`                        |
| Start & enable Apache | `sudo systemctl start httpd && sudo systemctl enable httpd`                                |
| Clone repo            | `sudo git clone https://github.com/kodekloudhub/learning-app-ecommerce.git /var/www/html/` |
| Update DB host        | `sudo sed -i 's/172.20.1.101/localhost/g' /var/www/html/index.php`                         |

---

### 💡 Notes / Tips

* Use `systemctl enable` to auto-start services after reboot.
* Always verify `mariadb` and `httpd` are running before testing the site.
* `sed` helps make quick configuration edits safely.
* When web and DB share the same host, set `localhost` in app configs.
* Check Apache’s default port (80) is open if the page doesn’t load.

---

### ✅ References

* [MariaDB Official Docs](https://mariadb.org/documentation/)
* [Apache HTTP Server Docs](https://httpd.apache.org/)
* [PHP MySQL Integration](https://www.php.net/manual/en/book.mysqli.php)
* [KodeKloud – E-Commerce LAMP App GitHub Repo](https://github.com/kodekloudhub/learning-app-ecommerce)
