# 📚 Vasa Knjiga API Gateway
Vasa Knjiga API Gateway, built in [nginx](https://nginx.com/). It's main purpose is for routing, logging and protecting private routes aswell rate limiting. It is a entry point for client application.

## 📋 Prerequisites
[nginx](https://nginx.com/)

Download nginx, if you are linux user (ubuntu). You can type following command in your terminal 

```
apt-get upgrade && apt-get install nginx -y
```

## Getting started (Linux)

Firstly you would need to go inside nginx directory and create directory called api

```
cd /etc/nginx && mkdir api && cd api
```

After that, I recommend to initialise git repository 

```
git init 
```

Once you initialised git in /etc/nginx/api, we would need to add git origin

```
git remote add origin https://github.com/vojinpavlovic/vasaknjiga_gateway.git
```

Now if everything is fine, we can fetch vasaknjiga_gateway

```
git fetch 
```

Since main is default branch, but in any case we will checkout to that branch

```
git checkout main 
```

Note: Why not just clone? You can do that, but once you clone you will be left with /etc/nginx/api/vasaknjiga_gateway/[source files] and in order to avoid that, you can follow steps above. There is different way to organize your files. Since ```server.conf``` is main file, you can rename it to domain.com and move it to sites-enabled. Of course, whatever suits for you. But keep in mind, if you change your folder name you would need to change includes paths. 

Once your files are ready, you would need to use your editor (for this example I will use nano but you can use what you prefer) and include server.conf into main nginx configuration file. Since if you followed these steps we would need to go one directory back

```
cd ..
```

Then we would open nginx.conf

```
  nano nginx.conf
```

Find start of http block and include your files at the top

```
http {
  include api/server.conf;
  #.. rest of the configuration
}
```

If everything looks fine, we can restart but before that we will test if everything is fine 

```
nginx -t
```

You would see something like this

```
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

In case test failed and you did everything with this tutorial. Do not hesitate! You can open issue, I will gladly help.

So if test is fine, we would need to restart nginx service

```
systemctl restart nginx
```

And that's it your vasaknjiga gateway is running!
