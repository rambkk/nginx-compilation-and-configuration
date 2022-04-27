This is an example configuration to use built in config for authentication. \
(Note: This might not be suitable for more complex requirements)

The /auth should be on port 80 (HTTP) not port 443 (HTTPS). \
This setting below should prevent the access of /auth from outside network.

Add this just under "http {"
```nginx
http {
	    map $http_auth_user $pstatus {
	      "~@mydomain.com$" OK;
	        default	Invalid;
	    }
	    map $http_auth_user $pserver {
	      "~@mydomain.com$" 127.0.0.1;
	        default	Invalid;
	    }
	    map $http_auth_protocol $pport {
	      imap		22143;
	      pop3		22110;
	      smtp		22025;
	        default	Invalid;
	    }
```

Add an entry under the listen 80 ...;
```nginx
    server {
        listen       80 default_server;
        server_name  _;

        location @gossl {
                return  301 https://$host$request_uri;
        }

        location = /auth {
            error_page 403 = @gossl;
            if ($remote_addr != 127.0.0.1) {
                return 403;
            }
                add_header Auth-Status $pstatus;
                add_header Auth-Server $pserver;
                add_header Auth-Port   $pport;
                return 204;
        }

...
```
To use this configuration, the nginx-mail.conf should use the following "auth_http" entry:
```nginx
auth_http  127.0.0.1/auth;
```

The example above would redirect the access from outside to HTTPS with the same URL. \
This is to make the outside users not to be able to detect the availability of /auth.
To simplify, the "error_page 403 = @gossl;" line could be removed.
