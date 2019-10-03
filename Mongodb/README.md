# Mongodb 
## 1 安装与启动
### 1.1安装:  
安装在自定义盘符,eg 安装在D/Mongodb   
Mongodb目录下有Server目录 server/4.2 => 有bin & data & log  
1. Server  
  1. 4.2  
    1. bin
    2. data -> db
    3. log  
### 1.2 启动  
启动命令行  
cd bin目录下 输入 mongod -dbpath  `"D:\MongoDB\Server\4.2\data\db" `
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/mongoddbpath.png"></img>
出现以下界面 最后一行显示 waiting for connections on port 27017  
再开启一个终端  `D:\MongoDB\Server\4.2\bin>mongo`
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/mongo.png"></img>  
## 2 基本命令  
1. 查看存在数据库命令  
`show dbs`  
2. 查看数据库版本命令  
`db.version()` 
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/db.png"></img>  
3. 切换库 (如果没有就创建这个库)  
`use user`
4. 插入数据  
`db.user.insert({})`
5. 更新数据  
`db.user.updata({})`
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/dbuser.png"></img>  
6. 删除数据
`db.user.remove({})`
7. 删除整个user集合
`db.user.drop() `   
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/remove.png"></img>  
