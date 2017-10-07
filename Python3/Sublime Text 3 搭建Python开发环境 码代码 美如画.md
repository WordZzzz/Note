# Sublime Text 3 搭建Python开发环境 码代码 美如画

[toc]

## 前言

&emsp;&emsp;Sublime Text：一款具有代码高亮、语法提示、自动完成且反应快速的编辑器软件，不仅具有华丽的界面，还支持插件扩展机制，用她来写代码，绝对是一种享受。相比于难于上手的 Vim ，浮肿沉重的 Eclipse ， VS ，即便体积轻巧迅速启动的 Editplus 、 Notepad++ ，在 Sublime Text 面前也略显失色，无疑这款性感无比的编辑器是 Coding 和 Writing 最佳的选择，没有之一。

&emsp;&emsp;Sublime Text 3 的功能实在是太强大了，搭配各种 package ，码代码、美如画。对于 Sublime Text 3 的介绍网上一大堆，博主就不再这里赘述了。本篇博文主要是记录一下博主如何在 Sublime Text 3 下优雅的编写、编译、运行 python 代码。

## 安装

&emsp;&emsp;WordZzzz使用的版本是 Sublime Text Build 3143 ，大家自行下载后直接安装即可，安装完之后需要 License 来激活我们的软件。

&emsp;&emsp;直接将下面的 License 复制过去就好，亲测可用：

```
—– BEGIN LICENSE —– 
TwitterInc 
200 User License 
EA7E-890007 
1D77F72E 390CDD93 4DCBA022 FAF60790 
61AA12C0 A37081C5 D0316412 4584D136 
94D7F7D4 95BC8C1C 527DA828 560BB037 
D1EDDD8C AE7B379F 50C9D69D B35179EF 
2FE898C4 8E4277A8 555CE714 E1FB0E43 
D5D52613 C3D12E98 BC49967F 7652EED2 
9D2D2E61 67610860 6D338B72 5CF95C69 
E36B85CC 84991F19 7575D828 470A92AB 
—— END LICENSE ——
```

## 配置

### Package Control

&emsp;&emsp;按 Ctrl+` 调出 console ，粘贴以下代码到底部命令行并回车：

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

&emsp;&emsp;重启 Sublime Text 3。如果在 Perferences->package settings 中看到 package control 这一项，则安装成功。按下 Ctrl+Shift+P 调出命令面板输入 install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。

&emsp;&emsp;下面介绍几个比较实用的 package 。

### SideBarEnhancements

&emsp;&emsp;SideBarEnhancements 扩展了侧边栏中菜单选项的数量，从而提升你的工作效率。诸如 “New file” 和 “Duplicate” 这样的选项对于 ST3 来说实在是太重要了，而且仅凭 “Delete” 这一个功能就让这个插件值得下载。这个功能将你会在你删除文件的时候把它放入回收站。虽然这个功能乍一看没什么用，但是当你没有使用这样的功能而彻底删除了一个文件的时候，除非你用了版本管理软件，否则你将很难恢复这个文件。

### Anaconda

&emsp;&emsp;Anaconda 是一个终极 Python 插件。它为 ST3 增添了多项 IDE 类似的功能，例如：

- Autocompletion 自动完成，该选项默认开启，同时提供多种配置选项。
- Code linting 使用支持 pep8 标准的 PyLint 或者 PyFlakes。
- McCabe code complexity checker 让你可以在特定的文件中使用 McCabe complexity checker. 
- Goto Definitions 能够在你的整个工程中查找并且显示任意一个变量，函数，或者类的定义。
- Find Usage 能够快速的查找某个变量，函数或者类在某个特定文件中的什么地方被使用了。
- Show Documentation： 能够显示一个函数或者类的说明性字符串(当然，是在定义了字符串的情况下)

&emsp;&emsp;但是，刚安装完之后，打开一个 python 文档，所有代码都会被白色细线框中，如图所示；

&emsp;&emsp;强迫症的我看着好难受，决心要搞一搞这东西。后来发现在 Sublime > Preferences > Package Settings > Anaconda > Settings – Default 下修改 linting behaviour 选项即可，我这里改成了只有在保存的时候linting工作。

```
    /*
        Sets the linting behaviour for anaconda:

        "always" - Linting works always even while you are writing (in the background)
        "load-save" - Linting works in file load and save only
        "save-only" - Linting works in file save only
    */
    "anaconda_linting_behaviour": "save-only",
```

### SublimeREPL

&emsp;&emsp;这可能是对程序员来说最有用的插件。SublimeREPL 允许你在 Sublime Text 中运行各种语言（NodeJS ，Python，Ruby， Scala 和 Haskell 等等）。

&emsp;&emsp;在 Sublime > Tools > SublimeREPL 下我们可以看到 SublimeREPL 支持运行的所有语言。

&emsp;&emsp;下面的代码是在 AppData\Roaming\Sublime Text 3\Packages\SublimeREPL\config\Python 下的 Default.sublime-commands 文件，从中我们可以看到 SublimeREPL 所支持的 python 的各种运行方式。

```
[
    {
        "caption": "SublimeREPL: Python",
        "command": "run_existing_window_command", "args":
        {
            "id": "repl_python",
            "file": "config/Python/Main.sublime-menu"
        }
    },
    {
        "caption": "SublimeREPL: Python - PDB current file",
        "command": "run_existing_window_command", "args":
        {
            "id": "repl_python_pdb",
            "file": "config/Python/Main.sublime-menu"
        }
    },
    {
        "caption": "SublimeREPL: Python - RUN current file",
        "command": "run_existing_window_command", "args":
        {
            "id": "repl_python_run",
            "file": "config/Python/Main.sublime-menu"
        }
    },
    {
        "command": "python_virtualenv_repl",
        "caption": "SublimeREPL: Python - virtualenv"
    },
    {
        "caption": "SublimeREPL: Python - IPython",
        "command": "run_existing_window_command", "args":
        {
            "id": "repl_python_ipython",
            "file": "config/Python/Main.sublime-menu"
        }
    }
]
```

&emsp;&emsp;接下来配置快捷键，打开 Sublime > Preferences > Key Building ，在右侧栏（ User 部分）添加下面的代码。下面的代码用 F5 来执行当前 Python 脚本，用 F4 来实现切换至 IPython 命令行窗口。

```
[
	{"keys":["f5"],
	"caption": "SublimeREPL: Python - RUN current file",
	"command": "run_existing_window_command", "args":
	{"id": "repl_python_run",
	"file": "config/Python/Main.sublime-menu"}}
	,
	{"keys":["f4"],
	"caption": "SublimeREPL: Python - IPython",
	"command": "run_existing_window_command", "args":
	{"id": "repl_python_ipython",
	"file": "config/Python/Main.sublime-menu"}}
]
```

&emsp;&emsp;当然，如果你电脑里面安装了两个版本的 Python ，而你想指定使用某个版本，则需要修改下面的代码。下面的代码是在 AppData\Roaming\Sublime Text 3\Packages\SublimeREPL\config\Python 下的 Main.sublime-menu 文件，主要修改 "cmd" 后面跟着的 python 命令。比如我电脑里 python2.7 的执行程序命名是 python.exe ，而 python3.6 的执行程序命名为 python3.exe ，我想要使用 python3 ，所以把所有 "cmd" 后面跟着的命令都改为 "python3" 。

```
[
     {
        "id": "tools",
        "children":
        [{
            "caption": "SublimeREPL",
            "mnemonic": "R",
            "id": "SublimeREPL",
            "children":
            [
                {"caption": "Python",
                "id": "Python",

                 "children":[
                    {"command": "repl_open",
                     "caption": "Python",
                     "id": "repl_python",
                     "mnemonic": "P",
                     "args": {
                        "type": "subprocess",
                        "encoding": "utf8",
                        "cmd": ["python3", "-i", "-u"],
                        "cwd": "$file_path",
                        "syntax": "Packages/Python/Python.tmLanguage",
                        "external_id": "python",
                        "extend_env": {"PYTHONIOENCODING": "utf-8"}
                        }
                    },
                    {"command": "python_virtualenv_repl",
                     "id": "python_virtualenv_repl",
                     "caption": "Python - virtualenv"},
                    {"command": "repl_open",
                     "caption": "Python - PDB current file",
                     "id": "repl_python_pdb",
                     "mnemonic": "D",
                     "args": {
                        "type": "subprocess",
                        "encoding": "utf8",
                        "cmd": ["python3", "-i", "-u", "-m", "pdb", "$file_basename"],
                        "cwd": "$file_path",
                        "syntax": "Packages/Python/Python.tmLanguage",
                        "external_id": "python",
                        "extend_env": {"PYTHONIOENCODING": "utf-8"}
                        }
                    },
                    {"command": "repl_open",
                     "caption": "Python - RUN current file",
                     "id": "repl_python_run",
                     "mnemonic": "R",
                     "args": {
                        "type": "subprocess",
                        "encoding": "utf8",
                        "cmd": ["python3", "-u", "$file_basename"],
                        "cwd": "$file_path",
                        "syntax": "Packages/Python/Python.tmLanguage",
                        "external_id": "python",
                        "extend_env": {"PYTHONIOENCODING": "utf-8"}
                        }
                    },
                    {"command": "repl_open",
                     "caption": "Python - IPython",
                     "id": "repl_python_ipython",
                     "mnemonic": "I",
                     "args": {
                        "type": "subprocess",
                        "encoding": "utf8",
                        "autocomplete_server": true,
                        "cmd": {
                            "osx": ["python3", "-u", "${packages}/SublimeREPL/config/Python/ipy_repl.py"],
                            "linux": ["python3", "-u", "${packages}/SublimeREPL/config/Python/ipy_repl.py"],
                            "windows": ["python3", "-u", "${packages}/SublimeREPL/config/Python/ipy_repl.py"]
                        },
                        "cwd": "$file_path",
                        "syntax": "Packages/Python/Python.tmLanguage",
                        "external_id": "python",
                        "extend_env": {
                            "PYTHONIOENCODING": "utf-8",
                            "SUBLIMEREPL_EDITOR": "$editor"
                        }
                    }
                    }
                ]}
            ]
        }]
    }
]

```

&emsp;&emsp;别忘了， Sublime Text 3 也有自己的 build 功能，即也支持 python 等语言的代码构建（ ctrl + b ）。同样的，我们如何添加不同的 python 版本到我们的构建系统呢？很简单，Sublime > Tools > Build System > New Build System，分别添加如下代码之后，再分别保存为 python2.sublime-build 和 python3.sublime-build ，这样，当我们再次打开 Sublime > Tools > Build System 之后，就会发现我们新添加的 python2 和 python3 构建系统了。

```
{
	"cmd": ["D:/Program Files/Python/Python27/python.exe", "-u", "$file"],
	"file_regex": "^[ ]*File \"(...*?)\", line([0-9]*)",
	"selector": "source.python"
}
```

```
{
	"cmd": ["D:/Program Files/Python/Python36/python3.exe", "-u", "$file"],
	"file_regex": "^[ ]*File \"(...*?)\", line([0-9]*)",
	"selector": "source.python"
}
```

### SublimeTmpl

&emsp;&emsp;快速生成文件模板

&emsp;&emsp;，SublimeTmpl能新建html、css、javascript、php、python、ruby六种类型的文件模板，所有的文件模板都在插件目录的templates文件夹里，可以自定义编辑文件模板。

&emsp;&emsp;SublimeTmpl默认的快捷键:

```
ctrl+alt+h html
ctrl+alt+j javascript
ctrl+alt+c css
ctrl+alt+p php
ctrl+alt+r ruby
ctrl+alt+shift+p python
```

&emsp;&emsp;这里我想修改一下python模板，所以就需要进行如下操作：Sublime > Preferences > Package Settings > SublimeTmpl > Settings – User 添加如下代码。然后 ctrl+alt+shift+p 来新建一个模板试试看。

```
{  
    "disable_keymap_actions": false, // "all"; "html,css"  
    "date_format" : "%Y-%m-%d %H:%M:%S",  
    "attr": {  
        "author": "WordZzzz",  
        "email": "wordzzzz@foxmail.com",  
        "link": "http://blog.csdn.net/u011475210"  
    }  
} 
```

&emsp;&emsp;快捷键也是可以更改的，全部在 Sublime > Preferences > Package Settings > SublimeTmpl 的设置中。

&emsp;&emsp;如果想要新建其他类型的文件模板的话，先自定义文件模板方在templates文件夹里，再分别打开Default (Windows).sublime-keymap、Default.sublime-commands、Main.sublime-menu、SublimeTmpl.sublime-settings这四个文件照着里面的格式自定义想要新建的类型，这里就不详细介绍了，请各位自己折腾哈~


## 快捷键

- 跳转到任意内容 (“cmd+p”) 用来快速查找和打开文件。你仅仅只需要工程中文件的一部分路径或者文件名你就可以很容易的打开这个文件。这在一个大型的 Django 工程中显得非常方便。
- 跳转到指定行 (“ctrl+g”) 让你在当前文件中跳转到指定行数。
- 跳转到标志 (“cmd+r”) 可以列出当前文件中所有的函数或者类，让你更方便查找。你可以通过输入关键字来查找你所需要的函数或者类。
- 跳转到行首 (cmd+left-arrow-key) 与 跳转到行尾 (cmd+right-arrow-key)
- 删除当前行(ctrl+shift+k)
- 多重编辑 是我迄今为止最喜欢的快捷键
选定一个单词，点击 “cmd+d”来选择同样的单词，再次点击 “cmd+d”*继续选择下一个单词…
或者 “cmd+单击”来指定多个你想要同时修改的地方。
- 块编辑 (option+left-mouse-click) 用于选择一整块的内容。通常在整理 CSV 文件的时候用于删除空白内容。

## 自定义命令

&emsp;&emsp;你可以很容易地使用 Python 来编辑你自己的自定义命令和快捷键组合。例如：

- 拷贝当前文件路径到剪贴板 – 链接
- 关闭除当前活动标签页以外的所有其他标签页 – 链接

&emsp;&emsp;通过文件选项打开你的 Package 文件夹(Sublime > Preferences > Browse Packages)，然后打开 User 文件夹，接下来将上述的 Python 文件添加到 “/Sublime Text 3/Packages/User” 文件夹中。

&emsp;&emsp;最后请在 Key Bindings – User file (Sublime Text > Preferences > Package Settings > AdvancedNewFile > Key Bindings – User) 文件中完成快捷键绑定。

```
[
  // Copy file name
  {
    "keys": ["cmd+shift+c"],
     "command": "copy_path_to_clipboard"
  },
  // Close all other tabs
  {  
    "keys": ["cmd+alt+w"],
    "command": "close_tabs"
  }
]
```

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**