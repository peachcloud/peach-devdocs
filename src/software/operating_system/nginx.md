# Nginx Configuration

[Nginx](https://www.nginx.com/) is used as a reverse proxy for the `peach-web` application. Requests to `http://peach` and `http://www.peach` on port `80` are passed to the `peach-web` application on `http://127.0.0.1:3000`.

The configuration file for `nginx` can be found at `/etc/nginx/sites-available/peach.conf`. The contents are as follows:

```nginx
server {
        listen 80;
        server_name peach www.peach;
        location / {
                proxy_pass http://127.0.0.1:3000;
        }
}
```

## Nginx Cheatsheet

Symlink `sites-available/*.conf` to `sites-enabled/*.conf`:

`sudo ln -s /etc/nginx/sites-available/peach.conf /etc/nginx/sites-enabled/`

Check correctness of configuration:

`sudo nginx -t`

Reload nginx:

`sudo nginx -s reload`
