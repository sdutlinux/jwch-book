sublime
===================================================================

:作者: 李大双 ldshuang@gmail.com

.. image::  /_static/sublime.png

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
