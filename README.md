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








# Conclusiones
Bibliografía


