Gunicorn, Python WSGI HTTP Server for UNIX
==============================================
:作者: zhwei http://zhangweide.cn/

Gunicorn 'Green Unicorn' is a Python WSGI HTTP Server for UNIX. It's a pre-fork worker model ported from Ruby's Unicorn project. The Gunicorn server is broadly compatible with various web frameworks, simply implemented, light on server resources, and fairly speedy.

Gunicorn配合Nginx反向代理实现无痛部署符合WSGI协议的Python Web框架。

.. image:: http://docs.gunicorn.org/en/latest/_images/gunicorn.png

官方文档
------------------

* http://gunicorn.org/
* http://docs.gunicorn.org/en/latest/index.html


安装
------------------

setuptools
^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    pip install gunicorn
    # or
    easy_install gunicorn

快速上手
--------------------

.. code-block:: bash

    $ sudo pip install virtualenv
    $ mkdir ~/environments/
    $ virtualenv ~/environments/tutorial/
    $ cd ~/environments/tutorial/
    $ ls
    bin  include  lib
    $ source bin/activate
    (tutorial) $ pip install gunicorn
    (tutorial) $ mkdir myapp
    (tutorial) $ cd myapp/
    (tutorial) $ vi myapp.py
    (tutorial) $ cat myapp.py

    def app(environ, start_response):
        data = "Hello, World!\n"
        start_response("200 OK", [
            ("Content-Type", "text/plain"),
            ("Content-Length", str(len(data)))
        ])
        return iter([data])

    (tutorial) $ ../bin/gunicorn -w 4 myapp:app
    2010-06-05 23:27:07 [16800] [INFO] Arbiter booted
    2010-06-05 23:27:07 [16800] [INFO] Listening at: http://127.0.0.1:8000
    2010-06-05 23:27:07 [16801] [INFO] Worker spawned (pid: 16801)
    2010-06-05 23:27:07 [16802] [INFO] Worker spawned (pid: 16802)
    2010-06-05 23:27:07 [16803] [INFO] Worker spawned (pid: 16803)
    2010-06-05 23:27:07 [16804] [INFO] Worker spawned (pid: 16804)

常用参数::

    -D  后台运行
    --pid 生成pid到相应文件
    -b 指定ip和端口
    eg：
    gunicorn code:application -D --pid /var/run/gunicorn.pid -b 127.0.0.1:5555

常用配合的Nginx配置文件
-----------------------

::

    worker_processes 1;

    user nobody nogroup;
    pid /tmp/nginx.pid;
    error_log /tmp/nginx.error.log;

    events {
        worker_connections 1024;
        accept_mutex off;
    }

    http {
        include mime.types;
        default_type application/octet-stream;
        access_log /tmp/nginx.access.log combined;
        sendfile on;

        upstream app_server {
            server unix:/tmp/gunicorn.sock fail_timeout=0;
            # For a TCP configuration:
            # server 192.168.0.7:8000 fail_timeout=0;
        }

        server {
            listen 80 default;
            client_max_body_size 4G;
            server_name _;

            keepalive_timeout 5;

            # path for static files
            root /path/to/app/current/public;

            location / {
                # checks for static file, if not found proxy to app
                try_files $uri @proxy_to_app;
            }

            location @proxy_to_app {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_redirect off;

                proxy_pass   http://app_server;
            }

            error_page 500 502 503 504 /500.html;
            location = /500.html {
                root /path/to/app/current/public;
            }
        }
    }



