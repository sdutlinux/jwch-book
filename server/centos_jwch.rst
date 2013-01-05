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
  
  curl -L https://get.rvm.io | bash -s stable
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

rvm installation not working: “RVM is not a function” 
解决::
  
  $ source ~/.rvm/scripts/rvm


rails 
--------------------

http://www.modrails.com/

安装passenger ::

    $ gem install passenger




