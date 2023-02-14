-------------------------------------------------

			# MONGODB-WORK #

-------------------------------------------------

########################################################
### MONGODB

https://www.mongodb.com

  https://www.mongodb.com/try/download/community
  https://www.mongodb.com/try/download/shell
  https://www.mongodb.com/try/download/database-tools
  https://www.mongodb.com/products/compass
  https://www.mongodb.com/products/vs-code

- Tutorial 
https://www.mongodb.com/docs/manual/
https://www.mongodb.com/docs/manual/tutorial/
https://learn.mongodb.com/learning-paths/introduction-to-mongodb
https://www.mongodbtutorial.org/

- Sample Data 
https://github.com/neelabalan/mongodb-sample-dataset

########################################################
# Install 
https://www.mongodb.com/docs/manual/installation/

https://www.mongodb.com/try/download/community
https://www.mongodb.com/try/download/shell
https://www.mongodb.com/try/download/database-tools

실행 패스 설정
export PATH="$PATH:/설치경로/mongodb-macos-x86_64-6.0.4/bin"
cd /설치경로/mongodb-macos-x86_64-6.0.4/bin
vi startmongo.sh
설치경로/mongodb-macos-x86_64-6.0.4/bin/mongod --dbpath /설치경로/mongodb-macos-x86_64-6.0.4/data/db
export PATH="$PATH:/설치경로/mongosh-1.6.2-darwin-x64/bin"
export PATH="$PATH:/설치경로/mongodb-database-tools-macos-x86_64-100.6.1/bin"

########################################################
# Baisc 
https://www.mongodb.com/docs/manual/introduction/
https://www.mongodb.com/docs/mongodb-shell/

1. 문서 -> json 형태 
2.컬렉션 (스키마 X, 서브 컬렉션) -> 문서의 모음
3.데이터베이스 -> 컬렉션을 모아두는 것 
4.MongoDB start -> ./mongod --dbpath [[data_directory_path]]
ex) ./mongod --dbpath /Users/devsunset/dev/program/mongodb-macos-x86_64-6.0.4/data/db
5.MongoDB Shell  -> ./mongosh mongodb://localhost:27017
6.MongoDB Database-tools 
7.MongoDB Compass
8.MongoDB for VS Code

$ ./mongosh mongodb://localhost:27017
test> db 
test

* Create 
test> 
test> db.board.insert(post)
test> post = {"title" : "post2", "content" : "post content2", "date" : new Date()}
test> db.board.insert(post)
* Read
test> db.board.find()
test> db.board.findOne()
* Update 
test> post.comments = []
text> db.board.insert(post)
* Delete 
test> db.board.remove({title : "post2"})

test> help 
test> db.help
test> db.board.help

test> db.version 
test> db.getCollection("version")

* MongoDB Data Types
https://www.mongodb.com/docs/mongodb-shell/reference/data-types/

########################################################
# CUD
https://www.mongodb.com/docs/manual/crud/

* Create
https://www.mongodb.com/docs/manual/reference/method/db.collection.insert/
db.board.insert({"title":"post test","content":"content test","date": new Date()})

test> for(i=0 ; i<1000; i++){
... db.board.insert({"title": "post_"+i,"content":"content_"+i,"date":new Date()})
... }

* Update 
https://www.mongodb.com/docs/manual/reference/method/db.collection.update/
문서치환
제한자 사용
- $set
- $inc (증가와 감소)
- 배열 제한자 
	- $push
	- $ne 
	- $addToSet
	- $each
	- $pop
	- $pull

test> var data = db.board.findOne({"title":"post_1"})
test> print(data)
{
  _id: ObjectId("63e74d6879bcc937c523e725"),
  title: 'post_1',
  content: 'content_1',
  date: ISODate("2023-02-11T08:10:16.760Z")
}
test> data.content = 'content update test'
test> delete data.date
test> print(data)
{
  _id: ObjectId("63e74d6879bcc937c523e725"),
  title: 'post_1',
  content: 'content update test',
  date: ISODate("2023-02-11T08:10:16.760Z")
}
test> db.board.update({"title":"post_1"},{$set:data},{upsert:true})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

* Delete
https://www.mongodb.com/docs/manual/reference/method/db.collection.remove/
test> db.board.remove({"title":"post_0"})
test> db.board.drop()

########################################################
# 조회 
https://www.mongodb.com/docs/manual/crud/

use sample_mflix

* db.collection.find()

db.movies.find()
db.movies.findOne()
db.movies.find({"runtime":65})
db.movies.find({"title":"Gourmet Club"})
db.movies.find({"runtime":65,"title":"Gourmet Club"})
db.movies.find({},{"runtime":1, "title":1})
db.movies.find({},{"runtime":1, "title":1, "_id":0})

* 조건
$lt <
$lte <=
$gt >
$gte >= 
&ne !=
$in 
$nin
$or
$not
$mod
$exists
정규표현식
배열 $all $size
$slice
$elemMatch
$where 
limit
skip
sort
cursor
$maxscan
$min 
$max
$hint 
$explain 
$snapshot 

db.movies.find({"runtime" : {"$gte": 60, "$lte" : 90}})
released_date = new Date("01/01/1996")
db.movies.find({"released": {"$lt":released_date}})
db.movies.find({"runtime" : {"$ne" : "60"}})
db.movies.find({"type": {"$in": ["movie","series"]}})
db.movies.find({"type": {"$nin": ["movie"]}})
db.movies.find({"$or": [{"type":"movie"}, {"year": 1918}]})
db.movies.find({"$or": [{"type":{"$in": ["movie","series"]}}, {"year": 1918}]})
db.movies.find({"year": {"$mod": [5,1]}})
db.movies.find({"year": {"$not": {"$mod": [5,1]}}})
db.movies.find({"year": null})
db.movies.find({"month": null})
db.movies.find({"year": {"$in":[null],"$exists" : true}})
db.movies.find({"title": /Blue/i})
db.movies.find({"title": /Blue?/i})
db.movies.find({"countries" : "Italy"},{"countries":1)
db.movies.find({"countries" : {$all : ["Italy","Spain"]}},{"countries":1)
db.movies.find({"countries.1" : "Italy"},{"countries":1})
db.movies.find({"countries" : {"$size":3}},{"countries":1})
db.movies.find({}, {"cast":{"$slice":1}})
db.movies.find({}, {"cast":{"$slice":-1}})
db.movies.find({}, {"cast":{"$slice":[1,2]}})
db.movies.find({"tomatoes.viewer.rating":3.6})
db.movies.find({"$where": "this.type+this.year == 'movie1918'"})
db.movies.find().limit(3)
db.movies.find().skip(3)
db.movies.find().sort({"year":1})
db.movies.find().sort({"year":-1})
var cursor = db.movies.find();
cursor.forEach(function(x){
  print(x.title);
})
var cursor = db.movies.find().sort({"released":1}).limit(1).skip(10);
var cursor = db.movies.find().limit(1).sort({"released":1}).skip(10);
var cursor = db.movies.find().skip(10).sort({"released":1}).limit(1);
cursor.forEach(function(x){
  print(x.title);
})

########################################################
# 색인 
https://www.mongodb.com/docs/manual/indexes/

use sample_mflix

* ensureIndex & dropIndexes
db.movies.find({"title":"The Blue Bird"})
db.movies.ensureIndex({"title":1})
db.movies.ensureIndex({"title":1}, {"background": true})
db.movies.ensureIndex({"released":1,"title":1})
db.movies.ensureIndex({"tomatoes.dvd":1}) 
db.movies.ensureIndex({"type":1,"year":1},{"name":"index_test"})
db.movies.ensureIndex({"title":1},{"unique": true})
db.movies.ensureIndex({"title":1},{"unique": true, "dropDups" : true})
db.movies.ensureIndex({"title":1,"lastupdated":1},{"unique": true})
db.movies.dropIndexes({"type":1,"year":1},{"name":"index_test"})

* explain & hint 
db.movies.find().explain()
db.movies.find({"title":"The Blue Bird"}).explain()
db.movies.find({"year":1978, "title":"/.*/"}).explain()
db.movies.find({"year":1978, "title":"/.*/"}).hint({"title":1}).explain()

########################################################
# 집계
https://www.mongodb.com/docs/manual/aggregation/

use sample_mflix

db.movies.count()
db.movies.count({"type":"movie"})
db.runCommand({"distinct" : "movies","key":"type"})
db.movies.aggregate( [ { $group : { _id : "$year" } } ] )
db.movies.aggregate([{"$group" : {_id:{year:"$year",type:"$type"}, count:{$sum:1}}}, {$sort:{"count":-1}}])

* Map Reduce
https://coding-start.tistory.com/293

> db.collection.mapReduce(
    <map>
    ,<reduce>
    {
      out: <collection>
      ,query:<document>
      ,sort:<document>
      ,limit:<number>
      ,finalize:<function>
      ,scope:<document>
      ,jsMode:<boolean>
      ,verbose:<boolean>
    }
  )

> db.orders.insertMany([{
...      cust_id: "abc123",
...      ord_date: new Date("Oct 04, 2012"),
...      status: 'A',
...      price: 40,
...      items: [ { sku: "mmm", qty: 5, price: 2.5 },
...               { sku: "nnn", qty: 5, price: 2.5 } ]
... },
... {
...      cust_id: "abc123",
...      ord_date: new Date("Oct 04, 2012"),
...      status: 'A',
...      price: 25,
...      items: [ { sku: "mmm", qty: 5, price: 2.5 },
...               { sku: "nnn", qty: 5, price: 2.5 } ]
... },
... {
...      cust_id: "abc123",
...      ord_date: new Date("Oct 04, 2012"),
...      status: 'A',
...      price: 10,
...      items: [ { sku: "mmm", qty: 5, price: 2.5 },
...               { sku: "nnn", qty: 5, price: 2.5 } ]
... }]);
 
> var mapFunction1 = function() {emit(this.cust_id, this.price)};
> var reduceFunction1 = function(keyCustId, valuesPrices) {return Array.sum(valuesPrices);};
> db.orders.mapReduce(mapFunction1,reduceFunction1,{ out: "map_reduce_example"});
{
    "result" : "map_reduce_example",
    "timeMillis" : 47,
    "counts" : {
        "input" : 4,
        "emit" : 4,
        "reduce" : 1,
        "output" : 1
    },
    "ok" : 1
}
> db.map_reduce_example.find()
{ "_id" : "abc123", "value" : 100 }

########################################################
# 고급 기능

* Database Commands
  https://www.mongodb.com/docs/v4.2/reference/command/

  db.listCommands()

* 제한 컬렉션 
  size 고정 컬렉션  초과시 에이지-아웃 순으로 삭제 후 입력 되는  환형 구조 인덱스가 없음
  입력순으로 조회됨 

  생성 시 설정 
  db.createCollection("limit_collection", {capped: true, size: 3})

  기존 컬렉션을 변경 
  db.createCollection("limit_collection_change")
  db.runCommand({convertToCapped: "limit_collection_change", size: 3})

  db.limit_collection.find()
  db.limit_collection.find().sort({"$natural" : -1})

  tailable-cursors
  https://www.mongodb.com/docs/manual/core/tailable-cursors/

* GridFS 
  $ mongofiles --help
  $ echo "Hello World" > test.txt
  $ mongofiles put test.txt
  $ mongofiles list 
  $ rm -rf test.txt
  $ mongofiles get test.txt

  test> db.fs.files.distinct("filename")
  [ 'test.txt' ]

########################################################
# 관리 

* MongoDB 시작

  mongod --help 

  * command line 시작 
  --dbpath   default : /data/db , C:\data\db
  --port 
  --fork 
  --logpath , --logappend 
  --config 

  ex) ./mongod --port 27017 --fork --logpath mongodb.log 

  * config 참조 시작 
  https://www.mongodb.com/docs/manual/reference/configuration-options/

  ex) mongodb.conf (YAML 설정 방식)
  processManagement:
    fork: true
  net:
    bindIp: localhost
    port: 27017
  storage:
    dbPath: /var/lib/mongo
  systemLog:
    destination: file
    path: "/var/log/mongodb/mongod.log"
    logAppend: true
  storage:
    journal:
        enabled: true

  ex) ./mongod --config ~/.mongodb.conf 

* MongoDB 중지 

  * SIGINT, SIGTERM 시그널 전송 
  터미널 창에서 구동하고 있다면 Ctrl-C , MongoDB PID 값이 1234 라면 kill -2 1234 or kill 1234 
  현재의 작업이나 파일 사전 자겁이 종료할 때까지 대기했다가 모든 연결 닫고 안전하게 종료 
  kill -9 사용 하면 안됨 위의 처리 없이 바로 종료 

  * shutdown
  use admin
  db.shutdownServer()
     
* 모니터링 
https://www.mongodb.com/docs/manual/administration/monitoring/

db.runCommand({"serverStatus" : 1})

mongostat --help 
mongostat 

mongotop --help
mongotop

* 보안과 인증 
https://www.mongodb.com/docs/manual/reference/method/db.createUser/

  db.createUser(user, writeConcern)

  {
    user: "<name>",
    pwd: passwordPrompt(),      // Or  "<cleartext password>"
    customData: { <any information> },
    roles: [
      { role: "<role>", db: "<database>" } | "<role>",
      ...
    ],
    authenticationRestrictions: [
      {
        clientSource: ["<IP>" | "<CIDR range>", ...],
        serverAddress: ["<IP>" | "<CIDR range>", ...]
      },
      ...
    ],
    mechanisms: [ "<SCRAM-SHA-1|SCRAM-SHA-256>", ... ],
    passwordDigestor: "<server|client>"
  }

  use test
  db.createUser( { user: "test",
                  pwd: passwordPrompt(),  // Or  "<cleartext password>"
                  customData: { employeeId: 12345 },
                  roles: [ { role: "clusterAdmin", db: "admin" },
                            { role: "readAnyDatabase", db: "admin" },
                            "readWrite"] },
                { w: "majority" , wtimeout: 5000 } )

  mongod 시작시 --auth 옵션 설정 

  ./mongosh mongodb://localhost:27017 로 접속시 권한 없음 

  mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]

  ./mongosh mongosh mongodb://test:admin123@localhost:27017/test

  ssh 터널링

  mongod --bind_ip localhost 
  mongod --noscripting

* 백업과 복구 
  --dbpath 경로의 파일 복사 (서비스 중지 후 복사)

   운영 중에 가능 하나 다른 클라이언트에 영향을 줌 
  mongodump --help
  mongodump 

  mongorestore --help
  mongorestore

  ./mongodump -d test -o backup
  ./mongorestore -d test --drop backup/test (-d 옵션을 주면 기존 컬렉션 삭제 후 복원 주지  않으면 복원시 기존꺼에 합쳐짐)

  fsync & lock 
  fsync 운영 중인 데이터 온전하게 복사 
  use admin
  db.runCommand({"fsync" : 1 , "lock" : 1}); 
  db.fsyncLock()
  백업
  db.fsyncUnlock()  

  slave backup 

  복구 
  mongod --repair 

########################################################
# 복제 

########################################################
# 샤딩 