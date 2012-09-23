====================
Golang
====================

------------------------
golang web 开发
------------------------

^^^^^^^^^^^^^^^^^^
revel 框架
^^^^^^^^^^^^^^^^^^

主页: `http://robfig.github.com/revel/ <http://robfig.github.com/revel/>`_
github: `https://github.com/robfig/revel <https://github.com/robfig/revel>`_

revel 是Golang的mvc结构的web开发框架 

基于mvc模式

* model 
* views
* controller 

.. Goroutine per Request
.. 使用go-routine处理请求

.. Revel builds on top of the Go HTTP server, which creates a go-routine (lightweight thread) to process each incoming request. The implication is that your code is free to block, but it must handle concurrent request processing.

.. Revel 构建在Go HTTP server之上，Go http server 会使用go-routine(golang的协程) 处理每次请求，


