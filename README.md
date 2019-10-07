# Sample Nginx Reverse Proxy

> Check nginx config `nginx -t`

Link virtual host config from `sites-available` to `sites-enabled`

``` bash
ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/example
```

then chack again nginx config `nginx -t` if are you sure with the config then you can reload nginx:

> nginx -s reload

You can update config, check nginx and reload nginx.