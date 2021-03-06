##  先官网下载

##  配置镜像

安装插件:

```bash
vagrant plugin install --plugin-clean-sources --plugin-source https://gems.ruby-china.com/ vagrant-disksize
```

这样以后想从镜像站安装插件只需要使用命令

```bash
vagrant-plugin-install <plugin>...
```

## Vagrant Box 镜像

> ### Ubuntu 
>
> ```bash
> vagrant init ubuntu-bionic https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/bionic/current/bionic-server-cloudimg-amd64-vagrant.box
> ```
>
> ### CentOS
>
> ```bash
> vagrant init centos7 https://mirrors.ustc.edu.cn/centos-cloud/centos/7/vagrant/x86_64/images/CentOS-7.box
> ```