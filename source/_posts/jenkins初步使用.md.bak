---
title: jenkins初步使用
date: 2018-10-05 21:02:05
tags:
- jenkins
- 自动化部署
categories:
- 工具
---

## linux环境下安装jenkins
1.第一种是网上下载jenkins war包拷到/webapp目录下<br>
2.第二种是在线安装,通过yum方式安装下载<br>
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo<br>
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key<br>
sudo yum install jenkins<br>

## 启动jenkins
service jenkins start<br>
<font color="Chocolate">注意:这里最好修改jenkins默认端口号8080，以防冲突</font><br>
vi /etc/sysconfig/jenkins<br>
查找/JENKINS_PORT，修改JENKINS_PORT="8080"，默认为“8080”，我修改为了9999<br>

## 访问jenkins
地址:http://ip:修改后的端口号<br>
第一次登录是需要解锁，依据提示的路径,拷贝相应的密码到浏览器输入框。<br>
之后提示安装自定义插件还是推荐插件，此处我选择推荐插件<br>
<font color="Chocolate">注意：这里，可能是由于网络的原因，我的一些插件迟迟下载不下来，页面卡死在下载插件那里，我就强制重启jenkins。ps -ef|grep jenkins,找出相应的jenklins进程，杀死进程：kill 进程号，之后重启jenkins,service jenkins start，重新访问，便可以进入到主界面，依据提示设置用户名密码那些。</font>

![image](image/jenkins.png)

后续jenkins各项操作陆续更新。。。。
