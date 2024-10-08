在Linux系统中，`ls`指令是用来列出目录内容的命令，它的基本用法如下：

```
ls [选项] [文件或目录]
```

![image-20240714135255592](D:\blog\linux\assets\image-20240714135255592.png)

### 常用选项：

- `-l`：以详细列表方式显示文件或目录的详细信息，包括权限、所有者、文件大小、修改日期等。
  
  示例：
  ```
  ls -l
  ```

- `-a`：显示所有文件（包括隐藏文件，以`.`开头的文件）。
  
  示例：
  ```
  ls -a
  ```

- `-h`：结合 `-l` 使用时，以人类可读的格式显示文件大小（例如，KB、MB、GB等）。
  
  示例：
  ```
  ls -lh
  ```

- `-r`：以相反的顺序显示文件或目录。

  示例：
  ```
  ls -r
  ```

- `-t`：按修改时间排序，最新修改的文件或目录在前面。

  示例：
  ```
  ls -t
  ```

### 示例：

1. 显示当前目录的所有文件和目录：
   ```
   ls
   ```

2. 显示当前目录中的所有文件（包括隐藏文件）及其详细信息：
   ```
   ls -la
   ```

3. 显示指定目录（例如 `/home/user/Documents`）中的所有文件和目录：
   ```
   ls /home/user/Documents
   ```

`ls`命令在Linux系统中非常常用，可以根据不同的选项和参数来满足不同的列目录需求。