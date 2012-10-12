====================
Ruby 语言
====================

:编辑者: 李大双 ldshuang@gmail.com

开发环境
-------------------

#. 操作系统，ubuntu linux 或者 mac os 
#. `如何快速正确的安装 Ruby, Rails 运行环境 <http://ruby-china.org/wiki/install_ruby_guide>`_ 
#. 编编器 vim, emacs, sublime 

教程资源
------------------

#. `Learn Ruby The Hard Way <http://lrthw.github.com/>`_
#. `Ruby Quicktips <http://rubyquicktips.com/>`_

Rails 
----------

Nginx+puma 部署rails应用
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Nginx 配置文件

.. code-block:: bash 
    :linenos:

    upstream cosm {
      # 与启动命令指定的相同
      server unix:///tmp/cosm.sock;
    } 
    
    server {
      listen 80;
      server_name cosm.dashuang.name;
    
      # ~2 seconds is often enough for most folks to parse HTML/CSS and
      # retrieve needed images/icons/frames, connections are cheap in
      # nginx so increasing this is generally safe...
      keepalive_timeout 5;
    
      # Rails 应用路径
      root /home/group/cosm/current/public;
      #access_log /jwch/log/nginx.access.log;
      #error_log /jwch/log/nginx.error.log info;
    
      # this rewrites all the requests to the maintenance.html
      # page if it exists in the doc root. This is for capistrano's
      # disable web task
      if (-f $document_root/maintenance.html) {
        rewrite  ^(.*)$  /maintenance.html last;
        break;
      }
    
      location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    
        # If the file exists as a static file serve it directly without
        # running all the other rewite tests on it
        if (-f $request_filename) {
          break;
        }
    
        # check for index.html for directory index
        # if its there on the filesystem then rewite
        # the url to add /index.html to the end of it
        # and then break to send it to the next config rules.
        if (-f $request_filename/index.html) {
          rewrite (.*) $1/index.html break;
        }
    
        # this is the meat of the rack page caching config
        # it adds .html to the end of the url and then checks
        # the filesystem for that file. If it exists, then we
        # rewite the url to have explicit .html on the end
        # and then send it on its way to the next config rule.
        # if there is no file on the fs then it sets all the
        # necessary headers and proxies to our upstream mongrels
        if (-f $request_filename.html) {
          rewrite (.*) $1.html break;
        }
    
        if (!-f $request_filename) {
        # 这里和上边的 upstream 相同
          proxy_pass http://cosm;
          break;
        }
      }
    
      # Now this supposedly should work as it gets the filenames with querystrings that Rails provides.
      # BUT there's a chance it could break the ajax calls.
      location ~* \.(ico|css|gif|jpe?g|png)(\?[0-9]+)?$ {
         expires max;
         break;
      }
    
      location ~ ^/javascripts/.*\.js(\?[0-9]+)?$ {
         expires max;
         break;
      }
    
      # Error pages
      # error_page 500 502 503 504 /500.html;
      location = /500.html {
        root /home/group/cosm/current/public;
      }
    }


2. 启动puma

.. code-block:: bash 
    :linenos: 

    puma  -e production -b unix:///tmp/cosm.sock --pidfile #{rails_current_path}/tmp/pids/puma.pid > /dev/null 2>&1

3. capistrano 配置

.. code-block:: ruby 
    :linenos: 

    require 'bundler/capistrano'                
    require 'rvm/capistrano'
    
    default_run_options[:pty] = true  # Must be set for the password prompt
    
    set :rvm_ruby_string, 'ruby-1.9.3-p194'
    set :rvm_type, :system 
    set :use_sudo, false
    set :user, "group"  # ssh user 
    set :application, "cosm"
    
    set :port, 80
    set :rails_env, "production"
    set :deploy_to, "/home/group/#{application}"
    
    set :repository,  "git@github.com:lidashuang/cosm.git"
    set :deploy_via, :remote_cache
    set :scm_username, 'lidashuang'
    set :scm, :git
    set :branch, "master"
    set :git_shallow_clone, 1
    
    ssh_options[:forward_agent] = true
    server "210.44.176.208", :app, :web, :db, :primary => true
    
    # set path
    set(:releases_path)     { File.join(deploy_to, version_dir) }
    set(:shared_path)       { File.join(deploy_to, shared_dir) }
    set(:current_path)      { File.join(deploy_to, current_dir) }
    set(:release_path)      { File.join(releases_path, release_name) }
    
    # set gems
    set :bundle_without, [:development,:test]
    
    namespace :rvm do
      task :trust_rvmrc do
        run "rvm rvmrc trust #{current_release}"
      end
    end
    
    namespace :deploy do
      task :start, :roles => :app do
        run "cd #{current_path}; 
             setsid puma  -e production -b unix:///tmp/cosm.sock 
             --pidfile #{current_path}/tmp/pids/puma.pid > /dev/null 2>&1"
      end
    
      task :stop, :roles => :app do
        run "kill -QUIT `cat #{current_path}/tmp/pids/puma.pid`"
      end
    
      task :restart, :roles => :app do
        run "kill -USR2 `cat #{current_path}/tmp/pids/puma.pid`"
      end
    
      task :config_file do
        run "ln -s  #{shared_path}/database.yml #{release_path}/config/database.yml"
      end
    
      task :create_database do
        run "cd #{release_path}; bundle exec rake db:create RAILS_ENV=#{rails_env}"
      end
    end
    
    after "bundle:install", "deploy:cp_config_file", "deploy:create_database", "deploy:migrate"
    after "deploy:create_symlink", "deploy:cleanup"
    
