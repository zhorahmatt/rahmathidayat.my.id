---
title: "Nginx Reverse Proxy Go"
date: 2021-07-21T14:19:44+08:00
draft: false
comments: true
---

This is the recap of my recent work at office. so i can check it again if i have the same problem later.

### Using Nginx as Reverse Proxy
A simple way to access your go app is using reverse proxy in nginx. Nginx makes this really easy so without deep understanding, you can do it by yourself. Our apps are running on 2 different port (9002 and 9003), the configuration will be like : 

```
server {
        
        server_name your-server-name;

        location / {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://localhost:9001;
        }
        location /v3/setting {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://localhost:9002;
        }
}
```

using `proxy_set_header X-Real-IP $remote_addr` simply means you make sure the request to your server include the real ip so you can use that to check or whatever you want to use. the ports `9001 and 9002` are still private so only serve from single source.