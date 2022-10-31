## Linux

1. 创建多级目录： `mkdir -p  /home/tom`

   删除文件夹： `rm -rf `    

   + `-r` 递归删除
   + `-f` 强制删除不提示

​	查看文件内容：`less`

![image-20220927201536882](https://jzh119.oss-cn-beijing.aliyuncs.com/img/202209272015923.png)

2. 指令 > 和 >> : 输出重定向 、 追加
3. history 10 查看历史命令
4. 搜索命令：

+ `find`从指定目录递归的向下遍历其子目录：`find /homw -name hello.cpp`

+ `locate`快速定位文件路径  `locate hello.cpp`
+ `which` 可以查看某个指令在哪个目录下 ： `which ls`

5. grep 指令和管道符指令：

   + grep ：过滤查找

   + 管道符： 表示将前一个命令的处理结果输出传递给后面的命令处理

![image-20220927202343951](https://jzh119.oss-cn-beijing.aliyuncs.com/img/202209272023991.png)

```c++
[wangxg@login05 src]$ cat main.cpp | grep main -n
12:int main(int argc,char** argv)
```

6. 文件压缩：

+ `zip -r /test` 递归压缩
+ `unzip -d /test t.zip` 指定解压目录 

7. `ls -l `

![image-20220927203031278](https://jzh119.oss-cn-beijing.aliyuncs.com/img/202209272030313.png)

8. crontab进行定时任务的设置。

   > 定时、反复执行的，搭配时间

![image-20220927203902075](https://jzh119.oss-cn-beijing.aliyuncs.com/img/202209272039114.png)

9. at 定时任务： 一次性定时计划执行

`ps -ef` 检测当前正在运行的指令有哪些

`ps -ef | grep atd` 检测 atd指令是否在执行

> 8 、 9 部分可以查看 linux手册，韩顺平的



10. linux分区、挂载

11. ps 命令查询有哪些进程正在执行

    > ps -ef 以全格式显示所有的进程

12. kill 终止进程

13. 服务：

![image-20220928151936659](https://jzh119.oss-cn-beijing.aliyuncs.com/img/202209281519778.png)