# gitlab 安装
##  1、安装并配置必要的依赖关系
``` bash
sudo yum install curl policycoreutils openssh-server openssh-clients postfix
```
## 2、添加GitLab仓库,并安装到服务器上
``` bash
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo yum install gitlab-ce
```
## 3、配置Gitlab支持HTTPS
>  1、创建证书目录
``` bash
mkdir -p /etc/gitlab/ssl && chmod 700 /etc/gitlab/ssl && cd /etc/gitlab/ssl
```
> 2、生成私钥`gitlab.mgw1986.com.key`
``` bash
 openssl genrsa -out "gitlab.mgw1986.com.key" 2048
```
> 3、根据私钥生成证书申请文件csr
``` bash
openssl req -new -key "gitlab.mgw1986.com.key" -out "gitlab.mgw1986.com.csr"
```
![](https://i.imgur.com/vY9HHQh.png)
> 4、使用私钥对证书申请进行签名从而生成证书
``` bash
openssl x509 -req -days 365 -in "gitlab.mgw1986.com.csr" -out "gitlab.mgw1986.com.crt" -signkey "gitlab.mgw1986.com.key"
```
> 5、创建pem证书
``` bash
openssl dhparam -out "dhparam.pem" 2048
```
> 6、修改目录权限
```bash 
chmod 600 *
```
> 7、修改gitlab 配置文件
```bash
vim /etc/gitlab/gitlab.rb
#external_url http 改为https
external_url 'https://gitlab.mgw1986.com'
#nginx['redirect_http_to_https'] false 改为true
nginx['redirect_http_to_https'] = true
#配置nginx['ssl_certificate'] 设置上面的生成的crt文件
nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.mgw1986.com.crt"
#配置nginx['ssl_certificate_key']  设置上面的生成的key文件
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.mgw1986.com.key"
#配置nginx['ssl_dhparam'],设置上面的生成的pem文件
nginx['ssl_dhparam'] = "/etc/gitlab/ssl/dhparams.pem"
```
> 8、gitlab重新配置
``` bash
gitlab-ctl reconfigure
```