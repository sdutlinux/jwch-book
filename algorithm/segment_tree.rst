===============
线段树
===============


资料: 
--------

#. `数据结构之线段树 <http://dongxicheng.org/structure/segment-tree/>`_
#. `线段树 <http://wenku.baidu.com/view/32652a2d7375a417866f8f51.html>`_
#. https://speakerdeck.com/lidashuang/xian-duan-shu
#. go实现 https://github.com/toberndo/go-stree


定义
-------

首先,线段树是一棵“树”,而且是一棵完全二叉树。同时,“线段”两字反映出线段树的 另一个特点:每个节点表示的是一个“线段”,或者说是一个区间。事实上,一棵线段树的根 节点表示的是“整体”的区间,而它的左右子树也是一棵线段树,分别表示的是这个区间的左 半边和右半边。