教务处迁移记录
===============================

系统 :: 

   [root@localhost ~]# cat /etc/issue
   CentOS release 6.3 (Final)
   Kernel \r on an \m


安装依赖
---------------------

依赖包:: 
  
  sudo yum install gcc g++ make automake autoconf curl-devel openssl-devel zlib-devel httpd-devel apr-devel apr-util-devel sqlite-devel

可以使用这个命令安装依赖,开发包::
  
  yum groupinstall "Development Tools"

安装ruby
------------------

rvm 
^^^^^^^^^^^^^^^^^^ 

新建一个rails用户，用于部署，应用程序部署到这个用户的主目录::
  
  adduser rails  

安装:: 
  
  curl -L https://get.rvm.io | sudo bash -s stable
  source ~/.rvm/scripts/rvm

可能还有依赖，可以使用::
  
  rvm requirements

比如会列出::

    # For Ruby / Ruby HEAD (MRI, Rubinius, & REE), install the following:
      ruby: yum install -y gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison iconv-devel ## NOTE: For centos >= 5.4 iconv-devel is provided by glibc


ruby 
--------------------

安装:: 

  $ rvm install ruby 
  $ rvm use --default 1.9.3

问题
-------------------------

#. rvm installation not working: “RVM is not a function” 

解决::
  
  $ source ~/.rvm/scripts/rvm

#. libxml2 is missing 

::
  
  yum install libxml2
  yum install libxml2-devel libxslt libxslt-devel

#. Make sure that `gem install mysql2 -v '0.3.11'` succeeds before bundling 

::
 
  yum install mysql-devel

rails 
--------------------

http://www.modrails.com/

安装passenger ::

    $ gem install passenger


deploy 
------------------------

ssh 公钥::

  ssh-copy-id rails@210.44.176.247

创建gemset, 使用rails 用户::

  rvm gemset create jwch-web

配置好capistrano 之后
建立相关目录::
  
  cap deploy:setup

备份文件迁移,在原来机器部署目录的shared 文件夹里的这些目录打包，复制到新机器的shared 目录里::

  database.yml upload_old  uploads  workflow_pic

  tar cvf  backup.tar database.yml uploads/ upload_old/ workflow_pic/ 

  scp backup.tar rails@192.168.3.81:~/jwch-web/shared 
    


