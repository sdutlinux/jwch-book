新手入门
=============================

入门项目
------------------

实现 http://jwch.sdut.edu.cn/hero

要求: :: 

    使用python 语言, 框架使用flask, 前端可以使用bootstrap

    数据库可使用mysql+sqlite3

    要在 linux下开发, 代码使用git管理, 并托管到github上.

    要部署到生产环境

    有问题加群 sdutlinux@im.partych.at

指南
-----------------------

linux 相关
^^^^^^^^^^^^^^^^^^^^^

* 推荐学习平台 
    http://linuxcast.net/


html css javascript
^^^^^^^^^^^^^^^^^^^^^^
* w3c 教程

BootStrap
""""""""""""""""""""

* 官网 http://twitter.github.io/bootstrap/  

* 国内 http://www.bootcss.com/

python
^^^^^^^^^^^^^^^^^^^^^

* 简明python教程   

    web版: http://woodpecker.org.cn/abyteofpython_cn/chinese/

    pdf版: http://pan.baidu.com/share/link?shareid=517538&uk=3642093566

* learn python the hard way  http://learnpythonthehardway.org/book/

flask
""""""""""""""""""""""

* 安装::

    sudo pip install Flask

* 一个最小的应用 hello world

    .. code-block:: python
    
        from flask import Flask
        app = Flask(__name__)
    
        @app.route('/')
        def hello_world():
            return 'Hello World!'
          
        if __name__ == '__main__':
            app.run()
    
* 文档地址 http://docs.torriacg.org/docs/flask/

sqlite3
""""""""""""""""""""""""""

* 安装::

    sudo pip install sqlite3

* 创建数据库::

          sqlite3 dbname.sqlite3
          # 创建完数据库后,使用下面命令退出
          .quit
        
* 在Flask中使用sqlite3

    .. code-block:: python

        import sqlite3
        from flask import g
    
        DATABASE = '/path/to/database.db'
    
        def connect_db():
            return sqlite3.connect(DATABASE)
    
        @app.before_request
        def before_request():
            g.db = connect_db()
    
        @app.teardown_request
        def teardown_request(exception):
            if hasattr(g, 'db'):
                g.db.close()
    
详见 http://docs.torriacg.org/docs/flask/patterns/sqlite3.html#sqlite3


开发工具 
^^^^^^^^^^^^^^^^^^^^^^^

vim sublime 
""""""""""""""""""

* https://github.com/gmarik/vundle

other 
"""""""""""""

* goagent 教程  http://www.chinagdg.com/thread-1493-1-1.html 
