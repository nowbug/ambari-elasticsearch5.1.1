# ambari-elasticsearch5.1.1


Ambari Elasticsearch服务是Ambari的自定义服务，允许您通过Ambari安装和管理Elasticsearch。此服务作为社区项目提供，不受Hortonworks支持。此外，此服务旨在用于测试和开发，不应在生产环境中使用。此服务适用于Ambari 2.4.x和Elasticsearch 5.x.

# 系统要求

在部署Ambari Elasticsearch服务之前，Elasticsearch需要特定的操作系统配置更改才能正常运行。

将Elasticsearch配置为绑定到非回送地址时，Elasticsearch将执行称为“引导程序检查”的其他系统检查。如果这些引导检查失败，Elasticsearch将关闭。您可以在这里阅读更多关于这些检查：https : //www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
### /etc/security/limits.conf
修改/etc/security/limits.conf以包含以下设置：


```
elasticsearch    -       nofile         65536
elasticsearch    -       nproc          2048
elasticsearch    -       memlock        unlimited
```

### /etc/sysctl.conf中

```
# Controls mmap counts
vm.max_map_count = 262144
```
### /etc/sudoers (最好用visudo命令)

注释掉 
```
#Default requiretty
```
 一行并wq!强制保存

### /var/run/ambari-server

```
sudo chown -R ambari /var/run/ambari-server
```

ulimit -n 65536
ulimit -a

##### sysctl -p   可能需要刷新生效

你可以在这里阅读更多关于Elasticsearch配置设置：https：//www.elastic.co/guide/en/elasticsearch/reference/current/system-config.html

应该对您计划部署Elasticsearch的任何节点进行这些更改。更改完成后，重启服务器以确保更改生效是个不错的主意。
# QAQ
如果你安装了es 发现一切顺利，但是并没有日志我猜你多半是在测试集群或者虚拟机那么你可能需要注意jvm的默认参数的内存，这个不应该大于你的实际内存，否则es将不会真正的运行，当然也不会有错误日志写在你设想的/log 目录中

# 安装

放在指定的目录并重启你的ambari服务

```
sudo service ambari-server restart
```

一旦Ambari服务器服务重新启动，您应该看到Elasticsearch作为可用的服务从“添加服务”屏幕进行安装。

# 兼容性
****该服务已经过以下测试**：**

- CentOS 7.2
- Ambari 2.4.2.0
- HDP 2.5.3.0
- Elasticsearch 5.1.1

#### 限制
---
目前适用以下限制：

- 该服务当前将所有节点作为master = true，data = true和ingest = true进行部署。
- 该服务仅在CentOS / RHEL 6.x 7.x上进行过测试。
- 该服务可以添加自定义配置
- 快速链接目前不工作。
- 该服务目前不支持Kerberos。
- 该服务目前没有Ambari Service Advisor或Ambari Alert功能。

#### 参考GitHub
1》https://github.com/Jaraxal/ambari-elasticsearch-service

- CentOS 6.x
- Ambari 2.4.2.0
- HDP 2.5.3.0
- Elasticsearch 5.x
- [x] 最后一次更新在8个月前   2017年 2月份


2》https://github.com/winfys/ambari-elasticsearch-service
- CentOS 6.x
- Ambari 2.5.1.0
- HDP 2.6.1.0
- Elasticsearch 5.4.1
- [x] 最后一次更新在5个月前   2017年 6月份

3》https://github.com/jcl10086/ambari-elasticsearch5.x
- 参考1号
- 有问题请发送邮件到作者邮箱：1427682825@qq.com
- [x] 最后一次更新在三个月前   2017年 8月份


4、https://github.com/teamsoo/elasticsearch-ambari
CentOS 7.x
Ambari 2.4.1.0
HDP 2.5.0.0-1245
至少需要3个数据节点*
暂时不支持es 5.x  只支持2.x


