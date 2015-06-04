##**HADep 用户手册**

####**前言**
本文为HADep使用手册，为使用该项目的用户提供帮助。关于工业界和学术界成熟的高可用解决方案调研总结请参考[这里](https://github.com/anthony0859/HAResearchSummary)。

####**环境配置**
安装JDK1.7及以上，[JDK下载网址](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html)

下载AcmeLib Runtime，[AcmeLib下载地址](http://acme.able.cs.cmu.edu/acmeweb/download.php)

下载HATransformation项目，[HATransformation下载地址](https://github.com/anthony0859/HATrans)

安装Docker1.5及以上运行环境，[Docker安装介绍](https://docs.docker.com/installation/#installation)，[Docker Compose使用介绍](https://docs.docker.com/compose/)

下载docker-gen项目，[docker-gen地址](https://github.com/jwilder/docker-gen)

####**使用**
HADep 目前分为两个部分：
 1. **HATransformation**负责把基于Acme描述的待部署的自定义体系架构对象转换成相应的高可用体系架构对象；
用户在file/input.acme文件里输入使用Acme描述的初始体系架构对象，其中构件属性必须包含其进行部署所必要的参数数据；针对需要进行高可用部署的构件，需要定义HAParams属性：ServiceCategory，Mode和Size。以Web系统中的WebServer构件定义为例：

```
    Component WebServer  = {
        Port receiveHttpRequestPort  = {
        }
        Port sendSQLRequestPort  = {
        }
        Property ServiceIP : string = "172.17.0.1";
        Property ServicePort : string = "80";
        Property HAParams = {[ServiceCategory = "stateless";],[Mode = "active/active";],[Size = "2";]};  
    }
```

 在完成自定义体系结构后，运行HATransformation工程即可。
 2.  **Dep**负责根据完成转换的高可用体系架构对象进行应用服务实例的自动化部署，基于Docker Compose这一云服务编排工具实现。因为在自动化部署过程中涉及到安装软件相关的配置文件的修改，docker-gen项目为这一需求提供了便捷，根据上面HATransformation生成的高可用体系结构来利用Compose完成实例的自动化部署。自动化部署的例子可以参考[Demo](https://github.com/anthony0859/docker)，是对Apache服务器构件进行主主模式高可用部署的例子，在docker目录下运行
 
```
    docker-compose up
```


####**查看结果**
**HATransformation**输出结果存放在file中，文件结构如下：

 - output.acme：基于Acme描述的完成转换后的高可用体系架构对象
 - DeploymentConstraint：哪些构件必须部署在同一主机上或者不能部署在同一主机上
 - 各个主机配置文件：主机上所有构件及其相应的配置参数

**Dep**在完成自动化部署后，这时实例的各个容器处于运行状态，可以直接进入容器使用应用服务或进行高可用性测试。








