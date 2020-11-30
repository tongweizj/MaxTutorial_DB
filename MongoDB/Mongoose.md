# Mongoose

## 一、mongoose 介绍
Mongoose 是在 node.js 异步环境下对 mongodb 进行便捷操作的对象模型工具。
Mongoose 是 NodeJS 的驱动，不能作为其他语言的驱动。

1. 通过关系型数据库的思想来设计非关系型数据库 
2. 基于 mongodb 驱动，简化操作

## 二、mongoose 的安装以及使用

官网:https://mongoosejs.com/

### 1. 在应用中安装 Model：mongoose

```bash

npm i mongoose --save

```

### 2、引入 mongoose 并连接数据库

```bash

const mongoose = require('mongoose'); 
mongoose.connect('mongodb://localhost/test');

// 如果有账户密码需要采用下面的连接方式: 
// mongoose.connect('mongodb://eggadmin:123456@localhost:27017/eggcms');

```

### 3、定义 Schema

数据库中的 Schema，为数据库对象的集合。

 - schema 是 mongoose 里会用到的一种数据模式， 可以理解为表结构的定义;
 - 每个 schema 会映射到 mongodb 中的一个 collection，它不具备 操作数据库的能力

```bash
var UserSchema = mongoose.Schema({ 
    name: String,
    age: Number,
    status: 'number'
})
``` 

### 4、创建数据模型

定义好了 Schema，接下就是生成 Model。
model 是由 schema 生成的模型，可以对数据库的操作。

注意:

* mongoose.model 里面可以传入两个参数也可以传入三个参数 
* mongoose.model(参数 1:模型名称(首字母大写)，参数 2:Schema)
 
###  5、查找数据

```bash

User.find({}, function(err,docs){
    if(err){
        console.log(err);
        return; 
    }
    console.log(docs); 
})
```

###  6、增加数据

```bash

var u=new User({     //实例化模型 传入增加的数据 
    name:'lisi2222333',
    age:20,
    status:true
})
u.save();

```

###  7、修改数据

```bash

User.updateOne(
    { name: 'lisi2222' }, { name: '哈哈哈' }, 
    function(err, res) {
        if(err){
            console.log(err);
            return; 
        }
        console.log('成功') 
    }
);

```

### 8、删除一条数据

```bash

User.deleteOne(
    { _id: '5b72ada84e284f0acc8d318a' }, 
    function (err) { 
        if (err) {
            console.log(err);
            return; 
        }
        // deleted at most one tank document
        console.log('成功'); 
    }
);
```

###  9、保存成功查找

```bash

var u = new User({ 
    name:'lisi2222333',
    age:20,
    status:true //类型转换 
})

u.save(function(err,docs){
    if(err){
        console.log(err);
        return; 
    }

    //console.log(docs); 
    User.find({},function(err,docs){
        if(err){
            console.log(err);
            return;
        }
        console.log(docs); })
    }
);

```



## 三、Mongoose aggregate 多表关联查询

```
var mongoose=require('./db.js');
var OrderSchema=mongoose.Schema({
    order_id:String, 
    uid:Number, 
    trade_no:String, 
    all_price:Number, 
    all_num:Number
})

var OrderModel=mongoose.model(
    'Order',
    OrderSchema,
    'order'); 

OrderModel.aggregate(
    [{
        $lookup:{
            from:'order_item', 
            localField:"order_id", 
            foreignField:"order_id", 
            as:"item"
        } 
    }],
    function(err,docs){ 
        console.log(docs)
    }
);
```