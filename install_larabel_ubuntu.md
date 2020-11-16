## How to install Laravel on Ubuntu 20.04 LTS

#

### Step 1: Install LAMP Stack

You must first install the LAMP stack on Ubuntu.

Note: Laravel required PHP 7.2 or higher version for the install.

Now, use the commands below to install **PHP**.

```
$ sudo apt install software-properties-common
```

```
$ sudo add-apt-repository ppa:ondrej/php
```

```
$ sudo apt install -y php7.4 php7.4-gd php7.4-mbstring php7.4-xml
```

And, Install **Apache2** by running the following command.

```
sudo apt install apache2 libapache2-mod-php7.4
```

Next, Install **MySQL** with the command below:

```
sudo apt install mysql-server php7.4-mysql
```

You may want to download **workbeanch** from
https://dev.mysql.com/downloads/workbench/

#### **TroubleShooting**

It appears that mysql-workbench depends on this service, but does not specify this dependency explicitly in the .deb file. The following installation worked on a multi-desktop system with both Gnome and KDE installed.

```
$ sudo apt install gnome-keyring
```

- Set the password for the mysql root user

  ```
  $ sudo mysql -u root -p
  ```

- Updating the password using:

  ```
  > ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'here-goes-the-new-password';
  ```

- Restart the service

  ```
  $  sudo service mysql stop
  $  sudo service mysql start
  ```

#

### Step 2 : Install Composer

Use the following commands to install the composer
Copy and paste the following on the terminal

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'c31c1e292ad7be5f49291169c0ac8f683499edddcfd4e42232982d0fd193004208a58ff6f353fde0012d35fdd72bc394') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

To be able to run composer from any directory move it to

```
mv composer.phar ~/.local/bin/composer
```

#

### Step 3 : Download and Install Laravel

Install Laravel, download the latest version, and install it with the following command.

```
$ cd /var/www
```

```
$ git clone https://github.com/laravel/laravel.git
```

Dependencies:

```
cd /var/www/laravel
```

```
sudo composer install
```

Permisions

```
$ sudo chown -R www-data.www-data /var/www/laravel
```

```
$ sudo chmod -R 755 /var/www/laravel
```

```
$ sudo chmod -R 777 /var/www/laravel/storage
```

#

### Step 4 : Create Environment Settings

```
cp .env.example .env
```

The following will modify the `.env` file adding the value to `APP_KEY` env var

```
php artisan key:generate
```

#

### Step 5 : Create MySQL User and Database

Logging with root

```
$ sudo mysql -u root -p
```

```
> CREATE DATABASE laravel;
```

```
> CREATE USER 'laravel'@'localhost' IDENTIFIED BY 'laravel';
```

```
> GRANT ALL ON laravel.* to 'laravel'@'localhost';
```

```
> FLUSH PRIVILEGES;
```

```
> quit
```

Now edit the .env file and update database settings.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel DB_USERNAME=laravel DB_PASSWORD=laravel
```

#

### Step 6 : Apache Configuration

```
$ nano /etc/apache2/sites-enabled/avalable.conf
```
