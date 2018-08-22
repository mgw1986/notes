# jenkins安装及部署
## 1、安装java 环境
``` bash
sudo yum install java
```
## 2、安装jenkins
```bash
#下载yum源
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
#导入密钥
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
#安装jenkins
sudo yum install jenkins
```
## 3、jenkins配置(非必须)
```bash
#修改jenkins的启动用户为mgw1986
sudo vim /etc/sysconfig/jenkins 
JENKINS_USER="mgw1986"
#修改jenkins的目录和日志目录的属主为mgw1986用户
sudo chown -R mgw1986.mgw1986 /var/lib/jenkins
sudo chown -R mgw1986.mgw1986 /var/log/jenkins
sudo chown -R mgw1986.mgw1986 /var/cache/jenkins
#启动jenkins
sudo systemctl start jenkins
```