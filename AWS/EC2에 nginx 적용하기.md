# EC2에 nginx 적용하기



## nginx ?

- 동시접속 처리에 특화된 웹 서버 소프트웨어로, 가벼움과 높은 성능을 지향하고있습니다.
- 요청에 응답하기 위해 비동기 이벤트 기반 구조
- 이벤트 기반(Event-driven) 구조는 서버에 많은 부하가 생길 경우의 성능을 예측하기 쉽게 해 주며, 고정된 프로세스만 생성하고, 내부에서 비 동기 방식으로 작업들을 처리하기 때문에 스레드/프로세스를 효율적으로 처리할 수 있습니다



------

## nginx와 Apache 비교 🔎



![img](./assets/img-1686363552809-1.png)



 

#### 💡Nginx

비동기, 이벤트 기반 구조 - Event-driven

고정된 프로세스만 사용

동시 접속 수 처리에 특화됨

------

#### 💡Apache

스레드/프로레스 기반 구조 - Thread-prgramming

한 개의 스레드 당 한개의 프로세스 사용

동시 접속 수가 많은 경우 많은 스레드가 생성되며 메모리, CPU 낭비가 심함

 

------

 # Nginx

> Nginx는 정적 컨텐츠를 제공해주는 프록시 서버이다. apache의 강력한 라이벌? 이며, 현재는 점유율이 앞서고 있다고 한다.


이제 기본 conf 파일을 설정하자.
conf 파일은 "/etc/nginx/nginx.conf"파일이다.

cat를 통해 파일을 살펴보자.

```bash
sudo vi /etc/nginx/nginx.conf
```

conf 파일을 살펴보면 다음과 같이  "sites-enabled" 경로의 파일들을 포함하라고 설정되어있다. 

```bash
include /etc/nginx/conf.d/*.conf
include /etc/nginx/sites-enabled/*.conf
```



nginx를 처음 설치하면 실행되지 않고 있다. 실행해보자.

```bash
sudo service nginx start
```

실행하고 status를 살펴보자!

```bash
sudo service nginx status
```

정상적인 실행이라면 다음과 같이 나온다.

```bash
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset:>
     Active: active (running) since Sun 2022-03-20 18:51:45 UTC; 43min ago
       Docs: man:nginx(8)
   Main PID: 1686 (nginx)
      Tasks: 2 (limit: 1147)
     Memory: 4.6M
     CGroup: /system.slice/nginx.service
             ├─1686 nginx: master process /usr/sbin/nginx -g daemon on; master_>
             └─1687 nginx: worker process

Mar 20 18:51:45 ip-172-31-39-107 systemd[1]: Starting A high performance web se>
Mar 20 18:51:45 ip-172-31-39-107 systemd[1]: Started A high performance web ser
```

Active부분에 "active(running)"이라고 나온 것을 볼 수 있다.



위의 파일들에 대한 설명

- sites-available : 해당 파일은 가상 서버에 관련된 서버의 설정들이 위치하는 디렉터리이다. 해당 디렉터리에 존재하는 설정파일들은 사용하지 않더라도 존재한다.
- sites-enabled : "sites-available"에 존재하는 설정 파일들 중, 사용하는 설정파일만 link하여 사용할 수 있도록 하는 디렉터리이다.
- nginx.conf : nginx 관련 설정을 블록단위로 설정하는 곳이다. 여기서 "sites-enabled"에 존재하는 파일들을 불러온다.

> 한마디로 "available"에 설정 파일을 쓴다.
> "enabled"에 available 설정 파일을 링크한다.
> "nginx.conf"파일에서 "enabled"의 "default"파일을 불러온다.\

우리는 enabled의 "default" 파일만 수정해주면 링크가되어 자동 적용이 된다.

------

# 리버스 프록시 설정하기

이전에 보았던 default 파일에 리버스 프록시를 설정해야한다.

현재 "listen"을 살펴보면 80포트로 요청을 받는 것을 알 수 있다.

이제 80포트 요청에 대한 서버를 향한 요청을 적어주자.

```bash
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
    
   		proxy_pass_header Server;

        proxy_set_header Host $http_host;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Scheme $scheme;

        proxy_pass http://전달할 주소:abcd;
        
        try_files $uri $uri/ =404;
	}
}
```

"location /"을 살펴보면 80포트로 들어온 값을 다시 abcd포트로 요청하는 내용이다.

이제 nginx를 재가동 해보자.

```bash
sudo service nginx restart
```



## 내가 사용한 내용

- http 로 들어오면 https 로 전환

```bash
server {
   listen 80 default_server;
   listen [::]:80 default_server;

    server_name ~.;
   
    if ($http_x_forwarded_proto != 'https') {
           return 301 https://$host$request_uri;
      }      


   location / {
      proxy_pass_header Server;
      proxy_set_header Host $http_host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Scheme $scheme;
      proxy_set_header X-NginX-Proxy true;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      
      proxy_pass http://127.0.0.1:8080;
      proxy_redirect off;
      
      #error_page 405 =200 $uri;
   }
}
```



https://velog.io/@jkijki12/%EB%B0%B0%ED%8F%AC-Aws-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%EC%97%90-Nginx-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0

