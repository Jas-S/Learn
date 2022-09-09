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

## Gradle Hello World
### Gradle中2大对象：
* Project：一个构建脚本就是一个project，任何一个Gradle构建都是由一个或多个project组成，类似Maven中的pom模块，每个project都是一个脚本文件。
* Task：顾名思义就是任务，是Gradle中最小的执行单元，类似于一个method或function函数，如编译，打包，生成javadoc等，一个project中可以有多个tasks。
### 用Gradle说Hello World
1. 新建build.gradle文件（名字不能更改否则无法执行）
2. 在其内部输入
```groovy
task say {
    println "Hello, world!"
}

task sayDoLast {
    doFirst {
        println "init"
    }
    doLast {
        println "destory"
    }
}
```
3. 在终端环境下执行 ```gradle -q say``` 并回车查看结果。
4. 参数解析：
* -q：输出QUIET级别及以上的日志信息
* -i：输出INFO级别及以上的日志信息
* -d：输出DEBUG级别及以上的日志信息  

每个task都是org.gradle.api.DefaultTask类型，在构建脚本中我们可以直接使用属性名使用该类中的相关属性，在底层Groovy会为我们调用相关的getter和setter方法去访问相关属性，这里先简单使用doFirst或doLast方法试试，后续会对task做详细讲解。  


