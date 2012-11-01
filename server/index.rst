======================
服务器相关
======================

:编辑者: 李大双 ldshuang@gmail.com

开发记录
---------------------

 :doc:`jxzyw`


备份方案
---------------------


需要备份的数据，如mysql,sqlserver 数据库，程序源码，程序配置等，统一存储在一台机器上, 定期备份。常用的工具
cron, rsync, scp, ruby的backup gem(指定任务), ruby 的 whenever(可以很人性话的写cron)


文章，资源
^^^^^^^^^^^^^^^^^^^^^
#. `用 Backup 和 Whenever 进行备份 <http://chloerei.com/2012/08/02/use-backup-and-whenever-to-bakcup-server/>`_
#. `用Backup來備份你的網站 <http://blog.eddie.com.tw/2011/05/24/backup-your-website/>`_

#. `backup gem <https://github.com/meskyanichi/backup>`_
#. `whenever <https://github.com/javan/whenever>`_


backup & whenever
^^^^^^^^^^^^^^^^^^^^^^^^^^

backup 可以让你很方便的执行备份任务，支持各种数据库，文件，可以将文件备份到dropbox,amazon s3, ftp, 等，可以使用scp,rsync, 也可以本地存储， 
安装::
  gem install backup
你要确保你安装了ruby, 安照这篇文章安装环境 `如何快速正确的安装 Ruby, Rails 运行环境 <http://ruby-china.org/wiki/install_ruby_guide>`_ 
通过官方wiki, 和上面的两篇文章就能学会使用。

查看帮助 help:: 

  rails@ubuntu:~$ backup -h
  Tasks:
  backup decrypt --encryptor=ENCRYPTOR --in=IN --out=OUT  # Decrypts encrypted files
  backup dependencies                                     # Display the list of dependencies for Backup, check the installation status, or install them through Backup.
  backup generate:config                                  # Generates the main Backup bootstrap/configuration file
  backup generate:model --trigger=TRIGGER                 # Generates a Backup model file Note: '--config-path' is the path to the directory where 'config.rb' is located. The model fil...
  backup help [TASK]                                      # Describe available tasks or one specific task
  backup perform -t, --triggers, --trigger=TRIGGER        # Performs the backup for the specified trigger. You may perform multiple backups by providing multiple triggers, separated by...
  backup version                                          # Display installed Backup version

whenever 是对linux 下crontab 在封装，可以像下面这样写定时任务，很爽吧。 

::  

  every 3.hours do
    runner "MyModel.some_process"
    rake "my:rake:task"
    command "/usr/bin/my_great_command"
  end

  every 1.day, :at => '4:30 am' do
    runner "MyModel.task_to_run_at_four_thirty_in_the_morning"
  end
  
  every :hour do # Many shortcuts available: :hour, :day, :month, :year, :reboot
    runner "SomeModel.ladeeda"
  end
  
  every :sunday, :at => '12pm' do # Use any day of the week or :weekend, :weekday
    runner "Task.do_something_great"
  end
  
  every '0 0 27-31 * *' do
    command "echo 'you can use raw cron syntax too'"
  end
  
  # run this task only on servers with the :app role in Capistrano
  # see Capistrano roles section below
  every :day, :at => '12:20am', :roles => [:app] do
    rake "app_server:task"
  end


备份记录
--------------------

2012-10-25 
^^^^^^^^^^^^^^^^^^^^^^

备份过程
主要对教务处数据进行备份

改一下，ruby gem 的源 ::

  $ gem source -r https://rubygems.org/
  $ gem source -a http://ruby.taobao.org

安装backup::

  gem install backup

执行::

  rails@group1:~$ backup generate:model --trigger jwch --databases="mysql" --storages="scp"
  Generated model file: '/home/rails/Backup/models/jwch.rb'.
  Generated configuration file: '/home/rails/Backup/config.rb'.
  
我们要备份mysql,使用sftp的方式备份到另外一台服务器上，生成完基本的模版后，编辑models/jwch.rb
配置要备份的数据库，和sftp服务器地址密码。

执行下命令测试下 ::

  rails@group1:~/Backup/models$ backup perform --trigger jwch
  [2012/10/25 22:05:57][error] Dependency::LoadError: Dependency missing
  [2012/10/25 22:05:57][error]   Dependency required for:
  [2012/10/25 22:05:57][error]   SSH Protocol (SSH Storage)
  [2012/10/25 22:05:57][error]   To install the gem, issue the following command:
  [2012/10/25 22:05:57][error]   > gem install net-ssh -v '~> 2.3.0'
  [2012/10/25 22:05:57][error]   Please try again after installing the missing dependency.
  [2012/10/25 22:05:57][error] CLIError: Errno::ENOENT: No such file or directory - /home/rails/Backup/log/backup.log

需要安装ssh的库::
  
  gem install net-ssh -v '~> 2.3.0'
  gem install net-scp -v '~> 1.0.4'

在另一台服务器上创建相关的目录，再执行备份命令::
  
  rails@group1:~/Backup/models$ backup perform --trigger jwch
  [2012/10/25 22:17:17][warning] CleanerError: Cleanup Warning
  [2012/10/25 22:17:17][warning]   The temporary backup folder '/home/rails/Backup/.tmp'
  [2012/10/25 22:17:17][warning]   appears to contain the package files from the previous backup!
  [2012/10/25 22:17:17][warning]   /home/rails/Backup/.tmp/2012.10.25.22.15.47.jwch.tar
  [2012/10/25 22:17:17][warning]   These files will now be removed.
  [2012/10/25 22:17:17][warning]   
  [2012/10/25 22:17:17][warning]   Please check the log for messages and/or your notifications
  [2012/10/25 22:17:17][warning]   concerning this backup: 'jwch.edu.cn (jwch)'
  [2012/10/25 22:17:17][warning]   The temporary files which had to be removed should not have existed.
  [2012/10/25 22:17:17][message] Performing Backup for 'jwch.edu.cn (jwch)'!
  [2012/10/25 22:17:17][message] [ backup 3.0.25 : ruby 1.9.3p286 (2012-10-12 revision 37165) [i686-linux] ]
  [2012/10/25 22:17:17][message] Database::MySQL started dumping and archiving 'jwch_pro'.
  [2012/10/25 22:17:18][message] Database::MySQL Complete!
  [2012/10/25 22:17:18][message] Packaging the backup files...
  [2012/10/25 22:17:18][message] Splitter configured with a chunk size of 250MB.
  [2012/10/25 22:17:18][message] Packaging Complete!
  [2012/10/25 22:17:18][message] Cleaning up the temporary files...
  [2012/10/25 22:17:21][message] Storage::SCP started transferring '2012.10.25.22.17.17.jwch.tar' to '192.168.3.45'.
  [2012/10/25 22:17:23][message] Storage::SCP: Cycling Started...
  [2012/10/25 22:17:23][message] Storage::SCP: Cycling Complete!
  [2012/10/25 22:17:23][message] Cleaning up the package files...
  [2012/10/25 22:17:23][warning] Backup for 'jwch.edu.cn (jwch)' Completed Successfully (with Warnings) in 00:00:06

现在要对用户上传的文件备份
  
生成脚本::
  
  rails@group1:~/Backup/models$ backup generate:model --trigger uploads --storages="sftp" --compressors=bzip2 --notifiers=mail --archives

使用bzip2 压缩， 加上邮件提醒, 然后进行配置， 

安装邮件相关的gem,用于发邮件 ::
  
  gem install mail -v '~> 2.4.0'


执行::

  rails@group1:~/Backup/models$ backup perform -t uploads
  [2012/10/25 23:04:39][message] Performing Backup for 'uploads file (uploads)'!
  [2012/10/25 23:04:39][message] [ backup 3.0.25 : ruby 1.9.3p286 (2012-10-12 revision 37165) [i686-linux] ]
  [2012/10/25 23:04:39][message] Backup::Archive has started archiving:
  [2012/10/25 23:04:39][message]   /home/rails/jwch-web/shared/uploads
  [2012/10/25 23:04:39][message] Using Compressor::Bzip2 for compression.
  [2012/10/25 23:04:39][message]   Command: '/bin/bzip2'
  [2012/10/25 23:04:39][message]   Ext: '.bz2'
  [2012/10/25 23:04:39][message] Backup::Archive Complete!
  [2012/10/25 23:04:39][message] Packaging the backup files...
  [2012/10/25 23:04:39][message] Splitter configured with a chunk size of 250MB.
  [2012/10/25 23:04:39][message] Packaging Complete!
  [2012/10/25 23:04:39][message] Cleaning up the temporary files...
  [2012/10/25 23:04:43][message] Storage::SCP started transferring '2012.10.25.23.04.39.uploads.tar' to '192.168.3.45'.
  [2012/10/25 23:04:43][message] Storage::SCP: Cycling Started...
  [2012/10/25 23:04:43][message] Storage::SCP: Cycling Complete!
  [2012/10/25 23:04:43][message] Cleaning up the package files...
  [2012/10/25 23:04:43][message] Notifier::Mail started notifying about the process.
  [2012/10/25 23:04:44][message] Backup for 'uploads file (uploads)' Completed Successfully in 00:00:05


添加whenever ::

  gem install whenever

配置文件::
  
  cd
  mkdir config
  wheneverize
  
  输出
  [add] writing `./config/schedule.rb'
  [done] wheneverized!

配置config/schedule.rb ::
  
  every 4.days, :at => '23:00' do
     command "backup perform -t jwch"
  end

  every 7.days, :at => '23:00' do
     command "backup perform -t uploads"
  end

执行::
  
  whenever
           
  输出
  rails@group1:~$ whenever
  0 23 1,5,9,13,17,21,25,29 * * /bin/bash -l -c 'backup perform -t jwch'

  0 23 1,8,15,22 * * /bin/bash -l -c 'backup perform -t uploads'

更新到crontab::

  rails@group1:~$ whenever --update-crontab
  
最后，把脚本放到bitbucket 上::
  
  https://bitbucket.org/sdutlinux/jwch-web-crontab-script
  https://bitbucket.org/sdutlinux/jwch-web-backup-script


