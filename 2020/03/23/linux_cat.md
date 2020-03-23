# Linux cat命令使用
cat 命令用于连接文件并打印到标准输出设备上。

### 重要参数说明
-n 或 --number：由 1 开始对所有输出的行数编号。

### 实例
1.清空 /etc/test.txt 文档内容：

`cat /dev/null > /etc/test.txt`

### 其他说明
dev/null：在类 Unix 系统中，/dev/null 称空设备，是一个特殊的设备文件，它丢弃一切写入其中的数据（但报告写入操作成功），读取它则会立即得到一个 EOF。