=================
搭建ftp 服务器
=================


vsftp 快速配置
--------------------

支持系统用户的快速配置::


    # 默认配置文件: /etc/vsftpd.conf
    #
    #
    #     下面是配置的选项及说明
    #
    #

    ######### 核心设置 ###########

    # 允许本地用户登录
    local_enable=YES

    # 本地用户的写权限
    write_enable=YES

    # 使用FTP的本地文件权限,默认为077
    # 一般设置为022
    local_umask=022

    # 切换目录时
    # 是否显示目录下.message的内容
    dirmessage_enable=YES

    #验证方式
    #pam_service_name=vsftpd

    # 启用FTP数据端口的数据连接
    connect_from_port_20=YES

    # 以独立的FTP服务运行
    listen=yes

    # 修改连接端口
    #listen_port=2121

    ######### 匿名登录设置 ###########

    # 允许匿名登录
    anonymous_enable=NO

    # 如果允许匿名登录
    # 是否开启匿名上传权限
    #anon_upload_enable=YES

    # 如果允许匿名登录
    # 是否允许匿名建立文件夹并在文件夹内上传文件
    #anon_mkdir_write_enable=YES

    # 如果允许匿名登录
    # 匿名帐号可以有删除的权限
    #anon_other_write_enable=yes

    # 如果允许匿名登录
    # 匿名的下载权限
    # 匿名为Other,可设置目录/文件属性控制
    #anon_world_readable_only=no

    # 如果允许匿名登录
    # 限制匿名用户传输速率,单位bite
    #anon_max_rate=30000

    ######### 用户限制设置 ###########

    #### 限制登录

    # 用userlist来限制用户访问
    #userlist_enable=yes

    # 名单中的人不允许访问
    #userlist_deny=no

    # 限制名单文件放置的路径
    #userlist_file=/etc/vsftpd/userlist_deny.chroot

    #### 限制目录

    # 限制所有用户都在家目录
    chroot_local_user=yes

    # 调用限制在家目录的用户名单
    #chroot_list_enable=YES

    # 限制在家目录的用户名单所在路径
    #chroot_list_file=/etc/vsftpd/chroot_list

    ######### 日志设置 ###########

    # 日志文件路径设置
    xferlog_file=/var/log/vsftpd.log

    # 激活上传/下载的日志
    xferlog_enable=YES

    # 使用标准的日志格式
    #xferlog_std_format=YES

    ######### 安全设置 ###########

    # 用户空闲超时,单位秒
    #idle_session_timeout=600

    # 数据连接空闲超时,单位秒
    #data_connection_timeout=120

    # 将客户端空闲1分钟后断开
    #accept_timeout=60

    # 中断1分钟后重新连接
    #connect_timeout=60

    # 本地用户传输速率,单位bite
    #local_max_rate=50000

    # FTP的最大连接数
    #max_clients=200

    # 每IP的最大连接数
    #max_per_ip=5

    ######### 被动模式设置 ###########

    # 是否开户被动模式
    pasv_enable=yes

    # 被动模式最小端口
    pasv_min_port=5000

    # 被动模式最大端口
    pasv_max_port=6000

    ######### 其他设置 ###########

    # 欢迎信息
    ftpd_banner=Welcome to Ftp Server!
