### 配置Java环境
```
$ sudo apt-get update
$ sudo apt-get install openjdk-8-jdk

$ sudo gedit /etc/profile

    #set java environment  
    export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_91   
    export JRE_HOME=${JAVA_HOME}/jre    
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib    
    export PATH=${JAVA_HOME}/bin:$PATH  

$ source /etc/profile

$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
$ sudo update-alternatives --config jar
$ sudo update-alternatives --config javadoc
```



### 源码依赖库

```
sudo apt-get install -y git flex bison gperf build-essential libncurses5-dev:i386 \
  libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib \
  tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386 \
  dpkg-dev libsdl1.2-dev libesd0-dev \
  git-core gnupg flex bison gperf build-essential  \
  zip curl zlib1g-dev gcc-multilib g++-multilib \
  libc6-dev-i386 \
  lib32ncurses5-dev x11proto-core-dev libx11-dev \
  lib32z-dev ccache \
  libgl1-mesa-dev libxml2-utils xsltproc unzip m4
```





```
source build/envsetup.sh
lunch
make -j8
emulator
```



### 问题集

#### 问题1 修改配置JAVA虚拟机的内存大小

```


#方法1
export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4g"

./prebuilts/sdk/tools/jack-admin kill-server
./prebuilts/sdk/tools/jack-admin start-server

#方法2
$gedit ./prebuilts/sdk/tools/jack-admin
1.找到如下语句：

JACK_SERVER_COMMAND="java -XX:MaxJavaStackTraceDepth=-1 -Djava.io.tmpdir=$TMPDIR $JACK_SERVER_VM_ARGUMENTS -cp $LAUNCHER_JAR $LAUNCHER_NAME"

2.将上述语句修改为：

JACK_SERVER_COMMAND="java -XX:MaxJavaStackTraceDepth=-1 -Djava.io.tmpdir=$TMPDIR $JACK_SERVER_VM_ARGUMENTS -Xmx4096m -cp $LAUNCHER_JAR $LAUNCHER_NAME"

主要是添加了-Xmx4096m参数，接下来在源码目录下执行如下命令重启jack-admin服务

./prebuilts/sdk/tools/jack-admin kill-server
./prebuilts/sdk/tools/jack-admin start-server

```