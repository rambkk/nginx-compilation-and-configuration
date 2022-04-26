For compiling and installing nginx on CentOS 7 (this might work on other versions, haven't tested)

```bash
cat /etc/centos-release

yum update
yum upgrade
timedatectl list-timezones
timedatectl set-timezone 'Asia/Bangkok'
yum install -y vim curl wget tree
yum groupinstall -y 'Development Tools'
```


Get latest version and extract:
```
https://nginx.org/download/
```

Get latest version of PCRE, Zlib and OpenSSL-1.1.*:
```
https://github.com/PCRE2Project/pcre2/releases
https://www.zlib.net/zlib-1.2.12.tar.gz
https://www.openssl.org/source/
```

```bash
yum install -y perl perl-devel perl-ExtUtils-Embed libxslt libxslt-devel libxml2 libxml2-devel gd gd-devel GeoIP GeoIP-devel

cd nginx*
tree -L 2 .
```

more modules, make sure to adjust the downloaded pcre and zlib location

```bash
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
            --with-pcre=../pcre2-10.40 \
            --with-pcre-jit \
            --with-zlib=../zlib-1.2.12 \
            --with-openssl=../openssl-1.1.1n \
            --with-openssl-opt=no-nextprotoneg \
            --with-debug

make
make install
```
# For installation of fcgiwrapper for running CGI programs check the fcgiwrapper installation document

```bash
useradd www-data --home /usr/local/nginx --shell /sbin/nologin
```

Check firewall settings:

```bash
firewall-cmd --list-all --zone=home
firewall-cmd --list-services
firewall-cmd --get-services #get names of services
#firewall-cmd --add-service=

firewall-cmd --list-ports
firewall-cmd --add-port=80/tcp
firewall-cmd --add-port=443/tcp
 
#firewall-cmd --permanent --add-service=http

firewall-cmd --runtime-to-permanent
firewall-cmd --reload

#firewall-cmd --remove-port=port-number/port-type
#firewall-cmd --runtime-to-permanent
```


References:
- https://www.howtoforge.com/how-to-build-nginx-from-source-on-centos-7/
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-controlling_traffic#sec-Predefined-Services
- https://www.vultr.com/docs/how-to-compile-nginx-from-source-on-centos-7/
