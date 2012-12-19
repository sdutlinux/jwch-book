git-flow 
========================

文章：

#. http://www.jeffkit.info/2010/12/842/ 
#. http://ihower.tw/blog/archives/5140/ 
#. http://nvie.com/posts/a-successful-git-branching-model/ 
#. git分支管理策略 http://www.ruanyifeng.com/blog/2012/07/git.html  

git-flow 是什么？
~~~~~~~~~~~~~~~~~~~

:github 主页: https://github.com/nvie/gitflow

git flow是对 http://nvie.com/posts/a-successful-git-branching-model/ 这个分支模型的命令封装。

git flow定义了下列分支 

主要分支
  1. master: 永远在 production-ready 状态
  2.develop: 最新的下次发布开发状态j
支援性分支
  1. Feature branches: 开发新功能都从 develop 分支出來，完成后 merge 回 develop
  2. Release branches: 准备要 release 的版本，只修 bugs。从 develop 分支出來，完成后 merge 回 master 和 develop
  3. Hotfix branches: 等不及 release 版本就必须马上修 master 上线的情況。会从 master 分支出來，完成后 merge 回 master 和 develop

安装git flow
~~~~~~~~~~~~~~~~~~~~~

wiki: https://github.com/nvie/gitflow/wiki/Installation  

ubuntu 下官方源里就有::

  sudo apt-get install git-flow


使用
~~~~~~~~~~~~~~~~

初始化::

     git flow init [-d] 

-d 参数表示所有按默认的来,不加-d 会手动配置

::

  λ ~/clojure/hello-world/ git flow init 
  Initialized empty Git repository in /home/lidashuang/clojure/hello-world/.git/
  No branches exist yet. Base branches must be created now.
  Branch name for production releases: [master] 
  Branch name for "next release" development: [develop] 

  How to name your supporting branch prefixes?
  Feature branches? [feature/] 
  Release branches? [release/] 
  Hotfix branches? [hotfix/] 
  Support branches? [support/] 
  Version tag prefix? [] 


创建 feature/release/hotfix/support 分支
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

列出，开始，完成 feature 分支用下面的命令:: 

  git flow feature
  git flow feature start <name> [<base>]
  git flow feature finish <name>

一个 feature完成 之后 ，会合并到develop 分支

对 feature 分支 push/pull::

  git flow feature publish <name>
  git flow feature pull <remote> <name>

release branches:: 

  git flow release
  git flow release start <release> [<base>]
  git flow release finish <release>
  For release branches, the <base> arg must be a commit on develop.

hotfix:: 

  git flow hotfix
  git flow hotfix start <release> [<base>]
  git flow hotfix finish <release>
  For hotfix branches, the <base> arg must be a commit on master.

branches:: 

  git flow support
  git flow support start <release> <base>
  For support branches, the <base> arg must be a commit on master.



    


