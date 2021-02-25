## 部署后端
```bash
cd mhgoi-blog/backend/api
mvn spring-boot:run
```

## 打包
```bash
cd mhgoi-blog/backend
./mvnw clean package -DskipTests=true
```

## 运行

创建一个覆盖配置文件,增加一个jdbc的配置片段，覆盖jar包中的参数
vi $(pwd)/override.yml
```yml
spring:
  profiles: prod-mysql8
  datasource:
    driverClassName: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/mhgoi_blog?characterEncoding=utf-8&useSSL=false&autoReconnect=true&allowPublicKeyRetrieval=true
    username: jerry
    password: jerry
```


```bash
java \
-server \
-Xms1g \
-Xmx1g \
-Djava.awt.headless=true \
-Dfile.encoding=UTF-8 \
-Djava.security.egd=file:/dev/./urandom \
-Duser.timezone=Asia/Shanghai \
-Djava.net.preferIPv4Stack=true \
-jar mhgoi-blog-api-1.0.0-SNAPSHOT.jar \
--spring.config.name=application,override \
--spring.config.location=classpath:/application.yml,file:///$(pwd)/override.yml \
--spring.profiles.active=prod-mysql8 \
--spring.pid.file=/tmp/mhgoi.pid
```

```bash
# 关闭应用
kill -15 $(cat /tmp/mhgoi.pid)
# or
kill -s TERM $(cat /tmp/mhgoi.pid)
# 杀死应用
kill -9 $(cat /tmp/mhgoi.pid)
# or
kill -s KILL $(cat /tmp/mhgoi.pid)
```



## systemd

vi `mhgoi.service`

```bash
[Unit]
Description=mhgoi server
After=syslog.target network.target

[Service]
#Type=forking
#PIDFile=/tmp/mhgoi.pid
Type=simple
Environment="PATH=/usr/local/java/bin:$PATH"
WorkingDirectory=/data/mhgoi
TimeoutStartSec=30
TimeoutStopSec=20
#User=jerry
#Group=admin
ExecStart=/usr/local/java/bin/java \
    -server \
    -Xms1g \
    -Xmx1g \
    -Djava.awt.headless=true \
    -Dfile.encoding=UTF-8 \
    -Djava.security.egd=file:/dev/./urandom \
    -Duser.timezone=Asia/Shanghai \
    -Djava.net.preferIPv4Stack=true \
    -jar mhgoi-blog-api-1.0.0-SNAPSHOT.jar \
    --spring.config.name=application,override \
    --spring.config.location=classpath:/application.yml,file:///data/mhgoi/override.yml \
    --spring.profiles.active=prod-mysql8 \
    --spring.pid.file=/tmp/mhgoi.pid
ExecStop=/bin/kill -s TERM $MAINPID
PrivateTmp=true
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
TasksMax=infinity
LimitNOFILE=512000
LimitNPROC=512000
[Install]
WantedBy=multi-user.target
```

```bash
cp mhgoi.service /etc/systemd/system/mhgoi.service;
systemctl daemon-reload;
systemctl enable mhgoi;
systemctl restart mhgoi;
systemctl --full --no-pager status mhgoi;
```


## troubleshooting

```bash
systemctl --full --no-pager status mhgoi;

journalctl -ex -u mhgoi --no-pager;

top -H -p $(systemctl show --property MainPID --value mhgoi)

./show-busy-java-threads --pid $(systemctl show --property MainPID --value mhgoi) --count 10 --jstack-path /usr/local/java/bin/jstack --use-ps

pstree -ahlst $(systemctl show --property MainPID --value mhgoi)

curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar


java -XX:+PrintFlagsFinal -version

jps -mlvV
jstack

jcmd -l
jcmd $(systemctl show --property MainPID --value mhgoi) VM.native_memory scale=MB
jcmd <pid> VM.system_properties
jcmd <pid> VM.flags
jcmd <pid> VM.command_line
jcmd <pid> GC.run

jstat
jstat -gccause $(systemctl show --property MainPID --value mhgoi)
jstat -gc $(systemctl show --property MainPID --value mhgoi) 2s 10
jstat -gcutil $(systemctl show --property MainPID --value mhgoi) 2s 10
jstat -gcmetacapacity $(systemctl show --property MainPID --value mhgoi)
jstat -gccapacity $(systemctl show --property MainPID --value mhgoi) 2s
jmap -clstats $(systemctl show --property MainPID --value mhgoi)


# NMT诊断
-XX:NativeMemoryTracking=detail \
-XX:+UnlockDiagnosticVMOptions \
-XX:+PrintNMTStatistics

journalctl -k --no-pager |grep -Ei "oom|kill|error|java"
```


## ref
* https://visualvm.github.io/download.html
* [https://github.com/fabric8io-images/run-java-sh](https://github.com/fabric8io-images/run-java-sh)
* https://github.com/bcicen/ctop/releases/download/v0.7.5/ctop-0.7.5-linux-amd64
* https://dzone.com/articles/jvm-tuning-using-jcmd
* https://foojay.io/command-line-arguments/openjdk-11/?tab=alloptions
* https://chriswhocodes.com/hotspot_options_openjdk11.html
* https://www.google.com/search?q=JVM+Diagnostics
* https://www.google.com/search?q=JVM+Performance+Tuning
* https://www.google.com/search?q=Java+Optimization