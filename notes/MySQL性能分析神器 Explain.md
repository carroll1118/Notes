@[toc]
> EXPLAIN 显示了 MySQL 如何使用索引来处理 SELECT 语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。
#  推荐阅读
* [linux安装MySQL5.7数据库](https://blog.csdn.net/qq_40722827/article/details/105015278)
* [MySQL索引建立选择和常见失效原因总结，这些你都得知道](https://blog.csdn.net/qq_40722827/article/details/105214794)
* [数据库索引（Index）实现原理，面试官常问~~~](https://blog.csdn.net/qq_40722827/article/details/103104067)
* [MySql性能优化之JOIN连接(有图，最全，最详细)](https://blog.csdn.net/qq_40722827/article/details/103119274)
* [MySQL高级性能优化知识，这些面试常问的东西你都知道吗？](https://blog.csdn.net/qq_40722827/article/details/105216079)
# Explain
### 查询执行计划
* 使用explain关键字,可以模拟优化器执行的SQL语句
* 从而知道MYSQL是如何处理sql语句的
* 通过Explain可以分析查询语句或表结构的性能瓶颈
### 作用
* 查看表的读取顺序
* 数据读取操作的操作类型
* 查看哪些索引可以使用
* 查看哪些索引被实际使用
* 查看表之间的引用
* 查看每张表有多少行被优化器执行
### 使用方法
* 在 SELECT 语句前加上 EXPLAIN 即可，如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401102717172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
### 分析包含信息
##### id
*  SELECT 识别符。这是 SELECT 的查询序列号.
```sql
EXPLAIN SELECT * FROM department d, 
(SELECT * FROM employee GROUP BY dep_id) t 
WHERE d.id = t.dep_id;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401103246885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_1,color_FFFFFF,t_70#pic_center)
* 表执行顺序：`employee` --> `d` --> `<derived2>`
* 总结：`id相同,顺序走；不同,看谁大，大的先执行`。
#####  select_type
* SELECT类型,可以为以下任何一种:
	* `SIMPLE`: 简单select查询,查询中不包含子查询或者UNION
	* `PRIMARY`: 查询中若包含任何复杂的子查询,最外层查询则被标记为primary.
		* ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401104555411.png#pic_center)
	* `UNION`: 若第二个select出现的union之后,则被标记为union；若union包含在from子句的子查询中,外层select将被标记为deriver
	* `DEPENDENT UNION`: UNION 中的第二个或后面的 SELECT 语句,取决于外面的查询
	* `UNION RESULT`: 从union表获取结果select，两个UNION合并的结果集在最后
		* ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401130306308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
	* `SUBQUERY`: 在select或where中包含了子查询.
		* ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401130019776.png#pic_center)
	* `DEPENDENT SUBQUERY`: 子查询中的第一个 SELECT,取决于外面的查询
	* `DERIVED`: 在from列表中包含的子查询被标记为derived(衍生),把结果放在临时表当中
		* ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200401130144979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
#####  table
* 输出的行所引用的表（显示这一行的数据是关于哪张表的）
#####  partitions
* 表分区（如果查询是基于分区表的话, 会显示查询访问的分区）
#####  type
* `system`: 表仅有一行(=系统表)。这是 const 联接类型的一个特例。
* `const`: 表最多有一个匹配行,const用于比较primary 或者 unique索引。直接查询主键或者唯一索引,它将在查询开始时被读取。因为仅有一行,在这行的列值可被优化器剩余部分认为是常数。const 表很快,因为它们只读取一次!
* `eq_ref`: 唯一性索引扫描。对于每个索引键,表中只有一条记录与之匹配。常见于主键或唯一索引扫描，对于每个来自于前面的表的行组合, 从该表中读取一行。这可能是最好的联接类型, 除了 const 类型。
* `ref`: 非唯一性索引扫描。对于每个来自于前面的表的行组合, 所有有匹配索引值的行将从这张表中读取。
* `ref_or_null`: 该联接类型如同 ref,但是添加了 MySQL 可以专门搜索包含 NULL 值的行。
* `index_merge`: 该联接类型表示使用了索引合并优化方法。
* `unique_subquery`: 该类型替换了下面形式的 IN 子查询的 ref: `value IN (SELECT primary_key FROM single_table WHERE some_expr) unique_subquery` 是一个索引查找函数, 可以完全替换子查询, 效率更高。
* `index_subquery`: 该联接类型类似于 unique_subquery。可以替换 IN 子查询, 但只适合下列形式的子查询中的非唯一索引: `value IN (SELECT key_column FROM single_table WHERE some_expr)`
* `range`: 只检索给定范围的行,使用一个索引来选择行。
* `index`: 该联接类型与 ALL 相同,除了只有索引树被扫描。这通常比 ALL 快,因为索引文件通常比数据文件小。
* `ALL`: 对于每个来自于先前的表的行组合, 进行完整的表扫描。

**要求：一般来说,保证查询至少达到range级别，最好能达到ref**
#####  possible_keys
* 显示可能应用在这张表中的索引,一个或者多个；查询涉及到的字段上若存在索引,则该索引将被列出,但不一定被查询实际使用
#####  key
* 实际使用的索引,如果为NULL,则没有使用索引 ；查询中若使用了覆盖索引 ,则该索引仅出现在key列表中；`possible_keys与key关系 ：理论应该用到哪些索引，实际用到了哪些索引；`覆盖索引 查询的字段和建立的字段刚好吻合,这种我们称为覆盖索引。
#####  key_len
* 表示索引中使用的字节数,可通过该列计算查询中使用的索引长度 。如果键是 NULL, 则长度为 NULL。
#####  ref
* 显示使用哪个列或常数与 key 一起从表中选择行。索引是否被引入到, 到底引用到了哪几个索引
#####  rows
* 显示 MySQL 认为它执行查询时必须检查的行数。多行之间的数据相乘可以估算要处理的行数。根据表统计信息及索引选用情况,大致估算出找到所需的记录所需要读取的行数，每长表有多少行被优化器查询过。
#####  filtered
* 显示了通过条件过滤出的行数的百分比估计值。满足查询的记录数量的比例，注意是百分比，不是具体记录数；filtered列的值依赖统计信息，并不十分准确。
#####  Extra
* 该列包含 MySQL 解决查询的详细信息
	* `Distinct`: MySQL 发现第 1 个匹配行后,停止为当前的行组合搜索更多的行。
	* `Not exists`: MySQL 能够对查询进行 LEFT JOIN 优化, 发现 1 个匹配 LEFT JOIN 标准的行后, 不再为前面的的行组合在该表内检查更多的行。
	* `range checked for each record (index map: #)`: MySQL 没有发现好的可以使用的索引, 但发现如果来自前面的表的列值已知, 可能部分索引可以使用。
	* `Using filesort`: MySQL 需要额外的一次传递, 以找出如何按排序顺序检索行。
	* `Using index`: 从只使用索引树中的信息而不需要进一步搜索读取实际的行来检索表中的列信息。
	* `Using temporary`: 为了解决查询, MySQL 需要创建一个临时表来容纳结果。
	* `Using where`: WHERE 子句用于限制哪一个行匹配下一个表或发送到客户。
	* `Using sort_union(...), Using union(...), Using intersect(...)`: 这些函数说明如何为 index_merge 联接类型合并索引扫描。
	* `Using index for group-by`: 类似于访问表的 Using index 方式,Using index for group-by 表示 MySQL 发现了一个索引,可以用来查询 GROUP BY 或 DISTINCT 查询的所有列, 而不要额外搜索硬盘访问实际的表。

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
