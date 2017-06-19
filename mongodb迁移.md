>本文主要讲解如何给mongodb增加身份认证，以及数据库的迁移

>以下操作仅在mongodb3.4.4验证

## 增加用户与身份认证

 1 添加管理员
 
 ```
 #进入命令行
 mongo
 
 #创建第一个用户，该用户需要有用户管理权限
 #这里设置其角色为root
 use admin
 db.createUser({user:"admin",pwd:"password",roles:["root"]})
 
 #第一个用户添加完成后，便需要认证才能继续添加其他用户
 db.auth("admin", "password")
 
 #显示1表示认证通过
 
 
 ```
 
 2 添加数据库普通用户
 
 给数据库meizi添加一个读写权限的用户
 
 ```
 mongo
 
 use meizi
 
 db.createUser({user:"user",pwd:"pws",roles:[{role:"readWrite",db:"meizi"}]})
 
 #成功后显示
 Successfully added user: {
    "user" : "user",
    "roles" : [
        {
            "role" : "readWrite",
            "db" : "meizi"
        }
    ]
}
 
 ```
 
 这样远程连接meizi数据库时，就需要携带账号密码了
 
 3 mongoose中使用身份认证
 
 ```
 mongodb://你的账号:密码@host ip:端口号/设置登录权限的数据库
 
 ```
 
 4 mongodb 开启强制认证
 
 打开`etc/mongod.conf`文件 ，在`#security`行修改为

```
security:
   authorization: enabled
   
```
然后重启mongo
 

## 数据库的导入导出

> 一种是针对于库的mongodump和mongorestore，一种是针对库中表的mongoexport和mongoimport。
>因为我们是整库的导入导出，所以我们使用mongodump mongoresstore


### mongodump备份数据库

1. 常用命令格式

```
mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -o 文件存在路径
```

2. 导出数据库meizi到桌面的dbsback目录

```
mongodump -h 127.0.0.1 --port 27017 -d meizi -o ~/Desktop/dbsback
```

###  mongorestore还原数据库

1.常用命令格式

```
mongorestore -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 --drop 文件存在路径
```

2. 还原数据库meizi到远程vps数据库

```
mongorestore -h ip --port 27017 -d meizi --drop ~/Desktop/dbsback/meizi
```

执行过程如下：

```
2017-06-05T13:40:34.306+0800	the --db and --collection args should only be used when restoring from a BSON file. Other uses are deprecated and will not exist in the future; use --nsInclude instead
2017-06-05T13:40:34.307+0800	building a list of collections to restore from /Users/cly/Desktop/dbsback/meizi dir
2017-06-05T13:40:34.607+0800	reading metadata for meizi.homes from /Users/cly/Desktop/dbsback/meizi/homes.metadata.json
2017-06-05T13:40:34.914+0800	reading metadata for meizi.details from /Users/cly/Desktop/dbsback/meizi/details.metadata.json
2017-06-05T13:40:35.221+0800	restoring meizi.details from /Users/cly/Desktop/dbsback/meizi/details.bson
2017-06-05T13:40:36.262+0800	restoring meizi.homes from /Users/cly/Desktop/dbsback/meizi/homes.bson
2017-06-05T13:40:36.453+0800	[#########...............]  meizi.details   727KB/1.74MB   (40.9%)
2017-06-05T13:40:36.453+0800	[########################]    meizi.homes  5.75KB/5.75KB  (100.0%)
2017-06-05T13:40:36.453+0800
2017-06-05T13:40:36.839+0800	[########################]  meizi.homes  5.75KB/5.75KB  (100.0%)
2017-06-05T13:40:36.839+0800	restoring indexes for collection meizi.homes from metadata
2017-06-05T13:40:37.156+0800	finished restoring meizi.homes (20 documents)
2017-06-05T13:40:39.452+0800	[#########...............]  meizi.details  727KB/1.74MB  (40.9%)
2017-06-05T13:40:42.451+0800	[#########...............]  meizi.details  727KB/1.74MB  (40.9%)
2017-06-05T13:40:45.450+0800	[#########...............]  meizi.details  727KB/1.74MB  (40.9%)
2017-06-05T13:40:48.452+0800	[#########...............]  meizi.details  727KB/1.74MB  (40.9%)
2017-06-05T13:40:51.453+0800	[#########...............]  meizi.details  727KB/1.74MB  (40.9%)
2017-06-05T13:40:54.453+0800	[################........]  meizi.details  1.16MB/1.74MB  (66.8%)
2017-06-05T13:40:57.452+0800	[########################]  meizi.details  1.74MB/1.74MB  (100.0%)
2017-06-05T13:40:58.097+0800	[########################]  meizi.details  1.74MB/1.74MB  (100.0%)
2017-06-05T13:40:58.097+0800	restoring indexes for collection meizi.details from metadata
2017-06-05T13:40:58.569+0800	finished restoring meizi.details (2734 documents)
2017-06-05T13:40:58.569+0800	done
 cly@hammer  ~/Desktop/dbsback  mongorestore -h 45.32.170.186 --port 27017 -d javbus --drop ~/Desktop/dbsback/javbus
2017-06-05T13:43:23.649+0800	the --db and --collection args should only be used when restoring from a BSON file. Other uses are deprecated and will not exist in the future; use --nsInclude instead
2017-0
```

## 远程保活

启动命令时：

```
nohup npm run start &

```

查看已经启动的node命令

```
ps -ef | grep node
```

查看端口号所占的pid
```
netstat -pan | grep 9010
```

查看当前系统状态

```
top
```
