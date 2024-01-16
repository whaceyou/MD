## 开启虚拟机密码登录

```shell
vi /etc/ssh/sshd_config
```
- 修改PasswordAuthentication yes后
```shell
service sshd restart
```

## 网络配置
```shell
vi /etc/sysconfig/network-scripts/ifcfg-eth1
```
- 新增
GATEWAY=192.168.12.1
DNS1=114.114.114.114
DNS2=8.8.8.8
```shell
#VAGRANT-BEGIN
# The contents below are automatically generated by Vagrant. Do not modify.
NM_CONTROLLED=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.12.19
NETMASK=255.255.255.0
GATEWAY=192.168.12.1
DNS1=114.114.114.114
DNS2=8.8.8.8
DEVICE=eth1
PEERDNS=no
#VAGRANT-END
```

- 重启网卡
```shell
service network restart
```