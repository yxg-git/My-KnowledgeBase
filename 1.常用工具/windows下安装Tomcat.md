# windows下安装Tomcat

## 一、安装包下载

- 官网下载：http://tomcat.apache.org/

- 网盘链接：https://pan.baidu.com/s/1bRmRcLC_pEbV48ne9npLZA 提取码：e400 
  

> 下载后将其解压到一个没有空格和中文的目录下

![image-20211222222937785](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222222937785.png)



## 二、启动

- 双击`bin\startup.bat`，在浏览器输入 `localhost:8080`

![image-20211222223956767](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222223956767.png)

> 如果双击之后没有反应，或者窗口一闪而过，那就说明你的Java环境变量不对，正确配置环境变量
>
> 如果你不知道怎么配，可以参考一下这篇文章：https://blog.csdn.net/qq_45282568/article/details/122093495

- 如果想要结束服务，按<kbd>CTRL</kbd>+<kbd>C</kbd>即可
- 修改tomcat端口号：修改tomcat安装目录下的`conf`文件夹下的`server.xml`

![image-20211222224119213](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224119213.png)

![image-20211222224241008](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224241008.png)

## 三、idea集成tomcat

- 点击<kbd>添加配置</kbd>

![image-20211222224351236](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224351236.png)

- <kbd>添加</kbd> --> <kbd>Tomcat 服务器</kbd> --> <kbd>本地</kbd>

![image-20211222224600880](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224600880.png)

- 点击配置

![image-20211222224725508](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224725508.png)

- 配置tomcat的安装目录，点击确定即可

![image-20211222224856113](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222224856113.png)
