# 掌握Python调试器pdb
今天，咱们来介绍介绍这个Python自带的Debug工具--pdb

### pdb有2种用法
```python
# 非侵入式方法（不用额外修改源代码，在命令行下直接运行就能调试）
python3 -m pdb test.py
# 侵入式方法（需要在被调试的代码中添加一行代码然后再正常运行代码）
import pdb;pdb.set_trace()
```

### 命令l和ll
`l`:

查看当前位置前后11行源代码（多次会翻页）

当前位置在代码中会用-->这个符号标出来

`ll`:

查看当前函数或框架的所有源代码

### 添加断点b和tbreak
```python
b
b lineno
b filename:lineno 
b functionname

# filename文件名，断点添加到哪个文件，如test.py
# lineno断点添加到哪一行
# function：函数名，在该函数执行的第一行设置断点
```

tbreak表示添加临时断点：
```python
tbreak
tbreak lineno
tbreak filename:lineno
tbreak functionname

# 执行一次后时自动删除（这就是它被称为临时断点的原因）
```

### 清除断点cl
```python
cl
cl filename:lineno
cl bpnumber [bpnumber ...]

# 1.不带参数用于清除所有断点，会提示确认（包括临时断点）
# 2.带参数则清除指定文件行或当前文件指定序号的断点
```

### 打印变量值p
```python
p expression
```

### 逐行调试命令n、s、r
```python
s
# 执行下一行（能够进入函数体）
n
# 执行下一行（不会进入函数体）
r
# 执行下一行（在函数中时会直接执行到函数返回处）
```

### 非逐行调试命令c、unt、j
```python
c
# 持续执行下去，直到遇到一个断点
unt lineno
# 持续执行直到运行到指定行（或遇到断点）
j lineno
# 直接跳转到指定行（注意，被跳过的代码不执行）
```

### 查看函数参数a
在函数中时打印函数的参数和参数的值

### 打印变量类型whatis
```python
whatis expression
```
打印表达式的类型，常用来打印变量值

### 启动交互式解释器
`interact`

启动一个python的交互式解释器，使用当前代码的全局命名空间（使用ctrl+d返回pdb）

### 打印堆栈信息w
`w`

打印堆栈信息，最新的帧在最底部。箭头表示当前帧。

### 退出pdb
`q`

## 个人建议
建议只用pdb.set_trace()，因为这种方式即可用于flask、django这种流行的web框架，又可用于一段很小的代码片段。