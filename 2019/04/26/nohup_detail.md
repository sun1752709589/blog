# 详解Linux的nohup和&
nohup是【no hang up】的缩写，就是不挂断的意思。

nohup命令运行由Command参数和任何相关的Arg参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用nohup命令运行后台中的程序。要运行后台中的nohup命令，添加&（ 表示`and`的符号）到命令的尾部。

nohup命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你关闭终端之后继续运行相应的进程。而且在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中。

## 操作系统中有三个常用的流
```shell
0：标准输入流 stdin
1：标准输出流 stdout
2：标准错误流 stderr
```

## Linux中>和>>的区别
```shell
>> 是追加内容
> 是覆盖原有内容
```

## 示例
nohup bundle exec rails s 2>&1 >> rails.log &

在上面的例子中，0 – stdin (standard input)，1 – stdout (standard output)，2 – stderr (standard error) ；
2>&1是将标准错误2重定向到标准输出&1，标准输出&1再被重定向输入到rails.log文件中。

## nohup和&的区别及联系
&：指在后台运行，但当用户推出(挂起)的时候，命令自动也跟着退出。

nohup：不挂断的运行，注意并没有后台运行的功能，用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行。

注意：nohup没有后台运行的意思；&才是后台运行！

所以，我们可以把这二者结合起来用：nohup COMMAND ARGS &；这样就能使命令永久的在后台执行。

比如：nohup bundle exec rails s &；这样将【bundle exec rails s】任务放到后台，但是依然可以使用标准输入，终端能够接收任何输入，重定向标准输出和标准错误到当前目录下的nohup.out文件，即使关闭shell退出当前session依然继续运行。
再比如：nohup bundle exec rails s 2>&1 >> rails.log &；将日志输出到rails.log。

## /dev/null文件的作用
nohup bundle exec rails s 2>&1 >> rails.log 2>&1 /dev/null &

/dev/null是一个无底洞，任何东西都可以定向到这里，但是却无法打开。说白了就是没有放回功能的垃圾桶。一半会将不关心的输出日入/dev/null

## 结论
所以我们一般这么使用nohup【nohup bundle exec rails s 2>&1 >> /tmp/rails.log 2>&1 /dev/null &】