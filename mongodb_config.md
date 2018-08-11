```
vi /etc/yum.repos.d/mongodb-org-3.4.repo
```
* 在里头编写
```
[mogodb-org]

name=MongoDB Repository

baseurl=http://mirrors.aliyun.com/mongodb/yum/redhat/7Server/mongodb-org/3.4/x86_64/

gpgcheck=0

enabled=1
```
```
yum install -y mongodb-org
```
* 开放远程连接
```
# 查看 ```mongodb``` 的信息
service mongod status
```
* 找到后进入该目录修改配置文件
```
vi mongod.conf
```
* 注释掉这个
```
# bindIp: 127.0.0.1
```
* 退出配置文件并重新启动
```
service mongod restart
```
* 然后最后就是如果假设在服务器上啦，就又也要配置安全组啊，是入方向的，端口号 ```27017``` 啊，授权对象还是 ```0.0.0.0/0```
* 配好之后就可以正常远程访问了，比如在 ```spring boot``` 服务里面添加连接服务器 ```mongodb``` 数据库
* 最后一步就是设置密码，为了安全起见嘛
```
# 进入Mongo
mongo

> use admin
# 给 admin 设置用户密码
> db.createUser({user: 'root', pwd: '123456', roles: ['root']})
# 验证 1未成功，0为失败
> db.auth('root', '123456')
# 当然，每个库都要设置密码的，刚刚是给root，现在切换到自己创建的库
> use yourdb
# 为这个库添加用户
> db.createUser({user:'username', pwd:'password',roles: [{role:'readWrite',db:'yourdb'}]})
> db.auth('username', 'password')
```
* 重新带参数后台启动 ```mongodb```
```
nohup mongo --auth
```
* 如果要在 spring boot 上配置 ```uri```
```
mongodb://username:password@ipAddress:27017/yourdb
```