# NGINX

![Nginx Logo](https://miro.medium.com/max/1400/1*z7-lgU_f9u7IXzuMa9Cebw.png)

## Useful commands in nginx for ubuntu

##### sudo nginx OR sudo systemctl start nginx  (for starting nginx)
##### sudo nginx -t (update nginx setting after updating config file)
##### sudo systemctl reload nginx (for reloading nginx)
##### sudo systemctl restart nginx (for restating nginx)
##### sudo systemctl stop nginx (for stopping nginx)

## Basic structure for nginx server config file (having both frontend and backend deployed)
![image](https://user-images.githubusercontent.com/44976021/179823620-7bb10265-1f5d-4abd-8859-c992bbe94c63.png)

```
upstream loadbalancer {
  least_conn;		# load balancing algorithm provided by nginx
  server localhost:3500;
  server localhost:3501;
  server localhost:3502;
  server localhost:3503;
}

server {

	index index.html index.htm index.nginx-debian.html;
	server_name example.com www.example.com;

        # react app & front-end files
        location / {
                root /root/frontend/build;
                try_files $uri /index.html;
        }
	
	# backend app
        location /api/ {
                proxy_pass http://loadbalancer/;
                proxy_buffering         on;
        }

}
```

## Important and Helpful files of nginx
##### 1) error log file path ->  /var/log/nginx/error.log (holds information what wrong happend while making request to server)
##### 2) config file path ->  /etc/nginx/sites-available/default (holds the data as shown above)
##### 3) access file path -> /var/log/nginx/access.log (tracks record of successfull requests on server)

## Best Source For Deploying Both Front End And Backend On Same Server
https://medium.com/geekculture/deploying-a-react-app-and-a-node-js-server-on-a-single-machine-with-pm2-and-nginx-15f17251ee74

## For Generating SSl Certificate for server
```
sudo snap install core; 
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```
You can also test Certbot’s automatic renewal: ```sudo certbot renew --dry-run```



## basic structure for /etc/nginx/nginx.conf (just for idea purpose)

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        multi_accept on;

}


http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        types_hash_max_size 2048;
        server_tokens off;
        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
	default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##
	access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;
     
        ##
        # Gzip Settings
        ##

        gzip on;
        # gzip_min_length 100;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 3;
        # gzip_buffers 16 8k;
	# gzip_http_version 1.1;
        # gzip_types text/css text/xml application/xml application/xml+rss text/javascript;
        # gzip_disable "msie6";
        ##
        # Virtual Host Configs
        ##
        
        # client_body_buffer_size 16k;
        # client_header_buffer_size 1k;
        client_max_body_size 25m;
        # large_client_header_buffers 2 1k;
        
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}

```

