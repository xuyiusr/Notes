# CoreDump配置

1 . 建立储存CoreDump文件的文件夹： mkdir /corefile

2 . 设置coredump文件大小： ulimit -c unlimited

3 . 执行以下两条命令：

     a . echo "1" > /proc/sys/kernel/core_uses_pid       //将1写入到该文件里

     b . echo "/corefile/core-%e-%p-%t" > /proc/sys/kernel/core_pattern  

4 . 修改配置文件/etc/profile

     a. 修改ulimit -S -c 0 > /dev/null 2>&1 为 ulimit -S -c unlimited > /dev/null 2>&1 。没有则直接添加。

     b . 执行命令生效该文件：source /etc/profile

5 . 检查配置是否生效 : kill -s SIGSEGV $$