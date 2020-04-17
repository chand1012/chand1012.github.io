In this tutorial, we will be setting up a Flask server using Gunicorn and NGINX on Ubuntu 18.04 LTS. 

## Requirements

- Any system running Ubuntu 18.04 LTS with SSH enabled.
- An SSH client.

## Installing

After connecting via SSH to your server as root, run the following commands to install the required programs:


    apt update
    apt upgrade -y
    apt install nginx python3 python3-pip virtualenv


This will install Python, NGINX, and the virtual enviroment needed to run the app. It will also install PIP to download the additional packages. After that concludes we can move on with setting up the application itself.

## Setup

### Network

Before continuing, we want to restrict port 8080 incoming so that only NGINX can access Gunicorn. To do that, execute the following command:

```
iptables -A INPUT -p tcp --destination-port 8080 -j DROP
```

This will prevent your users from accessing the app without going through the NGINX proxy. 

### Flask Setup

Next we need to set up the file structure for the application. Execute the following in order:


    mkdir -p /var/www/flask
    cd /var/www/flask
    virtualenv hello-world --python=python3
    source hello-world/bin/activate
    cd hello-world
    pip3 install gunicorn flask


Next, we will write the program for the app itself. Enter the following command:

```
nano app.py
```

Then enter the following code in the script


    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def index():
        return "Hello from Flask!"


### Gunicorn

Next we need to run the app with Gunicorn. To do this, execute the following command:

```
gunicorn -b 127.0.0.1:8080 app:app 
```

This will start the app on localhost only with the port 8080. If you wish to run the app with multithreading enabled, specify a number of workers with the `-w` flag:

```
gunicorn -w 2 -b 127.0.0.1:8080 app:app 
```

[Gunicorn's Documentation](https://docs.gunicorn.org/en/stable/settings.html#workers) recommends that you use between two and four workers per core on your VPS.

Use `Ctrl+C` to exit the application.



Now, most people do not want to SSH into their server every time you want people to access your web app, so we will be making a service file to run the app on boot of the server. Execute the following command to create the file:

```
nano /lib/systemd/system/flask.service
```

Paste the following configuration in to the file:


    [Unit]
    Description=Gunicorn Flask Application
    After=network.target
    After=systemd-user-sessions.service
    After=network-online.target

    [Service]
    User=root
    Type=simple
    ExecStart=/var/www/flask/hello-world/start.sh
    TimeoutSec=30
    Restart=on-failure
    RestartSec=15
    StartLimitInterval=350
    StartLimitBurst=10

    [Install]
    WantedBy=multi-user.target


This will enable launch of the app on boot of your VPS using `systemd`. NGINX is already configured to do this when you install it. Now we need the `start.sh` file. Run the following to create the file:

```
nano /var/www/flask/hello-world/start.sh
```

Then paste this bash code to launch the app:


    #!/bin/bash
    echo Starting Flask example app.
    cd /var/www/flask
    source hello-world/bin/activate
    cd hello-world
    gunicorn -w 2 -b 127.0.0.1:8080 app:app


Then allow the file to be executable with:

```
chmod +x /var/www/flask/hello-world/start.sh
```

In the last line of the `start.sh` file, the number `2` can be replaced with 2-4 times the number of cores on your VPS. Finally, to enable startup of the application on boot, execute the following:

```
systemctl enable flask
```

### NGINX

Next we need to set up NGINX. NGINX allows us to set up a reverse proxy that redirects traffic to our app being hosted with Gunicorn. 

First deactivate the default config, then change to the `sites-enabled` directory and create a  new file:


    rm /etc/nginx/sites-enabled/default
    cd /etc/nginx/sites-enabled
    nano reverse-proxy.conf


Paste the following configuration into the file:


    server {
        listen 80;
        listen [::]:80;
        server_name _;
        access_log /var/log/nginx/reverse-access.log;
        error_log /var/log/nginx/reverse-error.log;

        location / {
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header HOST $http_host;
                    proxy_pass http://127.0.0.1:8080;
                    proxy_redirect off;
        }
    }


After restarting nginx with `systemctl restart nginx`, you have a working app hosted on your server. Navigate to the app IP address and you should see `Hello from Flask!` in the top left corner of your favorite browser. This app will function as normal, but is especially suseptible to attacks as it doesn not have HTTPS. If you wish to configure that as well, continue along.

## Adding HTTPS

We will be using a self-signed certificate on your application. This is adequate for testing, but you will want a key signed by a Certificate Authority, like Let's Encrypt, later down the line.

### Generating the Key

To create a self signed cert, execute the following command:

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/selfsigned.key -out /etc/ssl/certs/selfsigned.crt
```

You can change the number after the `-days` flag to whatever number you want, and it will last that long. If you are only using this key for testing, one year should be fine. Follow the on screen prompts to finish signing your own key.

Next, we need to generate a Diffie-Hellman group, which is used for additional security. This will take about 20 minutes on a server.

```
openssl dhparam -out /etc/nginx/dhparam.pem 4096
```

### Adding to NGINX

To add to the web server, you will have to change the server configs. Remove the config you created previously and edit the configuration file as you did before:


    rm /etc/nginx/sites-enabled/reverse-proxy.conf
    nano /etc/nginx/sites-enabled/reverse-proxy.conf


Then paste the following config into your file:


    server {
        listen 80;
        listen [::]:80;
        server_name example.com;

        return 302 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl;
        listen [::]:443 ssl;
        ssl_certificate /etc/ssl/certs/selfsigned.crt;
        ssl_certificate_key /etc/ssl/private/selfsigned.key;

        ssl_dhparam /etc/nginx/dhparam.pem;
        location / {
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header HOST $http_host;
                    proxy_pass http://127.0.0.1:8080;
                    proxy_redirect off;
        }
    }


Before closing and saving, replace `example.com` with the IP Address of your VPS, or if you have an A record DNS pointing to your VPS's IP, use that instead. After this is complete, restart NGINX with `systemctl restart nginx`. 

There will be a warning that comes up on your browser saying that your website is dangerous, this is just warning you that the certificate was not signed by a real authority and can be ignored. You now have a functioning reverse proxy with HTTPS for your Flask server. 
