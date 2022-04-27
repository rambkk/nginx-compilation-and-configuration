stunnel installation:
CentOS:
```bash
yum install stunnel
```

Ubuntu:
```bash
apt install stunnel
```

Sample configuration of stunnel configuration file usually named stunnel.conf:

```INI
client = yes

[imapsout]
accept = 127.0.0.1:22143
connect = imaps.mydomain.com:993

[pop3sout]
accept = 127.0.0.1:22110
connect = pop3s.mydomain.com:995


[smtpsout]
accept = 127.0.0.1:22025
connect = smtps.mydomain.com:465
```

This configures stunnel server to listen on 127.0.0.1 interface and listens on ports 22153,22110,22465 and forwards the connection accordingly.
