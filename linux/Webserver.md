# Web Server

## Apache on Debian

| Command                                | Usage                                             |
| :------------------------------------- | :------------------------------------------------ |
| `sudo apt install apache2`             | Install the Apache web server                     |
| `dpkg -l \| grep apache`               | Check which Apache packages are installed         |
| `ls -l /var/www`                       | Verify whether DocumentRoot exists                |
| `sudo a2ensite <sitename>`             | Enable a virtual host                             |
| `sudo a2dissite <sitename>`            | Disable a virtual host                            |
| `sudo a2enmod ssl`                     | Enable the SSL module                             |
| `sudo systemctl reload apache2`        | Reload configuration without stopping the service |
| `sudo systemctl restart apache2`       | Fully restart Apache                              |
| `htpasswd -c <file> <user>`            | Create a new `.htpasswd` file with the first user |
| `htpasswd <file> <user>`               | Add an additional user to `.htpasswd`             |
| `wget <url>`                           | Test a website from the CLI                       |
| `grep ^Listen /etc/apache2/ports.conf` | Check which ports Apache is listening on          |
| `sudo mkdir -p /var/www/<site>`        | Create the DocumentRoot for a website             |
| `openssl req -new -x509 ...`           | Generate a self-signed SSL certificate            |

## Apache on RHEL/CentOS

| Command                                                              | Usage                                   |
| :------------------------------------------------------------------- | :-------------------------------------- |
| `rpm -q httpd`                                                       | Check if Apache is installed            |
| `sudo dnf install httpd`                                             | Install Apache                          |
| `sudo systemctl start httpd`                                         | Start Apache                            |
| `sudo systemctl status httpd`                                        | Check Apache service status             |
| `ps -C httpd`                                                        | Display running `httpd` processes       |
| `echo "ServerName <name>" \| sudo tee -a /etc/httpd/conf/httpd.conf` | Resolve FQDN warning                    |
| `wget <url>`                                                         | Test web server output                  |
| `grep ^DocumentRoot /etc/httpd/conf/httpd.conf`                      | Find configured DocumentRoot            |
| `sudo semanage port -a -t http_port_t -p tcp <port>`                 | Allow custom port in SELinux            |
| `sudo systemctl restart httpd`                                       | Restart Apache                          |
| `sudo systemctl reload httpd`                                        | Reload Apache configuration             |
| `sudo firewall-cmd --add-port=<port>/tcp --permanent`                | Open firewall port                      |
| `sudo firewall-cmd --reload`                                         | Apply firewall rule changes             |
| `htpasswd -c /var/www/.htpasswd <user>`                              | Create a new `.htpasswd` file with user |
| `htpasswd /var/www/.htpasswd <user>`                                 | Add an additional user                  |
| `openssl genrsa -out ca.key 2048`                                    | Generate private key                    |
| `openssl req -new -key ca.key -out ca.csr`                           | Generate CSR                            |
| `openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt` | Generate self-signed certificate        |
| `grep ^Listen /etc/httpd/conf/httpd.conf`                            | Display listening ports                 |

## MySQL / MariaDB

### CLI

- Log in as root (password prompt will follow):

  ```bash
  mysql -u root -p
  ```

- Log in as root with password `sekrit` _(no space between `-p` and the password_):

  ```bash
  mysql -u root -psekrit
  ```

- Log in as a user on a remote host and select a database:

  ```bash
  mysql -h host -u user -p mydb
  ```

- Set root password (initial setup):

  ```bash
  mariadb-admin -u root password "my_new_password"
  ```

### Basic Queries

For system administration, log in to the `mysql` system database (`mysql -u root -p mysql`).

Every SQL query must end with a semicolon `;`.

| Task                         | Query                                  |
| :--------------------------- | :------------------------------------- |
| List databases               | `SHOW DATABASES;`                      |
| Change active database       | `USE dbname;`                          |
| Switch to `mysql` database   | `USE mysql;`                           |
| List tables                  | `SHOW TABLES;`                         |
| Display table structure      | `DESCRIBE tablename;`                  |
| Display users                | `SELECT user, host FROM mysql.user;`   |
| Display database permissions | `SELECT host, db, user FROM mysql.db;` |
| Exit CLI                     | `exit` or `Ctrl-D`                     |

### Non-Interactive Secure Installation

The default `mysql_secure_installation` script is interactive. The snippets below automate the equivalent steps: setting a root password, removing the test database, and removing anonymous users.

### Set Root Password (Idempotent)

Sets the root password only if one has not been configured yet:

```bash
  readonly mariadb_root_password="fogMeHud8"

if mysqladmin -u root status > /dev/null 2>&1; then
    mysqladmin -u root password "${mariadb_root_password}" > /dev/null 2>&1
    printf "Database root password set.\n"
else
    printf "Skipping database root password: already set.\n"
fi
```

### Remove Test Database and Anonymous Users

```bash
mysql --user=root --password="${mariadb_root_password}" mysql <<_EOF_
DELETE FROM user WHERE User='';
DELETE FROM user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
DROP DATABASE IF EXISTS test;
DELETE FROM db WHERE Db='test' OR Db='test\\_%';
FLUSH PRIVILEGES;
_EOF_
```

### Create Database and User

Creates a database and user (drops existing instances first):

```bash
readonly db_user="myuser"
readonly db_name="mydb"
readonly db_password="TicJart2"

mysql --user=root --password="${mariadb_root_password}" <<_EOF_
DROP USER IF EXISTS '${db_user}'@'%';
DROP DATABASE IF EXISTS ${db_name};
CREATE DATABASE ${db_name};
CREATE USER '${db_user}'@'\%' IDENTIFIED BY '${db_password}';
GRANT ALL PRIVILEGES ON ${db_name}.* TO '${db_user}'@'%';
FLUSH PRIVILEGES;
_EOF_
```

### Backup & Restore

Example using database `drupal`:

- **Backup**

  ```bash
  mysqldump -u root -p drupal > drupal_backup.sql
  ```

- **Restore**

  ```bash
  mysql -u root -p drupal < drupal_backup.sql
  ```
