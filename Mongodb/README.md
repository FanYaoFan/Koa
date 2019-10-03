# Mongodb 
## 1 安装与启动
### 1.1安装:  
安装在自定义盘符,eg 安装在D/Mongodb   
Mongodb目录下有Server目录 server/4.2 => 有bin & data & log  
目录结构  
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
8. 创建集合
`db.createCollection("user")`
9. 删除数据库 
`db.dropDatabase()`  
## 3 JS与mongodb 
### 3.1 批量增加
实际开发中,一般是先声明一个数组,然后循环产生数据push到这个数组中,在db.集合.inert插入数据.性能远远大于每次循环插入数据 如图  
### 3.2 update修改器  
#### 3.2.1 $set&$unset  
$set用来指定一个键值(key),进行修改  如图 set.png
$unset 指定key值进行删除  
#### 3.2.2 $inc 对数字进行计算  
如图 inc
#### 3.2.3 multi 
其值又true和false两个值,true代表全部修改,false代表只修改一个(默认值)  
想为每一个数据添加一个属性, `db.集合.update( {}, {$set : {hobby : []}})`这样写只添加一个  
`db.集合.update( {}, {$set : {hobby : []}}, {multi : true})` 这样每个数据就都添加了hobby这条数据
如图 multi  
#### 3.2.4 upsert选项
upsert是在找不到这个值的情况下,直接插入这条数据
### 3.3 update数组修改器
#### 3.3.1 $push 
$push的功能是追加数组中的值，但我们也经常用它操作内嵌稳文档，就是{}对象型的值。  
`db.集合.update({name:'3熊'},{$push:{"hobby":'draw'}})`
#### 3.3.2 $pop
$pop只删除一次,并不是删除所有数组的值,而且它有两个选项 +1和-1
1. +1 从数组末端删除
2. -1 从数组开端删除  
`db.集合.update({name:'3熊'},{$pop:{"hobby": 1}}`
#### 3.3.3 $ne & $addToSet
$ne   
检查一个值是否存在,如果不存在再执行操作.他的爱好没有打游戏就添加打游戏这条数据
`db.集合.update({num:0,"hobby":{$ne:'playGame'}},{$push:{"hobby":'playGame'}})`  
$addToSet 
操作比ne更直观和方便
 `db.集合.update({num:0,"hobby":{$addToSet:{"hobby":'Game'}})`  
#### 3.3.4 $each
它可以传入一个数组，一次增加多个值进去，相当于批量操作，性能同样比循环操作要好很多，这个是需要我们注意的，工作中也要先组合成数组，然后用批量的形式进行操作。  
比如一次性给 2熊这个人 添加 四个爱好 [ 唱,跳,rap,打篮球]  
`var hobbyList = [ "sing", "Dance" ,"rap", "basketball"]`  
`db.集合.updata ( {"name": "2熊"}, {$addToSet : {hobby : [$each : hobbyList]}})`
## 4 findAndModify  
在操作数据库时，对数据的修改是需要有足够的安全措施的，其实在实际工作中，我们用db.collections.update的时候不多，在修改时我们都会用findAndModify(修改)，它可以给我们返回来一些必要的参数，让我们对修改多了很多控制力，控制力的加强也就是对安全的强化能力加强了。   
### 4.1 常用属性    
•	query：需要查询的条件/文档  
•	sort: 进行排序  
•	remove：[boolean]是否删除查找到的文档，值true，可以删除。  
•	new:[boolean]返回更新前的文档还是更新后的文档。  
•	fields：需要返回的字段  
•	upsert：没有这个值是否增加  

### 4.2 db.runCommand()
它是数据库运行命令的执行器，执行命令首选就要使用它，因为它在Shell和驱动程序间提供了一致的接口。（几乎操作数据库的所有操作，都可以使用runCommand来执行） 
`db.集合.update({sex:1},{$set:{money:1000}},false,true)`  
`var resultMessage=db.runCommand({getLastError:1})`  
`printjson(resultMessage);`    
false：第一句末尾的false是upsert的简写，代表没有此条数据时不增加;  
true：true是multi的简写，代表修改所有    
getLastError:1 :表示返回功能错误，这里的参数很多  
printjson：表示以json对象的格式输出到控制台  
## 5 不等修饰符 
1. 小于($lt):英文全称less-than
2. 小于等于($lte)：英文全称less-than-equal
3. 大于($gt):英文全称greater-than
4. 大于等于($gte):英文全称greater-than-equal
5. 不等于($ne):英文全称not-equal 
