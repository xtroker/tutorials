# Configuring Cerbot (Letsencrypt) on AWS S3

## 1. Get certbot first:

```
$ wget https://dl.eff.org/certbot-auto
$ chmod a+x certbot-auto
```

## 2. Shutdown Server

```
$ nginx -s stop
```

### 3. Write a config file

```
$ mkdir -p /etc/letsencrypt/configs/
$ nano /etc/letsencrypt/configs/groupmaps.com.conf
```

- Add the following to the file
  ```
  # domains to retrieve certificate
  domains = devapi.groupmaps.com
  # increase key size
  rsa-key-size = 4096
  # the CA endpoint server
  server = https://acme-v02.api.letsencrypt.org/directory
  # the email to receive renewal reminders, IIRC
  email = example@example.com
  # turn off the ncurses UI, we want this to be run as a cronjob
  text = True
  ```

### 4. Execute the certbot

```
$ sudo ./certbot-auto --standalone --config /etc/letsencrypt/configs/groupmaps.com.conf certonly
```

This didn't worked first time but We found a solution [here](https://stackoverflow.com/questions/59525274/lets-encrypt-certbot-on-aws-linux)

- Edit the file `/usr/bin/certbot-auto` to recognize your version of Linux:
  ```
  $ sudo nano /usr/bin/certbot-auto
  ```
- Find this line in the file and change it like so (likely near line near 780):
  ```bash
    elif [ -f /etc/redhat-release ]; then
    and replace whole line with this:
    elif [ -f /etc/redhat-release ] || grep 'cpe:.*:amazon_linux:2' /etc/os-release > /dev/null 2>&1; then
  ```
- Save and exit

- After that everything worked fine, this should be the output
  > IMPORTANT NOTES:
  >
  > - Congratulations! Your certificate and chain have been saved at:
  >   /etc/letsencrypt/live/devapi.groupmaps.com/fullchain.pem
  >   Your key file has been saved at:
  >   /etc/letsencrypt/live/devapi.groupmaps.com/privkey.pem
  >   Your cert will expire on 2020-11-26. To obtain a new or tweaked
  >   version of this certificate in the future, simply run certbot-auto
  >   again. To non-interactively renew _all_ of your certificates, run
  >   "certbot-auto renew"

### 5. Lets modify the nginx configuration

```
$ nano /etc/nginx/nginx.conf
```

- We will redirect all http traffic

  On the http section

  ```nginx
  server {
      listen 80 default_server;

      server_name _;

      return 301 https://$host$request_uri;
  }
  ```

- On the TLS settings
  ```nginx
  ssl_certificate "/etc/letsencrypt/live/devapi.groupmaps.com/fullchain.pem";
  ssl_certificate_key "/etc/letsencrypt/live/devapi.groupmaps.com/privkey.pem";
  ```

Is going to end up like this

```nginx
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;


    server {
        listen 80 default_server;

        server_name _;

        return 301 https://$host$request_uri;
    }

# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2;
        listen       [::]:443 ssl http2;
        server_name  _;
        root         /usr/share/nginx/html;

        location / {
            proxy_pass http://localhost:5000;
        }


        ssl_certificate "/etc/letsencrypt/live/devapi.groupmaps.com/fullchain.pem";
        ssl_certificate_key "/etc/letsencrypt/live/devapi.groupmaps.com/privkey.pem";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

}
```

### 6. Reload **nginx** config

```
$ nginx -s reload
```

### 7. Start the server

```
$ service nginx start
```

### 8. Check in a brower that everything went well

    https://devapi.groupmaps.com/api/

### 9. Renewing the certificate

To Manually renew the certificates only run the following line

```
/home/ec2-user/certbot-auto renew --nginx --no-self-upgrade --config /etc/letsencrypt/configs/groupmaps.com.conf
```

**We will now enable a cron job to make sure the certificate is auto renew**

- Modify the crontab

  ```
  $ sudo nano /etc/crontab
  ```

- Add a line similar to the following and save the file.

  > 39 1,13 \* \* \* root /home/ec2-user/certbot-auto renew --nginx --no-self-upgrade --config /etc/letsencrypt/configs/groupmaps.com.conf

- Note from amazon AWS docs

  Schedules a command to be run at 01:39 and 13:39 every day. The selected values are arbitrary, but the Certbot developers suggest running the command at least twice daily. This guarantees that any certificate found to be compromised is promptly revoked and replaced.

- Restart the cronjobs service
  ```
  $ sudo systemctl restart crond
  ```
