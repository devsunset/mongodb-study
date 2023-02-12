-------------------------------------------------

			# MONGODB-WORK #

-------------------------------------------------

########################################################
### MONGODB

https://www.mongodb.com
https://www.mongodb.com/docs/manual/
https://www.mongodb.com/docs/manual/tutorial/

https://www.mongodbtutorial.org/

https://www.mongodb.com/try/download/community
https://www.mongodb.com/try/download/shell
https://www.mongodb.com/try/download/database-tools

실행 패스 설정
export PATH="$PATH:/Users/devsunset/dev/program/mongodb-macos-x86_64-6.0.4/bin"

https://www.mongodb.com/products/compass
https://www.mongodb.com/products/vs-code

https://learn.mongodb.com/learning-paths/introduction-to-mongodb

########################################################
# Baisc 
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
test> post = {"title" : "post1", "content" : "post content1", "date" : new Date()}
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
# Query 

https://github.com/neelabalan/mongodb-sample-dataset

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