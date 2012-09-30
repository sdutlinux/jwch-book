======================
服务器相关
======================

:编辑者: 李大双 ldshuang@gmail.com

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
