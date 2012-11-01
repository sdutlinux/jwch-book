教学资源网迁移记录
=====================

项目主页: https://github.com/jwch/teaching

原因:

#. 由原先asp程序，迁移到ruby程序。这样就可以不用2003, sql server,减少维护成本，新同学的学习成本
#. 框架打算用ruby的 padrina,这个框架是基于sinatra 框架的包装，添加常用的工具。轻量级的

padrina:

1. http://www.padrinorb.com/
2. https://github.com/padrino/padrino-framework


----------------------
笔记
-----------------------

命令行工具－Project Generator

项目生成::
  
  $ padrino g project <the_app_name> </path/to/create/app> --<component-name> <value>

详细: http://www.padrinorb.com/guides/generators 





-------------------------
开发记录
-------------------------
创建项目::
    
  padrino g project teaching  -t rspec -d activerecord 

generators controller example::
  
  padrino g controller welcome get:index 

