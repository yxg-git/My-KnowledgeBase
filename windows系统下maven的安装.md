# windows系统下maven的安装

## 一、软件下载

1. 官网下载

点击[maven下载链接](https://maven.apache.org/download.cgi),点击下载

![image-20211222195204724](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222195204724.png)

2. 网盘链接

[网盘链接](https://pan.baidu.com/s/1Vhp-b3r2FFRzIo7CSbHW5Q)提取码：idj1 

> 下载下来之后将其解压到一个没有中文没有空格的目录下

![image-20211222195502233](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222195502233.png)

## 二、配置JDK环境变量

- 这边文章有详细的安装步骤：https://blog.csdn.net/qq_45282568/article/details/122093495?spm=1001.2014.3001.5501

## 三、配置maven环境变量

- 在高级系统设置中配置环境变量

![image-20211222195823506](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222195823506.png)

- 变量名为**MAVEN_HOMR**，变量值为你安装解压的目录路径

![image-20211222195943204](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222195943204.png)

- 点击path，编辑

![image-20211222200055198](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222200055198.png)

- 点击新建，输入**%MAVEN_HOME%\bin**

![image-20211222200434810](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222200434810.png)

## 四、测试

- 在终端中输入mvn -v，显示如下，表示安装成功

![image-20211222200339743](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222200339743.png)

## 五、配置maven仓库

- 在你习惯的目录下建一个文件夹，命名为**maven_repository**
- 将链接：https://pan.baidu.com/s/11R30ZQcpegFmC3h4uRO-hQ 提取码：a8x1 下载的仓库解压到该目录下
- 在MAVE_HOME/conf/settings.xml 文件中配置本地仓库位置（maven 的安装目录下）

![image-20211222201207015](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222201207015.png)

- 打开**settings.xml**，配置如下

![image-20211222201314848](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222201314848.png)

## 六、将maven集成到idea中

- 在<kbd>文件</kbd> --> <kbd>设置</kbd>中搜索**maven**，做如下设置

![image-20211222222233453](https://gitee.com/yxg-git/typora-image/raw/master/img/image-20211222222233453.png)