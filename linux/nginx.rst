nginx web服务器
=========================


反向代理服务器
----------------------

一篇不错的文章： http://www.packtpub.com/article/using-nginx-reverse-proxy 

创建 proxy.conf ::
  
  proxy_redirect          off;
  proxy_set_header        Host            $host;
  proxy_set_header  X-Real-IP       $remote_addr;
  proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
  client_max_body_size    10m;
  client_body_buffer_size 128k;
  proxy_connect_timeout   90;
  proxy_send_timeout      90;
  proxy_read_timeout      90;s
  proxy_buffers           32 4k


在主机配置里include 这代理配置就可以了 ::   
  
  server {
  listen 80;
  server_name example1.com;
  access_log /var/www/example1.com/log/nginx.access.log;
  error_log /var/www/example1.com/log/nginx_error.log debug;
  location / {
    include proxy.conf;
    proxy_pass http://127.0.0.1:8080;
    }
 }
