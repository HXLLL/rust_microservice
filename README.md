# Microservice

使用Rust实现的简单Microservice框架，在这个框架下写了一个简单的线上聊天室的后端。
写的时候参考了[这个教程](http://www.goldsborough.me/rust/web/tutorial/2018/01/20/17-01-11-writing_a_microservice_in_rust/).

用户可以通过HTTP POST请求发送消息，服务器会把所有消息显示在一个html页面上，同时还会显示消息发送的时间。

## 用到的库
- hyper: 使用hyper作为服务器的框架
- diesel: 使用diesel连接后端数据库
- chrono: 使用chrono将时间戳转化为人类可以理解的格式
- env_logger, log: 日志记录
- serde, serde_derive, serde_json: 数据序列化
- maud: 前端展示
- futures: 异步I/O
- url: url处理

## 测试步骤：
```
1. 安装依赖
  1.1 安装PostgreSQL
  1.2 安装libpq和libmysqlclient
2. 配置数据库
  2.1 执行 sudo su - postgres -c "initdb --locale en_US.UTF-8 -E UTF8 -D '/var/lib/postgres/data'"
  2.2 启动数据库 
    sudo systemctl start postgresql.service
  2.3 添加新用户
    sudo -i -u postgres
    [postgres]$ createuser --interactive
    # 根据提示创建一个用户
  2.4 创建数据库
    createdb db1
  2.5 执行schemas/messages.sql创建对应的表
3. 编译项目
  cargo build
4. 启动服务器
  DATABASE_URL="postgresql://<user>@localhost/db1" cargo run
  其中<user>为创建数据库的用户名
5. 使用post向服务器发送消息，访问http://localhost:9999 即可查看当前所有的消息
例：
  curl -X POST -d 'username=hxl&message=hello world' 'localhost:9999'  # 名为hxl的用户发送了'hello world'
  curl -X POST -d 'username=world&message=hi hxl' 'localhost:9999'  # 名为world的用户发送了'hi hxl'
  在浏览器中访问 http://localhost:9999 即可看到这两条消息
```