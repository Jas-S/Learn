# 发展
* Ant：2000年发布，纯Java开发。
* Maven：2004年发布，基于pom.xml配置
* Gradle：2012年发布，Google背书的一款项目管理工具
  * 优势：简洁，groovy语法，build速度快（Spring源码采用Gradle）
  * 劣势：跟新频繁，改动较大且没有考虑向上兼容。学习成本高。需要额外学习groovy脚本语言。


# 概念
坐标：确定一个jar包在仓库中的位置。

# Gradle安装配置
## 安装
环境：Windows，Java，Mac平台推荐使用Homebrew包管理器进行安装。
版本：
* binary-only：只有可执行文件
* complete：除了可执行文件还包含源码及说明文档  

配置（Windows）  
* 新建GRADLE_HOME环境变量，指向gradle根目录。
* 在path中加入项%GRADLE_HOME%¥bin, 类似于JDK或Maven的配置。
* 打开CMD，执行gradle -v，输出版本信息则安装成功。

> #### 注意  
> 这里只是让大家能全局享受gradle，于mvn或java -jar等类似，实际工作中由于个项目版本不一，并不会使用本地配置的gradle，而是采用wrapper的方式进行配置。

# Gradle集成IDE
