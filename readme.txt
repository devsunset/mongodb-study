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

https://www.mongodb.com/try/download/community

RUNNING
  For command line options invoke:
    $ ./mongod --help

  To run a single server database:
    $ sudo mkdir -p /data/db
    $ ./mongod
    $
    $ # The mongo javascript shell connects to localhost and test database by default:
    $ ./mongo
    > help

./mongod --dbpath [[data_directory_path]]
ex) ./mongod --dbpath /Users/devsunset/dev/program/mongodb-macos-x86_64-6.0.4/data/db

########################################################

1. 문서 -> json 형태 
2.컬렉션 (스키마 X, 서브 컬렉션) -> 문서의 모음
3.데이터베이스 -> 컬렉션을 모아두는 것 
4.MongoDB start -> ./mongod --dbpath [[data_directory_path]]
5.MongoDB Shell  -> ./mongosh mongodb://localhost:27017
6.MongoDB Database-tools 
7.MongoDB Compass
8.MongoDB for VS Code 
9.