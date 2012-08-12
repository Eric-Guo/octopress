---
layout: post
title: "starting-to-use-a-modern-python-scripted-editor-sublime-text-2"
date: 2012-01-28 16:12
comments: true
external-url:
categories: [Sublime, Python]
---
I used the <a href="http://www.vim.org/" target="_blank">Vim</a> and <a href="http://www.ultraedit.com/" target="_blank">UltraEdit</a> quite a long time, but I'm always dissatisfy with the vim so unique key movement and larger and larger size of UltraEdit, recently I decided to switch to the <a href="http://www.sublimetext.com/" target="_blank">Sublime</a>, Sublime can be programmed with my most prefer language Python. Sublime also provide a same experience on Windows/OSX/Linux but only need to pay 1 license. So learning Sublime from the scratch is for a long time profitable and save money.<!--more--> After install the Sublime, I found the <a href="http://wbond.net/sublime_packages/package_control/installation" target="_blank">Sublime Package Control</a> is also helpful, which can install the sublime plug-in from web directly; you can install the <a href="http://wbond.net/sublime_packages/alignment" target="_blank">Alignment</a> as a try after install package control and there are <a href="http://wbond.net/sublime_packages/community" target="_blank">hundreds of packages</a> already available.  Currently I'm using below User Settings (with <a href="http://www.sublimetext.com/docs/2/vintage.html" target="_blank">Global Settings Vintage Mode enable</a>):

```python Preferences.sublime-settings
// Eric Guo's User Preferences Settings
{
	"close_windows_when_empty": true,
	"trim_trailing_white_space_on_save": true,
	"vintage_start_in_command_mode": true
}
```

```python Distraction Free.sublime-settings
// Eric Guo's Sublime 2 User Distraction Free Settings
{
	"rulers":[],
	"word_wrap": false
}
```

```python Default (Windows).sublime-keymap
// Eric Guo's Sublime 2 User Key Bindings
[
	{ "keys": ["ctrl+0"], "command": "reset_font_size" },
	{ "keys": ["alt+/"], "command": "auto_complete" },
	{ "keys": ["alt+/"], "command": "replace_completion_with_auto_complete", "context":
		[
			{ "key": "last_command", "operator": "equal", "operand": "insert_best_completion" },
			{ "key": "auto_complete_visible", "operator": "equal", "operand": false },
			{ "key": "setting.tab_completion", "operator": "equal", "operand": true }
		]
	},
	{ "keys": ["alt+shift+e"], "command": "open_folder_in_explorer" }
]
```
which the open_folder_in_explorer is a plugin on  %APPDATA%\Roaming\Sublime Text 2\Packages\User:

```python OpenFolderInExplorer.py
import sublime, sublime_plugin, os, subprocess

class OpenFolderInExplorerCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        if self.view.file_name():
            folder_name, file_name = os.path.split(self.view.file_name())
        command = "explorer " + folder_name
        subprocess.Popen(command)
```