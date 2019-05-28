# 使用logrotate分割你的日志


## 重要的 logrotate 选项：
```ruby
compress             --> 压缩日志文件的所有非当前版本
daily,weekly,monthly --> 按指定计划轮换日志文件
delaycompress        --> 压缩所有版本，除了当前和下一个最近的
endscript            --> 标记 prerotate 或 postrotate 脚本的结束
errors "emailid"     --> 给指定邮箱发送错误通知
missingok            --> 如果日志文件丢失，不要显示错误
notifempty           --> 如果日志文件为空，则不轮换日志文件
olddir "dir"         --> 指定日志文件的旧版本放在 “dir” 中
postrotate           --> 引入一个在日志被轮换后执行的脚本
prerotate            --> 引入一个在日志被轮换前执行的脚本
rotate 'n'           --> 在轮换方案中包含日志的 n 个版本
sharedscripts        --> 对于整个日志组只运行一次脚本
size='logsize'       --> 在日志大小大于 logsize（例如 100K，4M）时轮换
```