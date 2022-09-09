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

# Gradle构建Web项目
## 创建Gradle项目
1. 手动创建
   1. 执行 ```gradle init``` 创建项目
2. 快速搭建（https://start.sping.io）
   1. Gradle版的SpringBoot
   2. Maven版的SpringBoot

项目结构解析
```
|-build.gradle                         ①
|-gradlew                              ②
|-grablew.bat                          ③
|-settings.gradle                      ④
|-gradle                               ⑤
|  └-wrapper
|     |- gradle-wrapper.jar
|     |- gradle-wrapper.properties
└-src                                  ⑥
  |-main
  └-test
```
1. 项目自动编译时要读取的配置文件。比如指定项目的依赖包等。build.gradle有两种，一个是根目录里的全局配置，一个是在各个模块目录里面。全局的配置中主要是声明仓库源，gradle的版本号说明等。
2. linux下的gradle执行脚本，可用来执行gradle指令，如：./gradlew build
3. Windows下的gradle执行脚本，可用来执行gradle指令。
4. 包含必要的一些设置，例如，任务或项目之间的依赖关系等。
5. 包含wrapper文件夹及2个子文件，作用是配置gradle执行环境，以便在没有安装gradle的机器上也可以执行gradle指令。
6. 项目源码

build.gradle基础结构
```Groovy
/********* 普通程序gradle init 初始化 *********/
plugins {
    // Apply the java plugins to add support for Java
    id 'java'
    ...
}
repositories {
    // Use jcenter for resolving dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
}
dependencies {
    implementation 'com.google.guava:guava:27.1-jre'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.4.2'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.4.2'
}
application {
    mainClassName = 'first.init.App'
}
test {
    useJunitPlatform()
}

/********* SpringBoot 项目基础配置 ***********/
plugins {
    id 'org.springframework.boot' version '2.6.4'
    id 'io.sping.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group = 'me.jas.learnGradle'
version = '1.0.0'
sourceCompatibility = '1.8'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJunitPlatform()
}
```
>mavenCentral()和jCenter()区别  
>jCenter相比mavenCentral构件更多，性能也更好。但有些构件仅存在于mavenCentral仓库中。



