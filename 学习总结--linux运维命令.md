

- ps -ef | grep redis：查找进程安装目录
- ls -l /proc/(进程号)/cwd
- windows下查看端口占用情况并关闭相关进程
  - **查看端口8080被哪个进程占用**：netstat -ano | findstr "8080"
  - **查看进程号为5768对应的进程**：tasklist | findstr "5768

- 搜索历史命令 Ctrl + R