# aspnetcore Web

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

Add service file `/etc/systemd/system/aspnetcoreWeb.service`

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

> :zany_face: `/etc/systemd/system/aspnetcoreWeb.service` need **r-x** premission.

> :warning:Create directory **store** and **wwwroot** manually in `/var/www/publish/` by yourself

## 4. (Optional) Configure SELinux

Install `semanage`
```
dnf -y install policycoreutils-python-utils
```

Change the Diretory and its Contents SELinux Types

```
semanage fcontext -a -t httpd_sys_content_t "/var/www/publish(/.*)?"
```

Apply new Contents SELinux Types
```
restorecon -R -v /var/www/publish
```

> :zany_face: `/var/www/publish` need **r-x** premission for user **apache** at least.
