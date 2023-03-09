

## nginx

```
sudo vi /etc/nginx/sites-available/default
```

```
location / {
    proxy_pass http://localhost:8080/;
    proxy_redirect off;
    charset utf-8;
    
    proxy_set_header X-Real_IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-NginX-Proxy true;
	
}

location /api {
    proxy_pass http://localhost:3000/api;
    proxy_redirect off;
    charset utf-8;
    
    proxy_set_header X-Real_IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-NginX-Proxy true;
	
}
```



오타검사

```
 sudo nginx -t
```

```
sudo systemctl restart nginx

sudo systemctl start nginx
sudo systemctl stop nginx
```

```
sudo ufw allow 80
sudo ufw allow 3000
```



퍼블릭 IPv4 DNS 로 접속