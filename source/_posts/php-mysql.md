---
  date: 2016-05-05 17:14:31
  title: PHP中操作MYSQL数据库常用函数
  tags: [PHP, 'mysql']
  categories:
    - [PHP]
    - [数据库]
  description:
---


1、mysql_connect()-建立数据库连接 

格式： resource mysql_connect([string hostname [:port] [:/path/to/socket] [, string username] [, string password]]) 

例： $conn = @mysql_connect("localhost", "username", "password") or die("不能连接到Mysql Server"); 说明：使用该连接必须显示的关闭连接 

2、mysql_pconnect()-建立数据库连接 

格式： resource mysql_pconnect([string hostname [:port] [:/path/to/socket] [, string username] [, string password]]) 

例： $conn = @mysql_pconnect("localhost", "username", "password") or dir("不能连接到Mysql Server"); 说明：使用该连接函数不需要显示的关闭连接，它相当于使用了连接池 

3、mysql_close()-关闭数据库连接 

例： $conn = @mysql_connect("localhost", "username", "password") or die("不能连接到Mysql Server"); @mysql_select_db("MyDatabase") or die("不能选择这个数据库，或数据库不存在"); echo "你已经连接到MyDatabase数据库"; mysql_close(); 

4、mysql_select_db()-选择数据库 

格式： boolean mysql_select_db(string db_name [, resource link_id]) 

例： $conn = @mysql_connect("localhost", "username", "password") or die("不能连接到Mysql Server"); @mysql_select_db("MyDatabase") or die("不能选择这个数据库，或数据库不存在"); 

5、mysql_query()-查询MySQL 

格式： resource mysql_query (string query, [resource link_id]) 

例： $linkId = @mysql_connect("localhost", "username", "password") or die("不能连接到Mysql Server"); @mysql_select_db("MyDatabase") or die("不能选择这个数据库，或者数据库不存在"); $query = "select * from MyTable"; $result = mysql_query($query); mysql_close(); 说明：若SQL查询执行成功，则返回资源标识符，失败时返回FALSE。若执行更新成功，则返回TRUE，否则返回FALSE 

6、mysql_db_query()-查询MySQL 

格式： resource mysql_db_query(string database, string query [, resource link_id]) 

例： $linkId = @mysql_connect("localhost", "username", "password") or die("不能连接到MysqlServer"); $query = "select * from MyTable"; $result = mysql_db_query("MyDatabase", $query); mysql_close(); 说明：为了使代码清晰，不推荐使用这个函数调用 

7、mysql_result()-获取和显示数据 

格式： mixed mysql_result (resource result_set, int row [, mixed field]) 

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); for($count=0;$count<=mysql_numrows($result);$count++) { $c_id = mysql_result($result, 0, "id"); $c_name = mysql_result($result, 0, "name"); echo $c_id,$c_name; } 说明：最简单、也是效率最低的数据获取函数 

8、mysql_fetch_row()-获取和显示数据 

格式： array mysql_fetch_row (resource result_set) 

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); while (list($id, $name) = mysql_fetch_row($result)) { echo("Name: $name ($id) <br />"); } 说明：函数从result_set中获取整个数据行，将值放在一个索引数组中。通常会结使list()函数使用 

9、mysql_fetch_array()-获取和显示数据 

格式： array mysql_fetch_array (resource result_set [, int result_type]) 

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); while($row = mysql_fetch_array($result, MYSQL_ASSOC)) { $id = $row["id"]; $name = $row["name"]; echo "Name: $name ($id) <br />"; } 又

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); while($row = mysql_fetch_array($result, MYSQL_NUM)) { $id = $row[0]; $name = $row[1]; echo "Name: $name ($id) <br />"; } 说明： result_type的值有： MYSQL_ASSOC: 字段名表示键，字段内容为值 MYSQL_NUM: 数值索引数组，操作与mysql_fetch_ros()函数一样 MYSQL_BOTH: 即作为关联数组又作为数值索引数组返回。result_type的默认值。 

10、mysql_fetch_assoc()-获取和显示数据 

格式： array mysql_fetch_assoc (resource result_set) 相当于调用 mysql_fetch_array(resource, MYSQL_ASSOC); 

11、mysql_fetch_object()-获取和显示数据 

格式： object mysql_fetch_object(resource result_set) 

例： $query = "select id, name from MyTable order by name"; while ($row = mysql_fetch_object($result)) { $id = $row->id; $name = $row->name; echo "Name: $name ($id) <br />"; } 说明：返回一个对象，在操作上与mysql_fetch_array()相同 

12、mysql_num_rows()-所选择的记录的个数 

格式： int mysql_num_rows(resource result_set) 

例： query = "select id, name from MyTable where id > 65"; $result = mysql_query($query); echo "有".mysql_num_rows($result)."条记录的ID大于65"; 说明：只在确定select查询所获取的记录数时才有用。 

13、mysql_affected_rows()－受Insert,update,delete影响的记录的个数 

格式： int mysql_affected_rows([resource link_id]) 

例： $query = "update MyTable set name='CheneyFu' where id>=5"; $result = mysql_query($query); echo "ID大于等于5的名称被更新了的记录数：".mysql_affected_rows(); 说明：该函数获取受INSERT,UPDATE或DELETE更新语句影响的行数 

14、mysql_list_dbs()-获取数据库列表信息 

格式： resource mysql_list_dbs([resource link_id]) 

例： mysql_connect("localhost", "username", "password"); $dbs = mysql_list_dbs(); echo "Databases: <br />"; while (list($db) = mysql_fetch_rows($dbs)) { echo "$db <br />"; } 说明：显示所有数据库名称 

15、mysql_db_name()-获取数据库名 

格式： string mysql_db_name(resource result_set, integer index) 说明：该函数获取在mysql_list_dbs()所返回result_set中位于指定index索引的数据库名 

16、mysql_list_tables()-获取数据库表列表 

格式： resource mysql_list_tables(string database [, resource link_id]) 

例： mysql_connect("localhost", "username", "password"); $tables = mysql_list_tables("MyDatabase"); while (list($table) = mysql_fetch_row($tables)) { echo "$table <br />"; } 说明：该函数获取database中所有表的表名 

17、mysql_tablename()-获取某个数据库表名 

格式： string mysql_tablename(resource result_set, integer index) 

例： mysql_connect("localhost", "username", "password"); $tables = mysql_list_tables("MyDatabase"); $count = -1; while (++$count < mysql_numrows($tables)) { echo mysql_tablename($tables, $count)."<br />"; } 说明：该函数获取mysql_list_tables()所返回result_set中位于指定index索引的表名 

18、mysql_fetch_field()-获取字段信息 

格式： object mysql_fetch_field(resource result [, int field_offset]) 

例： mysql_connect("localhost", "username", "password"); mysql_select_db("MyDatabase"); $query = "select * from MyTable"; $result = mysql_query($query); $counts = mysql_num_fields($result); for($count = 0; $count < $counts; $count++) { $field = mysql_fetch_field($result, $count); echo "<p>$field->name $field->type ($field->max_length) </p>"; } 说明： 返回的对象共有12个对象属性： name: 字段名 table: 字段所在的表 max_length:字段的最大长度 not_null: 如果字段不能为null,则为1,否则0 primary_key: 如果字段为主键，则为1，否则0 unique_key: 如果字段是唯一键，则为1， 否则0 multiple_key: 如果字段为非唯一，则为1，否则0 numeric: 如果字段为数值则为1，否则0 blob: 如果字段为BLOB则为1，否则为0 type: 字段的数据类型 unsigned: 如果字段为无符号数则为1，否则为0 zerofill: 如果字段为“零填充”则为1， 否则为0 

19、mysql_num_fields()-获取查询的字段个数 

格式： integer mysql_num_fields(resource result_set) 

例： $query = "select id,name from MyTable order by name"; $result = mysql_query($query); echo "这个查询的字段数是：".mysql_num_fields($result)."<br />"; 

20、mysql_list_fields()-获取指定表的所有字段的字段名 

格式： resource mysql_list_fields (string database_name, string table_name [, resource link_id]) 

例： $fields =mysql_list_fields("MyDatabase", "MyTable"); echo "数据库MyDatabase中表MyTable的字段数： ".mysql_num_fields($fields)."<br />"; 

21、mysql_field_flags()-获取指定的字段选项 

格式： string mysql_field_flags (resource result_set, integer field_offset) 

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); $row=mysql_fetch_wor($row); 

22、mysql_field_len()-获取指定的字段的最大长度 

格式： integer mysql_field_len (resource result_set, integer field_offset) 

例： $query = "select name from MyTable"; $result = mysql_query($query); $row = mysql_fetch_row($result); echo mysql_field_len($result, 0)."<br />"; 说明： 如果mysql_field_len($reseult, 0) = 16777215 那么numer_format(mysql_field_len($result))等于16,777,215 

23、mysql_field_name()-获取字段名 

格式： string mysql_field_name (resource result_set, int field_offset) 

例： $query = "select id as PKID, name from MyTable order by name"; $result = mysql_query($query); $row = mysql_fetch_row($result); echo mysql_field_name($result, 0); // Result: PKID 

24、mysql_field_type()-获取字段类型 

格式： string mysql_field_type (resource result_set, int field_offset) 

例： $query = "select id, name from MyTable order by name"; $result = mysql_query($query); $row = mysql_fetch_row($result); echo mysql_field_type($result, 0); // Result: int 

25、mysql_field_table()-获取字段所在表名 

格式： string mysql_field_table (resource result_set, int field_offset) 

例： $query = "select id as PKID, name from MyTable order by name"; $result = mysql_query($query); $row = mysql_fetch_row($result); echo mysql_field_table($result, 0); // Result: MyTable


