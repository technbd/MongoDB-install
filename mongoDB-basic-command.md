
## MongoDB Basic commands:

If using older versions, use `mongo` instead of `mongosh`: 

- `mongo` (deprecated) → The old MongoDB shell (used before MongoDB 5.0).
- `mongosh` (new shell) → The modern shell with better support for JavaScript, TypeScript, and improved CLI experience.



### Connect to MongoDB shell:

```
mongo 
```


```
mongosh

or,

mongosh --host 127.0.0.1 --port 27017
```


### Database Commands:

#### Show all databases:

```
show dbs

admin   40.00 KiB
config  60.00 KiB
local   72.00 KiB
```


#### Create new or switch to a database:

```
use mydb1

switched to db mydb1
```


#### Show current database:

```
db

mydb1
```


#### Check database stats: 

```
db.stats()

{
  db: 'mydb1',
  collections: 1,
  views: 0,
  objects: 1,
  avgObjSize: 67,
  dataSize: 67,
  storageSize: 36864,
  indexes: 1,
  indexSize: 36864,
  totalSize: 73728,
  scaleFactor: 1,
  fsUsedSize: 16169959424,
  fsTotalSize: 96589578240,
  ok: 1
}
```



#### Drop a database:

```
use mydb1
db.dropDatabase()

{ ok: 1, dropped: 'mydb1' }
```



### Collection Commands:


#### Show all collections in the database:

```
show collections
```


#### Create a collection:

_Syntax:_
```
db.createCollection(COLLECTION_NAME)
```


```
db.createCollection("myCollection")
db.createCollection("myCollection2")
```



#### Drop a collection:

_Syntax:_
```
db.COLLECTION_NAME.drop()
```


```
db.myCollection2.drop()
```



### Insert Documents:


#### Insert a single document:

_Syntax:_
```
db.COLLECTION_NAME.insert(document)
```


```
db.myCollection.insertOne({ name: "Alice", age: 25, city: "New York" })
```



#### Insert multiple documents:

```
db.myCollection.insertMany([
  { name: "Bob", age: 30, city: "Los Angeles" },
  { name: "Charlie", age: 35, city: "Chicago" }
])
```




### Query collection Documents:

#### Find all documents:

_Syntax:_

```
db.COLLECTION_NAME.find()
```


```
db.myCollection.find()
```


```
db.myCollection.find().pretty()
```




#### Find one document:

```
db.myCollection.find({ age: 25 })

db.myCollection.findOne({ name: "Alice" })
```


```
db.myCollection.find({ age: 35 })
```



#### Find documents with specific fields:

```
db.myCollection.find({}, { name: 1, city: 1, _id: 0 })
```




#### Total document count: 

```
db.myCollection.count();

or,

db.myCollection.countDocuments();
```



### Delete Documents:


#### Delete one document:

```
db.myCollection.deleteOne({ name: "Alice" })
```



#### Delete multiple documents:

```
db.myCollection.deleteMany({ age: { $gt: 30 } })
```





### Dump Backup & Restore:

If you need to backup & restore entire databases. 

- `mongodump`	: Creates a binary backup of the database
- `mongorestore` : Restores a database from a mongodump backup


```
wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-rhel88-x86_64-100.11.0.tgz
```


```
tar -xvzf mongodb-database-tools-rhel88-x86_64-100.11.0.tgz
```


```
cd mongodb-database-tools-rhel88-x86_64-100.11.0

cp bin/* /usr/local/bin
```


```
mongodump --version
```



_Backup:_
```
mongodump --db myDatabase --out /backup/path/
```


```
mongodump --db mydb1 --out /home/uistl0/
```



_Restore_
```
mongorestore --db myDatabase /backup/path/myDatabase
```


```
mongorestore --db mydb1 /home/uistl0/mydb1
```





### Export Data:

_Basic Syntax:_
```
mongoexport --db <database> --collection <collection> --out <filename> --jsonArray --port <port>
```


_Export collection to JSON:_
```
mongoexport --db mydb1 --collection myCollection --out data.json --jsonArray
```


_Export collection to CSV:_
```
mongoexport --db mydb1 --collection myCollection --out data.csv --type=csv --fields name,age,city
```






### Import Data:

_Basic Syntax:_

```
mongoimport --db <database> --collection <collection> --file <filename> --jsonArray --port <port>
```


_Import JSON file:_

```
mongoimport --db mydb1 --collection myCollection --file data.json --jsonArray
```


_Import CSV file:_

```
mongoimport --db mydb1 --collection myCollection --type csv --file data.csv --headerline
```



### Create an Admin User:

By default, MongoDB allows access without authentication. To enable authentication:

```
nano /etc/mongod.conf

security:
  authorization: enabled
```


```
systemctl restart mongod
```


#### Crate user:

To manage users, you first need an admin user.

- `role: "root"` : gives full access to all databases.
- `role: "readWrite"` : allows the user to insert, update, and delete documents.
- `role: "read"` : Read-only access to a database
- `dbAdmin`	: Administrative tasks (indexes, users) but no data read/write
- `userAdmin` :	Manage users and roles in a database


```
use admin

db.createUser({"user": "root", "pwd": "yourpass", "roles": ["root"]})


db.createUser({"user": "root", "pwd": "yourpass", "roles": [{ role: "root", db: "admin" }] })
```



#### Login with authentication:

- `--host` [arg] : Server to connect to
- `--port` [arg] : Port to connect to
- `-u` , `--username` [arg] : Username for authentication:
- `-p` , `--password` [arg] : Password for authentication 
- `--authenticationDatabase` [arg] : User source (defaults to dbname)


_Use to login directly:_ 

```
mongosh -u root -p --authenticationDatabase <database>
```


```
mongosh -u root -p --authenticationDatabase admin

or,

mongosh --host 127.0.0.1 --port 27017 --authenticationDatabase admin -u <user> -p <pwd>
```



_Use inside the MongoDB shell to authenticate:_

```
mongosh
use admin


db.auth("root", "yourpass")

or,

db.auth( "root", passwordPrompt() )


Enter password
********{ ok: 1 }
```




```
db.runCommand({ connectionStatus: 1 })


{
  authInfo: {
    authenticatedUsers: [ { user: 'root', db: 'admin' } ],
    authenticatedUserRoles: [ { role: 'root', db: 'admin' } ]
  },
  ok: 1
}
```


Show all users in the current database::

```
db.getUsers()
```


```
show users
```






### Create a Database User:

To create a user for a specific database:

```
use mydb1
```


```
db.createUser({
  user: "user1",
  pwd: "dbpass",
  roles: [{ role: "readWrite", db: "mydb1" }]
})
```


```
db.getUsers()
```


```
show users
```



#### Login with authentication:

```
mongosh -u user1 -p --authenticationDatabase mydb1

```



#### Update User Password:

```
use mydb1
```


```
db.updateUser("user1", { pwd: "mypass" })
```



#### Delete a User:
```
db.dropUser("root")

db.dropUser("user1")
```




### Links:

- [MongoDB Cheat Sheet](https://www.mongodb.com/developer/products/mongodb/cheat-sheet/)


These are the fundamental MongoDB commands. Let me know if you need further details or more advanced queries! 