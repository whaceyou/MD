## 1.下载solr



[apache](http://apache.org/)下寻找Lucene 寻找solr

[jdk]( https://www.oracle.com/)寻找jdk

## 2.安装jdk和solr

上传压缩包,并将压缩包移动到/usr/local目录下

解压命令: 

tar -xvf  jdk...

### 2.1配置jdk环境变量

- 进入jdk目录,pwd获取路径
- vim /etc/profile 按i进入编辑模式 到末尾
- JAVA_HOME=路径
- PATH=$JAVA_HOME/bin:$PATH
- CLASSPATH=.
- 按ESC 输入:wq
- 命令行输入resource /etc/profile是修改立即生效
- 执行java -version查询是否成功

### 2.2解压solr

tar -xvf solr...

### 2.3 启动solr

> 不建议使用管理员启动 solr,加 -force 强制启动

bin/solr start -force

> 开放 8983 端口

firewall-cmd --zone=public --add-port=8983/tcp --permanent firewall-cmd --reload

### 2.4浏览器访问 solr 控制台

> ip为虚拟机IP地址

[solr控制台](http://192.168.64.170:8983)

![image-20200309144816969](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200309144816969.png)

## 3.创建 core

> 数据库中 pd_item 表中的商品数据, 在 solr 中保存索引数据, 一类数据, 在 solr 中创建一个 core 保存索引数据,创建一个名为 pd 的 core, 首先要准备以下目录结构:一个core类似于数据库的表

```c
# solr目录/server/solr/
#                    pd/
#                     conf/
#                     data/
cd /usr/local/solr-8.1.1

mkdir server/solr/pd
mkdir server/solr/pd/conf
mkdir server/solr/pd/data

```



> conf 目录是 core 的配置目录, 存储一组配置文件, 我们以默认配置为基础, 后续逐步修改

```c
cd /usr/local/solr-8.1.1

cp -r server/solr/configsets/_default/conf server/solr/pd

```

## 4.创建名为 pd 的 core

![image-20200309150923435](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200309150923435.png)

## 5.中文分词测试

![image-20200309151006506](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200309151006506.png)



## 6. 中文分词工具 -  ik-analyzer

https://github.com/magese/ik-analyzer-solr

![image-20200309151453102](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200309151453102.png)

> 下载 ik-analyzer 分词 jar 文件,传到 `solr目录/server/solr-webapp/webapp/WEB-INF/lib`
>
> 为了后续操作方便,我们把后面用到的jar文件一同传到服务器,包括四个文件:
> ik-analyzer-8.1.0.jar
> mysql-connector-java-5.1.46.jar
> solr-dataimporthandler-8.1.1.jar
> solr-dataimporthandler-extras-8.1.1.jar
>
> 复制6个文件到 `solr目录/server/solr-webapp/webapp/WEB-INF/classes`

```c
# classes目录如果不存在,需要创建该目录
mkdir /usr/local/solr-8.1.1/server/solr-webapp/webapp/WEB-INF/classes

```

```c
这6个文件复制到 classes 目录下
resources/
    IKAnalyzer.cfg.xml
    ext.dic
    stopword.dic
    stopwords.txt
    ik.conf
    dynamicdic.txt

```

### 6.1 配置 managed-schema

> 修改 `solr目录/server/solr/pd/conf/managed-schema`,添加 ik-analyzer 分词器

```xml
<!-- ik分词器 -->
<fieldType name="text_ik" class="solr.TextField">
  <analyzer type="index">
      <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="false" conf="ik.conf"/>
      <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
  <analyzer type="query">
      <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="true" conf="ik.conf"/>
      <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
</fieldType>

```

### 6.2 重启 solr 服务

```c
cd /usr/local/solr-8.1.1

bin/solr restart -force

```

## 7.使用 ik-analyzer 对中文进行分词测试

> 填入以下文本, 选择使用 `text_ik` 分词器, 观察分词结果:
>
> Solr是一个高性能，采用Java5开发，基于Lucene的全文搜索服务器。同时对其进行了扩展，提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展并对查询性能进行了优化，并且提供了一个完善的功能管理界面，是一款非常优秀的全文搜索引擎。

![image-20200309151949527](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200309151949527.png)

## 8.设置停止词

> 上传停止词配置文件到 `solr目录/server/solr-webapp/webapp/WEB-INF/classes`

```c
stopword.dic
stopwords.txt
```

> 重启服务,观察分词结果中,停止词被忽略

```c
bin/solr restart -force
```

## 9.准备 mysql 数据库数据

> - 用 sqlyog 执行 pd.sql
> - 授予 root 用户 跨网络访问权限
>   注意: 此处设置的是远程登录的 root 用户,本机登录的 root 用户密码不变

```sql
grant all on *.* to 'root'@'%' identified by 'root';
```

> 随机修改30%的商品,让商品下架,以便后面做查询测试

```sql
UPDATE pd_item SET STATUS=0 WHERE RAND()<0.3
```



































































