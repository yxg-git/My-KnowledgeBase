# Linux

## 一、用户及文件权限管理

1. 查看用户

   ```shello
   who am i
   
   # 或者
   
   who mom likes 
   
   whoami # 查看当前登录用户的用户名
   ```

   ![image-20220304093228389](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/image-20220304093228389.png)

   > 输出的第一列表示打开当前伪终端的用户的用户名，第二列的 `pts/0` 中 `pts` 表示伪终端，所谓伪是相对于 `/dev/tty` 设备而言的，伪终端就是当你在图形用户界面使用 `/dev/tty7` 时每打开一个终端就会产生一个伪终端，`pts/0` 后面那个数字就表示打开的伪终端序号，你可以尝试再打开一个终端，然后在里面输入 `who am i`，看第二列是不是就变成 `pts/1` 了，第三列则表示当前伪终端的启动时间

2. 创建用户

   ```shell
   sudo adduser 'username' # 添加用户
   
   sudo passwd 'username' # 设置密码
   
   su -l username # 切换用户
   
   exit # 推出当前用户
   ```

   ![image-20220304094421838](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/image-20220304094421838.png)
   
   > #### `adduser` 和 `useradd` 的区别是`useradd` 只创建用户，不会创建用户密码和工作目录，创建完了需要使用 `passwd <username>` 去设置新用户的密码。`adduser` 在创建用户的同时，会创建工作目录和密码（提示你设置）
   
3. 用户组

   在 Linux 里面每个用户都有一个归属（用户组），用户组简单地理解就是一组用户的集合，它们共享一些资源和权限，同时拥有私有资源。

   - 查看用户组

     ```shell
     # 方法一：使groups命令
     groups `用户名`
     
     # 方法二：查看/etc/group文件
     cat /etc/group | sort
     ```

   - 创建工作组

     ```shell
     groupadd 'groupname'
     ```

   - 将其他用户添加进sudo用户组

     ```shell
     groups "用户名"
     
     sudo usermod -G sudo "用户名"
     
     groups "用户名"
     ```

4. 删除用户和用户组

   ```shell
   sudo userdel '用户名' --remove-home # 会删除用户目录
   
   sudo userdel '用户名' # 不会删除用户目录
   
   gpasswd -d A '用户组名'
   ```

5. 文件权限

   ```shell
   # 查看文件
   ls -l
   
   ll
   
   # 显示所有文件，包括隐藏文件
   ls -a 
   ls -al 
   
   # 显示所有文件大小
   ls -asSh
   
   ```

   ![image-20220304101227460](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/image-20220304101227460.png)

   ![img](https://doc.shiyanlou.com/linux_base/3-9.png)

   ![img](https://doc.shiyanlou.com/linux_base/3-10.png)
   
6. 变更文件所有者

   ```shell
   sudo chown '用户名' '文件名'
   ```

7. 修改文件权限

   ```shell
   chmod '权限值' '文件名'
   ```

   ![img](https://doc.shiyanlou.com/linux_base/3-14.png)

## 二、Linux目录结构及文件基本操作

1. 新建空白文件

   ```shell
   touch test
   ```

   > `touch` 命令，其主要作用是来更改已有文件的时间戳的（比如，最近访问时间，最近修改时间），但其在不加任何参数的情况下，只指定一个文件名，则可以创建一个指定文件名的空白文件（不会覆盖已有同名文件）

2. 新建目录

   ```shell
   mkdir mydir
   
   # 使用-p参数，可创建多级文件夹
   mkdir -p father/son/grandson
   ```

   > 若当前目录已经创建了一个 test 文件，再使用 `mkdir test` 新建同名的文件夹，系统会报错文件已存在。这符合 Linux 一切皆文件的理念。
   >
   > 若当前目录存在一个 test 文件夹，则 `touch` 命令，则会更改该文件夹的时间戳而不是新建文件。

3. 复制文件

   ```shell
   cp test father/son/grandson
   ```

4. 复制目录

   ```shell
   # 使用-r参数复制多级目录
   cp -r father family
   ```

5. 删除文件

   ```shell
   # 删除单个文件
   rm test
   
   # 使用-f参数强制删除
   rm -f test
   ```

6. 删除目录

   ```shell
   rm -r family
   
   # 权限不足删除不了的目录需要加-f参数
   rm -rf family
   ```

7. 移动文件

   ```shell
   mv file1 Documents
   ```

8. 重命名文件

   ```shell
   mv file1 myfile
   ```

   要实现批量重命名，`mv` 命令就有点力不从心了，我们可以使用一个看起来更专业的命令 `rename` 来实现。不过它要用 perl 正则表达式来作为参数，关于正则表达式我们要在后面才会介绍到，这里只做演示，你只要记得这个 `rename` 命令可以批量重命名就好了，以后再重新学习也不会有任何问题，毕竟你已经掌握了一个更常用的 `mv` 命令。

   `rename` 命令并不是内置命令，若提示无该命令可以使用 `sudo apt-get install rename` 命令自行安装。

   ```shell
   # 使用通配符批量创建 5 个文件:
   touch file{1..5}.txt
   
   # 批量将这 5 个后缀为 .txt 的文本文件重命名为以 .c 为后缀的文件:
   rename 's/\.txt/\.c/' *.txt
   
   # 批量将这 5 个文件，文件名和后缀改为大写:
   rename 'y/a-z/A-Z/' *.c
   ```

9. 查看文件

   ```shell
   cat /etc/passwd
   
   # 可以加-n参数显示行号
   cat -n passwd
   
   # 默认只显示一屏内容，终端底部显示当前阅读的进度
   more /etc/passwd
   
   # 查看后10行数据
   tail /etc/passwd
   
   # 自定义行数
   tail -n 1 /etc/passwd
   ```

10. 查看文件类型

    ```shell
    file /bin/ls
    ```

    > 与 Windows 不同的是，如果你新建了一个 shiyanlou.txt 文件，Windows 会自动把它识别为文本文件，而 `file` 命令会识别为一个空文件。因为在 Linux 中文件的类型不是根据文件后缀来判断的。当你在文件里输入内容后才会显示文件类型。

    ![image-20220307092942635](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/image-20220307092942635.png)

## 三、环境变量与文件查找

1. 变量

   ```shell
   # 新建变量
   declare tmp
   
   # 为变量赋值
   tmp=yaoxiaogang
   
   # 读取变量值
   echo $tmp
   ```

2. 搜索文件

   ```shell
   # whereis
    
   
   # which 
   
   # find
   
   # locate
   ```

   

## 四、文件打包与解压缩


## 五、文件系统操作与磁盘管理

