# 使用gitlab



### 安装步骤



>### 各种依赖

​	1.安装SSH协议

```bash
sudo yum install -y curl policycoreutils-python openssh-server
```

​	2.设置SSH服务开机自启动

```bash
sudo systemctl enable sshd
```

​	3.启动SSH服务

```bash
sudo systemctl start sshd
```

​	4.安装防火墙

```bash
yum install firewalld systemd -y
```

​	5.开启防火墙

```bash
service firewalld  start
```

​	6.添加HTTP服务到firewalld

```bash
sudo firewall-cmd --permanent --add-service=http
```

​	7.重启防火墙

```bash
sudo systemctl reload firewalld
```

​	8.安装Postfix以发送邮件

```bash
sudo yum install postfix
```

​	9.将postfix服务设置成开机自启动

```bash
sudo systemctl enable postfix
```

​	10.启动postfix

```bash
sudo systemctl start postfix
```

​	11.安装wget，用于从外网上下载插件

```bash
sudo  yum -y install wget
```

​	12.安装vim编辑器

```bash
sudo yum install vim -y
```



> ## 使用镜像安装



官方

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

sudo EXTERNAL_URL="http://git.lomofu.com" yum install -y gitlab-ce 

sudo EXTERNAL_URL="http://180.76.240.3" yum install -y gitlab-ce # 安装 GitLab
```



使用国内镜像安装（推荐）

```bash
ls -l /etc/yum.repos.d/ # 查看源配置项
mv /etc/yum.repos.d/gitlab_gitlab-ce.repo /etc/yum.repos.d/gitlab_gitlab-ce.repo.bak # 备份源配置项（也可以直接删除 rm）
```



步骤：

添加gitlab镜像

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.5.7-ce.0.el7.x86_64.rpm
```

安装

```bash
rpm -i gitlab-ce-10.5.7-ce.0.el7.x86_64.rpm
```

修改gitlab配置文件指定服务器ip和自定义端口：

```bash
vim /etc/gitlab/gitlab.rb
```

将9090端口添加到防火墙中

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent 
```

重启防火墙

```bash
sudo systemctl reload firewalld
```

重置gitlab

```bash
gitlab-ctl reconfigure
```

启动gitlab

```bash
gitlab-ctl restart
```



### 卸载GitLab

```bash
sudo gitlab-ctl stop # 停止
sudo rpm -e gitlab-ce # 卸载
ps aux | grep gitlab # 查看守护进程
kill -9 18777 # 杀掉守护进程
find / -name gitlab | xargs rm -rf # 删除所有包含gitlab的文件
```





### 生成ssh

确认本地是否已经有

```
cat ~/.ssh/id_rsa.pub
```

```bash
ssh-keygen -t rsa -C 'lomofu@qq.com'
```

如果你想修改密码，可以使用命令

```bash
ssh-keygen -p <keyname>
```

以下命令可以直接复制到剪贴板

```bash
pbcopy < ~/.ssh/id_rsa.pub
```

使用以下命令：

```bash
ssh -T git@git.lomofu.com
```

注意，要将example.com替换为gitlab的域名，