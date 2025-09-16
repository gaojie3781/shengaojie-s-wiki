| 特殊符号 | 作 用           |
| ---- | ------------- |
| ~    | 代表当前登录用户的主目录  |
| ~用户名 | 表示切换至指定用户的主目录 |
| -    | 代表上次所在目录      |
| .    | 代表当前目录        |
| ..   | 代表上级目录        |

```bash
[root@localhost ~]# cd /usr/local/src  
#进入/usr/local/src目录  
[root@localhost src]# cd -  
/root  
[root@localhost ~]#  
#"cd -"命令回到进入 src 目录之前的主目录  
[root@localhost ~]# cd -  
/usr/local/src  
[root@localhost src]#  
#再执行一遍"cd -"命令，又回到了 /usr/local/src 目录

```


