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
https://data-flair.training/blogs/mongodb-data-types/

