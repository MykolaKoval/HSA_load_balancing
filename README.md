# HSA load balancing
Load balancing technics

<h4>Ngrok</h4>

[Ngrok quick start](https://ngrok.com/docs/getting-started/)

Put your app online:
```
ngrok http http://localhost:8080
```

Create own domain via ngrok: ``domain=learning-cub-moved.ngrok-free.app``
```
ngrok http --domain=learning-cub-moved.ngrok-free.app http://localhost:8080
```

<h4>GeoIP support</h4>

* [Nginx GeoIP2 module](https://github.com/nginx-modules/ngx_http_geoip2_module)
* [Nginx docker image with GeoIP support](https://hub.docker.com/r/anroe/nginx-geoip2)

<h3>Task</h3>

1. Set up load balancer on nginx that will have 1 server for UK, 2 servers for US, and 1 server for the rest. 
2. In case of failure, it should send all traffic to backup server. 
3. Health check should happen every 5 seconds.

<h3>Description</h3>

Configured necessary setup. Setup is tested with TouchVPN. 

To test backup and health checks please stop ``others-server`` container. 

Only passive health checks are configured.

Geo-balancer logs
```
2024-09-12 12:00:52 192.168.65.1 - [12/Sep/2024:09:00:52 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.38.148.59" country_code=US
2024-09-12 12:02:14 192.168.65.1 - [12/Sep/2024:09:02:14 +0000] "GET /country/ HTTP/1.1" 200 33 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "185.80.221.236" country_code=NL
2024-09-12 12:02:19 192.168.65.1 - [12/Sep/2024:09:02:19 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "185.80.221.236" country_code=NL
2024-09-12 12:02:19 192.168.65.1 - [12/Sep/2024:09:02:19 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "185.80.221.236" country_code=NL
2024-09-12 12:02:20 192.168.65.1 - [12/Sep/2024:09:02:20 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "185.80.221.236" country_code=NL
2024-09-12 12:02:26 192.168.65.1 - [12/Sep/2024:09:02:26 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "185.80.221.236" country_code=NL
2024-09-12 12:02:37 192.168.65.1 - [12/Sep/2024:09:02:37 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.253.236.163" country_code=UA
2024-09-12 12:04:16 192.168.65.1 - [12/Sep/2024:09:04:16 +0000] "GET /country/ HTTP/1.1" 200 35 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.253.236.163" country_code=UA
2024-09-12 12:04:30 192.168.65.1 - [12/Sep/2024:09:04:30 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.253.236.163" country_code=UA
2024-09-12 12:04:32 192.168.65.1 - [12/Sep/2024:09:04:32 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "-" country_code=US
2024-09-12 12:04:40 192.168.65.1 - [12/Sep/2024:09:04:40 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.253.236.163" country_code=UA
2024-09-12 12:04:43 192.168.65.1 - [12/Sep/2024:09:04:43 +0000] "GET /country/ HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36" "92.253.236.163" country_code=UA
2024-09-12 12:04:16 2024/09/12 09:04:16 [error] 29#29: *27 connect() failed (113: No route to host) while connecting to upstream, client: 192.168.65.1, server: , request: "GET /country/ HTTP/1.1", upstream: "http://172.20.0.7:80/country/", host: "learning-cub-moved.ngrok-free.app"
```