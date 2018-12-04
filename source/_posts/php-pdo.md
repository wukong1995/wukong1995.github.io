---
  date: 2016-05-26 12:23:50
  title: PHP中PDO的使用
  tags: ['PHP', 'mysql']
  categories: ['PHP', '数据库']
  description:
---

1.PDO连接MySQL数据库 
```php
$pdo = new PDO("mysql:host=localhost;dbname=db_demo",用户名,密码); 
```
 默认不是长连接，若要使用数据库长连接，需要在最后加如下参数:

```php
new PDO("mysql:host=localhost;dbname=db_demo","root","","array(PDO::ATTR_PERSISTENT => true) "); 
```

2.PDO中常用的函数及其解释如下:
`PDO::query()`主要是用于有记录结果返回的操作，特别是SELECT操作
`PDO::exec()`主要是针对没有结果集合返回的操作，如INSERT、UPDATE等操作
`PDO::lastInsertId()` 返回上次插入操作，主键列类型是自增的最后的自增ID
`PDOStatement::fetch()`是用来获取一条记录     `PDOStatement::fetchAll()`是获取所有记录集到一个中

3.使用示例
```php
$sql = "select price from shop where id=?";
$stmt = $pdo->prepare($sql);
$stmt->execute($id);
$data=$stmt->fetch(PDO::FETCH_ASSOC);

$sql = "insert into shop(id,name,price) values(?,?,?)";
$stmt = $pdo->prepare($sql);
$stmt->execute($id,$name,$price);
$data=$stmt->rowCount();
```


