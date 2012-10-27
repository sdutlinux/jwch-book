=======================
lnmp快速安装
=======================

.. _lnmp:

:lnmp官网: http://lnmp.org

lnmp一键安装包
-----------------------------

安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash 
    :linenos:

    wget -c http://soft.vpser.net/lnmp/lnmp0.9.tar.gz

    tar zxvf lnmp0.9.tar.gz

    cd lnmpo.9/
    
    #CentOS/RadHat执行：

    ./centos.sh

    #Debian执行：

    ./debian.sh

    #Ubuntu执行：

    ./ubuntu.sh

    #LNMP升级到LNMPA，执行：

    ./apache.sh

    #LNMP升级到LNMPA，执行：

    ./apache.sh


管理
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash 
    :linenos:

    LNMP状态管理： /root/lnmp {start|stop|reload|restart|kill|status}

    LNMPA状态管理： /root/lnmpa {start|stop|reload|restart|kill|status}
    
    Nginx状态管理：/etc/init.d/nginx {start|stop|reload|restart}
    
    PHP-FPM状态管理：/etc/init.d/php-fpm {start|stop|quit|restart|reload|logrotate}

    PureFTPd状态管理： /root/pureftpd {start|stop|restart|kill|status}

    MySQL状态管理：/etc/init.d/mysql {start|stop|restart|reload|force-reload|status}

    Apache状态管理：/root/httpd {start|stop|restart|graceful|graceful-stop|configtest|status}


    添加虚拟主机    /root/vhost.h

    phpinfo     http://$domain/phpinfo.php

    phpmyadmin  http://$domain/phpmyadmin/

    探针    http://$domain/p.php

    PureFtp用户管理：http://$domain/ftp/


配置文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash 
    :linenos:

    Nginx主配置文件：/usr/local/nginx/conf/nginx.conf

    MySQL配置文件：/etc/my.cnf

    PHP配置文件：/usr/local/php/etc/php.ini
    
    PureFtpd配置文件：/usr/local/pureftpd/pure-ftpd.conf

    PureFtpd MySQL配置文件：/usr/local/pureftpd/pureftpd-mysql.conf

    Apache配置文件：/usr/local/apache/conf/httpd.conf


技术支持
-----------------------------------

技术支持论坛：http://bbs.vpser.net/forum-25-1.html

