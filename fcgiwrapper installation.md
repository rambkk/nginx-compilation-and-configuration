fcgiwrapper installation guide for Ubuntu and CentOS
For running CGI applications directly under nginx web server (perl/php/bash/...)

nginx configuration example:

```nginx
server {
      listen .....;
      ......;
      
      location /cgi-bin/ {
                  gzip off;
                  root	   /usr/local/nginx/html;
                  fastcgi_pass   unix:/var/run/fcgiwrap.socket;
                  fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                  include        /usr/local/nginx/conf/fastcgi_params;
                  #fastcgi_index  index.php;
              }
              
      ......;
}
```

Ubuntu 20.04 guide for installing nginx fcgiwrapper:
```bash
apt -y install fcgiwrap
systemctl enable fcgiwrap
systemctl start fcgiwrap
systemctl status fcgiwrap
```

References:
- https://www.cyberciti.biz/faq/how-to-install-fcgiwrap-for-nginx-on-ubuntu-20-04/

-----

CentOS 7 installation of fcgiwrapper:
```bash
yum install -y epel-release
yum install -y spawn-fcgi
yum install -y fcgiwrap
```

Edit file: /etc/sysconfig/spawn-fcgi 
```bash
FCGI_SOCKET=/var/run/fcgiwrap.socket
FCGI_PROGRAM=/usr/sbin/fcgiwrap
FCGI_USER=www-data
FCGI_GROUP=www-data
FCGI_EXTRA_OPTIONS="-M 0660"
OPTIONS="-u $FCGI_USER -g $FCGI_GROUP -s $FCGI_SOCKET -S $FCGI_EXTRA_OPTIONS -F 1 -P /var/run/spawn-fcgi.pid -- $FCGI_PROGRAM"
```

Enable the spawn-fcgi
```bash
systemctl enable --now spawn-fcgi
service spawn-fcgi start
```

References:
- https://programmerclick.com/article/63852538203/
- https://www.centlinux.com/2019/08/configure-perl-fastcgi-on-centos-7-nginx-server.html

