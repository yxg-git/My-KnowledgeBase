# Git

## Git的常用指令

1. 查看git版本号

   ```shell
   git --version
   ```

2. 设置git账号

   ```shell
   git config user.name（查看git账户） 
   git config user.email（查看git邮箱） 
   git config --global user.name “账户名”（设置全局账户名） 
   git config --global user.email “邮箱”（设置全局邮箱） 
   cd ~/.ssh（查看是否生成过SSH公钥）
   ```

3. 生成ssh公钥

   ```shell
   ssh-keygen -t rsa - C "邮箱"
   ```

4. 初始化git仓库

   ```shell
   git init
   ```

5. 查看文件状态

   ```shell
   git status 
   ```
   
5. 添加

   ```SHELL
   git add 文件名
   git add -A . # 添加所有内容
   git add . # 添加新文件和编辑过得文件但不包括删除得文件
   git add -u # 表示添加编辑或者删除得文件，不包括新添加得文件
   ```

7. 提交

   ```shell
   git commit -m "提交信息"
   ```

8. 查看日志

   ```shell
   git log
   ```

9. 查看所有操作记录

   ```shell
   git reflog
   ```

10. 切换版本

    ```shell
    git reset --hard 版本唯一索引值
    ```

11. 创建分支

    ```shell
    git branch 分支名
    ```

12. 切换分支

    ```shell
    git checkout 分支名
    ```

13. 合并分支

    ```shell
    git merge 分支名
    ```

14. 删除分支

    ```shell
    git branch -d 分支名
    ```

15. 查看分支列表

    ```shell
    git branch
    ```

16. 推送

    ```shell
    git remote add 远程名称 远程仓库url
    git push -u 仓库名称 分支名
    ```

17. 克隆

    ```shell
    git clone 仓库地址
    ```

18. 拉取

    ```shell
    git pull 远程仓库名 分支名
    ```
    
18. 查看远程地址

    ```markdown
    git remote -v
    ```
    
18. 添加忽略文件

    ```shell
    touch .gitign
    ```

## Git的分支命名规则，分支类型

#### 一、命名规则

- **master**：master
- **develop**：develop
- **其他分支**：feature/hotfix-姓名全拼-来源-年月日时分-功能描述
  - 功能分支：feature-yaoxiaogang-develop-2022221549-demo
  - 预发布分支：1.0.5-RC1
  - 发布分支：1.0.5-RELEASE

#### 二、分支类型

- **master**：跟线上代码保持一致，用于修复线上bug时拉取代码
- **develop**：开发当中最新的代码，合并到develop的代码需要具备随时可上线的特性
- **feature**（功能分支）：个人开发的功能分支，从develop拉取，用于开发功能
- **hotfix**（热修复分支）：专门用于修复线上紧急bug
- **tag**：每次发版本后进行版本的备份

## 代码分支合并原则

开发人员往develop提交代码时，提交mager请求给模块名称人，则模块负责人进行代码复查和合并。
