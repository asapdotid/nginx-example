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

## Custom Error page

You can custom error page with example in `snippets/error.conf`:

``` bash
error_page 404  @404;
error_page 500 501 502 @502;

# error_page 404 /404.html;
location @404 {
	root /var/www/error;
	try_files /404.html =404;
	internal;
}
# error_page 502 /maintenance.html;
location @502 {
    root /var/www/error;
    try_files /maintenance.html =502;
    internal;
}
```

and

> Create Html files for error handling in `/var/www/error` directory:

``` bash
mkdir -p /var/www/error
chown -R www-data:www-data /var/www/error
```

place your custom error page to this directory, such as:

- 404.html
- maintenance.html
- etc.

then include to the default config or vhost config:

``` bash
...

# Error handling
include snippets/error.conf;

...
```

## Custom Block direct Access IP Public

> For some reason you can block user to access direct IP Public, which simple configuration.
> You can permanen redirect to primary domain name or block to error handling like 404.
> This configuration place to `nginx.conf` only one script load, better not include to the virtual host app.

``` bash
server {
  listen 80;
  listen [::]:80;     
  server_name _;
  return 301 https://domain.com$request_uri;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;      
  server_name _;
  return 301 https://domain.com$request_uri;
}
```

or

``` bash
server {
  listen 80;
  listen [::]:80;     
  server_name _;
  include snippets/error.conf;
  return 404;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;      
  server_name _;
  include snippets/error.conf;
  return 404;
}
```

> You can add or modify vhost config then check nginx configuration and reload nginx configuration.

For discussion please visit the [Nginx Indonesia Telegram group](https://t.me/id_nginx).
Or contact Telegram `@asapdotid`
