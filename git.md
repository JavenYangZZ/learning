#GIT简介
##安装git
- 在Linux上安装Git
- 在Mac OS X上安装Git
- 在Windows上安装Git
    * 安装完成后，还需要最后一步设置，在命令行输入：
    ```
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"
    ```
    global参数，这台机器上所有的Git仓库都会使用这个配置。
##创建版本库
-   什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
-   两步：创建一个空目录+把这个目录变成Git可以管理的仓库
    ```
    $ mkdir learngit
    $ cd learngit
    $ pwd
    ```
    ```
    $ git init
    ```
