Linux 中的重定向是指将命令的输入、输出方向改变的机制，默认情况下，命令的输入来自键盘，输出显示在终端。**通过重定向，可以将输出保存到文件、从文件读取输入，或忽略某些输出**。



# 输出重定向（改变命令的输出方向）


1. `>`：覆盖写入文件

将命令的标准输出（stdout）写入指定文件，**覆盖文件原有内容**。

```bash
# 将 "hello" 写入 file.txt（若文件存在则清空原有内容）
echo "hello" > file.txt

# 将 ls 命令的输出写入 list.txt
ls > list.txt
```

 2. `>>`：追加写入文件

将命令的标准输出（stdout）追加到指定文件末尾，**不覆盖原有内容**。
```bash
# 向 file.txt 追加一行内容
echo "world" >> file.txt

# 将当前日期追加到 log.txt
date >> log.txt
```

 3. `2>`：错误输出重定向

将命令的错误输出（stderr，如错误提示）写入指定文件，**覆盖原有内容**。

```bash
# 将错误信息写入 error.log（例如执行一个不存在的命令）
ls non_existent_file 2> error.log
```

4. `2>>`：错误输出追加

将错误输出追加到指定文件末尾。

```bash
# 错误信息追加到 error.log
rm non_existent_file 2>> error.log
```

5. `&>` 或 `>&`：合并标准输出和错误输出

将命令的所有输出（包括 stdout 和 stderr）写入同一文件。
```bash
# 合并输出到 all.log（覆盖模式）
command &> all.log

# 合并输出并追加到 all.log
command &>> all.log
```

# 二、输入重定向（改变命令的输入来源）

1. `<`：从文件读取输入

让命令从指定文件读取输入，而非默认的键盘。

```bash
# 统计 file.txt 的行数（等价于 wc -l file.txt）
wc -l < file.txt

# 用 cat 显示文件内容（等价于 cat file.txt）
cat < file.txt
```

 2. `<<`：Here Document

允许在命令行中直接输入多行文本作为命令的输入，直到遇到指定的终止符（通常是自定义的标识符，如 `EOF`）。

```bash
# 向 file.txt 写入多行内容（终止符 EOF 需单独成行，且前后无多余字符）
cat > file.txt << EOF
第一行内容
第二行内容
第三行内容
EOF

# 在脚本中用于批量输入（例如给 mysql 命令传入多条 SQL）
mysql -u root -p << EOF
show databases;
use test;
select * from users;
EOF
```

# 三、特殊用法：丢弃输出

如果不需要命令的输出（尤其是错误信息），可以将其重定向到 `/dev/null`（系统的 “黑洞” 设备，写入的数据会被丢弃）。

```bash
# 丢弃标准输出（只执行命令，不显示结果）
command > /dev/null

# 丢弃错误输出（忽略错误提示）
command 2> /dev/null

# 丢弃所有输出（完全静默执行）
command &> /dev/null
```