## 服务器上部署两套应用，但80端口只有一个，怎么办

```
upstream www.a.com {
    server localhost:8080;
}
upstream pan.a.com {
    server localhost:8081;
}
 
server {
    listen       80;
    server_name  www.a.com;
    location / {
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_pass http://www.a.com;
    }
}
server {
    listen       80;
    server_name  pan.a.com;
    location / {
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_pass http://pan.a.com;
    }

————————————————
版权声明：本文为CSDN博主「shuangyueliao」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/shuangyueliao/article/details/83109734
```