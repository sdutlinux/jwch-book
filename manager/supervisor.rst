====================
supervisor
====================

Supervisor是一个C/S系统，用来监控和控制多个服务进程，只限于UNIX-like操作系统。


官方文档
------------------

* http://supervisord.org/


安装
------------------

setuptools
^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    pip install supervisor
    # or
    easy_install supervisor

初始使用
--------------------

创建配置文件
^^^^^^^^^^^^^^^^^

需要以root身份执行
.. code-block:: bash

    echo_supervisord_conf > /etc/supervisord.conf

调整配置文件
^^^^^^^^^^^^^^^^^^^^

#. 增加web管理界面

取消配置文件中的下列行，并按需配置用户名密码

.. code-block:: cfg

    [inet_http_server]
    port=*:9001
    username=your_username           ; (default is no username (open server))
    password=your_password           ; (default is no password (open server))



添加服务
------------------

配置文件详解


.. code-block:: cfg

    [supervisorctl]
    serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
    ;serverurl=http://127.0.0.1:9001 ; use an http:// url to specify an inet socket
    ;username=chris              ; should be same as http_username if set
    ;password=123                ; should be same as http_password if set
    ;prompt=mysupervisor         ; cmd line prompt (default "supervisor")
    ;history_file=~/.sc_history  ; use readline history if available

    ; The below sample program section shows all possible program subsection values,
    ; create one or more 'real' program: sections to be able to control them under
    ; supervisor.
    ; 下小节程序用于配置
    ; 
    ; 
    ; 

    ;[program:theprogramname]
    ;command=/bin/cat              ; the program (relative uses PATH, can take args)
    ;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
    ;numprocs=1                    ; number of processes copies to start (def 1)
    ;directory=/tmp                ; directory to cwd to before exec (def no cwd)
    ;umask=022                     ; umask for process (default None)
    ;priority=999                  ; the relative start priority (default 999)
    ;autostart=true                ; start at supervisord start (default: true)
    ;autorestart=unexpected        ; whether/when to restart (default: unexpected)
    ;startsecs=1                   ; number of secs prog must stay running (def. 1)
    ;startretries=3                ; max # of serial start failures (default 3)
    ;exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
    ;stopsignal=QUIT               ; signal used to kill process (default TERM)
    ;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
    ;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
    ;killasgroup=false             ; SIGKILL the UNIX process group (def false)
    ;user=chrism                   ; setuid to this UNIX account to run the program
    ;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
    ;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
    ;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
    ;stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
    ;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
    ;stdout_events_enabled=false   ; emit events on stdout writes (default false)
    ;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
    ;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
    ;stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
    ;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
    ;stderr_events_enabled=false   ; emit events on stderr writes (default false)
    ;environment=A="1",B="2"       ; process environment additions (def no adds)
    ;serverurl=AUTO                ; override serverurl computation (childutils)

    ; The below sample eventlistener section shows all possible
    ; eventlistener subsection values, create one or more 'real'
    ; eventlistener: sections to be able to handle event notifications
    ; sent by supervisor.

    ;[eventlistener:theeventlistenername]
    ;command=/bin/eventlistener    ; the program (relative uses PATH, can take args)
    ;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
    ;numprocs=1                    ; number of processes copies to start (def 1)
    ;events=EVENT                  ; event notif. types to subscribe to (req'd)
    ;buffer_size=10                ; event buffer queue size (default 10)
    ;directory=/tmp                ; directory to cwd to before exec (def no cwd)
    ;umask=022                     ; umask for process (default None)
    ;priority=-1                   ; the relative start priority (default -1)
    ;autostart=true                ; start at supervisord start (default: true)
    ;autorestart=unexpected        ; whether/when to restart (default: unexpected)
    ;startsecs=1                   ; number of secs prog must stay running (def. 1)
    ;startretries=3                ; max # of serial start failures (default 3)
    ;exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
    ;stopsignal=QUIT               ; signal used to kill process (default TERM)
    ;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
    ;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
    ;killasgroup=false             ; SIGKILL the UNIX process group (def false)
    ;user=chrism                   ; setuid to this UNIX account to run the program
    ;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
    ;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
    ;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
    ;stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
    ;stdout_events_enabled=false   ; emit events on stdout writes (default false)
    ;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
    ;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
    ;stderr_logfile_backups        ; # of stderr logfile backups (default 10)
    ;stderr_events_enabled=false   ; emit events on stderr writes (default false)
    ;environment=A="1",B="2"       ; process environment additions
    ;serverurl=AUTO                ; override serverurl computation (childutils)

    ; The below sample group section shows all possible group values,
    ; create one or more 'real' group: sections to create "heterogeneous"
    ; process groups.

    ;[group:thegroupname]
    ;programs=progname1,progname2  ; each refers to 'x' in [program:x] definitions
    ;priority=999                  ; the relative start priority (default 999)
