Make sure to change the "auth_http" settings and the SSL settings:
```nginx
load_module "modules/ngx_mail_module.so";
mail {
  server_name m.domain.com;
  #auth_http  127.0.0.1:82;
  auth_http  127.0.0.1/auth;
  #auth_http  127.0.0.1:81/cgi-bin/auth-mail.pl;
  #auth_http  127.0.0.1/cgi-bin/auth-mail.pl;
  proxy  on;
  proxy_pass_error_message on;
  proxy_smtp_auth on;
  xclient off;
  imap_auth plain login;
  pop3_auth plain apop;
  smtp_auth plain login;
  imap_capabilities "IMAP4rev1"; 
  pop3_capabilities "TOP" "USER";
  smtp_capabilities "PIPELINING" "ENHANCEDSTATUSCODES" "8BITMIME";

ssl_certificate /etc/letsencrypt/live/...../fullchain.pem; # managed by Certbot
ssl_certificate_key /etc/letsencrypt/live/...../privkey.pem; # managed by Certbot
include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

server {
    listen      143;
    listen	993 ssl;
    protocol    imap;
    starttls    on;
    auth_http_header User-Agent "Nginx IMAP4 proxy";
  }
server {
    listen      110;
    listen	995 ssl;
    starttls    on;
    protocol    pop3;
    pop3_auth   plain;
    auth_http_header User-Agent "Nginx POP3 proxy";
  }
server {
    listen 25;
    listen 587;
    listen 465 ssl;
    starttls on;
    protocol smtp;
    auth_http_header User-Agent "Nginx SMTP proxy";
    timeout 12000;
  }
}
```
