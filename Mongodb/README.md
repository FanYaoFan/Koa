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
10. 保存  
`db.集合.save(document)`  
11. limit  
用于读取指定数量的文档,
`db.集合.find().limit(number)`    
number表示要获取文档的条数,如果没有指定参数则显示集合中的所有文档  
12. skip    
用于跳过指定数量的文档    
`db.集合.find().skip(2)` 
如果文档_id已经存在则修改,如果文档_id不存在则添加 
13. 投影   
在查询到的返回结果中,只选择必要的字段,而不是选择一个文档的整个字段  
一个文档有5个字段,需要显示的只有3个,投影其中3个字段即可  
`db.集合名称.find( {}, {字段名称: 1,...})`  
对于需要显示的字段,设置为1即可,不设置就不显示(id列默认显示,不显示需要设置为0)  
eg `db.stu.find( {},{name:1,gender:1})`  
`db.stu.find({},{_id:0,name:1,gender:1})`  
14. 统计个数 count  
用于统计结果集中文档条数  
`db.集合.find({条件}).count()`  
15. 排序 sort()    
`db.集合.find().sort({字段: 1/-1})`  
1为升序,-1为降序
16. 消除重复    
`db.集合名称.distinct('去重字段',{条件})`  
## 3 JS与mongodb 
### 3.1 批量增加
实际开发中,一般是先声明一个数组,然后循环产生数据push到这个数组中,在db.集合.inert插入数据.性能远远大于每次循环插入数据 
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/for-insert.png"></img>  
### 3.2 update修改器  
#### 3.2.1 $set&$unset  
$set用来指定一个键值(key),进行修改      
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/set.png"></img>  
$unset 指定key值进行删除    
#### 3.2.2 $inc 对数字进行计算  
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/inc.png"></img>  
#### 3.2.3 multi 
其值又true和false两个值,true代表全部修改,false代表只修改一个(默认值)  
想为每一个数据添加一个属性, `db.集合.update( {}, {$set : {hobby : []}})`这样写只添加一个  
`db.集合.update( {}, {$set : {hobby : []}}, {multi : true})` 这样每个数据就都添加了hobby这条数据
如图 multi  
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/multi.png"></img>
#### 3.2.4 upsert选项
upsert是在找不到这个值的情况下,直接插入这条数据
### 3.3 update数组修改器
#### 3.3.1 $push 
$push的功能是追加数组中的值，但我们也经常用它操作内嵌稳文档，就是{}对象型的值。     
`db.集合.update({name:'3熊'},{$push:{"hobby":'draw'}})`  
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/push.png"></img>
#### 3.3.2 $pop
$pop只删除一次,并不是删除所有数组的值,而且它有两个选项 +1和-1
1. +1 从数组末端删除
2. -1 从数组开端删除  
`db.集合.update({name:'3熊'},{$pop:{"hobby": 1}}`  
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/pop.png"></img>
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
## 6 数据类型 
* Object ID : 文档ID  
* String: 字符串,最常用,必须是有效的UTF-8  
* Boolean: 存储一个布尔值 true/false  
* Integer: 整数可以是32/64,取决于服务器  
* Double: 存储浮点值  
* Arrays:数组或列表,多个值存储到一个键  
* Object:用于嵌入式文档,即一个值为一个文档  
* Null: 存储null值  
* Timestamp:时间戳  
* Date: 存储当前日期或时间的UNIX时间格式  
Object  
每个文档都有一个属性,为_id,保证每个文档的唯一性  
可以自己设置_id插入文档;如果没有提供,那么MongoDB会为每个文档提供一个独特的_id,类型为ObjectID.  
组成   
object是一个12字节的十六进制数,
1. 前四个字节为当前时间戳,
2. 接下来3个字节的机器ID,
3. 接下来的2个字节中MongoDB的服务进程id,
4. 最后3个简单的增利值
## 7 高级操作  aggregate 
聚合(aggregate)主要用于计算数据,类似sql中的sum(),avg()  
`db.集合.aggregate([{管道: {表达式}])`  
管道在unix/linux中一般用于当前命令的输出结果作为下一个命令的输入  
ajx|grep mongo  
### 7.1 常用管道  
1. $group: 将集合中的文档分组,可用于统计结果
` db.stu.aggreate( [ $group : {_id : "name"}])`  
<img src="https://github.com/FanYaoFan/Koa/blob/master/Mongodb/img/advance/group.png"></img>  
2. $match: 过滤数据,只输出符合条件的文档
`db.stu.aggregate([ {$match : {age : {$gt : 20 }}}])`   
//=> 查询年龄大于20
3. $project: 修改输入文档的结构,eg 重命名,增加,删除字段,创建计算结果
`db.stu.aggregate([{$project : {_id : 0,name: 1,age : 1}}])`  
查询学生的姓名,年龄  
4. $sort: 将文档排序后输出
5. $limit: 限制聚合管道返回的文档数
6. $skip: 跳过指定数量的文档,并返回余下的文档
7. $unwind : 将数组类型的字段进行拆分
### 7.1.1 $group 
将集合中的文档分组,可用于统计结果 
_id表示分组的依据,使用某个字段的格式为$字段
### 7.2 表达式  
$sum : 计算总和 $sum1同count表示计数
$avg : 计算平均值
$min : 获取最小值  
$max : 获取最大值
$push : 在结果文档中插入值到一个数组中
$first : 根据资源文档的排序获取第一个文档数据 
$last : 根据资源文档的排序获取最后一个文档数据  
## 8 索引  
1. 查询数据超过表数据量30%时，不要使用索引字段查询。实际证明会比不使用索引更慢，因为它大量检索了索引表和我们原表。
2. 数字索引，要比字符串索引快的多，在百万级甚至千万级数据量面前，使用数字索引是个明确的选择
3. 把你经常查询的数据做成一个内嵌数据（对象型的数据），然后集体进行索引  
4. 查看状态 `db.集合.status()`  
### 8.1 建立索引  
`db.集合.ensureIndex( {username : 1})`   
1表示升序 -1表示降序
查看现有索引  
`db.集合.getIndexes()` 
建立唯一索引,实现唯一的约束功能  
db.集合,ensureIndex({"name" : 1},{"uique" : true})
### 8.2 复合索引  
指定索引查询(hint)  
数字的索引要比字符串的索引快
### 8.3 删除索引 
当索引性能不佳或起不到作用时,我们需要删除索引
`db.randomInfo.dropoIndex('ranNum0_1)`索引的唯一ID  
### 8.4 全文索引  
有些时候需要在大篇幅的文章中搜索关键词，比如我写的文章每篇都在万字以上，这时候你想搜索关键字是非常不容易的  
1. $text:表示要在全文索引中查东西。  
2. $search:后边跟查找的内容  
`db.info.find( {$text: {$search : "查找的内容"})`  
## 9 admin  
为了更安全的访问mongodb,需要访问者提供用户名和密码,于是需要在mongodb中创建用户.  
采用了角色-用户-数据库的安全管理方式  
创建管理员   
use admin   
`db.createUser({user: 'admin',pws : '123',roles : [{role : 'root', db :'admi'}]})`
### 9.1 角色  
__常用系统角色__   
1. root 只在admin数据中可用,超级账号,超级权限  
2. Read 允许用户读取指定的数据库  
3. readWrite:允许用户读写指定数据库  
use admin -> show collections 默认是只有一个集合 (system.version).创建用户可以用db.createUser   
`db.ceateUser({ user :"fan", pwd : "123456", cunstomData Z:{ name : 'big', email : '123456@qq.com', age : 18 }})`  
__内置角色__  
1.	数据库用户角色：read、readWrite；
2.	数据库管理角色：dbAdmin、dbOwner、userAdmin;
3.	集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManage；
4.	备份恢复角色：backup、restore；
5.	所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
6.	超级用户角色：root
7.	内部角色：__system
### 9.2 用户操作  
`db.system.users.find()` //查找用户  
`db.system.users.remove({user:"fan"}) `//删除用户  
## 10 建权 
重启MongoDB服务器，然后设置必须使用建权登录。
### 10.1  
`mongod --auth`  
启动后，用户登录只能用用户名和密码进行登录，原来的mongo形式链接已经不起作用了。相应的用户权限也对应妥当。实际项目中我们启动服务器必须使用建权形式  
登录  
如果在配置用户之后，用户想登录，可以使用mongo的形式，不过需要配置用户名密码：
`mongom  -u fan -p 123456 127.0.0.1:27017/admin`   
### 10.2 自启动服务 
如图

net start MongoDB 启动服务
然后  D:\MongoDB\Server\4.2\data\db> mongo  直接进入mongodb 不用再输老长的命令了   
## 11 管理 : 备份和还原
### 11.1 数据备份  
 ` mongodump`
*     --host 127.0.0.1  
*     --port 27017  
*     --out D:/databack/backup  
*     --collection myCollections  
*     --db test  
*     --username username  
*     --password password  
*  比如现在我们备份所有MongoDB里的库到D盘的databack文件夹下，
*  就可以把命令写成这样
* ` mongodump --host 127.0.0.1 --port 27017 --out D:/databack/`
### 11.2 数据恢复  
 `mongorestore`
* 数据恢复  
* mongorestore  
      --host 127.0.0.1  
*     --port 27017  
*     --username username   
*     --password password    
*     <path to the backup>    
* 比如我们现在不小心删除了一个collections的数据，要进行恢复。比如删除了students集合。   
* eg: db.students.drop() 不小心删除   
* 数据恢复 :  mongorestore --host 127.0.0.1 --port 27017 D:/databack/



