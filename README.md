# AYNTKA-MySQL

**A**ll **y**ou **n**eed **t**o **k**now **a**bout **MySQL**

## 基本概念的介绍

### 数据库 Database

> A **Database** is a collection of data stored in a format that can easily be accessed

为了更方便的管理我们的数据，我们一般会使用一种叫数据库管理系统（Database Management System，简称DBMS）的系统级软件，它提供了用户连接的接口，方便我们可以在连接数据库后进行插入、查询、修改和删除数据。

现在主流的数据库分为2类，一类是关系型（Relational）数据库，一类是非关系型（NoSQL）数据库。

在关系型数据库中我们把数据存储在利用关系互相连接的表中，每张表中都会存储一种特定类型的对象数据。SQL或者SQUEL就是我们用来处理这些关系型DBMS的语言。 目前主流的关系型数据库有MySQL、微软的SQL Server，还有Oracle，Postgres等。每种关系型数据库提供的SQL可能略有不同，但基本都遵守SQL语言规范。

在非关系型数据库中，没有表或者关系，它们一般都无法使用SQL语言来进行数据库操作，往往有自己的数据库查询语言。

### SQL还是SEQUEL

最早的SQL的全称为Structured English QUEry Language，但因为SEQUEL是一个飞机商标，就被简化为SQL。

推荐发音，在我们指关系数据库查询语言时，读`sequel`，在`MySQL`中读`s--q--l`

### 数据库中的逻辑存储结构

一般的数据库都提供了数据库、数据表、行、列等这样的逻辑结构，方便我们进行数据库的操作，这些逻辑结构并不代表着数据的真实存储形式。

* 数据库（database）：一些相关的表组成的集合

* 数据表（table）：表是数据的矩阵，在一个数据库中表看起来就像一个简单的电子表格

* 列（column）：列代表了数据表的某一种属性或字段，一列（数据元素）包含了相同的数据类型

* 行（row）：一行是一组相关的数据，我们称之为记录（Record），一行一行的数据组成一数据表。

* 表头（header）：表头是由列名组成

* 键（key）：一般我们从数据表中获取某一行数据时，都需要通过一个键值来定位到某一行。

## 本教程中使用的数据库说明

本教程中一共涉及到4个数据库（database）

### sql_store数据库

sql_store是一个类似于在线电商的后台数据库，一共有7张表，其中orders是整个数据库的核心表，记录的是一次购物订单，每个购物订单订单对应一个消费者，对应多个商品，对应一个货运。可以想象为我们在京东或淘宝上的一个订单，里面包括了多个商品，最终这个订单是有一个物流来承运。

![](./images/sql_store.png)

**customers表**


| 字段名      | 数据类型                                   | 意义     |
| ----------- | ------------------------------------------ | -------- |
| customer_id | int, not null, Primary Key, auto_increment | 客户id   |
| first_name  | varchar(50), not null                      | 名字     |
| last_name   | varchar(50), not null                      | 姓       |
| birth_date  | date                                       | 生日日期 |
| phone       | varchar(50)                                | 手机号   |
| address     | varchar(50), not null                      | 地址     |
| city        | varchar(50), not null                      | 城市     |
| state       | char(2), not null                          | 州       |
| points      | int, not null,default 0                    | 积分     |

**orders表**

| 字段名       | 数据类型                                   | 意义     |
| ------------ | ------------------------------------------ | -------- |
| order_id     | int, not null, Primary Key, auto_increment | 订单号   |
| customer_id  | int, not null                              | 客户id   |
| order_date   | date, not null                             | 订单日期 |
| status       | tinyint,not null, default 1                | 订单状态 |
| comments     | varchar(2000)                              | 订单备注 |
| shipped_date | date                                       | 发货日期 |
| shippper_id  | smallint                                   | 运货商id |

**order_items表**

| 字段名     | 数据类型                                   | 意义   |
| ---------- | ------------------------------------------ | ------ |
| order_id   | int, not null, Primary Key, auto_increment | 订单id |
| product_id | int, not null, Primary Key                 | 商品id |
| quantity   | int, not null                              | 数量   |
| unit_price | decimal(4,2), not null                     | 单价   |

**order_statuses表**

order_statuses表提供了每种订单状态编号的含义，包括：processed 已处理、shipped 已寄出、delivered 已送达，它一个枚举表。

| 字段名          | 数据类型                       | 意义           |
| --------------- | ------------------------------ | -------------- |
| order_status_id | tinyint, not null, Primary Key | 订单状态id     |
| name            | varchar(50), not null          | 订单状态枚举值 |

**order_item_notes表**

| 字段名     | 数据类型                   | 意义 |
| ---------- | -------------------------- | ---- |
| note_id    | int, not null, Primary Key |      |
| order_id   | int, not null              |      |
| product_id | int, not null              |      |
| note       | varchar(255), not null     |      |

**products表**

| 字段名            | 数据类型                                   | 意义     |
| ----------------- | ------------------------------------------ | -------- |
| product_id        | int, not null, Primary Key, auto increment | 商品id   |
| name              | varchar(50), not null                      | 商品名称 |
| quantity_in_stock | int, not null                              | 库存数量 |
| unit_price        | decimal(4, 2), not null                    | 商品单价 |

**shippers表**

| 字段名     | 数据类型                                        | 意义         |
| ---------- | ----------------------------------------------- | ------------ |
| shipper_id | smallint, not null, primary key, auto increment | 快递公司id   |
| name       | varchar(50), not null                           | 快递公司名字 |

### sql_inventory数据库

sql_inventory是一个库存数据库，它比较简单，里面只有一张商品库存的表，和sql_store中的商品表是一样的。在整个教程中使用的非常少。

### sql_hr数据库

sql_hr表示是公司人员管理的数据库，包括了2张表，一张是员工表，一张是办公室的信息表。

![](./images/sql_hr.png)

**employees表**

其中report_to字段表示汇报对象，它指向的是本表中的一项，是一个自联结关系。

| 字段名      | 数据类型                    | 意义     |
| ----------- | --------------------------- | -------- |
| employee_id | int, not null, Primary Key, | 员工id   |
| first_name  | varchar(50), not null       | 名字     |
| last_name   | varchar(50), not null       | 姓       |
| job_title   | varchar(50), not null       | 职务     |
| salary      | int, not null               | 工资     |
| report_to   | int                         | 汇报对象 |
| offfice_id  | int, not null               | 办公室id |

**offices表**

|           | 数据类型                                   | 意义     |
| --------- | ------------------------------------------ | -------- |
| office_id | int, not null, Primary Key, auto_increment | 办公室id |
| address   | varchar(50), not null                      | 地址     |
| city      | varchar(50), not null                      | 城市     |
| state     | varchar(50), not null                      | 州       |

### sql_invoicing数据库

sql_invoicing是财务数据库，第个客户可能对应多个交易，每个交易对应一张发票，每张发票可能会产生多笔支付。

![](./images/sql_invoicing.png)

**clients表**

| 字段名     | 数据类型                   | 意义     |
| ---------- | -------------------------- | -------- |
| client_id  | int, not null, Primary Key | 客户id   |
| name       | varchar(50), not null      | 名字     |
| address    | varchar(50), not null      | 地址     |
| city       | varchar(50), not null      | 城市     |
| state      | char(2), not null          | 州       |
| phone      | varchar(50)                | 手机号   |

**invoices表**

| 字段名        | 数据类型                           | 意义       |
| ------------- | ---------------------------------- | ---------- |
| invoice_id    | int, not null, Primary Key         | 发票id     |
| number        | varchar(50), not null              | 发票号     |
| client_id     | int, not null                      | 客户id     |
| invoice_total | decimal(9, 2), not null            | 发票总额   |
| payment_total | decimal(9, 2), not null, default 0 | 已支付总额 |
| invoice_date  | date, not null                     | 发票日期   |
| due_date      | date, not null                     | 截止日期   |
| payment_date  | date                               | 付款时间   |

**payments表**

| 字段名         | 数据类型                   | 意义     |
| -------------- | -------------------------- | -------- |
| payment_id     | int, not null, Primary Key | 付款id   |
| client_id      | int, not null              | 客户id   |
| invoice_id     | int, not null              | 发票id   |
| date           | date, not null             | 付款日期 |
| amount         | decimal(9,2), not null     | 付款金额 |
| payment_method | tinyint, not null          | 支持方法 |

**payment_methods表**

| 字段名            | 数据类型                                       | 意义           |
| ----------------- | ---------------------------------------------- | -------------- |
| payment_method_id | tinyint, not null, primary key, auto increment | 支持方法id     |
| name              | varchar(50), not null                          | 支付方法枚举值 |

## 单数据表的数据的查询

### 简单的查询示例

```sql
-- 查询整个表中的数据
select * from customers;

-- 查询符合条件的记录
select * from customers where customer_id=1;

-- 对查询结果进行排序
select * from customers order by points;

-- 查询指定列的数据
select customer_id, first_name, points from customers;
```

### 使用`select`子句对查询的数据进行加工

```sql
-- 多增加了new price这一列，是通过unit_price来计算加工出来的
select name, unit_price, unit_price * 1.1 as 'new price' from products;

-- 对customers表中的state这一列进行去重后展示
select distinct state from customers;
```

### 使用`where`子句设置复杂查询条件

```sql
-- 在date类型上可以直接使用比较运算符（>、<、=、!=、<>）
select * from customers where birth_date > '1990-01-01';

-- 多条件组合查询，可以使用 AND、OR、NOT来进行逻辑组合
select * from customers where not (birth_date > '1990-01-01' or points > 1000);

-- 在where子句里使用列与列的运算
select * from order_items where order_id=6 and quantity * unit_price > 30;

-- 使用in来匹配列表
select * from products where quantity_in_stock in (49, 38, 72);

-- 使用between来匹配范围
select * from customers where birth_date between '1990-01-01' and '2000-01-01';

-- 使用LIKE进行字符串模糊匹配
-- % 代表任意个任意字符
-- _ 代表任意一个字符
-- 查询手机号以9结尾的行
select * from customers where phone like '%9';
-- 查找first name中以b开头，y结尾，并且中间包括4个任意字符的行
select * from customers where first_name like 'b____y';

-- LIKE的完美替代：REGEXP
-- ^匹配单词开头
-- $匹配单词结尾
-- a|b 匹配多个模式
-- [a-h]匹配一个范围列表
select * from customers where last_name regexp '^B[RU]';

-- 查找空值
-- 查找所有还未发货的订单，即shipped_data是空
select * from orders where shipped_date is null;
```

### 使用`order by`子句对查询结果进行排序

可以按表中某一列的值进行排序，也可以是由已有的列进行加工计算出来的值，默认是升序排列，如果要降序排序，需要加上`DESC`。

```sql
查询order_id=2的所有订单，并按订单总价格进行降序排列
select * from order_items where order_id=2 order by quantity*unit_price DESC;
```

### 使用`limit`子句来限定查询结果的条数

```sql
-- 查询用户积分最高的3位用户
select * from customers order by points desc limit 3;
-- 设置查询的偏移值，从offset=6（offset从0编号）的地方开始查询，只列出3项
select * from customers limit 6, 3;
```

## 多数据表的联合查询

可以使用`join`进行多表查询，当使用`join`时必须用`on`指定两边表对齐到的列。

```sql
-- 查询orders表，并且从customers表中获取用消费者的名字
select order_id, c.customer_id, first_name, last_name
from orders
join customers as c
on orders.customer_id = c.customer_id;

-- 跨库联合查询，需要在其他库的表名前加上库名
select * 
from order_items as oi 
join sql_inventory.products as p 
on oi.product_id = p.product_id;

-- 自连接，员工表中汇报对象本身也是公司员工之一
select
    e.employee_id,
    e.first_name,
    m.first_name as manager
from employees as e
join employees as m on e.reports_to = m.employee_id;

-- 多表联合查询
select p.payment_id, c.name, p.date, p.amount, pm.name
from payments as p
join clients as c on p.client_id=c.client_id
join payment_methods as pm on p.payment_method=pm.payment_method_id;

-- 当在连接某些组合主键的表时，需要使用多个条件的连接语句
select *
from order_items oi
join order_item_notes oin
on oi.order_id = oin.order_id and oi.product_id = oin.product_id;

-- 使用left join或right join进行外连接
-- 一般来说使用left join就可以了，不建议使用right join
-- left join的意思就是保留左表中的数据，并在右边中筛选符合条件的数据
-- 如果左表中的一些行，没有在右表中找到符合条件的，则对应的列就是空
select order_date, order_id, c.first_name, s.name,  os.name
from orders as o
join customers as c on o.customer_id = c.customer_id
left join shippers as s on o.shipper_id = s.shipper_id
join order_statuses as os on o.status = os.order_status_id;

-- 使用`using`来简化join条件
select *
from order_items oi
join order_item_notes oin
using (order_id, product_id);

-- 自然连接`nartural join`
-- 让引擎自动去判断连接条件，这个例子中是使用customer_id
select * from customers natural join orders;

-- 交叉连接 cross join
-- 让两张表组合，比如一张表存放的是尺寸，一张表存放的是颜色
-- 那么就可以对这两张表进行cross join生成所有尺寸颜色组合
select * from size cross join color;
select * from size, color;
```

使用`union`可以让我们沿着行的方向进行结果合并。

```sql
-- 先筛选小于2000分的青铜
-- 再合并2000-3000分之间的白银
-- 最后合入3000分以上的黄金
select customer_id, first_name, points, 'Bronze' as type
from customers
where points < 2000
union
select customer_id, first_name, points, 'Silver' as type from customers
where points between 2000 and 3000
union
select customer_id, first_name, points, 'Gold' as type from customers
where points >=3000
order by first_name;
```

## 数据的插入、更新和删除

当我们全量字段插入时，对于主键，往往是自增的，指定default是最好的，对于可以为空的字段，我们可以设置有效的值，也可以设置为`null`，对于其他有默认值的字段，可以使用`default`设置字段为默认值。

```sql
insert into customers
values(default, 'John', 'Smith', '1990-01-01', null, 'address', 'city', 'CA', default);
```

指定特定的列，在这种情况下，我们可以自定义字段的顺序，比如下面的例子中，我们把`birth_date`调整到了第一位。

```sql
insert into customers (birth_date， first_name, last_name, address, city, state)
values('1990-01-01', 'John', 'Smith', 'address', 'city', 'CA');
```

一次插入多条数据，多条数据之间用逗号隔开。

```sql
insert into products(name, quantity_in_stock, unit_price)
values ('product 1', 12, 1.67), ('product 2', 3, 2.34), ('product 3', 7, 0.88);
```

利用`last_insert_id`来获取上次插入的数据的`id`

```sql
insert into orders(customer_id, order_date, status)
values (1, '2019-03-05', 1);

insert into order_items
values (last_insert_id(), 1, 1, 2.95), (last_insert_id(), 2, 1, 3.95);
```

使用`select`子句来进行数据插入

```sql
```

数据的更新示例

```sql
-- 使用有效值
update invoices
set payment_total = 86.32, payment_date = '2019-12-8'
where invoice_id = 3;

-- 使用默认值和空值
update invoices
set payment_total = default, payment_date = null
where invoice_id = 3;

-- 使用其他列的值
update invoices
set payment_total = invoice_total * 0.5, payment_date = due_date
where invoice_id = 3;
```

一次更新多行

```sql
-- 给1990年之前的用户的积分加50分
update customers
set points = points + 50
where birth_date < '1990-01-01';
```

使用子句编写复杂的数据查询

```sql
update invoices
set payment_total = invoice_total * 0.5, payment_date = due_date
where client_id in (select client_id from clients where state in('CA', 'NY'));
```
删除数据表中的指定的一行或多行：

```sql
delete from invoices
where client_id = (select client_id from clients where name = 'Nyworks')
```

## 数据统计与聚合

MySQL中支持的常用的统计函数有：

Function|功能描述
---|----
MAX()|返回最大值
MIN()|返回最小值
AVG()|返回平均数
SUM()|返回求和
COUNT()|返回总数目，不包括null值

```sql
select
    max(invoice_total) as highest,
    min(invoice_total) as lowest,
    avg(invoice_total) as average,
    sum(invoice_total * 1.1) as total,
    count(*) as total_records,
    count(distinct client_id) as client_number
from invoices
where invoice_date > '2019-07-01';
```

分组统计

```sql
select client_id, sum(invoice_total) as total
from invoices
where payment_date > '2019-01-01'
group by client_id
order by total DESC;
```

使用`having`进行分组后的筛选

```sql
select client_id, sum(invoice_total) as total
from invoices
where payment_date > '2019-01-01'
group by client_id
having total > 100;
```

分组汇总

使用`with rollup`可以对每个分组的结果进行再次汇总统计。

```sql
-- 按付款方式进行付款总额的统计
select pm.name as payment_method, sum(amount) as total
from payments
join payment_methods as pm on payment_method = pm.payment_method_id
group by pm.name with rollup;
```

```text
+----------------+--------+
| payment_method | total  |
+----------------+--------+
| Cash           | 10.00  |
| Credit Card    | 351.38 |
| <null>         | 361.38 |
+----------------+--------+
```

## 编写复杂的查询语句


我们可以在`where`语句的筛选条件中嵌入`select子句`，让筛选条件更加的灵活。

```sql
-- 查询员工表中所有薪水大于平均值的员工
-- 这里平均薪水就是使用了select子句
select * from employees 
where salary > (
    select avg(salary) from employees
);

-- 查询没有发票记录的客户信息
select * from clients
where client_id not in (
    select distinct client_id from invoices
);
```

我们可以使用`ALL、ANY`等关键字来完成单个值与集合的比较：

```sql
-- 查询所有发标金额大于客户3所有相关发票金额的发票
select * from invoice 
where invoice_total > all (
    select invoice_total from invoice where client_id = 3
);

-- 查询所有发票数量大于2张的客户
select * from clients
where client_id = any (
    select client_id from invoices group by client_id having count(*) > 2
);
```
这里使用`= any`实际上和`in`达到的效果是一样的。

使用`EXIST`

```sql
-- 查询所有没有被下过订单的商品

-- 第一种实现，它的缺点是后面的集合可能会特别大，影响性能
select * from products where product_id not in (
    select distinct product_id from order_items
);

-- 使用exists
select * from product as p where product_id not exists (
    select product_id from order_items where product_id = p.product_id
);
```

将查询的结果作为一个新表进行`join`

```sql
-- 查询所有工资高于同办公室平均工资的员工
select * from employees 
join (
    select office_id, avg(salary) as average_salary from employees
    group by office_id
) as office_average using (office_id)
where salary > average_salary;
```

## MySQL中的内置函数

内置函数主要用于处理数字、日期和文本。

### 常用的数值函数

函数|功能描述
---|---
ROUND(f,n=0)|对小数f进行四舍五入，保留n位小数
TRUNCATE(f,n=0)|对小数进行截断，保留n位小数
CEILING(f)|对小数f进行向上取整
FLOOR(f)| 对小数f进行向下取整
ABS(f)|对f取绝对值
RAND()|产生一个（0, 1)的随机小数


### 常用的字符串函数

函数|功能描述
---|---
LENGTH(str)|返回字符串str中字符的个数
UPPER(str|将字符串全部字符转换为大写
LOWER(str)|将字符串转换为小写
LTRIM(str)|去除字符串左边的空格
RTRIM(str)|去除字符串右边的空格
TRIM(str)|去除字符串两边的空格
LEFT(str,n)| 返回字符串最左边的n个字符
RIGHT(str,n)| 返回字符串最后n个字符
SUBSTRING(str, pos, n)| 返回字符串从pos位置（从1开始索引）开始的n个字符的子串
LOCATE(substr, str)|返回substr在str中第一次出现在位置，不区分大小写，搜索不到，则返回0
REPLACE(str, a, b)|将str中出现的字符串a替换为b
CONCAT(a, b)|将a和b两个字符串拼在一起

### 时间函数

函数|功能描述
---|---
NOW()|返回当前的时间点
CURDATE()|返回当前的日期
CURTIME()|返回当前的时间，不包括日期
YEAR(dt)|返回年
MONTH(dt)|返回月
MONTHNAME(dt)|返回月份字符串
DAY(dt)|返回日
DAYNAME(dt)|返回星期几
SECOND(dt)|秒
DATA_FORMAT(date, fomratstr)|可以将一个日期转为化一个自定义的格式
TIME_FORMAT(time, fomratstr)|可以将一个时间转为化一个自定义的格式
DATE_ADD(dt, INTERVAL n YEAR/...)|基于一个日期的加减
DATEDIFF(dt1, dt2)|计算两个日期之间的间隔
TIME_TO_SEC(time)|将时间转化为秒，以零点为起点


### IF语句

```sql
--- 如果条件为真，则返回value1，否则返回value2
if (condiction, value1, value2)
```

示例：对订单表进行筛选，如果是当前的订单，则显示活跃，否则显示已经归档

```sql
select order_id, 
       order_data, 
       if (YEAR(order_date) = YEAR(now()), 'Activae', 'Archived') as category
from orders;
```

### CASE语句

`case`语句可以处理多个分支的情况 

```sql
select order_id, 
       order_data, 
       case 
            when YEAR(order_date) = YEAR(now()) then 'Activate'
            when YEAR(order_date) < YEAR(now()) then 'Archived'
            else 'Future'
       end as category
from orders;
```

## 数据库视图

视图的三个作用：

* 简化查询
* 提供一种抽象机制来隔离变化
* 提供一种安全机制

### 简化查询

在复杂的查询语句中，我们的查询表很可能是基于另一个子查询语句的结果。另外也有一些查询我们会经常复用。在这些情况下我们可以使用视图的功能，可以将某一个复杂的查询的结果保存为一个视图，方便我们后续的复用。

创建视图使用的是`create view <view name> as select ...`这样的语法。

```sql
create view clients_balance as 
select c.client_id, name, sum(invoice_total)-sum(payment_total) as balance
from clients c
join invoices i using (client_id)
group by client_id, name;
```

对应着我们可以使用`drop view [if exists] <view name>`来删除视图。

如果我们想更新视图的schema，那么我样可以先drop掉原来的视图，再重新创建，我们也可以使用`create or replace <view name> as select ...`来创建或更新一个原有的视图。

### 对视图的更新（增、删、改）

我们可以对视图表进行查询，但如果我们想像更新普通的表一样更新视图，就有一些约束，包括：

* 不使用聚合函数（MAX, MIN, AVG, SUM等）
* 不使用group by 或 having子句
* select 或 from子句中不存在子查询，且where子句中的任何子查询都不引用from子句中的表
* 不使用union、union all 或 distinct
* 如果有多个表或视图，那么from子句只使用内部连接

另外，即使满足上面的3个条件，对视图的修改，可能会导致视图中的一些完整性约束失效，这时候，默认情况下，对应的行会从视图中丢失。比如对于上面我们创建的client_balance视图，如果我们更新payment_total，而没有更新blance，让会导致数据完整性约束失效。我们可以在创建视图语句的最后加上`WITH CHECK OPTION`来让这样的操作失败，而不是导致数据丢失。

对视图的更新会同步到原表（基表）上去，所以才会有上面的限制，这些限制条件都会使得对于视图的更新没有办法映射到原表上去。如果原来有3个字段，而视图只有2个字段，这时我们对视图中插入元素，必须要求原表图剩余的那个字段可以为空或有默认值。

### 视图提供了抽象机制

视图的另一方面的作用是，提供了一种抽象的机制，视图相当于一种约定的接口，这个接口下具体的实现（真实的表），我们可以自由实现，也可以不断的修改，只要不影响 视图的结构就行。

### 视图提供了安全机制

很多情况下，我们没有对一张表直接插入、更新、删除的权限，这个时候，我们可以创建一张该表的视图，然后在这个视图上进行修改。相反，我们的一些非常基础性的表，如果不想对外开放，我们就可以在其基础上创建一个视图，把一些内部（敏感的）列从视图排除掉，或者利用WHRER只筛选特定条件下的数据对外使用。

### 视图的存储原理

视图本身并没有存储数据，我们暂时可以把它想成一个sql代码函数，实际执行查询时，它会把这样的代码段嵌入到我们的查询中。但这样简单的理解无法解释，视图如何支持更新。

如果视图查询的表中的数据被更新了，那么视图这张虚拟表也会对应的更新。

MySQL中如何实现视图：TBD。

## 存储过程和函数

存储过程是一系列SQL语句组成的一个过程调用，它类似于函数，支持传入参数，但它与MySQL中函数不同的是，它可以返回直接返回一个数据表或多个值。存储过程有3个作用：

1. 存储和管理SQL代码：将一些复杂的数据库操作流程保存和管理起来，避免与应用程序逻辑代码耦合在一起。
2. 大部分的DBMS会对存储过程里的SQL语句进行优化
3. 可以不需要将整个数据库暴露出去，而只定义好一些存储过程，把这些存储过程暴露给用户，让用户不至于随意操作数据。

### 存储过程的创建、删除、更新和使用

```mysql
-- 由于存储过程中的语句都是以;结尾，为了和sql本身执行层面的语句区分开，先使用delimiter将sql的默认分隔符改为其他符号，一般改为双美元符
delimiter $$
create procedure get_clients()
begin
	-- sql1;
	-- sql2;
end$$
delimiter ;

-- 使用call关键字来调用存储过程
use sql_invoicing;
call get_clients();
-- 或者使用作用域的形式
call sql_invoicing.get_clients();

-- 删除存储过程
drop procedure if exists get_clients;

-- 修改存储过程，只需要将create改为alter就可以了
delimiter $$
alter procedure get_clients()
begin
	-- modified sql1;
	-- modified sql2;
end$$
delimiter ;
```

> tips：像views和procedures，我们在实际的应用程序中，我们最好用一些专题的文件夹来管理这些sql语句，比如views和procedures等。

### 带参数的存储过程

我们可以使用以下的语法来在创建带参数的存储过程，然后在后续的调用中，动态的传入不同的参数。

```mysql
use sql_invoicing;
drop procedure if exists get_clients_by_state;

delimiter $$

create procedure get_clients_by_state(state char(2))
begin
	-- 由于我们这里传的参数与数据表的字段同名，所以这里给表加一个alias name
	select * from clients as c where c.state = state;
end$$

delimiter ;

-- 带参数调用
call get_clients_by_state('CA');
```

如果我们想使用一些默认参数，比如像上面的`get_clients_by_state`，我们想通过传入`NULL`来表示，选择所有州的客户。

```mysql
create procedure get_clients_by_state(state char(2))
begin
	if state is null then
		select * from clients;
  else
		select * from clients as c where c.state = state;
	end if;
end$$
```

我们可以借助`ifnull`来简化上面的代码：

```mysql
create procedure get_clients_by_state(state char(2))
begin
	-- 当state为null时，筛选条件变成了c.state = c.state
	select * from clients as c where c.state = ifnull(state, c.state);
end$$
```

### 参数有效性判定

我们可以在存储过程的语句块中加入对于传入参数的有效性检查：

```mysql
if payment_amount <= 0 then
  -- 抛出错误，错误码为2203，错误信息为'invalid payment amout'
	signal sqlstate '22003' set message_text = 'Invalid payment amount';
end if;
```

需要注意的是：过犹不及（"Too much of a good thing is a bad thing"），加入过多的参数验证会让代码过于复杂难以维护，像 payment_amount 非空这样的验证就不需要添加因为 payment_amount 字段本身就不允许空值因此MySQL会自动报错。

### 输出参数

用的不多，下面给一个简单的示例，获取指定客户未支付过的发票的个数和总额。

```mysql
create procedure get_unpaid_invoices_for_client(
  client_id int, -- 输入参数
  out invoice_count int, -- 输出参数
  out invoice_total decimal(9,2)) -- 输出参数
begin
	select count(*), sum(i.invoice_total) into invoice_count, invoice_total
	from invoices i where i.client_id = client_id and payment_total = 0;
end
```

### 变量

一共有2种定义变量的方法

1. 用户或会话变量：`set @var=value`，使用的时候来是`@var`
2. 本地局部变量：`declare varname datetype [default value]`

变量的用法可以参考下面函数小节的例子。我们在函数体内定义了3个局部的变量。

### 函数

我们在前面介绍过很多MYSQL中的内置函数，比如聚合函数（AVG/SUM/MAX）、处理数值、文本、日期的函数，MYSQL同时允许我们自定义函数。

函数和储存过程的作用非常相似，唯一区别是函数只能返回单一值而不能返回多行多列的结果集，当你只需要返回一个值时就可以创建函数。另外，函数设置和函数体之间，有一段确定返回值类型和函数属性的语句段。

```mysql
DROP FUNCTION IF EXISTS get_risk_factor_for_client;

DELIMITER $$

CREATE FUNCTION get_risk_factor_for_client(client_id INT) 
RETURNS INTEGER
-- DETERMINISTIC：用于说明函数的输出只和输入相关，相同的输入一定会产生相同的输出
READS SQL DATA -- 用于说明函数内部会使用select来进行数据读取
-- MODIFIES SQL DATA：用于说明函数内部使用insert update 或 delete来更新数据
BEGIN
    DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;

    SELECT SUM(invoice_total), COUNT(*) INTO invoices_total, invoices_count
    FROM invoices i WHERE i.client_id = client_id;
    -- 注意不再是整体risk_factor而是特定顾客的risk_factor

    SET risk_factor = invoices_total / invoices_count * 5;
    RETURN IFNULL(risk_factor, 0);       
END

DELIMITER ;
```

调用函数的示例

```mysql
SELECT 
    client_id,
    name,
    get_risk_factor_for_client(client_id) AS risk_factor
FROM clients
```

## 触发器和事件

### 触发器的增、删、改、查

触发器的机制可以让我们在一个指定的数据表中的数据在插入、更新、删除时，自动的执行一系列SQL语句，这一系列的语句 ，可以用于更新相关联的表来保证数据完整性，也可以将这些更新记录到一个新表中，达到数据表审计的作用。

```mysql
-- 删除触发器
drop trigger if exists payment_after_insert;

delimiter $$

-- trigger的命名风格一般是：表名_after/before_insert/update/delete
create trigger payment_after_insert
	after insert on payments -- 触发条件语句
	for each row -- 触发频率
begin
	-- 使用NEW来引用新插入的一行记录或更新后的一行记录
	-- 使用OLD来引用删除前或更新前的一行记录
	update invoices set payment_total = payment_total + NEW.amount
	where invoice_id = NEW.invoice_id;
end$$

delimiter ;

-- 查看触发器
show triggers like 'payments%'
```

### 事件的使用

事件是一段根据计划执行的代码，可以执行一次，或者按某种规律执行，比如每天早上10点或每月一次

通过事件我们可以自动化数据库维护任务，比如删除过期数据、将数据从一张表复制到存档表 或者 汇总数据生成报告，所以事件十分有用。

MySQL的后台有一个事件调度器，它时刻寻找需求执行的事件。我们可以查看系统变量来确保MySQL启动了这个事件调度器：

```mysql
show variables like 'event%';
```

```text
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| event_scheduler | ON    |
+-----------------+-------+
```

我们也可以手动打开或关闭

```mysql
set global event_scheduler = ON/OFF;
```

例子：创建一个事件，每年执行一次，删除超过一年的日志记录。

```mysql
delimiter $$

create event yearly_delete_stale_audit_row
-- starts 和 ends是可省略的
-- 如果周期性的则使用`every`关键字，如果是一次性的，则使用`at`
on schedule every 1 year starts '2019-01-01' ends '2029-01-01'
do begin
	delete from payments_audit where action_data < now() - interval 1 year;
end$$

delimiter ;
```

事件的查看、删除、修改、启停

```mysql
show events like 'yearly%'; -- 查看每年运行的周期性事件

drop events if exists yearly_delete_stale_audit_row;

-- 修改的话，直接将create改为alter就可以了

-- 禁用或开启事件
alter event yearly_delete_stale_audit_row disable/enable;
```

## 数据库事务与并发

事务就是一组SQL语句构成的一个工作单元，这个工作单元中的语句都应成功完成，否则事务会运行失败，不会带来任何副作用。

### 事务的ACID属性

* Automicity 原子性： 一个事务中的所有语句就像原子一样不可拆分，要么都成功的被提交，要么被失败了，前面的已经执行的语句也会被撤销。
* Consistency 一致性：通过使用事务，数据库将始终保持一致的状态，比如不会出现有order记录，但order_item中没有记录的问题。
* Isolation 隔离性：事务之前独立执行，互不影响，如果有2个事务在操作相同的数据，那对应的数据行会被锁定，一次只有一个事备可以更新行。
* Durability 持久性：事务的对数据的影响一定会持久化到磁盘中，不会因为突然断电而导致丢失数据。

### 创建事务

```mysql
start transaction; -- 开始事务

insert into orders (customer_id, order_date, status) values (1, '2019-01-01', 1);
insert into order_items values (last_insert_id(), 1, 1, 1);

commit; -- 提交事务
```

### 事务的自动提交

MySQL中通过变量`autocommit`来控制是否对事务进行自动提交，如果是1，则是开启状态，对于事务中的语句，如果没有失败，则会自动提交。如果是0，则必须在事务结束时，在commit执行时对整个事务中的语句进行提交。

### 并发与锁定

并发就是指对于MySQL中的数据库和数据表，会存在多个用户同时操作的情况，这时如果一个用户正在读取一个正在被另一个用户修改或删除的数据时，就会存在问题。

并发导致的4种问题：

* Lost Updates：
* Dirty Reads：读取到的数据是另外一个事务执行中更新的，但另一个事务可以会失败或Rollback，导致读取的无效的数据。
* Non-repeating Reads：在事务中前后多次读取同一数据，会出现数据不一致的问题。
* Phantom Reads：一个事务正在使用条件过滤查询一些数据，比如积分大于1000的用户，但另一个事务正在对过滤中的一些条件列进行修改，把一些原本积极不足1000的用户更新为了超过1000的用户，这个事务会影响前面的事务的查询结果。

### 数据库的隔离级别

* READ UNCOMMITTED：基本无任何隔离，并发的事务可以彼此互相读取未Commit的数据。
* READ COMMITED：数据库将保证读取的数据一定是已经COMMIT的数据，不会存在，更新了但未提交的数据。这样可以避免“ 脏读”。
* REPEATABLE READ：它在READ COMMITTED基础上保证一个事务内对同一数据的多次读取是一致的。即使有另外一个事务在对这个数据做更新，那这个更新对当前的事务是不可见的。
* SERIALIZABLE：提供了最高的隔离级别，可以解决所有的并发问题。在这个级别下，我们的事务都是顺序执行的。

从上到下隔离级别越来越高，性能和并发就会受到越大的影响，但不会有太多的并发上的问题。 

```mysql
show variables like 'transaction_isolation';
set [session] transaction isolation level serializable;
```



## MySQL支持的数据类型

MySQL中一共支持大约5类的数据类型，包括了：String、Numeric、Date and Time、Blob、Spatial。

### String类型

* CHAR(x)：为固定长度的字符串，无论实际存储的字符串长度为多少，它都会占用x个字符的存储空间，最大的长度为255。

* VARCHAR(x)：变长存储的字符串，它会根据字符串的实际长度而动态的分配存储空间，但最大不超过x个字符空间。VARCHAR最大支持的长度为65535。
* TINYTEXT：最大可以存储255个字符
* TEXT：最大可以存储65535个字符，对于这个范围内的选择，我们最好是使用VARCHAR。
* MEDIUMTEXT：最长可以存储16MB的字符串
* LONGTEXT：最大可以存储4GB的字符串，可以用于存储一些日志等信息

需要注意的是上面字符串类型的存储能力都是按字符个数来定的，如果是存储非anscii码，那实际占用的存储空间，会更大一些。 

### 整数类型

* TINYINT（MySQL扩展类型）：占用1B空间，范围是：[-128, 127]
* SMALLINT：占用2B空间，范围是：[-32K, 32K]
* MEDIUMINIT（MySQL扩展类型）：占用BB空间，范围是：[-8M, 8M]
* INT/INTERGER：占用4B空间，范围是：[-2B, 2B]
* BIGINT（MySQL扩展类型）：占用8B空间，范围是：[-9Z, 9Z]

我们在使用整数类型时，也可以在后续加（x），它表示的是补0，如果数值位数不足x时，前面会补0。我们还可以在上述类型前面加上UNSIGNED用于表示无符号数，无符号数的范围是是从0开始的。

### 小数类型

* DECIMAL(p, s)：p表示小数的精度，也就是小数的位数，s表示小数点后的位置，它用于表示高精度的数字，比如在银行系统中。
* DEC、NUMERIC、FIXED 和DECIMAL是一个意思。
* FLOAT：4B浮点数，可以表示更大的数，但精度有限
* DOUBLE/REAL ：8B浮点数，比FLOAT可以表示的数的范围更大，精度也更高。

### 布尔类型

* BOOL/BOOLEAN：只会占用1个字节，它的字面值为TRUE和FALSE。

 ### 枚举类型

* ENUM(VALUE1, VALUE2, VALUE3)：枚举值整体构成了类型的定义。

实际中我们往往比较少的使用枚举类型，在需要用于枚举值的地方，我们往往会把枚举值列在一个单独的查询表（Lookup Table）中。

### 集合类型

* SET(VALUE1, VALUE2, VALUE2)：包含了有限个元素的集合，对应类型的字段可以是这个集合中若干个值构成的子集合。

ENUM和SET就类似于我们在Excel表格中的下拉选择框，枚举类型只允许选择一种，而集合类型则可以选择多种。

### DATE/TIME类型

* DATE：以YYYY-MM-DD格式的日期，在1000-01-01和9999-12-31之间。
* TIME：存储时间为HH::MM::SS格式
* DATETIME：占用8B，可以存储更大范围的时间点
* TIMESTAMP：占用4B，只能存储2038年之前的时间戳
* YEAR：以2位或4位数字格式来存储的年份，如果长度指定为2，即YEAR(2)，则年份就可以为1970至2069，如果长度指定为4，则年份范围为1901-2155，默认长度为4。

### Blob类型

我们可以用blob在数据库中存储图像、视频、PDF、word文件等，几乎所有的二进制数据。

* TINYBLOB: 255B
* BLOB: 65K
* MEDIUMBLOB: 16M
* LONGBLOB: 4GB 

不建议使用MySQL来存储大量的blob类型的数据，它会大大增加MySQL数据库的体积，导致我们不能很友好的对数据库进行备份，同时也会很影响性能，我们从数据库中读取blob数据，几乎一定比直接在文件系统中读取的慢。

### JSON类型

比如我们为我们`sql_store`数据库中的`products`表增加一列为`properties`用于表示商品的一些其他属性项，比如对于电视产品来说，我们可以如下更新它的properties字段。

```mysql
-- 直接使用json字符串来更新数据表中的json类型的行
update products
set properties = '
{
    "dimensions": [1, 2, 3],
    "weight": 10,
    "manufacturer": {"name": "sony"}
}
'
where product_id = 1;
```

除了用json字符串外，我们还可以用mysql提供的json函数（`JSON_OBJECT`、`JSON_ARRAY`）来构造json对象。

```mysql
update products
set properties = JSON_OBJECT(
  'weight', 10, 
  'dimensions', JSON_ARRAY(1,2,3),
  'manufacturer', JSON_OBJECT('name', 'sony')
)
where product_id = 1;
```

对JSON字段进行查询

 ```mysql
 select product_id, JSON_EXTRACT(properties, '$.weight') as weight
 from products
 where product_id=1;
 ```

简洁的写法：

 ```mysql
select product_id, 
       properties ->'$.weight' as weight, 
       properties -> '$dimensions[0]'as dimension , -- 对于数组，则直接用下标
       properties ->> '$.manufacturer.name' as manufacturer -- 加->>是为会去除双引号
from products
where product_id=1;
 ```

更新已经记录中已经存在的json对象的某一个字段。

```mysql
update products
set properties = JSON_SET(
  properties,
  '$.weight', 20,
  '$.age', 10 -- 新增字段
)
where product_id = 1;
```

`JSON_SET`函数接收原json对象`properties`以及要修改的字段和其值，返回了一个新的json对象。

同样的还有`JSON_REMOVE`：

```mysql
update products
set properties = JSON_REMOVE(
  properties,
  '$.age'
)
where product_id = 1;
```

### Spatial空间数据类型

**TBD**

## 数据库设计

是设计一个好的数据库或数据表，必须遵循一些基本的设计步骤，下面我们就来介绍其中的一些关键步骤。

### 数据建模

* 理解场景与需求
* 识别业务中的实体、事物或概念以及它们之间的关系，然后构建一个概念模型
* 构建逻辑模型，逻辑模型是独立于数据技术的抽象数据模型，我们在这一步往往会明确每个实体有哪些属性，以及每种属性的数据类型。
* 构建物理逻辑：在数据库之上构建具体的数据库与表，索引，主键，存储过程等等。

对于概念模型我们可以使用ER（Entity Relation）或者UML来表示。

### 实体之间的关系

在使用ER来描述数据时，我们将实体与实体之间的关系抽象为3种：一对一，一对多和多对多。在构建概念模型时，我们可以使用多对多，但在构建物理模型时，会受到数据库引擎本身的限制，像MySQL就是不支持多对多关系的，一般会通过一个链接表来解决。

### 选择主键

首先主键必须要能够唯一的标识数据表中的一行数据，其次主键一般选择的存储空间少或简单的类型，因为主键往往会作为别的表的外键，导致数据被重复存储，选择较轻量的类型，可以使得存储更加有效率。最后主键一般是不能够修改的字段。

### 外键的约束

对于一个选课系统来说，学生实体（students）和课程实体（coursers）之间是一个多对多的关系，一个学生可以选多门课，一门课也会被多个学生选择；在这里我们就会单独提取一个选课的实体（enrollments），用于记录学生选课这一事件，这个表也称为链接表。

对于enrollments表，我们可以选择它里面的student_id和course_id这两个外键作为它自己的联合主键，这样做的好处时，在插入数据时，可以有一定的数据校验的功能，比如避免一个学生多次选择同一门课（会产生相同的主键）。但它也有坏处，比如如果还有一张表的外键是enrollments表，那么它就必须也包括student_id和course_id。

对外键另外的约束行为就是，当我们对学生表或课程表的主键进行修改或删除时，enrollments表应该对应做一些数据调整，比如：如果我们将某一个学生从students中删除，那他对应的所有选课也应该在enrollments表中删除。当然我们也可以选择保留。

### 数据库的三大范式

 **第一范式**：每一行的所有列都应该有单一值，且不能出现重复列。这就要求取数据表中的列不能是一个列表。

**第二范式**：每一张表都应该只描述一种实体，表中的每一列都是在描述这个实体的属性。

**第三范式**：表中的列不应该派生自其他列，比如是通过其他列运算得出。

上面三大范式的核心思想都是想要去消除数据库中的数据冗余。

另外有一条很重要的建议是：不要过份的对所有概念进行建模，也不需要在很高的抽象层次进行建模来应对未来的需求，只需要为现下问题制定最佳解决方案就行。为当前的需求建模，而不是为整个世界建模。

## 数据库的创建、删除、属性设置

### database的创建与删除

```sql
-- 创建数据库
CREATE DATABASE IF NOT EXISTS my_database;
-- 删除数据库
DROP DATABASE IF EXISTS my_databases;
-- 使用数据库
USE my_dtabase;
```

使用select子句来创建一个数据表

```sql
create table invoices_archived as
select invoice_id, number, c.name as client, invoice_total, payment_total, invoice_date, payment_date, due_date
from invoices
join clients as c using (client_id)
where payment_date is not null;
```
###  table的创建与删除

```mysql
-- 创建数据表
create table if not exists customers(
	customer_id int primary key auto_increment,
    first_name varchar(50) not null,
    last_name varchar(50) not null,
    points int not null default 0,
    email varchar(50) not null unique
);
-- 删除数据表
drop table if exists customers;
-- 修改数据表
alter table customers 
	add address varchar(50) not null after last_name,
	add phone varchar(50) not null after address,
	modify column first_name varchar(55) default '',
	drop points;
```

### 主键的添加与删除

```mysql
-- 对一个已经存在的表的主键进行增加或删除
alter table orders
	add primary key (order_id, other), -- 添加主键
	drop primary key; -- 删除主键时不用指定对应的字段
```

### 外键的创建

```mysql
-- 在创建表时添加外键及约束
create table if not exists orders (
	order_id int primary key,
    customer_id int not null,
    foreign key fk_orders_customers (customer_id) 
    	references customers (customer_id) 
    	on update cascade
    	on delete no action
);
-- 在一个已经存在的表中修改外键或约束
alter table orders
	drop foreign key fk_order_customers,
	add foreign key fk_orders_customers (customer_id) 
    	references customers (customer_id) 
    	on update cascade
    	on delete no action
```

### 数据库与表的字符集与排序规则

字符集决定了对于一些非英语国家的文字，mysql内如何来编码存储。排序规则决定了对于对应的文字，怎么规定它的排序规则。

比如字符集为utf-8，默认的排序规则是uft8_general_ci，其中ci表示case in-insensitive（不区分大小写）。

我们可以在数据库、表、列这三个粒度上设置字符集与排序规则，一般来说我们不需要手动修改排序规则。

```mysql
-- 指定与修改数据库的字符集
create database db_name character set latin1;
alter database db_name character set utf8;
-- 指定与修改数据表的字符集
create table table1(
) character set latin1;
alter table table1 character set utf8;
-- 指定和修改某一列的字符集
create table table1(
	first_name varchar(50) character set latin1 not null
);
alter table table1 modify column first_name varchar(50) character set utf8 not null;
```

### 存储引擎

```mysql
alter table customers engine = InnoDB;
```

## 创建与使用索引

### 使用索引的好处

使用索引可以显著提供查询的性能。 索引本质是数据库引擎用来快速查找数据的数据结构，一般上来说都是一种树状的结构，InnoDB引擎使用的是B+树。索引往往比较小，所以可以很方便的放到内存中，来获取较好的查找效率。但是使用索引也不是完全没有代价的：

* 使用索引会增大数据库的存储空间，因为index是在数据表之外独立存储的。
* 拖慢数据库的操作，当我们对数据进行增、删、改时，都需要额外的同时更新索引。

基于上面的考虑，当前我们使用索引时，应该只为核心查询提供索引，而不是为数据表的所有列都建立索引。

### 在MySQL中创建、删除与查看索引

```mysql
-- 创建索引
create index idx_state on customers (state);

-- 查看索引
show indexes in customers;

-- 删除索引
drop index idx_state on customers;
```

 MySQL为自动的为主键和外键创建索引。

### 创建前缀索引

当我们对类型为CHAR、VARCHAR、TEXT、BLOB的列创建索引时，索引往往会占用较大的存储空间。

这个时候我们可以只对字段的一部分建立索引：

```mysql
-- 为last_name列建立索引，只使用最多前20个字符
create index idx_lastname on customers (last_name(20))
```

### 全文索引

 ```mysql
 -- 创建全文索引
 create fulltext index idx_title_body on posts (titile, body);
 -- 用关键字查找
 select * from posts where match(title, body) against('react redux');
 ```

### 复合检索

问题的引入：

```mysql
select customer_id from customers where state = 'CA' and points > 1000;
```

虽然我们在state和points列上都建立了索引，但执行上面的检索语句时，MySQL只会选择一个索引，这里会选择idx_state，先定位到state='CA'的所有行，再全部扫描一遍找到points>1000的记录。

```mysql
-- 在state和points两列上建立联合索引
create index idx_state_points on customers (state, points);
```

MySQL最多可以为16个列同时建立索引。

当我们使用复合检索时，需要注意这些列的顺序，一般来说有3点建议：1）经常查询的列放在前面 ；2）将基数大的列放在前面。3）充分结合自己的查询需求。

### 索引失效的问题

```mysql
-- 下面的查询将导致会搜索数据库的所有行
select customer_id from customers where state = 'CA' or points > 1000;
-- 我们可以优化为
select customer_id from customers where state = 'CA'
union
select customer_id from customers where points > 1000;
```

### 使用索引的加速排序

当我们在没有索引的列上进行排序时，将会使用fileSort，而在使用索引的列上排序时，则使用的是index排序。性能往往会相差10倍。

### 覆盖索引

covering index指的是我们查询的数据（比如primary key）可以直接从index中读取，而不用读取数据表。

### 使用索引的建议

先看看where子句后面的列是否有索引，再看看order by的列是否有索引，最后看一下select的内容是否在索引中。

## 数据库安全

### 创建用户

```mysql
-- 创建一个用户john并同时设置他的密度
create user john identified by '1234'
-- 创建一个用户john，同时限制他只能通过codewithmosh.com以及子域来连接数据库
create user john@'%.codewithmosh.com' identified by '1234'
```

### 查看所有用户

```mysql
select * from mysql.user;
```

### 删除用户

```mysql
drop user bob@hostname;
```

### 修改密码

```mysql
-- 设置别的账号的密码
set password for john = '1234'
-- 修改自己账号的密码
set password = '1234'
```

### 授予权限

创建一个普通用户，并授予sql_store数据库中所有表的查询、插入、更新、删除、执行权限。

```mysql
create user moon_app identified by '1234';
grant select, insert, update, delete, execute on sql_store.* to moon_app;
```

创建一个数据库管理员用户，`all`表示所有权限，`*.*`表示所有数据库的所有表格。

```mysql
grant all on *.* to amdin;
```

### 查看权限

```mysql
show grants for john;
```

### 撤销权限

```mysql
revoke create view on sql_store.* from moon_app;
```

相比较于授予权限，撤销权限，只需要把grant改为revoke，把to改为from。


## 参考资料

- [CodeWithMosh MySQL Course](https://www.bilibili.com/video/BV1UE41147KC)
- [MySQL Tutorial](https://www.mysqltutorial.org)
- [MySQL必知必会](https://m.douban.com/book/subject/3354490/)
- [数据库设计工具: DBDiagram](https://dbdiagram.io/)
- [数据库数据标记语言：DBML](https://www.dbml.org/)
- [mycli: 强大的MySQL命令行工具](https://github.com/dbcli/mycli)