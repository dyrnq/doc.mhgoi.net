# maven

## 下载

从[https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/)下载个maven包。

[https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz)

下载解压后设置MAVEN_HOME环境变量和PATH环境变量中追加$MAVEN_HOME/bin

```bash
jim@debian:~$ mvn -version
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /usr/local/maven
Java version: 11.0.9.1, vendor: AdoptOpenJDK, runtime: /usr/local/jdk-11.0.9.1_1
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.19.0-13-amd64", arch: "amd64", family: "unix"
```

## 配置mirror

修改`$MAVEN_HOME/conf/settings.xml`mirrors节点中添加如下内容：

```xml
<mirror>
    <id>huaweicloud</id>
    <mirrorOf>*</mirrorOf>
    <url>https://mirrors.huaweicloud.com/repository/maven/</url>
</mirror>
```

打开`$MAVEN_HOME/conf/settings.xml`中的localRepository配置

```xml
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
<localRepository>/usr/local/maven_repo</localRepository>
```
