

# 禁用redis中的某些命令


修改 redis.conf 文件，添加
```
rename-command FLUSHALL ""
rename-command FLUSHDB ""
rename-command CONFIG ""
rename-command KEYS ""
rename-command SHUTDOWN ""
rename-command DEL ""
rename-command EVAL ""
```
然后重启redis。重命名为"" 代表禁用命令，如想保留命令，可以重命名为不可猜测的字符串，如：
`rename-command FLUSHALL joYAPNXRPmcarcR4ZDgC`
