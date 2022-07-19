# NGINX

![Nginx Logo](https://miro.medium.com/max/1400/1*z7-lgU_f9u7IXzuMa9Cebw.png)

## Useful commands in nginx for ubuntu

##### sudo nginx OR sudo systemctl start nginx  (for starting nginx)
##### sudo nginx -t (update nginx setting after updating config file)
##### sudo systemctl reload nginx (for reloading nginx)
##### sudo systemctl restart nginx (for restating nginx)
##### sudo systemctl stop nginx (for stopping nginx)

## Basic structure for nginx server config file (having both frontend and backend deployed)

```
upstream loadbalancer {
  least_conn;
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

        location /api/ {
                proxy_pass http://loadbalancer/;
                proxy_buffering         on;
        }

}
```

