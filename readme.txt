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
https://wikidocs.net/book/8823

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

* Master & Slave (Master/slave replication is no longer supported)
mongod --master
mongod --slave --source master_address

$ mkdir -p ~/dbs/master
$ ./mongod --dbpath ~/dbs/master --port 10000 --master
$ mkdir -p ~/dbs/slave  (slave node 12개 이하일때 원활히 동작)
$ ./mongod --dbpath ~/dbs/slave --port 10001 --slave --source localhost:10000
  - mogoshell 에서 slave 추가 하는 방법도 있음 
  use local
  db.sources.insert({"host" : "localhost:27017"})

* Replica Set 
https://docs.mongodb.com/manual/replication/ 
자동 장애 넘김 기능이 있는 Master-Slave 구조 
Replica Set의 차이점은 여러 마스터 노드를 가진다는 점 

$ mkdir -p ~/dbs/node1 ~/dbs/node2
$ mongod --dbpath ~/dbs/node1 --port 10001 --replSet test/localhost:10002
$ mongod --dbpath ~/dbs/node2 --port 10002 --replSet test/localhost:10001

세번째 추가시 
$ mongod --dbpath ~/dbs/node3 --port 10003 --replSet test/localhost:10001
or
$ mongod --dbpath ~/dbs/node3 --port 10003 --replSet test/localhost:10001,dev.local:10002

$ mongosh localhost:10001/admin

rs.initiate( {
   _id : "test",
   members: [
      { _id: 0, host: "localhost:10001" },
      { _id: 1, host: "localhost:10002" }
   ]
})


$ mongosh mongodb://localhost:10001
use test
ok = {"X": 1, "y": 2}
db.sample.insertOne(ok)

$ mongosh mongodb://lcoalhost:10002
use test
db.getMongo().setReadPref('primaryPreferred')
db.sample.findOne()

* 노드
standard (투표 참가 , 복제본 O , 주노드가 될수 있음 )
passive (투표 참가 , 복제본 O , 주노드가 될수 없음 )
arbiter (투표에만 참가 , 복제본 X , 주노드가 될수 없음 )

standard, passive 의 차이점은 우선순위 차등제 priority 
priority 0 수동 노드 이며 절대 주 노드가 되지 않음 

local database는 복제 되지 않음 

복제 상태 점검 
  Master 접속 
  rs.status()
  db.printReplicationInfo()
  Slave 접속 
  db.printSecondaryReplicationInfo


* MongoDB Replica set
https://www.devkuma.com/docs/mongodb/replication/intro/
https://hoing.io/archives/4282

  * https://gslabs.tistory.com/131
  1. 설정 파일에 ReplicaSet 옵션을 추가한다.
    replication:
    replSetName: {ReplicaSet이름}

  2. MongoDB 서버 실행 후 Primary 가 될 MongoDB 서버로 접속한다.
    a. ReplicaSet 초기화한다.
    rs.initiate();

    b. Primary 이름을 변경한다.
    conf = rs.conf();

    conf.members[0].host = "IP주소:포트";

    rs.reconfig(conf);
    참고> MongoDB 는 hostname 을 기본 값으로 사용하기 때문에 문제가 생기는 경우가 있음.

  3. ReplicaSet 멤버를 추가한다.
    /* 일반 멤버 추가 */
    rs.add("IP주소:포트");
    /* Arbiter 멤버 추가 */
    rs.addArb("IP주소:포트");


  4. Delayed 멤버를 설정해야 될 경우
    conf = rs.conf();
    conf.members[{멤버번호}].hidden = true;
    conf.members[{멤버번호}].priority = NumberInt(0);
    conf.members[{멤버번호}].slaveDelay = NumberLong({지연시간(초)});

    rs.reconfig(conf);

----------------------------------------------------------

  * https://devnot.tistory.com/100
  1개의 PRIMARY DB 와 N개의 SECONDARY, 1개의 Arbiter 로 구성
  PRIMARY 는 데이터를 삽입할 메인 SECONDARY 는 PRIMARY 한테 데이터를 받아서 복사하는 DB 
  Arbiter 는 PRIMARY DB 가 죽었을 시 남은 SECONDARY DB 를 PRIMARY 로 승격시킴
  여기 replica set 등록하는 멤버 총 개수가 7개(PRIMARY + SECONDARY + Arbiter) 로 제한
  Arbiter 는 여러개를 등록가능하지만 살아있는것은 오직 1개뿐

  MongoDB replica config 파일
  (dbPath, logPath, port를 알맞게 변경) 
  mongod --config mongo.conf 로 실행 

  storage:
    dbPath: ~/dbs/node1

  systemLog:
    path: ~/dbs/log/node1/mongo.log
    logAppend: true
    destination: file
    traceAllExceptions: false

  processManagement:
    fork: true

  replication:
    replSetName: "devtest"

  security:
    authorization: enabled
    keyFile: ~/dbs/config/mongodb.key

  net:
    bindIp: 0.0.0.0
    port: 10000

  * Mongo KeyFile 생성 (다른 레플리카 DB 도 같은 keyFile 을 바라보게 파일 복사 후 위치 )
  https://docs.mongodb.com/manual/tutorial/enforce-keyfile-access-control-in-existing-replica-set/
  $ cd ~/dbs/config 
  openssl rand -base64 741 > mongodb.key
  chmod 600 mongodb.key

  mongosh mongodb://localhost:10000/admin

  rs.initiate( {
    _id : "devtest",
    members: [
        { _id: 0, host: "localhost:10000" },
        { _id: 1, host: "localhost:10001" },
        { _id: 2, host: "localhost:10002" }
    ]
  })

  hosts 에  127.0.0.1:10000, 127.0.0.1:10001, 127.0.0.1:10002, 127.0.0.1:10003 의 DB 를 연결해놓고
    Arbiter 를 등록하지 않으면 10000 번이 PRIMARY 인데 죽었을 경우 다른 DB 중 1개가 PRIMARY 로 승격되지 않음
  -> Arbiter 가 살아있어야지만 나머지 3개중 1개의 DB 를 PRIMARY 로 승격함
    주의할점 node 추가하기 전에 데이터가 있으면 전부 지워지고 덥어씌어버림

  *replica set 초기화 방법
  secondary 인 DB 를 초기화하고 싶을 때 DB 를 종료하고 config 폴더에 replica 옵션을 지운후 DB 를 실행

  use local 
  db.dropDatabase()

########################################################
# 샤딩 