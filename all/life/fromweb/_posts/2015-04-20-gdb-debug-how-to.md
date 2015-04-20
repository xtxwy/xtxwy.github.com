---
published: true
layout: default
---

## 说明

我只记录自己感兴趣的一些gdb调试知识。比较好且详细的几篇文章如下：

1. [陈皓的“用GDB调试程序”7篇文章](http://blog.csdn.net/haoel/article/category/9197)
2. [ubuntu上的“用GDB调试程序”(对陈皓文章的归一和总结)](http://wiki.ubuntu.org.cn/%E7%94%A8GDB%E8%B0%83%E8%AF%95%E7%A8%8B%E5%BA%8F)
3. [陈皓的“GDB中应该知道的几个调试方法”](http://coolshell.cn/articles/3643.html)

## 常用的GDB调试命令

### 启动程序

    gdb <program>
    gdb <program> core
    gdb <program> <pid>
    gdb <program> -symbols <file> OR -s <file>
    gdb <program> -se file
    gdb <program> -core <file> OR -c <file>
    gdb <program> -directory <dir> OR -d <dir>

core是程序非法执行后core dump后产生的文件，而pid就是attach方式调试。-s表示从指定文件中读取符号表，-se表示从指定文件中读取符号表信息，并把他用在可执行文件中。-core表示core dump的core文件，-d表示加入一个源文件的搜索路径。

### 常用调试命令 

#### 设置运行参数

    (gdb) set args <参数列表>
    (gdb) show args

#### 下断点

    (gdb) b main //在main函数入口下断点
    (gdb) b test.c:112 //在test.c第112行代码处下断点
    (gdb) b make_<按TAB键> //查看所有以make开头的函数
    (gdb) b <参数> if <condition> //比如:b main if argc==2
    (gdb) b +offset OR b -offset //当前行前面或后面偏移offset处
    (gdb) b *<address> //内存地址address
    (gdb) watch <expr> //当expr发生变化，断住
    (gdb) rwatch <expr> OR awatch <expr> //expr被读 OR 读或写

#### 自动显示变量值

    (gdb) display <expr>
    (gdb) display/<format> <addr>  
    
addr表示内存地址，format指定格式。有：x表示16进制，c字符，d十进制等。当程序停住时，或是在你单步跟踪时，这些变量会自动显示。

#### 查看信息

    (gdb) list <行号> //显示指定行周围的源代码
    (gdb) list <function>
    (gdb) list - OR list + //显示当前行前面或后面的源码
    (gdb) list <开始行> <结束行>
    (gdb) info <行号 OR 函数名 OR 文件名:行号 OR 文件名:函数名> //打印内存地址
    (gdb) info <registers>
    (gdb) disassemble func
    (gdb) print <expr> OR p <expr>
    (gdb) bt [n] //查看栈帧，栈顶往上n层
    (gdb) macro <宏> //查看宏展开的样子
    (gdb) info macro 

要启动查看宏功能，需要gcc编译时，加上-ggdb3参数

#### 运行一系列命令

    (gdb) break foo
    commands
    >print x
    >print y
    >end
    (gdb) define <name>
    >print x
    >print y
    >end

为foo断点设置运行的命令列表。define是设置name表示一组命令。

#### 改变程序的执行

    (gdb) print x=4  //设置x=4
    (gdb) set x=4
    (gdb) jump <行号>
    (gdb) return //不执行函数直接返回

#### 设置捕捉点

    (gdb) catch <event>

event有：throw, catch, exec, fork, vfork, load, unload

#### gdb中重编译程序

    (gdb) shell <command string>
    (gdb) make <args> //等价于:shell make <args>

#### 多线程调试

参考文章：[GDB中应该知道的几个调试方法](http://coolshell.cn/articles/3643.html)
