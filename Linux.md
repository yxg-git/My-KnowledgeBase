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

## 三、环境变量与文件查找

## 四、文件打包与解压缩

## 五、文件系统操作与磁盘管理

