本工具使用ansible playbook部署高可用的Elasticsearch集群。



## 使用方法：

### 一、准备资源

请按照inventory格式修改对应资源

```
[master]
10.10.90.100 hostname=es-master-01 role=master heap_size=16g
10.10.90.101 hostname=es-master-02 role=master heap_size=16g
10.10.90.102 hostname=es-master-03 role=master heap_size=16g
[client]
10.10.90.103 hostname=es-client-01 role=client heap_size=16g
10.10.90.104 hostname=es-client-02 role=client heap_size=16g
10.10.90.105 hostname=es-client-03 role=client heap_size=16g
[hot]
10.10.90.106 hostname=es-data-hot-01 role=data heap_size=32g
10.10.90.107 hostname=es-data-hot-02 role=data heap_size=32g
10.10.90.108 hostname=es-data-hot-03 role=data heap_size=32g
[hot:vars]
type=hot

[cold]
10.10.90.109 hostname=es-data-cold-01 role=data heap_size=32g
10.10.90.110 hostname=es-data-cold-02 role=data heap_size=32g
10.10.90.111 hostname=es-data-cold-03 role=data heap_size=32g
[cold:vars]
type=cold

[es:children]
master
client
hot
cold
```




### 二、使用方法

#### 2.1、安装ansible

在控制端机器执行以下命令安装ansible

```
yum -y install ansible
pip install netaddr
```



#### 2.2、部署集群

先执行格式化磁盘并挂载目录。如已经自行格式化磁盘并挂载，请跳过此步骤。

```
ansible-playbook fdisk.yml -i inventory -l data -e "disk=/dev/sdb dir=/data"
```
安装elasticsearch
```
ansible-playbook es.yml -i inventory
```

如启用了TLS，请使用以下命令为elasticsearch生成密码

自动生成密码

```
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto
```

手动指定密码

```
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```



#### 2.3、扩容master节点

扩容elasticsearch的master节点前，请确认{{ssl_dir}}目录中存在elasticsearch证书。

扩容时，请不要在inventory文件master组中保留旧服务器信息。

```

```

#### 4.4、扩容client节点

```

```

#### 4.5、扩容data节点

```

```



#### 4.5、替换集群证书

```

```



#### 4.6、升级elasticsearch版本

