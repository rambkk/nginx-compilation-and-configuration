Guide for compiling and installing nginx with mail proxy (imap proxy, pop proxy, smtp proxy) on Ubuntu.

Check OS info:
```bash
cat /etc/issue
cat /etc/os-release
```

Update the system and set timezone
```bash
apt update
apt upgrade
timedatectl list-timezones
timedatectl set-timezone 'Asia/Bangkok'
```

Installing required stuff:
```bash
apt install -y git build-essential libpcre3 libpcre3-dev libssl-dev libtool autoconf apache2-dev libxml2-dev libcurl4-openssl-dev automake pkgconf zlib1g-dev
```

removing existing nginx completely, be careful!
(sometimes this is needed to remove the default nginx installation)
```bash
apt purge nginx -y 
apt autoremove -y
```

get latest version from https://nginx.org/download/ and extract:
```bash
tar -zxvf nginx-.....tar.gz
cd nginx-.....
```

Note that the default installation location is /usr/local/nginx
Just simple configuration:
```bash
./configure --user=www-data --group=www-data --with-mail=dynamic --with-mail_ssl_module --with-http_ssl_module
```
Or with more modules:
```bash
apt install -y libxslt-dev libgd-dev libgeoip-dev libperl-dev
./configure --user=www-data \
            --group=www-data \
            --with-select_module \
            --with-poll_module \
            --with-threads \
            --with-file-aio \
            --with-http_ssl_module \
            --with-http_v2_module \
            --with-http_realip_module \
            --with-http_addition_module \
            --with-http_xslt_module=dynamic \
            --with-http_image_filter_module=dynamic \
            --with-http_geoip_module=dynamic \
            --with-http_sub_module \
            --with-http_dav_module \
            --with-http_flv_module \
            --with-http_mp4_module \
            --with-http_gunzip_module \
            --with-http_gzip_static_module \
            --with-http_auth_request_module \
            --with-http_random_index_module \
            --with-http_secure_link_module \
            --with-http_degradation_module \
            --with-http_slice_module \
            --with-http_stub_status_module \
            --with-http_perl_module=dynamic \
            --with-mail=dynamic \
            --with-mail_ssl_module \
            --with-stream=dynamic \
            --with-stream_ssl_module \
            --with-stream_realip_module \
            --with-stream_geoip_module=dynamic \
            --with-stream_ssl_preread_module \
            --with-compat \
            --with-pcre-jit \
            --with-openssl-opt=no-nextprotoneg \
            --with-debug

make
make install
```
For installation of fcgiwrapper for running CGI programs check the fcgiwrapper installation document

References:
- https://www.techrepublic.com/article/how-to-compile-nginx-for-modsecurity-support-on-ubuntu-server-20-04/


