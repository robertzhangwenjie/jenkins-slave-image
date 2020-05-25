# 基于jenkinsci/slave 定制的镜像

- 概述

- - 早前，基本都是使用虚拟机安装jenkins，然后将其作为jenkins       slave配置到master上，当没有任何job运行在该服务器时，依然会占用服务器的资源，云计算时代，利用docker       agent作为jenkins agent的弹性机制，在运行流水线时才创建镜像，运行结束后销毁，为了达到这个目的，我们需要创建Jenkins slave的基础镜像

- 解决方案

- - 利用jenkins提供的不同类型的基础镜像，制作特定环境的agent镜像，当某个job需要使用时指定创建，这样可以达到最小化镜像

  - - 例如

    - - 创建一个只运行nodejs项目的jenkins-slave-nodejs镜像，只有当运行的项目采用nodejs时才使用它

 

- 实施步骤

- - 找到对应的jenkins       slave镜像

  - - 例如        jenkinsci/slvae ，它包含了jdk并且提供了Jnlp的连接方式

    - https://hub.docker.com/r/jenkinsci/slave/dockerfile

    - 镜像文件解析

    - 1. 创建了一个jenkins的用户组
      2. 添加了一个jenkins用户
      3. 安装了jenkins的agent.jar
      4. 切换镜像用户为jenkins
      5. 申明了两个volume的位置
      6. 切换工作目录为/home/jenkins

  - 在github上找到基础的Nodejs镜像文件

  - - 注意: 镜像文件的tag例如stretch，alpine等这些表示基础镜像是属于不同的操作系统，因此选中时需要参考jenkinsci        slave镜像的基础镜像系统，这样才能兼容
    - https://github.com/nodejs/docker-node/blob/bf9fb2d1126062e27d0b763674064bb17da4718c/13/stretch-slim/Dockerfile

 

- 镜像文件解析

- 1. 创建用户和用户组node
  2. 设置NODE_VERSION的版本号为13.14.0
  3. 导入证书，下载对应的nodejs版本
  4. 下载YARN

- 基于jenkinsci/slave镜像，制作jenkinsci-slave-nodejs镜像

- -  分析

  - - jenkinscs/slave镜像创建了一个用户组jenkins，且切换了工作目录和当前用户，因此我们需要修改nodejs中的镜像步骤

    - - 删除创建用户组或者提前修改当前用户为root后再添加用户组
      - 切换工作目录
      - 如果切换了工作用户，最后需要切换回jenkins



 
