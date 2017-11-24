# -*- mode: ruby -*-
# vi: set ft=ruby :

$provision = <<SCRIPT
yum install \
vim \
git \
zip \
unzip \
epel-release \
yum-utils \
-y

# PHP
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum install --enablerepo=remi,remi-php71 \
php \
php-intl \
php-mbstring \
php-pdo \
php-mysqlnd \
php-xml \
-y

# Composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer

# MySQL5.6
yum -y remove mariadb-libs.x86_64
yum -y install http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
yum-config-manager --disable mysql57-community
yum-config-manager --enable mysql56-community
yum -y install mysql-community-server

systemctl start mysqld
mysql -u root -e"
CREATE DATABASE vagrant DEFAULT CHARACTER SET utf8mb4;
CREATE USER vagrant IDENTIFIED BY 'vagrant';
GRANT ALL PRIVILEGES ON vagrant.* TO vagrant@localhost IDENTIFIED BY 'vagrant';
"

# httpd
yum -y install httpd

systemctl start httpd
systemctl start mysqld
SCRIPT

$start = <<SCRIPT
systemctl restart httpd
systemctl restart mysqld
SCRIPT

Vagrant.configure("2") do |config|

  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.box = "bento/centos-7.4"
  config.vm.network "private_network", ip: "192.168.10.10"
  config.vm.provision "shell", inline: $provision
  config.vm.provision "shell", inline: $start, run: 'always'

end
