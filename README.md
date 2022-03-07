# Nginx FastCGI Fix Permission

## Table of Contents
* [About](#about)
* [How it works](#how-it-works)
* [Installation](#installation)
* [Changelog](#changelog)
* [Contributing](#contributing)
* [License](#license)

## About
This library was born with the objective of fix the permissions of the folder that nginx automatically generates, to store the cache of your site.

In our case, originally the cache folder comes out like this:  
```
ls -lsa
4 drwx------  2 nobody root 4096 Mar  4 17:54 cache_folder
```

The problem with this is that if you have a procedure on your site to clear the cache, such as the [Nginx Cache](https://br.wordpress.org/plugins/nginx-cache/) plugin for Wordpress, your user will not will be able to clear the contents of the cache folder.

## How it works
We set up our script to run on a hook called `ExecStartPost` on the Nginx service.

With that, **after** nginx starts or restarts, our script is run, fixing the folder permissions to what we want.

## Installation
1. Open the folder where you want to install the project and run:
`composer require ejetar/nginx-fastcgi-fix-permission`

2. Then edit the following file: `/lib/systemd/system/nginx.service`. Add the following line in the `[Service]` section `ExecStartPost=<path you installed the lib>/nginx-fastcgi-fix-permission/fix`.  
Example:
```
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/sh -c "/bin/kill -s HUP $(/bin/cat /var/run/nginx.pid)"
ExecStop=/bin/sh -c "/bin/kill -s TERM $(/bin/cat /var/run/nginx.pid)"
ExecStartPost=/opt/nginx-fastcgi-fix-permission/fix

[Install]
WantedBy=multi-user.target

[Service]
LimitNOFILE=65535
```
3. Execute `systemctl daemon-reload`
4. Restart nginx and you're done!

## Changelog
Nothing for now...

## Contributing
Contribute to this wonderful project, it will be a pleasure to have you with us. Let's help the free software community. You are invited to incorporate new features, make corrections, report bugs, and any other form of support.
Don't forget to star in this repository! 😀

## License
This library is a open-source software licensed under the MIT license.
