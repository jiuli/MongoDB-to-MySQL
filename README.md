##MongoDB to MySQL Data Streaming

Streaming data in MongoDB to MySQL database in realtime. Enable SQL query on
data in NoSQL database.

## Configurations:
背景：我要把mongodb中的表转为mysql的，可以更好和mysql中的组织表做关联，更好的定时生成报表。
在网上查了tungsten-replicator可以对不同数据库进行同步，但是发现Extractor读取的源库只包括MySQL和Oracle，不包括mongodb！
后来找到了https://github.com/doubaokun/MongoDB-to-MySQL，目前这个就是对它的修改，个人意见，还有很多可以完善之处，待后续处理。。。。。。。。。

1. Update the mongodb configuration in `config.json`

```json
{
  "service": "mycol001",
  "mongodb": {
    "host": "localhost",
    "port": 27017,
    "db": "test",
    "collection": "question"
  },
  "mysql": {
    "host": "localhost",
    "user": "qnaire_user",
    "password": "123456",
    "db": "test_report",
    "table": "report"
  },
  "sync_fields": {  //fields stored in MySQL
    "id": "string",
    "cid": "string",
    "updated": "datetime",
    "SID": "int",
    "title": "string",
    "project_id": "string",
    "question_type": "int",
    "questionpage_id": "string"
  },
  "trans_to" : {
    "field_names" : {  //field name of MongoDB transformed into field name of MySQL
      "_id": "id"
    }
  }
}

```

2. Import the old data in MongoDB collection to MySQL table:

    node app.js import

3. Start the daemon to streaming data

    node start app.js or forever start app.js

4. A MySQL table mongo_to_mysql will be created to store required information.

5. Update the transTo() to change field names or modify values during streaming.

