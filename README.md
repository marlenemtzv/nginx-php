# nginx-php
# TECNOLÓGICO DE ESTUDIOS SUPERIORES DEL ORIENTE DEL ESTADO DE MÉXICO
# Implementación de servidor nginx y php compilados desde código fuente

- Martínez Villalba Marlene
- Moctezuma Pérez David
- Morales Acosa Isaac

* GRUPO 6S12

# Objetivo General
Implementar un servidor NGINX compilado desde código fuente junto con PHP-FPM.

# Objetivos Específicos
- Compilar NGINX
- Compilar PHP
- Configurar FastCGI
- Implementar SystemD

# Desarrollo del proyecto
Primero verificamos la versión del compilador gcc en nuestra máquina virtual. En neustro caso es la 11.5.0

Usamos el comando dnf install -y dnf-plugins-core epel-release para tener herramientas de compilación; segudio de dnf confing-manager --set-enabled crb.

En caso de no tener instalado gcc y otras herramientas de compilación básica, ejecutamos el comando dnf groupinstall -y "Development Tools"

Descaragmos dependencias con  dnf install -y gcc gcc-c++ cmake make perl perl-devel pcre2 pcre2-devel zlib-devel o penssl-devel libxml2-devel libxslt-devel gd-devel perl-ExtUtils-Embed libatomic glibc-devel wget unizp ta r which git libmaxminddb libmaxminddb-devel

Descargamos wget https://nginx.org/download/nginx-1.28.0.tar.gz y listamos el ./src

Lo extraemos con

cambiamos a cd /usr/local/src/nginx-1.28.0

ejecutamos ./configure --prefix=/srv/nginx --user=nginx --group=nginx

usamos el comando make, seguido de make install para compilar

para verificar /srv/nginx/sbin/nginx -v

cd conf 
nano nginx.conf 
mkdir -p /srv/nginx/var/run
mkdir -p /srv/nginx/(client_temp ,proxy_temp, fastcgi_temp,uwsgi_temp,scgi_temp) 
mkdir -p/srv/nginx/var/cache/{client_temp, proxy_temp, fastcgi_temp,uwsgi_temp,scgi_ temp)
ls /opt/nginx/var/cache/ client temp fastcgi_temp proxy_temp scgi_temp uwsgi_temp 
chmod 700 /srv/nginx/var/cache/
chown -R nginx:nginx /srv/nginx/var/cache/ 
/usr/sbin/nginx -t 
Esperamos a recibir un mensaje de éxito

cd ..
cd .. 
chown -R nginx:nginx /srv/nginx/
Para lanzarlo usamos:
/srv/nginx/sbin/nginx


Para probarlo en el navegador instalamos links:
sudo dnf install links
links https://www.google.com
http://localhost

cd nginx
ls

cd html/
ls
nano index.html

nano /etc/systemd/system/nginx-init.service
nano /etc/systemd/system/nginx.service
systemctl daemon-reload
systemctl status nginx
systemctl enable --now nginx-init
systemctl enable --now nginx
systemctl status nginx-init
systemctl status nginx
systemctl stop nginx
systemctl start nginx


Después de la configuración básica de NGINX, continuamos con la instalación y compilación de PHP 8.4 desde código fuente para integrarlo con php-fpm y FastCGI mediante un socket UNIX.

Primero descargamos el código fuente de PHP: 
cd /usr/local/src
wget https://www.php.net/distributions/php-8.4.0.tar.gz
tar -xzf php-8.4.0.tar.gz
cd php-8.4.0

Creamos el usuario y grupo
groupadd php
useradd --system --no-create-home --shell /sbin/nologin -g nginx php

Instalamos dependencias necesarias para soporte de imágenes, internacionalización y compilación:
dnf install -y libxml2-devel sqlite-devel bzip2-devel curl-devel libjpeg-devel libpng-devel freetype-devel oniguruma-devel libicu-devel openssl-devel

Configuramos la compilación de PHP indicando el prefijo /srv/nginx, habilitando php-fpm y el socket UNIX:
./configure --prefix=/srv/nginx/php \
--enable-fpm \
--with-fpm-user=php \
--with-fpm-group=nginx \
--with-openssl \
--enable-mbstring \
--with-zlib \
--with-curl \
--enable-intl \
--with-jpeg \
--with-freetype \
--with-pdo-sqlite \
--enable-gd

compilamos con:
make
make install

para los archivos de configuración:
cp php.ini-production /srv/nginx/php/lib/php.ini
cp /srv/nginx/php/etc/php-fpm.conf.default /srv/nginx/php/etc/php-fpm.conf
cp /srv/nginx/php/etc/php-fpm.d/www.conf.default /srv/nginx/php/etc/php-fpm.d/www.conf

nano /srv/nginx/php/etc/php-fpm.d/www.conf

/srv/nginx/php/sbin/php-fpm -t

nano /etc/systemd/system/php-fpm8.4.service

systemctl daemon-reload
systemctl enable --now php-fpm8.4
systemctl status php-fpm8.4

nano /srv/nginx/conf/nginx.conf

probamos con /srv/nginx/sbin/nginx -t

reiniciamos servicios 
systemctl restart nginx
systemctl restart php-fpm8.4

creamos un archivo de prueba php
cd /srv/nginx/html
nano phpinfo.php

volvimos a probar en links
http://localhost/phpinfo.php




# Conclusiones
Con esta práctica logramos usar y comprender mejor los comandos que habíamos revisado en clase. 

Lo más importante que tuvimos a consideración fue revisar la versiones del código fuente para instalar. Al igual que los permisos para cada usuario y qué servicios podían ejecutar y a quienes pertenecían.

Comprendimos mejor cómo funcionan los directorios de instalación, los archivos de configuración y los servicios administrados por SystemD.

Lo que nos deja esta práctica es que es importante validar la configuración de cada servicio antes de arrancarlo o iniciarlo.

# Bibliografía

Gustavo Romero. (2026, 25 mayo). Instalación de NGINX y PHP desde código fuente [Video]. YouTube. https://youtu.be/i-DGl-R1uFw?si=-v8L1dE_97i4j8eQ
