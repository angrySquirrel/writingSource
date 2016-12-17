## python sublime 调试环境搭建
首先，如果在ubuntu的sublime环境下无法输入拼音，那么可以安装插件sublime-text-imfix
https://github.com/lyfeyaj/sublime-text-imfix

### 安装SublimeREPL
使用package control, SublimeREPL是运行在sublime中的解释器,REPL是read eval print loop的缩写，拥有对python语言的支持。
### split layout
可以使用快捷键shift+alt+8切成上下两个view
### 打开pdb
选中下面一个view，可以打开pdb，pdb是python debugger的缩写

### pdb命令
- h(elp)命令：会打印当前版本 Pdb可用的命令，如果要查询某个命令，可输入 h [command] ，例如 h l 查看 list命令
- l(ist)命令：可以列出当前将要运行的代码块
- b(break)：设置断点
比如: b 12 就是在当前脚本的第12行加上断点,  b sub 就是在当前脚本的 sub函数定义处加断点
- c(ont(inue))，让程序正常运行，直到遇到下一个断点
- n(ext)，让程序运行下一行，如果当前语句有一个函数调用，用n是不会进入被调用的函数体中的
- j(ump)，让程序跳转到指定的行数,但是中间的不会执行
- a(rgs)，打印当前函数的参数。比如下图就是展示断点进入到 testFun.add内部之后，打印 testFun.add的参数
- p，打印某个变量
- q，直接退出调试；或者使用 Ctrl+D的方式退出
