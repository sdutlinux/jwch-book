sublime
===================================================================

:作者: 李大双 ldshuang@gmail.com
		 刘哲 liuzhe0223@gmail.com

.. image::  /_static/sublime.png

中文文档 http://docs.sublimetext.tw/

vim 模式
----------------

在 Preferences：Settings - User 设置 ::

    {
        "ignored_packages": []
    }


绑定jj 键为ESC

在 Preferences：Settings - User 设置 ::

    [
        { "keys": ["j","j"], "command": "exit_insert_mode",
            "context":
            [
                { "key": "setting.command_mode", "operand": false },
                { "key": "setting.is_widget", "operand": false }
            ]
        },

        { "keys": ["j","j"], "command": "hide_auto_complete", "context":
            [
                { "key": "auto_complete_visible", "operator": "equal", "operand": true }
            ]
            },

        { "keys": ["j","j"], "command": "vi_cancel_current_action", "context":
            [
                { "key": "setting.command_mode" },
                { "key": "selection_empty", "operator": "equal", "operand": true, "match_all": false },
                { "key": "vi_has_input_state" }
            ]
        }
    ]

安装插件
-------------

按ctrl+`调出console,将下面代码粘贴至底部命令行中回车 ::

	import urllib2,os;
	pf='Package Control.sublime-package';
	ipp=sublime.installed_packages_path();
	os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())

重启sublime，perferences->package setting中出现 package control 则证明安装成功

安装成功后可用 ctrl+shirft+p 调出命令面板,输入install ，在感知的下拉框中可以选择 install package， 然后就可以安装插件
