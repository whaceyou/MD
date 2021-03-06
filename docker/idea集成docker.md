##  1.虚拟机先安装Docker

##  2. Docker开启远程访问

```c
vim /lib/systemd/system/docker.service
#修改ExecStart这行
ExecStart=/usr/bin/dockerd  -H tcp://0.0.0.0:2375  -H unix:///var/run/docker.sock
```

```c
#重新加载配置文件
systemctl daemon-reload    
#重启服务
 systemctl restart docker.service 
#查看端口是否开启
netstat -nlpt
#直接curl看是否生效
curl http://127.0.0.1:2375/info
```

>开放2375端口,并重启防火墙

```c
firewall-cmd --add-port=2375/tcp --permanent
firewall-cmd --reload
#查询2375是否开放
firewall-cmd --query-port=2375/tcp
```

> 测试

```c
curl http://127.0.0.1:2375/version
```

> 在windows浏览器输入

```c
http://192.168.64.170:2375/version
```

![image-20200310190550155](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200310190550155.png)

> 如果是云服务器,需要在安全组规则中添加2375端口

![image-20200310190842250](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200310190842250.png)



## 3.集成到idea

![image-20200310191155221](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200310191155221.png)

###  集成maven插件

- 修改项目的pom文件

```xml
<properties>
		<!--docker镜像的前缀-->
		<docker.image.prefix>docker</docker.image.prefix>
	</properties>
```

```xml
<plugin>
	<groupId>com.spotify</groupId>
	<artifactId>docker-maven-plugin</artifactId>
	<version>1.0.0</version>

	<configuration>
		<!--远程Docker的地址-->
		<dockerHost>http://服务器地址:2375</dockerHost>
		<!--镜像名称，前缀/项目名-->
		<imageName>前缀/${project.artifactId}</imageName>
		<dockerDirectory>src/main/docker</dockerDirectory>
		<resources>
			<resource>
				<targetPath>/</targetPath>
				<directory>${project.build.directory}</directory>
				<include>${project.build.finalName}.jar</include>
			</resource>
		</resources>
	</configuration>
</plugin>
```

- 在src的main下新建docker文件夹，将编写好的Dockerfile放到这个文件夹

![img](http://xuewei.world:8000/wp-content/uploads/2020/02/image-46-1024x349.png)



## 4.构建镜像

依次使用 `clean、package、docker:build` 命令

![image-20200310192748918](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200310192748918.png)