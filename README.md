# 📚 Vasa Knjiga API Gateway
Vasa Knjiga API Gateway, built in [nginx](https://nginx.com/). It's main purpose is for routing, logging and protecting private routes aswell rate limiting. It is a entry point for client application.

## 📋 Prerequisites
[nginx](https://nginx.com/)

Download nginx, if you are linux user (ubuntu). You can type following command in your terminal 

```
apt-get upgrade && apt-get install nginx -y
```

## 🏷️ Features
- Access Control
  - Session Managment
  - Rate Limiting
- Request routing
- Access & Error logs
- SSL Termination (soon)
- Load Balancing (soon)
- Micro Caching (soon)

## ✍️ Examples

### Session managment for protected routes
With Vasa Knjiga API Gateway, we can do session check with internal route /auth/required.
Your /auth/required endpoint is provided by your web server (nodejs), or you can use [Vasa Knjiga Auth Server](https://github.com/vojinpavlovic/vasaknjiga_auth)
In nginx, it is a internal location that acts as a subrequest when accessing some private resource.
As it acts as subrequest, it will pass to resource server if your auth web server returns 2xx http status, and return 4xx http status code if session is not valid.
I do recommend that you use 403 http code (forbidden), as it is already defined in errors.conf and will return JSON client friendly error.

We would need to define [auth_request](https://nginx.org/en/docs/http/ngx_http_auth_request_module.html) in specific location.


```
location /some/private/route/ {
  auth_request /auth/required
  proxy_pass http://some-protected-resource/
  # ... rest
}
```

### Rate limiting specific location

For rate limiting, we have eight levels. One is most non aggressive, and eight is most aggresive. You can define your rate limiting strategy. For more information you can check [nginx rate limiting docs](https://docs.nginx.com/nginx/admin-guide/security-controls/controlling-access-proxied-http/).
If you want to know what rate limiting levels do, check [limit_req.conf](https://github.com/vojinpavlovic/vasaknjiga_gateway/blob/main/limit_req.conf).

So, in your location we would need to add rate limiting. We would need to add limit_req zone=[your-rate-limit-zone];

```
location /some/location/ {
  limit_req zone=three;
  proxy_pass http://some-location/;
  # ... rest
}
```

### Access and error logs

For debugging purpose, finding malicious requests, DoS and DDoS and monitoring, every web server would like to have some sort of logging. Vasa Knjiga API Gateway has access log for OK requests and error log for BAD requests. For more information about logging and it's formatting you can check [nginx logging](https://docs.nginx.com/nginx/admin-guide/monitoring/logging/).

You don't have to create file in /var/log/nginx, nginx will do it for yourself. What you need to is actually define access/error log path and file.

I do use default formatting. You can do your own formatting with regex or compression.

```
location /some/location/ {
  access_log /var/log/nginx/some.location.access.log;
  error_log /var/log/nginx/some.location.error.log;
  proxy_pass http://some-location/;
}
```
