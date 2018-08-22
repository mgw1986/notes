# ansible安装及使用
## 1、yum 安装方式
``` bash
yum install -y epel-release
yum install -y ansible
```
## 2、源码安装（python3.6 + ansible2.5）

>1、 安装python3
``` bash
#1、安装依赖库
yum install -y gcc ncurses-libs zlib-devel mysql-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
#2、下载python3源码
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
#3、解压
tar -xf Python-3.6.5.tar.xz
#4、进入python目录并编译安装
cd Python-3.6.5
./configure --prefix=/usr/local/python3 --enable-optimizations
sudo make && sudo make altinstall
#5、安装virtualenv并初始化python3.6环境
pip3 install virtualenv
useradd deploy && su - deploy #可选
virtual -p /usr/local/bin/python3.6 .py3
#Python 3.6 可直接使用 python3 -m venv venv_name 创建虚拟环境。
# /usr/local/python3/bin/python3.6 -m venv py3
# source py3/bin/activate
```
> 2、下载ansible源代码并安装
``` bash
#安装依赖包，需要在python的虚拟环境安装，否则，ansible会提示未安装
pip3 install paramiko PyYAML jinja2
#下载ansible源码
git clone https://github.com/ansible/ansible.git
#切换分支为stable-2.6
git checkout stable-2.6
#安装ansible
source /home/mgw1986/py3/ansible/hacking/env-setup -q
#查看ansible版本
ansible --version
#拷贝配置文件
sudo mkdir -p /etc/ansible
cd /home/mgw1986/py3/ansible/examples
sudo cp hosts ansible.cfg /etc/ansible/
```
> 3、ssh免密登录
```bash
#编辑/etc/hosts文件，添加目标主机,如:192.168.2.193 gitlab.mgw1986.com
ssh-keygen -t rsa
ssh-copy-id ~/.ssh/id_rsa.pub root@gitlab.mgw1986.com
#测试ansible是可以成功访问目标主机!
ansible -m ping all -u root
```
![](https://i.imgur.com/mQYYqYa.png)