﻿# aspnetcore Web

## 1.Install dotnet 5.0

## 2.Configure Apache(httpd) to reverse proxy

```
<LocationMatch "/net/">
ProxyPreserveHost On
ProxyPass "http://127.0.0.1:5000/"
ProxyPassReverse "http://127.0.0.1:5000/"
</LocationMatch>
```

## 3.Configure systemd service
```
[Unit]
Description=.NET Web App running on CentOS 8

[Service]
WorkingDirectory=/var/www/publish
ExecStart=/usr/bin/dotnet /var/www/publish/aspnetcoreWeb.dll
KillSignal=SIGINT
SyslogIdentifier=dotnet-ALL
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```
