06 Bash特性
===========

6.1 调用Bash
------------

6.2 Bash启动文件
----------------

本章描述了Bash是怎样执行它的启动文件。如果任何文件存在但没有读权限，则Bash会报错。文件名中的波浪号会被扩展，如波浪号扩展章节的描述（查看 [3.5.2 波浪号扩展][3.5.2]）。

交互式shell在[交互式shell][6.3]章节有描述。

**以交互式登录shell，或带-`-login`参数调用**

当Bash是以交互式登录shell，或者带`--login`选项的非交互式shell调用时，如果`/etc/profile`文件存在，它会首先读取和执行该文件。在读取该文件后，它会以下顺序查找`~/.bash_profile`, `~/.bash_login`, `~/.profile`，并执行第一个存在且可读的文件。当shell以`--noprofile`选项启动时，该行为会被抑制。

当交互式登录shell退出，或者非交互式登录shell执行`exit`命令时, 如果`~/.bash_logout`存在，Bash会读取并执行该文件。

**以交互式non-login shell调用**

当交互式shell不是以一个登录shell启动时， 如果`~/.bashrc`文件存在， Bash会读取并执行它。该行为可以通过`--norc`选项抑制。`--rcfile`选项可以指定文件来强制替换`~/.bashrc`。

所以，你的`~/.bash_profile`文件通常包含

```
if [ -f ~/.bashrc ]; then . ~/.bashrc;fi
```

任意登录相关的初始化之后（或之前）。

**以非交互式调用**

当Bash以非交互式启动，例如，执行一个shell脚本，它在环境变量中查找BASH_ENV变量，如果变量存在则扩展它，读取并执行扩展后的文件名。Bash 的行为如同执行以下命令:

```
if [ -n "$BASH_ENV" ]; then . "$BASH_ENV"; fi
```

但是，PATH变量的值不会被用于搜索文件名。

如上所述, 如果使用`--login`选项调用非交互式shell, Bash将尝试从登录shell启动文件中读取和执行命令。

**以名称sh调用**

如果Bash是以sh的名称调用的, 那么它会尽量模仿sh的历史版本的启动行为, 同时也符合POSIX标准。

当以交互登录shell或者非交互式shell带`--login`选项调用时，它首先会尝试顺序的读取并执行`/etc/profile`和`~/.profile`。`--noprofile`选项可以用于忽略该行为。
当以sh的名称的交互式shell调用时，Bash会查找ENV变量，如果它有定义则扩展它的值。并且读取并执行值中指定的文件名。因此，以sh调用的shell不会尝试去读取执行其他任何的启动文件，`--rcfile`选项也没有效果。以sh调用的非交互式shell不会尝试读取任何其他启动文件。

当以sh名称调用时，Bash在读取启动文件后，进入POSIX模式。

**在POSIX模式中调用**

当Bash以POSIX模式启动，也就是带`--posix`选项启动，它遵循启动文件的POSIX标准。在这种模式下，交互式shell会扩展ENV变量，并且读取和执行扩展值中指定的文件。没有其他启动文件再会被读取。

**由远程 shell 守护进程调用**

Bash attempts to determine when it is being run with its standard input connected to a network connection, as when executed by the remote shell daemon, usually `rshd`, or the secure shell daemon `sshd`. If Bash determines it is being run in this fashion, it reads and executes commands from `~/.bashrc`, if that file exists and is readable. It will not do this if invoked as sh. The `--norc` option may be used to inhibit this behavior, and the `--rcfile` option may be used to force another file to be read, but neither rshd nor sshd generally invoke the shell with those options or allow them to be specified.

**用不相等的有效和真实UID/GID调用**

如果Bash有效的用户（组）ID并且它不等于真正的用户（组）ID启动，并且不提供`-p`选项时，不会读取启动文件，shell函数不从环境变量中继承，如果环境变量中出现`SHELLOPTS`,`BASHOPTS`, `CDPATH`, 和`GLOBIGNORE`变量，则会被忽略，并且该有效的用户ID会被设置为真实用户ID。如果调用时替换`-p`选项， 启动行为一样，但是有效的用户ID不会重置。

6.3 交互shell
-------------

6.4 Bash条件表达式
------------------

6.5 Shell算术运算表达式
-----------------------

6.6 Aliases
-----------

6.7 数组
--------

6.8 目录堆栈
------------

6.9 控制提示符
--------------

6.10 严格模式的shell
--------------------

6.11 Bash POSIX模式
-------------------

[3.5.2]: /zh_cn/03_shell基本特性.md#352-波浪号扩展
[6.3]: /zh_cn/06_Bash特性.md#63-交互shell