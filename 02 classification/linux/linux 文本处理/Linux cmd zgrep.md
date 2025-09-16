
`zgrep` 是一个用于在 **压缩文件** 中搜索文本内容的命令行工具，它可以直接处理 `.gz` 格式的压缩文件，而无需先手动解压，非常适合快速检索压缩日志或文档。

- 本质上是 `grep` 的 “压缩版”，语法和大部分选项与 `grep` 一致
- **支持管道操作**（例如 `zgrep "key" data.gz | less` 分页查看结果）
- 仅处理 `.gz` 格式，其他压缩格式（如 `.bz2`、`.xz`）需用 `bzgrep`、`xzgrep` 等对应工具

## 常用选项

- `-i`：忽略大小写（不区分大小写搜索）
- `-r`：递归搜索目录下的所有压缩文件
- `-n`：显示匹配内容所在的行号
- `-v`：反向匹配（显示不包含搜索内容的行）
- `-E`：使用扩展正则表达式（支持更复杂的模式匹配）
- `-c`：仅统计匹配的行数



1. 在 `file.txt.gz` 中搜索包含 "error" 的行：

```bash
   zgrep "error" file.txt.gz
```

2. 忽略大小写搜索 "Error" 并显示行号：

```bash
 zgrep -in "Error" file.txt.gz
```

3. 递归搜索 `/var/log` 目录下所有 `.gz` 压缩文件中包含 "failed" 的行：
```bash
zgrep -r "failed" /var/log/*.gz
```

 4. 统计压缩文件中匹配 "success" 的行数
 
```bash
zgrep -c "success" result.log.gz
```
