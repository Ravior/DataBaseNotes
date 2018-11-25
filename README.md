# DataBaseNotes
记录一些关系型以及非关系型数据库的sql优化、查询、规范信息等等

# 关系型数据库建库建表规范
1.表达是与否概念的字段，必须使用is_xxx的方法命令，数据类型是unsigned tinyint(1表示是，0表示否)。<br/>
2.表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。<br/>
3.表名不使用复数名词。<br/>
4.表必备三字段，id，gmt_create，gmt_modified<br/>
  说明：其中id必为主键，类型为bigint unsigned、单表时自增、步长为1。gmt_create,gmt_modified的类型均为datetime类型，前者现在时表示主动创建，后者过去分词表示被动更新。<br/>
5.表的命名最好是加上"业务名称_表的作用"

--------------------------------------------------------------------------------------------------------------------------------------

# MongoDB数据库
mongodb聚合规范写法：<br/>
参考资料：https://www.jianshu.com/p/e60d5cfbeb35

JAVA 处理 Spring data mongodb 时区问题 :<br/>
参考资料：https://my.oschina.net/xiaominmin/blog/1861590

★★★★★<br/>
在SPRING DATA MONGODB中使用聚合group by统计查询<br/>
参考资料：https://blog.csdn.net/u010084868/article/details/52622938

MongoDB 之 aggregate $group 巧妙运用 group多个字段：<br/>
https://blog.csdn.net/molashaonian/article/details/79402430<br/>
参考模板如下：
>db.getCollection('device').aggregate([<br/>
　　{$project: {deviceId: "$deviceId",lon: "$lon", lat: "$lat"}},<br/>
　　{$group: {_id: {<br/>
　　　　deviceId: "$deviceId",<br/>
　　　　lon: "$lon",<br/>
　　　　lat: "$lat"<br/>
　　}, <br/>
　　total:{ $sum: 1}}},<br/>
　　{$project: {_id: "$_id.deviceId",lon: "$_id.lon", lat: "$_id.lat", total: "$total"}}<br/>
])

★★★★★<br/>
mongodb的多表联查与与后续的数据处理（最多两张表关联，而且无法根据关联的那张表里的字段查询）<br/>
参考资料：https://blog.csdn.net/DDKii/article/details/81504805<br/>
https://blog.csdn.net/qq_39489635/article/details/77720789<br/>
参考模板如下：
> db.getCollection('imsiDevice').aggregate([ <br/>
　　{$match: {_id: ObjectId("5b8a0a43d15f8246565de010")}}, <br/>
　　{$lookup:{from:"place", localField:"placeId", foreignField:"_id", as: "places"}}, <br/>
　　{$unwind:"$places"}, <br/>
　　{$project: {deviceName:"$deviceName" ,placeName:"$places.placeName", provinceCode:"$places.provinceCode", <br/>
　　cityCode:"$places.cityCode", areaCode:"$places.areaCode", detailAddress:"$places.detailAddress"}}, <br/>
　　{$sort: {upTime: -1}}，<br/>
　　{$limit: 2}
　])

Spring-Data-Mongodb中的project()的用法<br/>
https://blog.csdn.net/hotdust/article/details/52605990

mongodb实战第二版pdf下载：<br/>
http://www.roadjava.com/s/spsb/gjzl/2018/05/mongodbszdebpdfxz.html

mongoDB的Criteria查询：多表联合查询（效率很低）<br/>
https://blog.csdn.net/EidenRitto/article/details/78521337

MongoDB的劣势：<br/>
1. 不支持事务；最新版本4.0以上已支持事务 <br/>
2. 不支持超过3个表的关联；<br/>
3. 不支持外表字段作为查询条件；<br/>

NoSql数据库使用半年后在设计上面的一些心得:<br/>
心得：①尽可能把一次展示所需的必要数据都存储到一起，②Mongodb不支持事务，所以务必在设计的时候考虑到这一点。核心业务数据尽可能通过结构设计做到数据插入的一致性。<br/>
https://www.cnblogs.com/AllenDang/p/3507821.html<br/>

mysql与mongo相比，事务与约束性更强。在处理较为价值较高的数据时，关系型数据库有它天然的优势，然而任何一种业务情况下，关系型数据库都不是银弹策略。任何抛开业务来谈论技术，都是在耍流氓。那么在什么情况下去选择mongo呢？<br/>
1. 数据价值不是重点<br/>
2. 属性查询需求较少<br/>
3. 数据之间约束较少<br/>
https://blog.csdn.net/cfl20121314/article/details/50734559<br/>

MongoDB进阶模式设计<br/>
http://www.mongoing.com/mongodb-advanced-pattern-design<br/>

《MongoDB实战》心得：<br/>
基于文档的数据模型可以表示丰富的、多层次的数据结构。它经常用来处理无须多表关联的工作。

根据MongoDB内嵌子文档增删改(一对多):<br/>
https://blog.csdn.net/walle167/article/details/51281199

mongodb数据库连接池（java版）<br/>
https://www.cnblogs.com/dmir/p/4780544.html

mongodb for java基本查询<br/>
https://www.cnblogs.com/zhoulf/p/4571647.html<br/>
mongodb多条件查询<br/>
https://www.cnblogs.com/sa-dan/p/6836055.html

mongodb更新文档<br/>
https://blog.csdn.net/u013066244/article/details/73849730

mongodb数组查询，针对key: [value1, value2] <br/>
https://blog.csdn.net/leshami/article/details/55049891

mongodb中查询返回指定字段<br/>
https://blog.csdn.net/u012086400/article/details/78652919

用java实现mongodb中查询返回指定字段<br/>
https://blog.csdn.net/albert0707/article/details/54098164

mongodb集群连接<br/>
https://blog.csdn.net/truong/article/details/74521636

mongodb大数据量分页查询的效率问题<br/>
https://blog.csdn.net/zhu_tianwei/article/details/44465415

Spark与mongodb整合完整版本<br/>
https://cloud.tencent.com/developer/article/1032526

Spark连接mongodb集群<br/>
https://piaosanlang.gitbooks.io/mongodb/content/03day/section99.html

Spark集成Mongodb的API<br/>
https://blog.csdn.net/txbsw/article/details/83377005

MongoDB下根据内嵌数组大小查询<br/>
https://blog.csdn.net/gao36951/article/details/40678875<br/>

Java MongoDB下根据数组大小进行查询的方法<br/>
https://blog.csdn.net/gao36951/article/details/40678875

mongodb find复杂条件查询 (or与and)<br/>
https://blog.csdn.net/tjbsl/article/details/80620303

mongodbexport 与 mongodbimport数据导入导出<br/>
https://www.cnblogs.com/qingtianyu2015/p/5968400.html

mongo中获取内嵌数组的长度<br/>
https://blog.csdn.net/luoduyu/article/details/78288258

MongoDB数据库某个字段求和<br/>
https://blog.csdn.net/andywa007/article/details/74950320

MongoDB数据库报错：<br/>
>com.mongodb.MongoQueryException: Query failed with error code 133 and error message 'Could not find host matching read preference { mode: "primary" } for set data' on server 192.168.31.230:39017<br/>
貌似是无法连接mongodb 的primary主服务器。<br/>

https://www.oschina.net/question/1428558_2159367

Spark从MongoDB库中抓取数据指定分区类型和分区大小<br/>
http://www.th7.cn/db/nosql/201706/242836.shtml<br/>
https://docs.mongodb.com/spark-connector/current/configuration/#cache-configuration

mongoDB 大数据量排序索引<br/>
http://orange2008.iteye.com/blog/1754168

mongoDB索引创建<br/>
http://blog.51cto.com/chenql/2071267

### MongoDB索引管理<br/>
创建索引： db.users.createIndex({name:1})<br/>
查看索引： getIndexes()方法可以用来查看集合的所有索引<br/>
http://blog.51cto.com/chenql/2071267

### mongoDB报“com.mongodb.MongoCursorNotFoundException，error code -5”
https://blog.csdn.net/zh0u_f/article/details/72897628<br/>
http://www.cnblogs.com/gaoze/p/7212436.html<br/>
https://blog.csdn.net/demon_LL/article/details/56843700?locationNum=12&fps=1<br/>

### mongodb 对 内嵌数据添加多个数据操作
https://blog.csdn.net/qq_20127333/article/details/51508863<br/>
https://blog.csdn.net/niclascage/article/details/47009989<br/>
https://blog.csdn.net/dream20nn/article/details/51306228?utm_source=blogxgwz1

### mongodb 根据条件删除
>db.getCollection('collisionTask').remove({_id:""})

### mongodb 性能测试
http://www.cnblogs.com/lovecindywang/archive/2011/03/02/1969324.htm

### mongodb 监控工具
开源的Ganglia、Zabbix、cacti、nagos，mongodb自带的mongostats/mongotop，MongoDB Monitoring Service<br/>
https://www.cnblogs.com/hy007x/p/7736403.html<br/>
https://www.cnblogs.com/ahaii/p/7146290.html?utm_source=itdadao&utm_medium=referral<br/>
https://blog.csdn.net/jianlong727/article/details/54586855<br/>

--------------------------------------------------------------------------------------------------------------------------------------

# HBase数据库
rowkey设计：<br/>
常用的rowkey设计为：salt (1 byte) + attr1_id (4 bytes) + timestamp (4 bytes) + attr2_id (4 bytes) <br/>
https://blog.csdn.net/chengyuqiang/article/details/79134549

rowkey避免热点：<br/>
https://my.oschina.net/lanzp/blog/477732

HBase二级索引：<br/>
https://blog.csdn.net/WYpersist/article/details/79830811<br/>
https://www.jianshu.com/p/0ccd187910e5
