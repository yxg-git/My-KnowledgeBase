# window系统下JDK1.8的安装

## 一、下载安装文件

1. 官网下载

- 直接访问官方网址[甲骨文官网](http://jdk.java.net/java-se-ri/8-MR3),点击下载

![image-20211222191433496](https://www.image.yaoxiaogang.cn/img/202205061013795.png)

2. 网盘链接下载

- 官网下载没有特殊方法的话下载可能比较慢，我这里准备了[网盘链接](https://pan.baidu.com/s/1ViOKvX8rkLOmnMmVaVJJYg） 提取码：giqs 


## 二、安装

- 傻瓜式安装，一直下一步，存储路径想改的可以改一下
- jdk安装完后会安装jre，也是改一下路径或者不改，**文件夹必须是空文件夹**，一直下一步即可
- 安装完毕后点击关闭

![image-20211222192701551](https://www.image.yaoxiaogang.cn/img/202205061013419.png)

## 三、配置环境变量

- 有的电脑会自动配置环境变量，这个要注意

- 如果你的电脑没有自动配置环境变量，那么右击<kbd>此电脑</kbd>--><kbd>属性</kbd>--><kbd>高级系统设置</kbd>--><kbd>环境变量</kbd>--><kbd>系统变量</kbd>--><kbd>新建</kbd>

![image-20211222193219578](https://www.image.yaoxiaogang.cn/img/202205061013006.png)



![image-20211222193559893](https://www.image.yaoxiaogang.cn/img/202205061013047.png)



- 在系统变量中找到path，点击编辑

![image-20211222193717182](https://www.image.yaoxiaogang.cn/img/202205061013488.png)



- 点击新建，输入**%JAVA_HOME%\bin**，然后点击上移将其移至最前面

![image-20211222193940071](https://www.image.yaoxiaogang.cn/img/202205061013537.png)

## 四、测试

- 在cmd中输入**java -version**

![image-20211222194025456](https://www.image.yaoxiaogang.cn/img/202205061013464.png)

- 这样显示就表示安装成功
