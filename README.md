# Nginx Sample Reverse Proxy

Remember location your `nginx` config:

`/etc/nginx/`

you can create directory `snippets`, `sites-available` and `sites-enabled` if not exist.

``` bash
mkdir -p /etc/nginx/snippets
mkdir -p /etc/nginx/sites-available
mkdir -p /etc/nginx/sites-enabled
```

if you cann't create directory, please use `sudo`.

list of `nginx` directory:

``` bash
╭─root at web-server in ~ using
╰─○ ll /etc/nginx
total 40K
drwxr-xr-x. 2 root root  255 Oct  4  2018 bots.d
drwxr-xr-x. 2 root root  111 Oct  7 04:09 conf.d
-rw-r--r--. 1 root root 1007 Aug 13 15:04 fastcgi_params
-rw-r--r--. 1 root root 2.8K Aug 13 15:04 koi-utf
-rw-r--r--. 1 root root 2.2K Aug 13 15:04 koi-win
-rw-r--r--. 1 root root 5.2K Aug 13 15:04 mime.types
lrwxrwxrwx. 1 root root   29 Aug 21 04:21 modules -> ../../usr/lib64/nginx/modules
-rw-rw-r--. 1 root root 6.5K Oct  7 03:24 nginx.conf
-rw-r--r--. 1 root root  636 Aug 13 15:04 scgi_params
drwxr-xr-x. 2 root root  239 Sep 23 11:08 sites-available
drwxr-xr-x. 2 root root  113 Oct  7 03:18 sites-enabled
drwxr-xr-x. 2 root root  123 Sep 17 11:43 snippets
drwxr-xr-x. 3 root root   27 Oct 18  2018 ssl
-rw-r--r--. 1 root root  664 Aug 13 15:04 uwsgi_params
-rw-r--r--. 1 root root 3.6K Aug 13 15:04 win-utf
```

> Check nginx config `nginx -t`

Link virtual host config from `sites-available` to `sites-enabled`

``` bash
ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/example
```

then chack again nginx config `nginx -t` if you are sure with the config then you can reload nginx:

> nginx -s reload

done!

> You can add or modify vhost config then check nginx and reload nginx.
