---
layout: tec
title: Apache Virtual Host(Name-based)
permalink: virtualhost.html
categories: Linux
---
### Apache Virtual Host

#### When to apply it?
If you want to run more than one web site on a single machine with only one IP address.

#### What does it need?
1. A server 
2. at least two configured domin name(depend on your #websites)
2. at least one IP attached to the server
3. Apache installed on the server
4. This tutorials XD.

#### Some Background Knowledge
There are two kinds of virtual host in Apache:`IP based` and `name-based`.The first one need as many IP addr. as the number of your websites. Hence, the name-based one is simpler and cheaper. Apache officially recommends the name-based one.

The IP-based vhosts determine which vhosts to serve by the IP addr. Apache will map each IP to each vhosts(the resources in the server).

The name-based vhosts build based on the IP-based vhosts. It determine which vhosts to serve by (priority descending):

1. IP addr. which is `<VirtualHost>` block in `httpd.conf`(the main configure file in `Apache`)
2. `<Servername>` block
3. `<ServerAlias>` block
When a request arrives, the server will find the best  matching `VirtualHost` argument based on the IP address and port used by the request. If there is more than one virtual host containing this best-match address and port combination, Apache will further compare the `ServerName` and `ServerAlias` to the server name present in the request.

#### How to apply it?
The Apache HTTPD has different installation layout in different OS or dist., that is, the `httpd.conf`locates at different dir. and even different form. This tutorial is mainly about `Ubuntu/Debian`. Other OS is similar. There ia a reference about the [Apache layouts](http://wiki.apache.org/httpd/DistrosDefaultLayout#Debian.2C_Ubuntu_.28Apache_httpd_2.x.29:).

The `httpd.conf` in Ubuntu is under `/etc/apache2/`:

```shell
.
├── apache2.conf
├── conf-available
├── conf-enabled
├── envvars
├── magic
├── mods-available
├── mods-enabled
├── ports.conf
├── sites-available
└── sites-enabled
```

The configure files for vhosts is in `./sites-enabled`. There is a `.conf` file for each vhost. The default one in Apache2.4.3 is `000-default.conf`:

```
<VirtualHost *:80>
	#ServerName www.example.com
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html


	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined


</VirtualHost>

```

The simplest form for the configure file only contains three items: `VirtualHost`, `ServerName`,`DocumentRoot`.

+ All of your websites share only one IP addr. Hence, the `VirtualHost` is `*.80`. 80 is the default port for http.
+ If you use more than one domin name, you should uncomment the `ServerName` and let the value be your domin name.
+ The `DocumentRoot` is the resources which you wish the domin name `www.example.com` to map to. 

Note that the name of the configure file is irrelevant to your domin name.

Now, if There are:

+ A server and a IP addr. attached to it
+ Two domin name `www.ex1.com` and `www.ex2.com`, mapping to resources `/var/www/html` and `var/www/html1` respectively.

The steps to apply name-based vhosts lists as below.Remember that the default `.conf` will be the vhost for `www.ex1.com` in default case and what you need to do is only add another `.conf` for `www.ex2.com`:

1. `sudo mkdir /var/www/html1`
2. `sudo vim /etc/apache2/100-ex2.conf`
3.  add the following code to it:

    ```
    <VirtualHost *:80>
	    ServerName www.ex2.com
	    DocumentRoot /var/www/html1
    </VirtualHost>
    ```
4. restart the apache2: `/etc/init.d/apache2 restart`

Now, you have two websites on your server sharing one IP addr.