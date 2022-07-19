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

