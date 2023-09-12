# 一、安装

## 1.1 docker 安装server

**1.创建相关配置目录**

```
mkdir -P /data/clickhouse/data
mkdir -P /data/clickhouse/conf
mkdir -P /data/clickhouse/log
```

**2.拉取镜像**

```
# 下载最新版本clickhouse

docker pull clickhouse/clickhouse-server

# 下载指定版本clickhouse

docker pull clickhouse/clickhouse-server:23.1.3.5-alpine
```


**3.查看 Network ports | ClickHouse Docs 中端口号配置 ，暂时只需要映射8123 9000 两个端口**

![img](readme.assets/f0dd70ae7fe04b818d0a4ca2bf9af390.png)

**4.创建临时容器，用以生成配置文件**

```
# 容器关闭后会自动删除掉

docker run -d --rm --name clickhouse-server --ulimit nofile=262144:262144 clickhouse/clickhouse-server
```

**5.将配置文件复制到 /data/clickhouse/conf 路径下**

```
docker cp clickhouse-server:/etc/clickhouse-server/config.xml /data/clickhouse/conf/config.xml
docker cp clickhouse-server:/etc/clickhouse-server/users.xml /data/clickhouse/conf/users.xml
```

**6.关闭临时容器**

```
docker stop clickhouse-server
```

**7.启动容器**

```
docker run -d --name=clickhouse-server \
-p 8123:8123 -p 9090:9000 \
--ulimit nofile=262144:262144 \
-v /data/clickhouse/data:/var/lib/clickhouse:rw \
-v /data/clickhouse/conf/config.xml:/etc/clickhouse-server/config.xml \
-v /data/clickhouse/conf/users.xml:/etc/clickhouse-server/users.xml \
-v /data/clickhouse/log:/var/log/clickhouse-server:rw \
clickhouse/clickhouse-server
```


**8.连接clickhouse**
使用dbeaver创建连接，选择clickhouse—输入ip+port（有账户密码的需输入对应信息）—测试连接，成功后点击确定即可。

![img](readme.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfMjUwMzc5Mjk=,size_20,color_FFFFFF,t_70,g_se,x_16.png)

![img](readme.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfMjUwMzc5Mjk=,size_20,color_FFFFFF,t_70,g_se,x_16-20230908172651614.png)

![img](readme.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfMjUwMzc5Mjk=,size_20,color_FFFFFF,t_70,g_se,x_16-20230908172702637.png)



## 1.2 docker 安装clickhouse-client 

```
docker pull yandex/clickhouse-client:20.3.21.2
```

启动

```
docker run --name liucf-clickhouse-client  -it --rm --link liucf-clickhouse-server:clickhouse-server-alias docker.io/yandex/clickhouse-client:20.3.21.2 --host 192.168.12.14 --port 9000 --user root --password 888888
```



# 二、简介

## 2.1 客户端

https://clickhouse.com/docs/zh/interfaces/http

## 2.2 数据格式

以下是支持的格式:

| 格式                                                         | 输入 | 输出 |
| ------------------------------------------------------------ | ---- | ---- |
| [TabSeparated](https://clickhouse.com/docs/zh/interfaces/formats#tabseparated) | ✔    | ✔    |
| [TabSeparatedRaw](https://clickhouse.com/docs/zh/interfaces/formats#tabseparatedraw) | ✔    | ✔    |
| [TabSeparatedWithNames](https://clickhouse.com/docs/zh/interfaces/formats#tabseparatedwithnames) | ✔    | ✔    |
| [TabSeparatedWithNamesAndTypes](https://clickhouse.com/docs/zh/interfaces/formats#tabseparatedwithnamesandtypes) | ✔    | ✔    |
| [Template](https://clickhouse.com/docs/zh/interfaces/formats#format-template) | ✔    | ✔    |
| [TemplateIgnoreSpaces](https://clickhouse.com/docs/zh/interfaces/formats#templateignorespaces) | ✔    | ✗    |
| [CSV](https://clickhouse.com/docs/zh/interfaces/formats#csv) | ✔    | ✔    |
| [CSVWithNames](https://clickhouse.com/docs/zh/interfaces/formats#csvwithnames) | ✔    | ✔    |
| [CustomSeparated](https://clickhouse.com/docs/zh/interfaces/formats#format-customseparated) | ✔    | ✔    |
| [Values](https://clickhouse.com/docs/zh/interfaces/formats#data-format-values) | ✔    | ✔    |
| [Vertical](https://clickhouse.com/docs/zh/interfaces/formats#vertical) | ✗    | ✔    |
| [JSON](https://clickhouse.com/docs/zh/interfaces/formats#json) | ✗    | ✔    |
| [JSONAsString](https://clickhouse.com/docs/zh/interfaces/formats#jsonasstring) | ✔    | ✗    |
| [JSONStrings](https://clickhouse.com/docs/zh/interfaces/formats#jsonstrings) | ✗    | ✔    |
| [JSONCompact](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompact) | ✗    | ✔    |
| [JSONCompactStrings](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompactstrings) | ✗    | ✔    |
| [JSONEachRow](https://clickhouse.com/docs/zh/interfaces/formats#jsoneachrow) | ✔    | ✔    |
| [JSONEachRowWithProgress](https://clickhouse.com/docs/zh/interfaces/formats#jsoneachrowwithprogress) | ✗    | ✔    |
| [JSONStringsEachRow](https://clickhouse.com/docs/zh/interfaces/formats#jsonstringseachrow) | ✔    | ✔    |
| [JSONStringsEachRowWithProgress](https://clickhouse.com/docs/zh/interfaces/formats#jsonstringseachrowwithprogress) | ✗    | ✔    |
| [JSONCompactEachRow](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompacteachrow) | ✔    | ✔    |
| [JSONCompactEachRowWithNamesAndTypes](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompacteachrowwithnamesandtypes) | ✔    | ✔    |
| [JSONCompactStringsEachRow](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompactstringseachrow) | ✔    | ✔    |
| [JSONCompactStringsEachRowWithNamesAndTypes](https://clickhouse.com/docs/zh/interfaces/formats#jsoncompactstringseachrowwithnamesandtypes) | ✔    | ✔    |
| [TSKV](https://clickhouse.com/docs/zh/interfaces/formats#tskv) | ✔    | ✔    |
| [Pretty](https://clickhouse.com/docs/zh/interfaces/formats#pretty) | ✗    | ✔    |
| [PrettyCompact](https://clickhouse.com/docs/zh/interfaces/formats#prettycompact) | ✗    | ✔    |
| [PrettyCompactMonoBlock](https://clickhouse.com/docs/zh/interfaces/formats#prettycompactmonoblock) | ✗    | ✔    |
| [PrettyNoEscapes](https://clickhouse.com/docs/zh/interfaces/formats#prettynoescapes) | ✗    | ✔    |
| [PrettySpace](https://clickhouse.com/docs/zh/interfaces/formats#prettyspace) | ✗    | ✔    |
| [Protobuf](https://clickhouse.com/docs/zh/interfaces/formats#protobuf) | ✔    | ✔    |
| [ProtobufSingle](https://clickhouse.com/docs/zh/interfaces/formats#protobufsingle) | ✔    | ✔    |
| [Avro](https://clickhouse.com/docs/zh/interfaces/formats#data-format-avro) | ✔    | ✔    |
| [AvroConfluent](https://clickhouse.com/docs/zh/interfaces/formats#data-format-avro-confluent) | ✔    | ✗    |
| [Parquet](https://clickhouse.com/docs/zh/interfaces/formats#data-format-parquet) | ✔    | ✔    |
| [Arrow](https://clickhouse.com/docs/zh/interfaces/formats#data-format-arrow) | ✔    | ✔    |
| [ArrowStream](https://clickhouse.com/docs/zh/interfaces/formats#data-format-arrow-stream) | ✔    | ✔    |
| [ORC](https://clickhouse.com/docs/zh/interfaces/formats#data-format-orc) | ✔    | ✔    |
| [RowBinary](https://clickhouse.com/docs/zh/interfaces/formats#rowbinary) | ✔    | ✔    |
| [RowBinaryWithNamesAndTypes](https://clickhouse.com/docs/zh/interfaces/formats#rowbinarywithnamesandtypes) | ✔    | ✔    |
| [Native](https://clickhouse.com/docs/zh/interfaces/formats#native) | ✔    | ✔    |
| [Null](https://clickhouse.com/docs/zh/interfaces/formats#null) | ✗    | ✔    |
| [XML](https://clickhouse.com/docs/zh/interfaces/formats#xml) | ✗    | ✔    |
| [CapnProto](https://clickhouse.com/docs/zh/interfaces/formats#capnproto) | ✔    | ✗    |
| [LineAsString](https://clickhouse.com/docs/zh/interfaces/formats#lineasstring) | ✔    | ✗    |
| [Regexp](https://clickhouse.com/docs/zh/interfaces/formats#data-format-regexp) | ✔    | ✗    |
| [RawBLOB](https://clickhouse.com/docs/zh/interfaces/formats#rawblob) | ✔    | ✔    |



# 三、MergeTree表引擎

## 3.1 库引擎

默认 Atomic

它支持非阻塞的[DROP TABLE](https://clickhouse.com/docs/zh/engines/database-engines/atomic#drop-detach-table)和[RENAME TABLE](https://clickhouse.com/docs/zh/engines/database-engines/atomic#rename-table)查询和原子的[EXCHANGE TABLES t1 AND t2](https://clickhouse.com/docs/zh/engines/database-engines/atomic#exchange-tables)查询。默认情况下使用`Atomic`数据库引擎。

**创建数据库**

```sql
  CREATE DATABASE test[ ENGINE = Atomic];
```



**使用方式**

### 3.1.1 **Table UUID**

数据库`Atomic`中的所有表都有唯一的[UUID](https://clickhouse.com/docs/zh/sql-reference/data-types/uuid)，并将数据存储在目录`/clickhouse_path/store/xxx/xxxyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy/`，其中`xxxyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy`是该表的UUID。

通常，UUID是自动生成的，但用户也可以在创建表时以相同的方式显式指定UUID(不建议这样做)。可以使用 [show_table_uuid_in_table_create_query_if_not_nil](https://clickhouse.com/docs/zh/operations/settings/settings#show_table_uuid_in_table_create_query_if_not_nil)设置。显示UUID的使用`SHOW CREATE`查询。例如:

```sql
CREATE TABLE name UUID '28f1c61c-2970-457a-bffe-454156ddcfef' (n UInt64) ENGINE = ...;
```



### 3.1.2 **RENAME TABLES**

`RENAME`查询是在不更改UUID和移动表数据的情况下执行的。这些查询不会等待使用表的查询完成，而是会立即执行。

### 3.1.3 DROP/DETACH TABLES

在`DROP TABLE`上，不删除任何数据，数据库`Atomic`只是通过将元数据移动到`/clickhouse_path/metadata_dropped/`将表标记为已删除，并通知后台线程。最终表数据删除前的延迟由[database_atomic_delay_before_drop_table_sec](https://clickhouse.com/docs/zh/operations/server-configuration-parameters/settings#database_atomic_delay_before_drop_table_sec)设置指定。

可以使用`SYNC`修饰符指定同步模式。使用[database_atomic_wait_for_drop_and_detach_synchronously](https://clickhouse.com/docs/zh/operations/settings/settings#database_atomic_wait_for_drop_and_detach_synchronously)设置执行此操作。在本例中，`DROP`等待运行 `SELECT`, `INSERT`和其他使用表完成的查询。表在不使用时将被实际删除。

### 3.1.4 EXCHANGE TABLES

`EXCHANGE`以原子方式交换表。因此，不是这种非原子操作：

```sql
RENAME TABLE new_table TO tmp, old_table TO new_table, tmp TO old_table;
```

可以使用一个原子查询：

```sql
EXCHANGE TABLES new_table AND old_table;
```



### 3.1.5 ReplicatedMergeTree in Atomic Database

对于[ReplicatedMergeTree](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/replication#table_engines-replication)表，建议不要在ZooKeeper和副本名称中指定engine-path的参数。在这种情况下，将使用配置的参数[default_replica_path](https://clickhouse.com/docs/zh/operations/server-configuration-parameters/settings#default_replica_path)和[default_replica_name](https://clickhouse.com/docs/zh/operations/server-configuration-parameters/settings#default_replica_name)。如果要显式指定引擎的参数，建议使用{uuid}宏。这是非常有用的，以便为ZooKeeper中的每个表自动生成唯一的路径。





## 3.2 表引擎 MergeTree

[-](https://zhuanlan.zhihu.com/p/569542193)

表引擎（即表的类型）决定了：

- 数据的存储方式和位置，写到哪里以及从哪里读取数据
- 支持哪些查询以及如何支持。
- 并发数据访问。
- 索引的使用（如果存在）。
- 是否可以执行多线程请求。
- 数据复制参数。

```
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1] [TTL expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2] [TTL expr2],
    ...
    INDEX index_name1 expr1 TYPE type1(...) GRANULARITY value1,
    INDEX index_name2 expr2 TYPE type2(...) GRANULARITY value2
) ENGINE = MergeTree()
ORDER BY expr
[PARTITION BY expr]
[PRIMARY KEY expr]
[SAMPLE BY expr]
[TTL expr [DELETE|TO DISK 'xxx'|TO VOLUME 'xxx'], ...]
[SETTINGS name=value, ...]
```

- **ENGINE**：ENGINE = MergeTree()，MergeTree引擎没有参数。
- ORDER BY：排序字段。比如ORDER BY (Col1, Col2)，值得注意的是，如果没有使用 PRIMARY KEY 显式的指定主键ORDER BY排序字段自动作为主键。如果不需要排序，则可以使用 ORDER BY tuple() 语法，这样的话，创建的表也就不包含主键。这种情况下，ClickHouse会按照插入的顺序存储数据。必选项。
- **PARTITION BY：**分区字段，例如要按月分区，可以使用表达式 toYYYYMM(date_column 具有Date的列)，这里的date_column是一个Date类型的列，分区名的格式会是"YYYYMM"。**可选。**
- **PRIMARY KEY：**指定主键，如果排序字段与主键不一致，可以单独指定主键字段。否则默认主键是排序字段。大部分情况下不需要再专门指定一个 PRIMARY KEY 子句，注意，**在MergeTree中主键并不用于去重，而是用于索引，加快查询速度。可选。**

另外，**如果指定了PRIMARY KEY与排序字段不一致，要保证PRIMARY KEY 指定的主键是ORDER BY 指定字段的前缀**，比如：

```text
--允许
... ...
ORDER BY (A,B,C)
PRIMARY KEY A

--报错
... ...
ORDER BY (A,B,C)
PRIMARY KEY B
DB::Exception: Primary key must be a prefix of the sorting key
```



这种强制约束保障了即便在两者定义不同的情况下，主键仍然是排序键的前缀，不会出现索引与数据顺序混乱的问题。

- **SAMPLE BY：**采样字段，如果指定了该字段，那么主键中也必须包含该字段。比如 SAMPLE BY intHash32(UserID) ORDER BY (CounterID, EventDate, intHash32(UserID))。**可选。**
- TTL：数据存活时间，可以为某一字段列或者一整张表设置TTL，设置中必须包含Date或DateTime字段类型。如果设置在列上，那么会删除字段中过期的数据。如果设置的是表级的TTL，那么会删除表中过期的数据。如果设置了两种类型，那么按先到期的为准。例如，TTL createtime + INTERVAL 1 DAY，即一天后过期。使用场景包括定期删除数据，或者定期将数据进行归档。

规则的类型 `DELETE|TO DISK 'xxx'|TO VOLUME 'xxx'`指定了当满足条件（到达指定时间）时所要执行的动作：移除过期的行，还是将数据片段（如果数据片段中的所有行都满足表达式的话）移动到指定的磁盘（`TO DISK 'xxx'`) 或 卷（`TO VOLUME 'xxx'`）。默认的规则是移除（`DELETE`）。可以在列表中指定多个规则，但最多只能有一个`DELETE`的规则。

[TTL](https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/mergetree#table_engine-mergetree-ttl)

- **SETTINGS：**额外的参数配置。**可选。**



### 3.2.1 **MergeTree引擎表目录解析**

创建表t_mt，并加载数据：

```text
CREATE TABLE t_mt
(
    `id` UInt8,
    `name` String,
    `age` UInt8,
    `birthday` Date,
    `location` String
)
ENGINE = MergeTree
PARTITION BY toYYYYMM(birthday)
ORDER BY (id, age)

#向表t_mt中插入数据
insert into t_mt values (1,'张三',18,'2021-06-01','上海'), (2,'李四',19,'2021-02-10','北京'), (3,'王五',12,'2021-06-01','天津'), (1,'马六',10,'2021-06-18','上海'), (5,'田七',22,'2021-02-09','广州');
```

以上创建好表t_mt，当插入数据完成后，在ClickHouse节点/var/lib/clickHouse/data/test/路径下会生成对应目录“t_mt”,进入此目录下，可以看到对应的分区目录，如图示：

![image-20230908195816988](readme.assets/image-20230908195816988.png)



以上分区目录也可以在系统表“system.parts”中查询得到：

```
8b28ea5ff09b :) select table,partition,name,active from system.parts where table='t_mt'

SELECT
    table,
    partition,
    name,
    active
FROM system.parts
WHERE table = 't_mt'

Query id: 3b850ecf-c596-40ca-9a4b-a6e55d34006d

┌─table─┬─partition─┬─name─────────┬─active─┐
│ t_mt  │ 202102    │ 202102_2_2_0 │      1 │
│ t_mt  │ 202106    │ 202106_1_1_0 │      1 │
└───────┴───────────┴──────────────┴────────┘

2 rows in set. Elapsed: 0.010 sec. 
```



进入到某一个分区目录片段“202102_2_2_0”中，我们可以看到如下目录：

```
root@8b28ea5ff09b:/var/lib/clickhouse/data/test/t_mt/202102_2_2_0# ll
total 44
drwxr-x--- 13 root       root       416 Sep  8 11:55 ./
drwxr-x---  6 clickhouse clickhouse 192 Sep  8 11:55 ../
-rw-r-----  1 root       root       330 Sep  8 11:55 checksums.txt
-rw-r-----  1 root       root       108 Sep  8 11:55 columns.txt
-rw-r-----  1 root       root         1 Sep  8 11:55 count.txt
-rw-r-----  1 root       root       166 Sep  8 11:55 data.bin
-rw-r-----  1 root       root        70 Sep  8 11:55 data.cmrk3
-rw-r-----  1 root       root        10 Sep  8 11:55 default_compression_codec.txt
-rw-r-----  1 root       root         1 Sep  8 11:55 metadata_version.txt
-rw-r-----  1 root       root         4 Sep  8 11:55 minmax_birthday.idx
-rw-r-----  1 root       root         4 Sep  8 11:55 partition.dat
-rw-r-----  1 root       root        38 Sep  8 11:55 primary.cidx
-rw-r-----  1 root       root       345 Sep  8 11:55 serialization.json
```

- **checksums.txt**：校验文件，使用二进制格式存储。它保存了余下各类文件(primary. idx、count.txt等)的size大小及size的哈希值，用于快速校验文件的完整性和正确性。

- **columns.txt**： 存储当前分区所有列信息。使用明文格式存储。

```text
[root@node1 202102_2_2_0]# cat columns.txt 
columns format version: 1
5 columns:
`id` UInt8
`name` String
`age` UInt8
`birthday` Date
`location` String
```



- **count.txt**：计数文件，使用明文格式存储。用于记录当前数据分区目录下数据的总行数。

```text
[root@node1 202102_2_2_0]# cat count.txt 
2
```



- **data.bin**：数据文件，使用压缩格式存储，默认为LZ4压缩格式，用于存储某一列的数据。**之前clickhoue版本是每一个列字段都拥有独立的.bin数据文件，并以列字段名称命名，在新版本ClickHouse中所有数据合并到data.bin中。**

**之前ClickHouse版本此目录数据如下：**

![img](https://pic2.zhimg.com/80/v2-24054710487a9894a054fe7c4708cce1_1440w.webp)

- **data.mrk3**：列字段标记文件，使用二进制格式存储。标记文件中保存了data.bin文件中数据的偏移量信息

- **default_compression_codec.txt**:存储数据压缩格式

- **partition.dat与minmax_[Column].idx**：如果指定了分区键，则会额外生成partition.dat与minmax索引文件，它们均使用二进制格式存储。partition.dat用于保存当前分区下分区表达式最终生成的值，即分区字段值；而minmax索引用于记录当前分区下分区字段对应原始数据的最小和最大值。比如当使用birthday字段对应的原始数据为2021-02-17、2021-02-23，分区表达式为PARTITION BY toYYYYMM(birthday)，即按月分区。partition.dat中保存的值将会是202102，而minmax索引中保存的值将会是2021-02-17、2021-02-23。

==ClickHouse MergeTree引擎表支持分区，索引，修改，并发查询数据，==**当查询MergeTree表数据时，首先向primary.idx文件中获取对应的索引，根据索引找到【data.mrk3】文件获取对应的数据块偏移量，然后再根据偏移量从【data.bin】文件中读取块数据。**

- **primary.idx**：一级索引文件，使用二进制格式存储。用于存放稀疏索引，一张MergeTree表只能声明一次一级索引，即通过ORDER BY或者PRIMARY KEY指定字段。借助稀疏索引，在数据查询的时能够排除主键条件范围之外的数据文件，从而有效减少数据扫描范围，加速查询速度。





## 3.3 多版本变化 VersionedCollapsingMergeTree

[-](https://zhuanlan.zhihu.com/p/564535547?utm_id=0)

这个引擎:

- 允许快速写入不断变化的对象状态。
- 删除后台中的旧对象状态。 这显著降低了存储体积。

引擎继承自 [MergeTree](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/mergetree#table_engines-mergetree) 并将折叠行的逻辑添加到合并数据部分的算法中。 `VersionedCollapsingMergeTree` 用于相同的目的 [折叠树](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/collapsingmergetree) 但使用不同的折叠算法，允许以多个线程的任何顺序插入数据。 特别是， `Version` 列有助于正确折叠行，即使它们以错误的顺序插入。 相比之下, `CollapsingMergeTree` 只允许严格连续插入。

- ==使用一个数据的多版本管理，有列sign状态，1是当前，-1是删除==
- ==version代表版本==但是是多线程无序更新的

见表语句

```
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
) ENGINE = VersionedCollapsingMergeTree(sign, version)
[PARTITION BY expr]
[ORDER BY expr]
[SAMPLE BY expr]
[SETTINGS name=value, ...]
```

**引擎参数**

```sql
VersionedCollapsingMergeTree(sign, version)
```



- `sign` — 指定行类型的列名: `1` 是一个 “state” 行, `-1` 是一个 “cancel” 行

  列数据类型应为 `Int8`.

- `version` — 指定对象状态版本的列名。

  列数据类型应为 `UInt*`.

使用 `Sign` 列写入行时。 如果 `Sign = 1` 这意味着该行是一个对象的状态（让我们把它称为 “state” 行）。 如果 `Sign = -1` 它指示具有相同属性的对象的状态的取消（让我们称之为 “cancel” 行）。 还可以使用 `Version` 列，它应该用单独的数字标识对象的每个状态。

```
┌──────────────UserID─┬─PageViews─┬─Duration─┬─Sign─┬─Version─┐
│ 4324182021466249494 │         5 │      146 │   -1 │       1 |
│ 4324182021466249494 │         6 │      185 │    1 │       2 |
└─────────────────────┴───────────┴──────────┴──────┴─────────┘
```

**使用注意事项**

1. 写入数据的程序应该记住对象的状态以取消它。 该 “cancel” 字符串应该是 “state” 与相反的字符串 `Sign`. 这增加了存储的初始大小，但允许快速写入数据。
2. 列中长时间增长的数组由于写入负载而降低了引擎的效率。 数据越简单，效率就越高。
3. `SELECT` 结果很大程度上取决于对象变化历史的一致性。 准备插入数据时要准确。 不一致的数据将导致不可预测的结果，例如会话深度等非负指标的负值。

**算法**

当ClickHouse合并数据部分时，它会删除具有相同主键和版本但 `Sign`值不同的一对行. 行的顺序并不重要。

当ClickHouse插入数据时，它会按主键对行进行排序。 如果 `Version` 列不在主键中，ClickHouse将其隐式添加到主键作为最后一个字段并使用它进行排序。



### 3.3.1 测试

建立表

```
CREATE TABLE test.t_version_collapsing_mt
(

    `id` UInt8,

    `name` String,

    `loc` String,

    `login_times` UInt8,

    `total_dur` UInt8,

    `sign` Int8,

    `version` UInt8
)
ENGINE = VersionedCollapsingMergeTree(sign,
 version)
PARTITION BY loc
PRIMARY KEY id
ORDER BY (id,
 total_dur)
SETTINGS index_granularity = 8192;
```





```
插入数据
insert into table t_version_collapsing_mt values(1,'张三','北京',1,30,-1,1),(2,'李四','上海',1,40,1,2);

查询数据
8b28ea5ff09b :) select * from t_version_collapsing_mt;

SELECT *
FROM t_version_collapsing_mt

Query id: eaf5de9f-1f39-4fc6-b36c-155e41326866

┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  1 │ 张三 │ 北京 │           1 │        30 │   -1 │       1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘

2 rows in set. Elapsed: 0.029 sec. 

#向表 t_version_collapsing_mt中插入以下数据，删除“张三”信息，更新“李四”信息
insert into table t_version_collapsing_mt values(1,'张三','北京',1,30,1,1),(2,'李四','上海',1,40,-1,2),(2,'李四','上海',2,100,1,2);

#查询表 t_version_collapsing_mt中的数据
node1 :) select * from t_version_collapsing_mt ;
8b28ea5ff09b :) select * from t_version_collapsing_mt;,2,100,1,2);
SELECT *
FROM t_version_collapsing_mt

Query id: 6f843eda-a4d6-4e2a-b867-b8dc800f6dae

┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  1 │ 张三 │ 北京 │           1 │        30 │   -1 │       1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           1 │        40 │   -1 │       2 │
│  2 │ 李四 │ 上海 │           2 │       100 │    1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  1 │ 张三 │ 北京 │           1 │        30 │    1 │       1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘

5 rows in set. Elapsed: 0.025 sec. 

#手动执行 optimize 命令，合并相同分区的数据，这里有可能需要执行多次
node1 :) optimize table t_version_collapsing_mt;


```

**注意：如果不想执行多次，也可以执行 optimize table t_version_collapsing_mt final语句**

```text
#查询表 t_version_collapsing_mt 中的数据如下：
node1 :) select * from t_version_collapsing_mt; 
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           2 │       100 │    1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
```



### 3.3.2 查询数据注意事项

要完成折叠，请使用 `GROUP BY` 考虑符号的子句和聚合函数。 例如，要计算数量，请使用 `sum(Sign)` 而不是 `count()`. 要计算的东西的总和，使用 `sum(Sign * x)` 而不是 `sum(x)`，并添加 `HAVING sum(Sign) > 0`

```
8b28ea5ff09b :) select * from t_version_collapsing_mt;

SELECT *
FROM t_version_collapsing_mt

Query id: 42a58cc5-0b8d-4232-bf9c-727a9728ad4b

┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           2 │       100 │    1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┬─version─┐
│  2 │ 李四 │ 上海 │           2 │       100 │   -1 │       2 │
└────┴──────┴──────┴─────────────┴───────────┴──────┴─────────┘

2 rows in set. Elapsed: 0.056 sec. 


# 聚合查询
8b28ea5ff09b :) select id,sum(total_dur * sign) as dur  from t_version_collapsing_mt group by id,login_times  having sum(sign) > 0;;
SELECT
    id,
    sum(total_dur * sign) AS dur
FROM t_version_collapsing_mt
GROUP BY
    id,
    login_times
HAVING sum(sign) > 0

Query id: 9578a2fd-f51c-4512-b32b-b564612ffe6d

Ok.

0 rows in set. Elapsed: 0.020 sec. 


# 不做聚合直接查询
8b28ea5ff09b :) select * from t_version_collapsing_mt final; 0;
SELECT *
FROM t_version_collapsing_mt
FINAL

Query id: 40b4b717-6495-4479-9b89-72e966679287

Ok.

0 rows in set. Elapsed: 0.025 sec. 

8b28ea5ff09b :) 

```

**final是非常低效的，数据量大了不建议使用**



## 3.4 GraphiteMergeTree

该引擎用来对 [Graphite](http://graphite.readthedocs.io/en/latest/index.html)数据进行瘦身及汇总。对于想使用CH来存储Graphite数据的开发者来说可能有用。

如果不需要对Graphite数据做汇总，那么可以使用任意的CH表引擎；但若需要，那就采用 `GraphiteMergeTree` 引擎。它能减少存储空间，同时能提高Graphite数据的查询效率。

该引擎继承自 [MergeTree](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/mergetree).





## 3.5 AggregatingMergeTree

可以使用 `AggregatingMergeTree` 表来做增量数据的聚合统计，包括物化视图的数据聚合。

引擎使用以下类型来处理所有列：

- [AggregateFunction](https://clickhouse.com/docs/zh/sql-reference/data-types/aggregatefunction)
- [SimpleAggregateFunction](https://clickhouse.com/docs/zh/sql-reference/data-types/simpleaggregatefunction)

该表引擎继承自MergeTree，可以使用 AggregatingMergeTree 表来做==增量数据统计聚合==。如果要按一组规则来合并减少行数，则使用 AggregatingMergeTree 是合适的。AggregatingMergeTree是通过预先定义的聚合函数计算数据并通过二进制的格式存入表内。



与SummingMergeTree的区别在于：SummingMergeTree对非主键列进行sum聚合，而AggregatingMergeTree则可以指定各种聚合函数。对某些字段需要进行聚合时，需要在创建表字段时指定成AggregateFunction类型。

```
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
) ENGINE = AggregatingMergeTree()
[PARTITION BY expr]
[ORDER BY expr]
[SAMPLE BY expr]
[TTL expr]
[SETTINGS name=value, ...]
```

创建 `AggregatingMergeTree` 表时，需用跟创建 `MergeTree` 表一样的[子句](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/mergetree)。

### 3.5.1 测试

```
# 创建表 t_aggregating_mt ，使用AggregatingMergeTree引擎，指定salary字段是聚合字段

CREATE TABLE t_aggregating_mt
(
    `id` UInt8,
    `name` String,
    `age` UInt8,
    `loc` String,
    `dept` String,
    `workers` UInt8,
    `salary` AggregateFunction(sum ,decimal32(2))
)
ENGINE = AggregatingMergeTree()
PARTITION BY loc
PRIMARY KEY id
ORDER BY (id, age)

```

对于AggregateFunction类型的列字段，在进行数据的写入和查询时与其他的表引擎有很大区别，在写入数据时，需要调用 *-State 函数；而在查询数据时，则需要调用相应的 *-Merge 函数。

对于上面的建表语句而言，需要使用sumState函数进行数据插入。

```
#向表 t_aggregating_mt 中插入数据，插入方式与之前方式不同
node1 :) insert into t_aggregating_mt select 1,'张三',18,'北京','java',18,sumState(toDecimal32(10000,2));
node1 :) insert into t_aggregating_mt select 2,'李四',19,'上海','java',22,sumState(toDecimal32(8000,2));
node1 :) insert into t_aggregating_mt select 3,'王五',20,'北京','java',26,sumState(toDecimal32(12000,2));
​
```



查询数据

```
# 错误的方式
8b28ea5ff09b :) select * from t_aggregating_mt; 

┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │      22 │ 5
                                                   │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  1 │ 张三 │  18 │ 北京 │ java │      18 │ @B     │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  3 │ 王五 │  20 │ 北京 │ java │      26 │ �O      │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘

3 rows in set. Elapsed: 0.052 sec. 

# 正确的方式
8b28ea5ff09b :) select * ,sumMerge(salary) from t_aggregating_mt group by id,name ,age, loc,dept,workers,salary ;

┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┬─sumMerge(salary)─┐
│  1 │ 张三 │  18 │ 北京 │ java │      18 │ @B     │            10000 │
│  3 │ 王五 │  20 │ 北京 │ java │      26 │ �O      │            12000 │
│  2 │ 李四 │  19 │ 上海 │ java │      22 │ 5
                                                   │             8000 │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┴──────────────────┘

3 rows in set. Elapsed: 0.018 sec. 


```



插入数据

向表中插入排序字段相同的数据进行分区聚合时，**数据按照建表指定的聚合字段进行合并，其他的非聚合字段会保留最初的那条数据**，新插入的数据对应的字段值会被舍弃。

```
# 向表中插入新的一条数据
node1 :) insert into t_aggregating_mt select 1,'张三三',18,'北京','前端',22,sumState(toDecimal32(5000,2));

#查询表中的数据，这里为了方便看到分区不合并，直接查询
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  3 │ 王五 │  20 │ 北京 │ java │      26 │ �O      │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │      22 │ 5
                                                   │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  1 │ 张三 │  18 │ 北京 │ java │      18 │ @B     │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┘
┌─id─┬─name───┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┐
│  1 │ 张三三 │  18 │ 北京 │ 前端 │      22 │  �      │
└────┴────────┴─────┴──────┴──────┴─────────┴────────┘

4 rows in set. Elapsed: 0.014 sec. 

#使用optimize 命令合并相同分区数据
node1 :) optimize table t_aggregating_mt;

#再次查询表 t_aggregating_mt 表数据，salary 字段已经按照相同分区数据聚合
8b28ea5ff09b :) select * ,sumMerge(salary) from t_aggregating_mt group by id,name ,age, loc,dept,workers,salary ;

┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workers─┬─salary─┬─sumMerge(salary)─┐
│  1 │ 张三 │  18 │ 北京 │ java │      18 │ `�      │            15000 │
│  3 │ 王五 │  20 │ 北京 │ java │      26 │ �O      │            12000 │
│  2 │ 李四 │  19 │ 上海 │ java │      22 │ 5
                                                   │             8000 │
└────┴──────┴─────┴──────┴──────┴─────────┴────────┴──────────────────┘

3 rows in set. Elapsed: 0.017 sec. 

```

### 3.5.2 物化视图使用

```
1、 #创建表 t_merge_base 表，使用MergeTree引擎
CREATE TABLE t_merge_base
(
    `id` UInt8,
    `name` String,
    `age` UInt8,
    `loc` String,
    `dept` String,
    `workerdays` UInt8,
    `salary` Decimal32(2)
)
ENGINE = MergeTree()
PARTITION BY loc
PRIMARY KEY id
ORDER BY (id, age)

2、 #创建物化视图 view_aggregating_mt ,使用AggregatingMergeTree引擎
CREATE MATERIALIZED VIEW view_aggregating_mt
ENGINE = AggregatingMergeTree
ORDER BY id AS
SELECT
    id,
    name,
    sumState(salary) AS ss
FROM t_merge_base
GROUP BY
    id,
    name

 CREATE MATERIALIZED VIEW test.view_aggregating_mt
(
    `id` UInt8,
    `name` String,
    `ss` AggregateFunction(sum, Decimal(9, 2))
)
ENGINE = AggregatingMergeTree
ORDER BY id
SETTINGS index_granularity = 8192 AS
SELECT
    id,
    name,
    sumState(salary) AS ss
FROM test.t_merge_base
GROUP BY
    id,
    name

3、 向表 t_merge_base 中插入数据
insert into t_merge_base values (1,'张三',18,'北京','大数据',24,10000),(2,'李四',19,'上海','java',22,8000),(3,'王五',20,'北京','java',26,12000);

4、 #查看 view_aggregating_mt视图数据
 select *,sumMerge(ss) from view_aggregating_mt group by id,name,ss
┌─id─┬─name─┬─ss─┬─sumMerge(ss)─┐
│  2 │ 李四 │ 5
                 │         8000 │
│  3 │ 王五 │ �O  │        12000 │
│  1 │ 张三 │ @B │        10000 │
└────┴──────┴────┴──────────────┘

5、#继续向表 t_merge_base中插入排序键相同的数据
insert into t_merge_base values (1,'张三三',18,'北京','前端',22,5000);

# 6、#查看 view_aggregating_mt视图数据
select *,sumMerge(ss) from view_aggregating_mt group by id,name,ss

┌─id─┬─name───┬─ss─┬─sumMerge(ss)─┐
│  2 │ 李四   │ 5
                   │         8000 │
│  1 │ 张三三 │  �  │         5000 │
│  3 │ 王五   │ �O  │        12000 │
│  1 │ 张三   │ @B │        10000 │
└────┴────────┴────┴──────────────┘

7、 手动执行optimize 命令，合并物化视图 view_aggregating_mt 相同分区数据
 optimize table view_aggregating_mt

8、#查询视图 view_aggregating_mt数据
select *,sumMerge(ss) from view_aggregating_mt group by id,name,ss

┌─id─┬─name─┬─ss─┬─sumMerge(ss)─┐
│  2 │ 李四 │ 5
                 │         8000 │
│  1 │ 张三 │ `�  │        15000 │
│  3 │ 王五 │ �O  │        12000 │
└────┴──────┴────┴──────────────┘

3 rows in set. Elapsed: 0.065 sec. 

```

**注意：通过普通MergeTree表与AggregatingMergeTree物化视图结合使用，MergeTree中存放原子数据，物化视图中存入聚合结果数据，可以提升数据查询效率。**



## 3.6 CollapsingMergeTree

[-](https://zhuanlan.zhihu.com/p/564119408)

`CollapsingMergeTree` 会异步的删除（折叠）这些除了特定列 `Sign` 有 `1` 和 `-1` 的值以外，其余所有字段的值都相等的成对的行。没有成对的行会被保留。更多的细节请看本文的[折叠](https://clickhouse.com/docs/zh/engines/table-engines/mergetree-family/collapsingmergetree#table_engine-collapsingmergetree-collapsing)部分。



**存在的问题：**

- CollapsingMergeTree对于写入数据的顺序有着严格要求，否则导致无法正常折叠。

- **因此，该引擎可以显著的降低存储量并提高 `SELECT` 查询效率。**

- **数据折叠保留规则：**

- 在同一个分区内order by 字段相同的数据存在多条，且sign值不同，数据保留规则如下：

  - **如果sign=1和sign=-1的行数相同并且最后一行数据sign=1,则保留第一行sign=-1的行和最后一行sign=1的行。**

  - **如果sign=1的行比sign=-1的行多，则保留最后一条sign=1的行。**

  - **如果sign=-1的行比sign=1的行多，则保留第一条sign=-1的行。**

  - **其他情况，不保留数据。**



CollapsingMergeTree就是一种通过以增代删的思路，**支持行级数据修改和删除**的表引擎。它通过定义一个sign标记位字段，记录数据行的状态。如果sign标记为1，则表示这是一行有效的数据；如果sign标记为-1，则表示这行数据需要被删除。当CollapsingMergeTree分区合并时，同一数据分区内，sign标记为1和-1的一组数据会被抵消删除。

每次需要新增数据时，写入一行sign标记为1的数据；需要删除数据时，则写入一行sign标记为-1的数据。此外，只有相同分区内的数据才有可能被折叠。

- CollapsingMergeTree建表语法如下：

```text
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
...
sign Int8
) ENGINE = CollapsingMergeTree(sign)
[PARTITION BY expr]
[ORDER BY expr]
[SAMPLE BY expr]
[SETTINGS name=value, ...]
```



### 3.6.1 测试实例

```

1、#创建表 t_collapsing_mt ,使用CollapsingMergeTree
CREATE TABLE t_collapsing_mt
(
    `id` UInt8,
    `name` String,
    `loc` String,
    `login_times` UInt8,
    `total_dur` UInt8,
    `sign` Int8
)
ENGINE = CollapsingMergeTree(sign)
PARTITION BY loc
PRIMARY KEY id
ORDER BY (id, total_dur)

2、#向表   t_collapsing_mt 中插入以下数据：
insert into t_collapsing_mt values(1,'张三','北京',1,30,1),(2,'李四','上海',1,40,1)

3、#查看表 t_collapsing_mt中的数据
select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  1 │ 张三 │ 北京 │           1 │        30 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘

4、#向表 t_collapsing_mt中继续插入一条数据，删除“张三”数据
insert into t_collapsing_mt values(1,'张三','北京',1,30,-1);

5、#查询表 t_collapsing_mt 中的数据 
select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  1 │ 张三 │ 北京 │           1 │        30 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  1 │ 张三 │ 北京 │           1 │        30 │   -1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘

6、#手动触发 optimize 合并相同分区数据
optimize table t_collapsing_mt;

7、#查询表 t_collapsing_mt 中的数据 
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘

8、#插入以下两条数据，来更新 “李四”数据
insert into t_collapsing_mt values(2,'李四','上海',1,40,-1),(2,'李四','上海',2,100,1);

9、#查询表 t_collapsing_mt 中的数据 
select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │   -1 │
│  2 │ 李四 │ 上海 │           2 │       100 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘

10、#手动执行 optimize 触发相同分区合并
optimize table t_collapsing_mt;

11、#查看表 t_collapsing_mt中的数据
select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           2 │       100 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘


```

注意：以上功能使用 collapsingMergeTree实现了分区合并。





### 3.6.2 **乱序写入需要更新或删除的数据**

```
#删除表 t_collapsing_mt ，重新创建表 t_collapsing_mt
这里建表语句与之前一样

#向表 t_collapsing_mt 中插入以下数据：
node1 :) insert into t_collapsing_mt values(1,'张三','北京',1,30,-1),(1,'张三','北京',1,30,1),(2,'李四','上海',1,40,1)

#查询表 t_collapsing_mt中的数据
node1 :) select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  1 │ 张三 │ 北京 │           1 │        30 │   -1 │
│  1 │ 张三 │ 北京 │           1 │        30 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘

#手动执行 optimize 命令，合并相同分区数据
node1 :) optimize table t_collapsing_mt;

#查询表 t_collapsing_mt表中的数据，数据没有变化
node1 :) select * from t_collapsing_mt;
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  2 │ 李四 │ 上海 │           1 │        40 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
┌─id─┬─name─┬─loc──┬─login_times─┬─total_dur─┬─sign─┐
│  1 │ 张三 │ 北京 │           1 │        30 │   -1 │
│  1 │ 张三 │ 北京 │           1 │        30 │    1 │
└────┴──────┴──────┴─────────────┴───────────┴──────┘
```



**注意：当数据插入到表中的顺序标记如果不是1,-1这种顺序时，合并相同分区内的数据不能达到修改和更新效果。**

如果数据的写入程序是单线程执行的，则能够较好地控制写入顺序；如果需要处理的数据量很大，数据的写入程序通常是多线程执行的，那么此时就不能保障数据的写入顺序了。在这种情况下，CollapsingMergeTree的工作机制就会出现问题。但是可以通过VersionedCollapsingMergeTree的表引擎得到解决。



## 3.7 ReplacingMergeTree

[-](https://zhuanlan.zhihu.com/p/560753530)

以上MergeTree不能对相同主键的数据进行去重，ClickHouse提供了ReplacingMergeTree引擎，可以针对同分区内相同主键的数据进行去重，它能够在**合并分区时删除重复的数据**。值得注意的是，ReplacingMergeTree只是在一定程度上解决了数据重复问题，由于自动分区合并机制在后台定时执行，所以**并不能完全保障数据不重复。**ReplacingMergeTree 适用于在后台清除重复的数据以节省空间。



- ReplaceingMergeTree建表语句：

```text
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
) ENGINE = ReplacingMergeTree([ver])
[PARTITION BY expr]
[ORDER BY expr]
[SAMPLE BY expr]
[SETTINGS name=value, ...]
```

`ver` — 版本列。类型为 `UInt*`, `Date` 或 `DateTime`。可选参数。

以上建表语句的解释如下：

- [ver] ：可选参数，指定列的版本，可以是UInt*、Date或者DateTime类型的字段作为版本号。该参数决定了数据去重的方式。当没有指定[ver]时，保留最后插入的数据，也就是最新的数据；如果指定了具体的[ver]列，则保留最大版本数据。

**二、使用ReplacingMergeTree是需要注意以下几点**

- **如何判断数据重复**

ReplacingMergeTree在去除重复数据时，是以ORDERBY排序键为基准的，而不是PRIMARY KEY。

- **何时删除重复数据**

在执行分区合并时，会触发删除重复数据。optimize的合并操作是在后台执行的，无法预测具体执行时间点，除非是手动执行。

- **不同分区的重复数据不会被去重**

ReplacingMergeTree是以分区为单位删除重复数据的。只有在相同的数据分区内重复的数据才可以被删除，而不同数据分区之间的重复数据依然不能被剔除。

- **数据去重的策略是什么**

如果没有设置[ver]版本号，则保留同一组重复数据中的最新插入的数据；如果设置了[ver]版本号，则保留同一组重复数据中ver字段取值最大的那一行。

- **optimize命令使用**

一般在数据量比较大的情况，尽量不要使用该命令。因为在海量数据场景下，执行optimize要消耗大量时间。



### 3.7.1 测试

```
1、#创建表 t_replacing_mt ,使用ReplacingMergeTree引擎
CREATE TABLE t_replacing_mt
(
    `id` UInt8,
    `name` String,
    `age` UInt8,
    `gender` String
)
ENGINE = ReplacingMergeTree
PARTITION BY gender
PRIMARY KEY id
ORDER BY (id, age)

2、#向表 t_replacing_mt 中插入以下数据：
insert into t_replacing_mt values (1,'张三',18,'男'), (2,'李四',19,'女'),(3,'王五',20,'男');

3、#查询表 t_replacing_mt 中的数据：
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘

4、#向表 t_replacing_mt  中插入id 为1的一行数据
insert into t_replacing_mt values (1,'张三',10,'男');

5、#查询表 t_replacing_mt  数据：
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘

6、#执行 optimize命令手动合并分区数据
optimize table t_replacing_mt;

7、#查询表 t_replacing_mt  数据，发现没有按照primary key 去重。
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │ 男     │
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘

8、#再次向表  t_replacing_mt  插入数据：
insert into t_replacing_mt values (1,'张三三',18,'男');

9、#查询表 t_replacing_mt  数据
node1 :) select * from t_replacing_mt;
┌─id─┬─name───┬─age─┬─gender─┐
│  1 │ 张三三 │  18 │ 男     │
└────┴────────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │  男    │
│  1 │ 张三 │  18 │  男    │
│  3 │ 王五 │  20 │  男    │
└────┴──────┴─────┴────────┘

10、#再次执行 optimize命令手动合并分区数据
optimize table t_replacing_mt;

11、#查询表 t_replacing_mt  数据
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name───┬─age─┬─gender─┐
│  1 │ 张三   │  10 │ 男     │
│  1 │ 张三三 │  18 │ 男     │
│  3 │ 王五   │  20 │ 男     │
└────┴────────┴─────┴────────┘
```

**注意：通过以上测试发现ClickHouse ReplacingMergeTree中去除重复数据时，是以ORDERBY排序键为基准的，而不是PRIMARY KEY。**

也就是说必须满足`id`、`age`相同才行



### 3.7.2 测试不指定[ver]列

**插入相同排序字段的数据，保留最新一条数据**

```
1、#删除表 t_replacing_mt 重建,使用ReplacingMergeTree引擎
create table t_replacing_mt(
id UInt8,
 name String,
 age UInt8,
 gender String
) engine = ReplacingMergeTree()
 order by id
 primary key id
 partition by gender;

2、#向表 t_replacing_mt 中插入以下数据
insert into t_replacing_mt values (1,'张三',18,'男'),(2,'李四',19,'女'),(3,'王五',20,'男');

3、#查询表 t_replacing_mt 中的数据
select * from t_replacing_mt ;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘

4、#向表 t_replacing_mt 中插入排序字段相同的一行数据
insert into t_replacing_mt values (1,'张三',10,'男');

5、#查询表 t_replacing_mt 中的数据
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │ 
└────┴──────┴─────┴────────┘

6、#执行 optimize命令手动合并分区数据
optimize table t_replacing_mt;

7、#查询表 t_replacing_mt 中的数据
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘
```

**注意：通过以上测试可以发现，ClickHouse ReplacingMergeTree中不指定[ver]列时，当插入排序字段相同的数据时，保留最新一条数据。**

### 3.7.3 **测试指定[ver]列时**

```
1、#删除表 t_replacing_mt 重新创建,使用ReplacingMergeTree引擎，指定[ver]
create table t_replacing_mt(
id UInt8,
name String,
age UInt8,
gender String
) engine = ReplacingMergeTree(age)
 order by id
 primary key id
partition by gender;

2、#向表 t_replacing_mt 中插入数据：
insert into t_replacing_mt values (1,'张三',18,'男'),(2,'李四',19,'女'),(3,'王五',20,'男');

3、#查询表 t_replacing_mt中数据：
select * from t_replacing_mt ;
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘

4、#向表 t_replacing_mt 中插入排序字段相同的一行数据
insert into t_replacing_mt values (1,'张三',10,'男');

5、#查看表 t_replacing_mt中的数据
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  10 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘

6、#对表 t_replacing_mt中的数据执行手动分区合并
optimize table t_replacing_mt;

7、#查看表 t_replacing_mt中的数据
select * from t_replacing_mt;
┌─id─┬─name─┬─age─┬─gender─┐
│  2 │ 李四 │  19 │ 女     │
└────┴──────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘

```

**注意：通过以上测试可以发现，在ClickHouse中创建ReplacingMergeTree时，如果指定了[ver]列，当存在Order by字段重复时，会保留ver列最大值对应的行。**



### 3.7.4 **测试不同分区中有相同的Order by 字段时，不去重**

```
1、#删除表 t_replacing_mt ，重新创建
create table t_replacing_mt(
id UInt8,
name String,
age UInt8,
gender String
) engine = ReplacingMergeTree()
order by id
primary key id
partition by gender;

2、#向表 t_replacing_mt 中插入以下数据：
insert into t_replacing_mt values (1,'张三',18,'男'),(2,'李四',19,'女'),(3,'王五',20,'男');

3、#再次向表 t_replacing_mt 中插入以下数据：
insert into t_replacing_mt values (1,'张三三',10,'女');

4、#对表 t_replacing_mt中的数据执行手动分区合并
optimize table t_replacing_mt;

5、#查看表中的数据
select * from t_replacing_mt;
┌─id─┬─name───┬─age─┬─gender─┐
│  1 │ 张三三 │  10 │ 女     │
│  2 │ 李四   │  19 │ 女     │
└────┴────────┴─────┴────────┘
┌─id─┬─name─┬─age─┬─gender─┐
│  1 │ 张三 │  18 │ 男     │
│  3 │ 王五 │  20 │ 男     │
└────┴──────┴─────┴────────┘
```

**注意：通过以上测试可以发现，在ClickHouse中创建ReplacingMergeTree时，不同分区中相同的Order by 字段不会去重。**





## 3.8 数据副本 ReplicatedMergeTree

[-](https://zhuanlan.zhihu.com/p/571989417)

ClickHouse数据存储时支持副本和分片，副本指的就是一份数据可以在不同的节点上存储，这些节点上存储的每份数据相同，数据副本是增加数据存储冗余来防止数据丢失。分片指的是ClickHouse一张表的数据可以横向切分为多份，每份中的数据不相同且存储在不同的节点上，分片的目的主要是实现数据的水平切分，方便多线程和分布式查询数据。

这里以由3台ClickHouse节点组成的ClickHouse集群对应的几张图来描述ClickHouse中的副本与分片，方便大家理解：

- **表temp只有一个分片，1个副本（数据本身可看成1个副本）**

![img](readme.assets/v2-84a9af802a1da61004adb07f21f57443_1440w.webp)



- **表temp只有一个分片，每个分片有1个副本**

![img](readme.assets/v2-79c41e9ed23d3e07a919f3d0cf3922e8_1440w.webp)



- **表temp有2个分片，每个分片有1个副本**

![img](readme.assets/v2-18c3fb081bc740baf55123be80b3ba78_1440w.webp)



### 3.8.1 数据副本

存储在ClickHouse中的数据想要有副本，创建表时需要在对应的表引擎前面加上“Replicated”前缀组成一种新的变种引擎，并且目前只有MergeTree系列表引擎才支持副本，如下图所示：

![img](readme.assets/v2-afb71af6f9a31f428ffd6a5689d82113_1440w.webp)



下面我们以ReplicatedMergeTree引擎来举例讲解ClickHouse中的数据副本。

创建副本表语法：

```text
Engine = ReplicatedMergeTree('zk_path','replica_name')
```



在上述创建语法中，有zk_path和replica_name两项配置，代表意思如下：

- **zk_path:**

在zookeeper中创建的数据表的路径，路径名称可以自定义，用户可以自己定义成希望的任何路径。ClickHouse提供了一些约定俗成的配置模板：/ClickHouse/tables/{shard}/table_name ,其中“/ClickHouse/tables”是约定俗成的路径固定前缀，表示存放数据表的根路径；“{shard}”表示分片编号，通常使用数值代替，例如：01,02,03,一张数据表可以有多个分片，而每个分片都拥有自己的副本；“table_name”表示数据表的名称，通常与物理表的名字相同。

- **replica_name:**

定义在zookeeper中创建的副本名称，该名称是区分不同副本实例的唯一标识，一种约定俗成的命名方式是使用所在服务器的域名称。

### 3.8.2 测试数据副本

创建副本表举例,我们在node1节点进入ClickHouse，执行如下建表语句：

```text
Create table person_info(
id UInt32,
name String,
age UInt32,
gender String,
loc String
) engine = ReplicatedMergeTree('/ClickHouse/tables/01/person_info','node1')
partition by loc
order by id;
```



在node2节点进入ClickHouse，执行如下建表语句：

```text
Create table person_info(
id UInt32,
name String,
age UInt32,
gender String,
loc String
) engine = ReplicatedMergeTree('/ClickHouse/tables/01/person_info','node2')
partition by loc
order by id;
```



以上两张表创建完成之后，在zookeeper中会看到创建“/ClickHouse/tables/01/person_info”路径，对此路径下的部分重要目录解释如下：

- **/metadata:**

保存元数据信息，包括主键、分区键、采样表达式。

![img](https://pic1.zhimg.com/80/v2-7df526339c9e4adfdf9bd0c4b738ad38_1440w.webp)



- **/columns：**

保存列字段信息，包括列名称和数据类型。

![img](https://pic2.zhimg.com/80/v2-32a9802f740742b2c5c28e17c6231a5d_1440w.webp)



- **/replicas:**

保存副本名称，对应设置参数中的replica_name。

![img](https://pic1.zhimg.com/80/v2-651eb419d0684cc76be58bd65fd68464_1440w.webp)



![img](https://pic4.zhimg.com/80/v2-65dbe9ca0cebec661de918ce269f3cb3_1440w.webp)



- **/leader_election:**

用于主副本的选举工作，主副本主要负责merge、Alter delte 、alter update操作。

![img](https://pic2.zhimg.com/80/v2-8667d30812b6e310e9b88d8caaec788d_1440w.webp)



![img](https://pic2.zhimg.com/80/v2-33e3114a0977e6d32e0b138d8dbcbb89_1440w.webp)



![img](https://pic4.zhimg.com/80/v2-99515ba4514716fb28fe638914e3dbb7_1440w.webp)



在node1向表“person_info”中插入以下数据：

```text
insert into person_info values (1,'zs',18,'m','beijing'),(2,'ls',19,'f','shanghai'),(3,'ww',20,'m','beijing'),(4,'ml',21,'m','shanghai')
```



插入数据之后，我们在node1上进行查询：

```text
select * from person_info;
```



![img](https://pic3.zhimg.com/80/v2-748c988b853edd2bf9a372627cc07b2a_1440w.webp)

由于有副本作用，在node2节点上我们同样也可以查询到表person_info中的数据：

```text
select * from person_info;
```



![img](https://pic4.zhimg.com/80/v2-c0d0200c2c3b5ea851a1b44c3858ab2b_1440w.webp)



以上在node1节点或者node2节点上表“person_info”中插入数据时，都会通过zookeeper的监听，立即同步到另外节点，可以在node1,node2节点“/var/lib/ClickHouse/data/default/person_info”路径下发现相同的一份数据。

![img](https://pic1.zhimg.com/80/v2-645e3ed3323b2a4a2643849864788298_1440w.webp)

![img](https://pic4.zhimg.com/80/v2-1a844022d01353bcf2e4486fa4c31b7b_1440w.webp)



### 3.8.3 数据分片

通过数据副本我们可以降低数据丢失的风险，到现在为止每个副本上都有表全量数据，当业务量十分庞大的场景下，依靠副本并不能解决单表的新能瓶颈，我们可以对一张表水平分为多个分片，这些分片分别存储在不同的ClickHouse集群节点中。例如一个ClickHouse集群有3台节点，我们在创建表temp时可以分成3个分片，这3个分片内的数据不相同，分别存储在不同的ClickHouse节点上，当然为了保证数据的高可用也可以给每个分片设置副本。



#### 3.8.3.1 创建3分片2副本样例

特别注意：在ClickHouse中，每个节点只能配置在一个标签下的中，不能与其他的标签下的节点名称相同。例如：配置一个ClickHouse集群拥有3个分片，且每个分片有2个副本，那么metrika.xml配置文件配置如下：

```
<remote_servers>
        <ClickHouse_cluster_3shards_2replicas>
            <shard>
                <internal_replication>true</internal_replication>
                <replica>
                    <host>node1</host>
                    <port>9000</port>
                </replica>
                <replica>
                    <host>node2</host>
                    <port>9000</port>
                </replica>
            </shard>
            <shard>
                <internal_replication>true</internal_replication>
                <replica>
                    <host>node3</host>
                    <port>9000</port>
                </replica>
                <replica>
                    <host>node4</host>
                    <port>9000</port>
                </replica>
            </shard>
            <shard>
                <internal_replication>true</internal_replication>
                <replica>
                    <host>node5</host>
                    <port>9000</port>
                </replica>
                <replica>
                    <host>node6</host>
                    <port>9000</port>
                </replica>
            </shard>
        </ClickHouse_cluster_3shards_1replicas>
    </remote_servers>
```



以上完成配置拥有3个分片，2个副本的ClickHouse集群需要6台节点。

在介绍副本时，为了创建多张表我们需要分别登录到不同的ClickHouse节点，在各自的ClickHouse节点上执行create建表命令，创建的表名称都是一样的，这是因为Create、Drop、Rename、Alter等DDL语句并不支持分布式执行，而在分布式的ClickHouse集群中我们可以使用新的语法实现分布式DDL，其语法格式为：

```text
CREATE/DROP/RENAME/ALTER TABLE xxx ON CLUSTER cluste_name
```



其中以上“xxx”代表创建的表名称，“cluster_name”对应前面集群配置文件metrika.xml中的集群名称，根据配置文件，ClickHouse会根据集群的配置信息，找到每个节点执行DDL语句，“xxx”表也会在各个节点上被创建。

```
Create table person_score on cluster ClickHouse_cluster_3shards_1replicas (
id UInt32,
name String,
age UInt32,
gender String,
score Decimal(9,2)
)engine = ReplicatedMergeTree('/ClickHouse/tables/{shard}/person_score','{replica}')
order by id;
```

如上例所示，这些参数可以包含宏替换的占位符，即大括号的部分。它们会被替换为配置文件里 ‘macros’ 那部分配置的值。示例：

```xml
<macros>
    <layer>05</layer>
    <shard>02</shard>
    <replica>example05-02-1</replica>
</macros>
```

**注意：**

- 以上“ClickHouse_cluster_3shards_1replicas”是在“/etc/ClickHouse-server/config.d/metrika.xml”配置文件中配置的ClickHouse集群的名称
- {shard}与{replica}两个变量是在metrika.xml中宏变量标签中配置的对应值，这样当在ClickHouse集群中的某台节点执行以上建表语句时，ClickHouse会自动在各个节点创建此表,这里每台ClickHouse节点上的表person_socre是本地表。

可以在zookeeper中找到查看对应的分片信息：

![img](https://pic4.zhimg.com/80/v2-72db39dbbd8c079c05bada21167147bf_1440w.webp)



向表person_score中插入数据，在哪台ClickHouse节点向本地表person_score中插入数据，那么数据就存入当前本地表对应的分片中。

```text
#在node1向node1本地表person_score中插入以下数据：
insert into person_score values (1,'zs',18,'m',100),(2,'ls',19,'f',200);

#在node1上查询本地表 person_score数据：
node1 :) select * from person_score;
```



![img](https://pic2.zhimg.com/80/v2-a84e55d483a4da653e042fc4fcb14575_1440w.webp)



```text
#在node2向node2本地表person_score中插入以下数据：
insert into person_score values (3,'ww',20,'m',300),(4,'ml',21,'m',400);

#在node2上查询本地表 person_score数据：
node2 :) select * from person_score;
```



![img](https://pic2.zhimg.com/80/v2-8750b593dff64d06d1c6b3a142fdde41_1440w.webp)



```text
#在node3向node3本地表person_score中插入以下数据：

insert into person_score values (5,'ml',22,'f',500),(6,'tq',23,'f',600);



#在node3上查询本地表 person_score数据：

node3 :) select * from person_score;
```



![img](https://pic1.zhimg.com/80/v2-a15136fc46e43cd23b355cf25d8576a0_1440w.webp)



以上我们创建的person_score表在ClickHouse集群节点node1、node2、node3上都是本地表，插入数据时插入到了对应节点的分片上，查询时也只能查询对应节点上的分片数据，如果我们想要通过一张表将各个ClickHouse节点上的person_score表进行查询，这时就需要使用Distribute表引擎，所以在实际工作中ClickHouse的数据分片需要结合Distriubute表引擎一同使用。



## 3.9 Distributed

[-](https://zhuanlan.zhihu.com/p/572359237)

Distributed引擎和Merge引擎类似，本身不存放数据,功能是在不同的server上把多张相同结构的物理表合并为一张逻辑表。

**Distributed分布式引擎语法：**

```text
Distributed(cluster_name, database_name, table_name[, sharding_key])
```



**对以上语法解释：**

- cluster_name：集群名称，与集群配置文件metrika.xml中的自定义名称相对应。
- database_name：数据库名称。
- table_name：表名称。
- sharding_key：可选的，用于分片的key值，在数据写入的过程中，分布式表会依据分片key的规则，将数据分布到各个节点的本地表。

注意：创建分布式表是读时检查的机制，也就是说对创建分布式表和本地表的顺序并没有强制要求。

我们在ClickHouse集群中各个节点上创建好了本地表person_socre,每个节点上也有不同的数据，我们需要创建分布式表来映射当前表所有数据，方便查询数据结果，如下图所示：

![img](readme.assets/v2-b46209533d2abd28963b2a9503be6e21_1440w.webp)



**从实体表层面上来看，一张分片表由两部分组成：**

- 本地表：通常以_local为后缀进行命名。**本地表是承接数据的载体，可以使用非Distributed的任意表引擎，一张本地表对应了一个数据分片。**
- 分布式表：通常以_all为后缀进行命名，分布式表只能使用Distribute表引擎，它与本地表形成一对多的映射关系，日后将通过分布式表代理操作多张本地表。

### 3.9.1 创建

**创建person_score_all分布式表：**

```text
Create table person_score_all on cluster ClickHouse_cluster_3shards_1replicas (
id UInt32,
name String,
age UInt32,
gender String,
score Decimal(9,2)
)engine = Distributed(ClickHouse_cluster_3shards_1replicas,default,person_score,id);
```



任意一台ClickHouse节点查询分布式表person_score_all中的数据：

```text
select * from person_score_all;
```



![img](readme.assets/v2-7afcb0f4900ed274aa9b2a9f155d30ae_1440w.jpeg)



有了分布式表之后，我们就可以向分布式表中插入数据，那么分布式表会根据配置的sharding_key将数据写入到不同的节点分片中。



### 3.9.2 插入数据

在任意节点向分布式表person_score_all中插入数据：

```text
insert into person_score_all values (7,'a1',30,'f',1000),(8,'a2',31,'f',1001),(9,'a3',32,'f',1002),(10,'a4',33,'f',1003),(11,'a5',34,'f',1004),(12,'a6',35,'f',1005);
```



任意节点查询对应的person_score_all表：

```text
select * from person_score_all;
```



![img](https://pic2.zhimg.com/80/v2-635ba9fecb37d78f1802c3d50988a2bd_1440w.webp)



```text
#可以针对每张本地表进行optimize 合并数据，不能针对分布式表进行合并
#在node1上执行如下命令
node1 :) optimize table person_score;

#在node2上执行如下命令
node1 :) optimize table person_score;

#在node3上执行如下命令
node1 :) optimize table person_score;

#最终查询分布式表person_score_all
```



![img](https://pic3.zhimg.com/80/v2-a3b72fe51b4265ef2f32d84159de575e_1440w.webp)



### 3.9.3 分片规则

分片键sharding_key要求返回一个整形类型的取值，包括Int系列和UInt系列，分片键sharding_key可以使如下几种情况：

可以是一个具体的整形列字段：

```text
Distributed(cluster,database,table,userid)
```



可以按照随机数划分：

```text
Distributed(cluster,database,table,rand())
```



可以按照某个整形列进行散列值划分：

```text
Distributed(cluster,database,table,intHash64(userid))
```



注意：如果不声明分片键，那么分布式表只能包含一个分片，这意味着只能映射一张本地表，否则，在写入数据时将会报错。如果分布式表只包含一个分片，也就失去了分布式的意义，所以虽然分片键是选填参数，但是通常都会按照业务规则进行设置。



### 3.9.4 删除分布式表

删除分布式表person_score_all，任意ClickHouse节点直接执行如下命令：

```text
drop table person_score_all on cluster ClickHouse_cluster_3shards_1replicas;
```



## 3.10 SummingMergeTree

[-](https://zhuanlan.zhihu.com/p/561022366)

当合并 SummingMergeTree 表的数据片段时，ClickHouse 会把所有具有相同主键的行合并为一行，该行包含了被合并的行中具有数值数据类型的列的汇总值，即如果存在重复的数据，会对对这些重复的数据进行合并成一条数据，类似于group by的效果，可以显著减少存储空间并加快数据查询速度。

如果用户**只需要查询数据的汇总结果，不关心明细数据**，并且数据的汇总条件是预先明确的，即GROUP BY的分组字段是确定的，可以使用该表引擎。

- SummingMergeTree建表语句：

```text
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
) ENGINE = SummingMergeTree([columns])
[PARTITION BY expr]
[ORDER BY expr]
[SAMPLE BY expr]
[SETTINGS name=value, ...]
```



**对以上建表语句的解释如下：**

[columns]: 将要被汇总的列,或者多个列，多个列需要写在元组中。可选参数。所选的列必须是数值类型，并且不可位于主键中。如果没有指定 [columns]，ClickHouse 会把所有不在主键中的数值类型的列都进行汇总。



**二、使用SummingMergeTree注意以下几点**

- **SummingMergeTree是根据什么对两条数据进行合并的**

用ORBER BY排序键作为聚合数据的条件Key。即如果排序key是相同的，则会合并成一条数据，并对指定的合并字段进行聚合。

- **仅对分区内的相同排序key的数据行进行合并**

以数据分区为单位来聚合数据。当分区合并时，同一数据分区内聚合Key相同的数据会被合并汇总，而不同分区之间的数据则不会被汇总。

- **如果没有指定聚合字段，会怎么聚合**

如果没有指定聚合字段，则会按照非主键的数值类型字段进行聚合。

- **对于非汇总字段的数据，该保留哪一条**

如果两行数据除了排序字段相同，其他的非聚合字段不相同，那么在聚合发生时，会保留最初的那条数据，新插入的数据对应的那个字段值会被舍弃。





### 3.10.1 测试

```
1、#创建表 t_summing_mt ,使用SummingMergeTree表引擎
create table t_summing_mt(
 id UInt8,
 name String,
 age UInt8,
 loc String,
 dept String,
 workdays UInt8,
 salary Decimal32(2)
 ) engine = SummingMergeTree()
 order by (id,age)
 primary key id
 partition by loc;

2、#向表 t_summing_mt 中插入以下数据
insert into t_summing_mt values (1,'张三',18,'北京','大数据',24,10000),(2,'李四',19,'上海','java',22,8000),(3,'王五',20,'北京','java',26,12000);

3、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

4、#向表 t_summing_mt 中插入一条排序键相同的数据
insert into t_summing_mt values (1,'马六',18,'北京','前端',27,15000);

5、#查看表 t_summing_mt中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬───salary─┐
│  1 │ 马六 │  18 │ 北京 │ 前端 │       27 │ 15000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

6、#手动执行optimize 命令触发合并相同分区数据
optimize table t_summing_mt;

7、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       51 │ 25000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘
注意：我们可以看到当不指定 聚合字段时，有相同排序字段行进行聚合时，会将数值类型的字段进行聚合合并。

8、#继续向表 t_summing_mt中插入以下数据：
insert into t_summing_mt values (1,'张三',18,'南京','java',18,12000);

9、#查看表 t_summing_mt中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 南京 │ java │       18 │ 12000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       51 │ 25000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

10、#手动指定optimize 命令合并相同排序key的数据
optimize table t_summing_mt;

11、#查看表 t_summing_mt中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 南京 │ java │       18 │ 12000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       51 │ 25000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘
```

**注意：不同分区内相同的排序key的数据不能被合并**

### 3.11.2 指定聚合字段

```
1、#删除表 t_summing_mt，重新创建表 t_summing_mt ，使用SummingMergeTree引擎
create table t_summing_mt(
 id UInt8,
 name String,
 age UInt8,
 loc String,
 dept String,
 workdays UInt8,
 salary Decimal32(2)
 ) engine = SummingMergeTree(salary)
 order by (id,age)
 primary key id
 partition by loc;

2、#向表 t_summing_mt 中插入以下数据
insert into t_summing_mt values (1,'张三',18,'北京','大数据',24,10000),(2,'李四',19,'上海','java',22,8000),(3,'王五',20,'北京','java',26,12000);

3、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

4、#向表 t_summing_mt 中插入一条排序键相同的数据
insert into t_summing_mt values (1,'马六',18,'北京','前端',27,15000);

5、#查看表 t_summing_mt中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬───salary─┐
│  1 │ 马六 │  18 │ 北京 │ 前端 │       27 │ 15000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

6、#手动执行optimize 命令触发合并相同分区数据
optimize table t_summing_mt;

7、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 25000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘
```

**注意：我们可以看到当指定一个聚合字段时，有相同排序字段行进行聚合时，会按照这个数值字段进行合并，其他的保留最开始一条数据的信息。**



### 3.11.3 指定多个字段

```
1、#删除表 t_summing_mt，重新创建表 t_summing_mt ，使用SummingMergeTree引擎
create table t_summing_mt(
 id UInt8,
 name String,
 age UInt8,
 loc String,
 dept String,
 workdays UInt8,
 salary Decimal32(2)
 ) engine = SummingMergeTree((salary,workdays))
 order by (id,age)
 primary key id
 partition by loc;

2、#向表 t_summing_mt 中插入以下数据
insert into t_summing_mt values (1,'张三',18,'北京','大数据',24,10000),(2,'李四',19,'上海','java',22,8000),(3,'王五',20,'北京','java',26,12000);

3、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

4、#向表 t_summing_mt 中插入一条排序键相同的数据
insert into t_summing_mt values (1,'马六',18,'北京','前端',27,15000);

5、#查看表 t_summing_mt中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       24 │ 10000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬───salary─┐
│  1 │ 马六 │  18 │ 北京 │ 前端 │       27 │ 15000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘

6、#手动执行optimize 命令触发合并相同分区数据
optimize table t_summing_mt;

7、#查看表 t_summing_mt 中的数据
select * from t_summing_mt;
┌─id─┬─name─┬─age─┬─loc──┬─dept───┬─workdays─┬───salary─┐
│  1 │ 张三 │  18 │ 北京 │ 大数据 │       51 │ 25000.00 │
│  3 │ 王五 │  20 │ 北京 │ java   │       26 │ 12000.00 │
└────┴──────┴─────┴──────┴────────┴──────────┴──────────┘
┌─id─┬─name─┬─age─┬─loc──┬─dept─┬─workdays─┬──salary─┐
│  2 │ 李四 │  19 │ 上海 │ java │       22 │ 8000.00 │
└────┴──────┴─────┴──────┴──────┴──────────┴─────────┘
```

**注意：我们可以看到当指定多个聚合字段时，有相同排序字段行进行聚合时，会按照指定的多个数值字段进行合并，其他的保留最开始一条数据的信息。**



# 四、日志引擎系列



## 4.1 Log

[-](https://zhuanlan.zhihu.com/p/559702435)

Log系列表引擎功能相对简单，主要用于快速写入小表（1百万行左右的表），然后全部读出的场景，即一次写入，多次查询。Log系列表引擎包含：TinyLog、StripeLog、Log三种引擎。

**几种Log表引擎的共性是：**

- 数据被顺序append写到本地磁盘上。
- 不支持delete、update修改数据。
- 不支持index（索引）。
- 不支持原子性写。如果某些操作(异常的服务器关闭)中断了写操作，则可能会获得带有损坏数据的表。
- insert会阻塞select操作。当向表中写入数据时，针对这张表的查询会被阻塞，直至写入动作结束。

**它们彼此之间的区别是：**

- TinyLog：不支持并发读取数据文件，查询性能较差；格式简单，适合用来暂存中间数据。
- StripLog：支持并发读取数据文件，查询性能比TinyLog好；将所有列存储在同一个大文件中，减少了文件个数。
- Log：支持并发读取数据文件，查询性能比TinyLog好；每个列会单独存储在一个独立文件中。

## 4.2 TinyLog

TinyLog是Log系列引擎中功能简单、性能较低的引擎。

它的存储结构由数据文件和元数据两部分组成。其中，数据文件是按列独立存储的，也就是说每一个列字段都对应一个文件。

由于**TinyLog数据存储不分块**，所以不支持并发数据读取，该引擎适合一次写入，多次读取的场景，对于处理小批量中间表的数据可以使用该引擎，这种引擎会有大量小文件，性能会低。



- 示例：

```text
1、#在ch中创建库 newdb，并使用
create database newdb;
use newdb;

2、#创建表t_tinylog 表，使用TinyLog引擎
create table t_tinylog(id UInt8,name String,age UInt8) engine=TinyLog;

3、#向表中插入数据
insert into t_tinylog values (1,'张三',18),(2,'李四',19),(3,'王五',20);

4、#查询表中的数据
select * from t_tinylog;

SELECT *
FROM t_tinylog
┌─id─┬─name─┬─age─┐
│  1 │ 张三  │  18 │
│  2 │ 李四  │  19 │ 
│  3 │ 王五  │  20 │
└────┴──────┴─────┘
3 rows in set. Elapsed: 0.003 sec. 

#在表中删除一条数据，这里是不支持delete。
node1 :) delete from t_tinylog where id = 1;//语句不适合CH
node1 :) alter table t_tinylog delete where id = 1;
:Exception: Mutations are not supported by storage TinyLog.
```



当在newdb库中创建表t_tinylog后，在ClickHouse保存数据的目录/var/lib/clickhouse/data/newdb/下会多一个t_tinylog目录，如图所示：

![img](readme.assets/v2-83373d03f71d929d9954997537abd316_1440w.png)



在向表t_tinylog中插入数据后，进入“t_tinylog”目录，查看目录下的文件，如下图所示：

![img](readme.assets/v2-2596d7cdf55fa8863206bc9509770821_1440w.webp)



我们可以发现，表t_tinylog中的每个列都单独对应一个*.bin文件，同时还有一个sizes.json文件存储元数据，记录了每个bin文件中数据大小。

## 4.3 StripeLog

相比TinyLog而言，**StripeLog数据存储会划分块，每次插入对应一个数据块**，拥有更高的查询性能（拥有.mrk标记文件，支持并行查询）。StripeLog 引擎将所有列存储在一个文件中，使用了更少的文件描述符。对每一次 Insert 请求，ClickHouse 将数据块追加在表文件的末尾，逐列写入。StripeLog 引擎不支持 ALTER UPDATE 和 ALTER DELETE 操作。

- 示例：

```text
1、#在库 newdb中创建表 t_stripelog，使用StripeLog引擎
create table t_stripelog(id UInt8,name String,age UInt8) engine = StripeLog;

2、#向表t_stripelog中插入数据，这里插入分多次插入，会将数据插入不同的数据块中
insert into t_stripelog values (1,'张三',18);
insert into t_stripelog values (2,'李四',19);

3、#查询表 t_stripelog数据
select * from t_stripelog;

SELECT *
FROM t_stripelog
┌─id─┬─name─┬─age─┐
│  1 │ 张三  │  18 │
└────┴──────┴─────┘
┌─id─┬─name─┬─age─┐
│  2 │ 李四  │  19 │
└────┴──────┴─────┘
2 rows in set. Elapsed: 0.003 sec. 
```



当在newdb库中创建表 t_stripelog后，在ClickHouse保存数据的目录/var/lib/clickhouse/data/newdb/下会多一个t_stripelog目录，如图所示：

![img](readme.assets/v2-f498b71b214226e1916cf91998701d84_1440w.png)



在向表t_stripelog中插入数据后，进入“t_stripelog”目录，查看目录下的文件，如下图所示：

![img](readme.assets/v2-a031da82b426a147b06ab6b7be50ad69_1440w.webp)



我们可以发现只有三个文件：

- data.bin:数据文件，所有列字段都写入data.bin文件中。
- index.mrk:数据标记文件，保存了数据在data.bin 文件中的位置信息，即每个插入数据列的offset信息，利用数据标记能够使用多个线程，并行度取data.bin压缩数据，提升查询性能。
- sizes.json:元数据文件，记录了data.bin和index.mrk大小信息。



## 4.4 Log

Log引擎表适用于临时数据，一次性写入、测试场景。Log引擎结合了TinyLog表引擎和StripeLog表引擎的长处，是Log系列引擎中性能最高的表引擎。

Log表引擎会将每一列都存在一个文件中，**对于每一次的INSERT操作，会生成数据块**，经测试，**数据块个数与当前节点的core数一致。**

- 示例：

```text
1、#在newdb中创建表t_log 使用Log表引擎
create table t_log(id UInt8 ,name String ,age UInt8 ) engine = Log;

2、#向表 t_log中插入数据，分多次插入，插入后数据存入数据块
insert into t_log values (1,'张三',18);
insert into t_log values (2,'李四',19);
insert into t_log values (3,'王五',20);
insert into t_log values (4,'马六',21);
insert into t_log values (5,'田七',22);

3、#查询表t_log中的数据
select * from t_log;

SELECT *
FROM t_log
┌─id─┬─name─┬─age─┐
│  1  │ 张三   │  18  │
│  2  │ 李四   │  19  │
└────┴─────┴─────┘
┌─id─┬─name─┬─age─┐
│  3  │ 王五  │  20   │
│  4  │ 马六  │  21   │
│  5  │ 田七  │  22   │
└────┴─────┴─────┘
5 rows in set. Elapsed: 0.004 sec.
```



当在newdb库中创建表 t_log后，在ClickHouse保存数据的目录/var/lib/clickhouse/data/newdb/下会多一个t_log目录，如图所示：

![img](readme.assets/v2-49165bcaa110fbb4454452d013092990_1440w.png)



在向表t_log中插入数据后，进入“t_log”目录，查看目录下的文件，如下图所示：

![img](readme.assets/v2-364e6a36bb0d602b336dd169d4017910_1440w.webp)



我们发现表t_log中的每个列都对应一个*.bin文件。其他两个文件的解释如下：

- __marks.mrk：数据标记，保存了每个列文件中的数据位置信息，利用数据标记能够使用多个线程，并行度取data.bin压缩数据，提升查询性能。
- sizes.json:记录了*.bin 和__mark.mrk大小的信息。



# 五、集成表引擎

[-](https://clickhouse.com/docs/zh/engines/table-engines/integrations)

![image-20230911155423564](readme.assets/image-20230911155423564.png)

# 六、special表系列

## 6.1 memory

Memory表引擎直接将数据保存在内存中，ClickHouse中的Memory表引擎具有以下特点:

- Memory 引擎以未压缩的形式将数据存储在 RAM 中，数据完全以读取时获得的形式存储。
- 并发数据访问是同步的，锁范围小，读写操作不会相互阻塞。
- 不支持索引。
- 查询是并行化的，在简单查询上达到最大速率（超过10 GB /秒），在相对较少的行（最多约100,000,000）上有高性能的查询。
- 没有磁盘读取，不需要解压缩或反序列化数据，速度更快（在许多情况下，与 MergeTree 引擎的性能几乎一样高）。
- 重新启动服务器时，表存在，但是表中数据全部清空。
- Memory引擎多用于测试。

**示例：**

```text
1、#在 newdb中创建表 t_memory ，表引擎使用Memory
create table t_memory(id UInt8 ,name String, age UInt8) engine = Memory;

CREATE TABLE t_memory
(
    `id` UInt8,
    `name` String,
    `age` UInt8
)
ENGINE = Memory
Ok.
0 rows in set. Elapsed: 0.008 sec.

2、#向表 t_memory中插入数据
insert into t_memory values (1,'张三',18),(2,'李四',19),(3,'王五',20);

INSERT INTO t_memory VALUES

Ok.
3 rows in set. Elapsed: 0.004 sec. 

3、#查询表t_memory中的数据
select * from t_memory;

SELECT *
FROM t_memory
┌─id─┬─name─┬─age─┐
│  1 │ 张三  │  18 │
│  2 │ 李四  │  19 │
│  3 │ 王五  │  20 │
└────┴──────┴─────┘
3 rows in set. Elapsed: 0.004 sec.

4、#重启clickhouse 服务
[root@node1 ~]# service clickhouse-server restart
Stop clickhouse-server service: DONE
Start clickhouse-server service: Path to data directory in /etc/clickhouse-server/config.xml: /var/lib/clickhouse/
DONE

5、#进入 newdb 库，查看表 t_memory数据，数据为空。
select * from t_memory;

SELECT *
FROM t_memory
Ok.
0 rows in set. Elapsed: 0.003 sec.
```



注意：”Memory”表引擎写法固定，不能小写。同时创建好表t_memory后，在对应的磁盘目录/var/lib/clickhouse/data/newdb下没有“t_memory”目录，基于内存存储，当重启ClickHouse服务后，表t_memory存在，但是表中数据全部清空。



## 6.2 **Merge**

Merge 引擎 (不要跟 MergeTree 引擎混淆) 本身不存储数据，但可用于同时从任意多个其他的表中读取数据，这里需要多个表的结构相同，并且创建的Merge引擎表的结构也需要和这些表结构相同才能读取。

读是自动并行的，不支持写入。读取时，那些被真正读取到数据的表如果设置了索引，索引也会被使用。

Merge 引擎的参数：一个数据库名和一个用于匹配表名的正则表达式：

> Merge(数据库, 正则表达式)

例如：Merge(hits, '^WatchLog') 表示数据会从 hits 数据库中表名匹配正则 ‘^WatchLog’ 的表中读取。

注意:当选择需要读取的表时，会匹配正则表达式匹配上的表，如果当前Merge表的名称也符合正则表达式匹配表名，这个Merge表本身会自动排除，以避免进入递归死循环，当然也可以创建两个相互无限递归读取对方数据的 Merge 表，但这并没有什么意义。

```
1、#在newdb库中创建表m_t1，并插入数据
create table m_t1 (id UInt8 ,name String,age UInt8) engine = TinyLog;
insert into m_t1 values (1,'张三',18),(2,'李四',19)

2、#在newdb库中创建表m_t2，并插入数据
create table m_t2 (id UInt8 ,name String,age UInt8) engine = TinyLog;
insert into m_t2 values (3,'王五',20),(4,'马六',21)

3、#在newdb库中创建表m_t3，并插入数据
create table m_t3 (id UInt8 ,name String,age UInt8) engine = TinyLog;
insert into m_t3 values (5,'田七',22),(6,'赵八',23)

4、#在newdb库中创建表t_merge，使用Merge引擎，匹配m开头的表
create table t_merge (id UInt8,name String,age UInt8) engine = Merge(newdb,'^m');

5、#查询 t_merge表中的数据
select * from t_merge;

SELECT *
FROM t_merge
┌─id─┬─name──┬─age─┐
│  1 │ 张三   │  18 │
│  2 │ 李四   │  19 │
└────┴───────┴─────┘
┌─id─┬─name─┬─age─┐
│  3 │ 王五  │  20 │
│  4 │ 马六  │  21 │
└────┴──────┴─────┘
┌─id─┬─name─┬─age─┐
│  5 │ 田七  │  22 │
│  6 │ 赵八  │  23 │
└────┴──────┴─────┘
6 rows in set. Elapsed: 0.008 sec. 
```

**注意：t_merge表不会在对应的库路径下产生对应的目录结构。**



## 6.3 **Distributed**

Distributed是ClickHouse中分布式引擎，之前所有的操作虽然说是在ClickHouse集群中进行的，但是实际上是在node1节点中单独操作的，与node2、node3无关，使用分布式引擎声明的表才可以在其他节点访问与操作。

Distributed引擎和Merge引擎类似，本身不存放数据,功能是在不同的server上把多张相同结构的物理表合并为一张逻辑表。

分布式引擎语法：

```text
Distributed(cluster_name, database_name, table_name[, sharding_key])
```



对以上语法解释：

- cluster_name：集群名称，与集群配置中的自定义名称相对应。配置在/etc/metrika.xml文件中，如下图：

![img](https://pic4.zhimg.com/80/v2-c67d742b15523ea4607e7c7edc7202b3_1440w.webp)

- database_name：数据库名称。
- table_name：表名称。
- sharding_key：可选的，用于分片的key值，在数据写入的过程中，分布式表会依据分片key的规则，将数据分布到各个节点的本地表。

注意：创建分布式表是读时检查的机制，也就是说对创建分布式表和本地表的顺序并没有强制要求。

**示例：**

```text
1、#在node1、node2、node3节点上启动ClickHouse 服务
[root@node1 ~]# service clickhouse-server start
[root@node2 ~]# service clickhouse-server start
[root@node3 ~]# service clickhouse-server start

2、#使用默认的default库，在每个节点上创建表 test_table
create table test_local (id UInt8,name String) engine= TinyLog
node2 :) create table test_local (id UInt8,name String) engine= TinyLog
node3 :) create table test_local (id UInt8,name String) engine= TinyLog

3、#在node1上创建分布式表 t_distributed，表引擎使用 Distributed 引擎
create table t_distributed(id UInt8,name String) engine = Distributed(clickhouse_cluster_3shards_1replicas,default,test_local,id);
注意：以上分布式表 t_distributed 只存在与node1节点的clickhouse中。

4、#分别在node1、node2、node3节点上向表test_local中插入2条数据
insert into test_local values (1,'张三'),(2,'李四');
node2 :) insert into test_local values (3,'王五'),(4,'马六');
node3 :) insert into test_local values (5,'田七'),(6,'赵八');

5、#查询分布式表 t_distributed 中的数据
select * from t_distributed;
SELECT *
FROM t_distributed
┌─id─┬─name──┐
│  1 │ 张三   │
│  2 │ 李四   │
└────┴───────┘
┌─id─┬─name─┐
│  5 │ 田七  │
│  6 │ 赵八  │
└────┴──────┘
┌─id─┬─name─┐
│  3 │ 王五  │
│  4 │ 马六  │
└────┴──────┘
6 rows in set. Elapsed: 0.010 sec. 

6、#向分布式表 t_distributed 中插入一些数据，然后查询 node1、node2、node3节点上的test_local数据，发现数据已经分布式存储在不同节点上
insert into t_distributed values (7,'zs'),(8,'ls'),(9,'ww'),(10,'ml'),(11,'tq'),(12,'zb');

7、#node1查询本地表 test_local
select * from test_local;
SELECT *
FROM test_local
┌─id─┬─name─┐
│  1 │ 张三  │
│  2 │ 李四  │
│  9 │ ww   │
│ 12 │ zb   │
└────┴──────┘
4 rows in set. Elapsed: 0.004 sec. 

8、#node2查询本地表 test_local
node2 :) select * from test_local;
SELECT *
FROM test_local
┌─id─┬─name─┐
│  3 │ 王五  │
│  4 │ 马六  │
│  7 │ zs   │
│ 10 │ ml   │
└────┴──────┘
4 rows in set. Elapsed: 0.004 sec. 

9、#node3查询本地表 test_local
node3 :) select * from test_local;
SELECT *
FROM test_local
┌─id─┬─name─┐
│  5 │ 田七  │
│  6 │ 赵八  │
│  8 │ ls   │
│ 11 │ tq   │
└────┴──────┘
4 rows in set. Elapsed: 0.005 sec. 
```



以上在node1节点上创建的分布式表t_distributed 虽然数据是分布式存储在每个clickhouse节点上的，但是只能在node1上查询t_distributed 表，其他clickhouse节点查询不到此分布式表。如果想要在每台clickhouse节点上都能访问分布式表我们可以指定集群，创建分布式表：

```text
#创建分布式表 t_cluster ,引擎使用Distributed 引擎
node1 :) create table t_cluster on cluster clickhouse_cluster_3shards_1replicas (id UInt8,name String) engine = Distributed(clickhouse_cluster_3shards_1replicas,default,test_local,id);
CREATE TABLE t_cluster ON CLUSTER clickhouse_cluster_3shards_1replicas
(
    `id` UInt8,
    `name` String
)
ENGINE = Distributed(clickhouse_cluster_3shards_1replicas, default, test_local, id)
┌─host──┬─port─┬─status─┬─error─┬─num_hosts_remaining─┬─num_hosts_active─┐
│ node3 │ 9000 │      0 │       │                   2 │                0 │
│ node2 │ 9000 │      0 │       │                   1 │                0 │
│ node1 │ 9000 │      0 │       │                   0 │                0 │
└───────┴──────┴────────┴───────┴─────────────────────┴──────────────────┘
3 rows in set. Elapsed: 0.152 sec. 
```



上面的语句中使用了ON CLUSTER分布式DDL（数据库定义语言），这意味着在集群的每个分片节点上，都会创建一张Distributed表，这样便可以从其中任意一端发起对所有分片的读、写请求。





# 七 DDL操作

DDL:Data Definition Language,数据库定义语言。在ClickHouse中，DDL语言中修改表结构仅支持Merge表引擎、Distributed表引擎及MergeTree家族的表引擎,SQL 中的库、表、字段严格区分大小写。

## 7.1 创建库

- 创建库基础语法：

```text
CREATE DATABASE [IF NOT EXISTS] db_name [ON CLUSTER cluster] [ENGINE = engine(...)]
```



## 7.2 查看数据库

- 查看数据库语法

```text
SHOW DATABASES;
```



## 7.3 删除库

- 删除库基础语法：

```text
DROP DATABASE [IF EXISTS] db [ON CLUSTER cluster]
```



- 示例：

```text
#创建库 test_db
node1 :) create database if not exists test_db;
CREATE DATABASE IF NOT EXISTS test_db
Ok.
0 rows in set. Elapsed: 0.007 sec.

#删除库
node1 :) drop database test_db;
DROP DATABASE test_db
Ok.
0 rows in set. Elapsed: 0.003 sec.
```



注意：在创建数据库时，在/var/lib/clickhouse/metadata/目录下会有对应的库目录和库.sql文件，库目录中会存入在当前库下建表的信息，xx.sql文件中存入的是建库的信息。如图：

![img](readme.assets/v2-cbaa7fab2fb0af3aebaf2032b389ad8b_1440w.webp)

当删除数据库时，/var/lib/clickhouse/metadata/目录下对应的库目录和xx.sql文件也会被清空。

## 7.4 创建表

创建表的基本语法：

```text
#第一种
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
) ENGINE = engine

#第二种
CREATE TABLE [IF NOT EXISTS] [db.]table_name AS [db2.]name2 [ENGINE = engine]

#第三种
CREATE TABLE [IF NOT EXISTS] [db.]table_name ENGINE = engine AS SELECT ...
```



注意：以上普通第一种建表语句是直接创建表。第二种创建表方式可以创建一个与db2中name2一样结构的表，也可以指定表引擎，也可以不指定，不指定默认与db2中的name2表引擎一样，不会将表name2中的数据填充到对应的新表中。第三种方式可以指定引擎创建一个与Select 子句的结果相同结构的表，并将Select子句的结果填充它。

- 示例：

```text
#第一种方式创建表
create table if not exists newdb.t1(
 id UInt8 default 0 comment '编号',
 name String default '无姓名' comment '姓名',
 age UInt8 default 18 comment '年龄'
 )engine = TinyLog;

CREATE TABLE IF NOT EXISTS newdb.t1
(
    `id` UInt8 DEFAULT 0 COMMENT '编号',
    `name` String DEFAULT '无姓名' COMMENT '姓名',
    `age` UInt8 DEFAULT 18 COMMENT '年龄'
)
ENGINE = TinyLog
Ok.
0 rows in set. Elapsed: 0.004 sec. 

# 第二种方式创建表
create table if not exists t2 engine = Memory as newdb.t_tinylog;
CREATE TABLE IF NOT EXISTS t2 AS newdb.t_tinylog
ENGINE = Memory
Ok.
0 rows in set. Elapsed: 0.006 sec.

# 第三种方式创建表
create table if not exists t3 engine = Memory as select * from newdb.t_tinylog where id >2;
CREATE TABLE IF NOT EXISTS t3
ENGINE = Memory AS
SELECT *
FROM newdb.t_tinylog
WHERE id > 2
Ok.
0 rows in set. Elapsed: 0.010 sec. 

#查询表t3数据
select * from t3;
SELECT *
FROM t3
┌─id─┬─name─┬─age─┐
│  3 │ 王五 │  20 │
└────┴──────┴─────┘
1 rows in set. Elapsed: 0.004 sec.
```



## 7.5 查看表

- 查看表语法：

```text
SHOW TABLES;
SHOW TABLES IN default;
```



## 7.6 查看表的定义

- 查看表定义语法：

```text
SHOW CREATE TABLE XXX;
```



- 示例：

```text
#查看表定义
node1 :) show create table t3;
SHOW CREATE TABLE t3
┌─statement─────────────────────────────────┐
│ CREATE TABLE newdb.t3
(
    `id` UInt8,
    `Name` String
)
ENGINE = TinyLog │
└───────────────────────────────────────────┘
1 rows in set. Elapsed: 0.002 sec. 
```



## 7.7 查看表的字段

- 查看表定义语法：

```text
DESC XXXX;
```



- 示例：

```text
#查看表t3的字段
node1 :) desc t3;
DESCRIBE TABLE t3
┌─name─┬─type───┬─default_type─┬─...
│ id   │ UInt8  │              │  ...
│ Name │ String │              │  ...
└──────┴────────┴──────────────┴──...
2 rows in set. Elapsed: 0.004 sec.
```



## 7.8 删除表

- 删除表的基本语法：

```text
DROP [TEMPORARY] TABLE [IF EXISTS] [db.]name [ON CLUSTER cluster]
```



- 示例：

```text
#删除表
node1 :) drop table t3;

DROP TABLE t3
Ok.
0 rows in set. Elapsed: 0.003 sec. 
```



## 7.9 修改表

- 修改表语法

```text
ALTER TABLE [db].name [ON CLUSTER cluster] ADD|DROP|CLEAR|COMMENT|MODIFY COLUMN ...
```

## 7.10 添加列

- 示例：

```text
#使用default 库，创建表 test1,使用MergeTree引擎
node1 :) use default;
node1 :) create table test1(id UInt8,name String)engine = MergeTree() order by id partition by name;
CREATE TABLE test1
(
    `id` UInt8,
    `name` String,
    `loc` String
)
ENGINE = MergeTree()
PARTITION BY loc
ORDER BY id
Ok.
0 rows in set. Elapsed: 0.005 sec.Ok.

#查看表test1表结构
node1 :) desc test1;
DESCRIBE TABLE test1
┌─name─┬─type───┬...
│ id   │ UInt8  │...
│ name │ String │...
│ loc  │ String │...
└──────┴────────┴...
3 rows in set. Elapsed: 0.004 sec. 

#添加表字段
node1 :) alter table test1 add column age UInt8;

#查看表结构，添加字段成功
node1 :) desc test1;
DESCRIBE TABLE test1
┌─name─┬─type───┬...
│ id   │ UInt8  │...
│ name │ String │...
│ loc  │ String │...
│ age  │ UInt8  │...
└──────┴────────┴...
4 rows in set. Elapsed: 0.003 sec. 
```

## 7.11 删除列

- 示例：

```text
#删除表test1中的name age字段
node1 :) alter table test1 drop column age;

#查看表 test1表结构
node1 :) desc test1;
DESCRIBE TABLE test1
┌─name─┬─type───┬...
│ id   │ UInt8  │...
│ name │ String │...
│ loc  │ String │...
└──────┴────────┴...
2 rows in set. Elapsed: 0.004 sec. 
```

## 7.12 清空列

注意，不能清空排序、主键、分区字段。

- 示例：

```text
#向表 test1中插入以下几条数据
node1 :)  insert into table test1  values (1,'张三','北京'),(2,'李四','上海'),(3,'王五','北京');

#查看表中的数据
┌─id─┬─name─┬─loc──┐
│  1 │ 张三 │ 北京 │
│  3 │ 王五 │ 北京 │
└────┴──────┴──────┘
┌─id─┬─name─┬─loc──┐
│  2 │ 李四 │ 上海 │
└────┴──────┴──────┘

#清空 test1 name列在’北京’分区的值
node1 :) alter table test1 clear column name in partition '北京';

#查看表中的数据
node1 :) select * from test1;
┌─id─┬─name─┬─loc──┐
│  1 │      │ 北京 │
│  3 │      │ 北京 │
└────┴──────┴──────┘
┌─id─┬─name─┬─loc──┐
│  2 │ 李四 │ 上海 │
└────┴──────┴──────┘

#清空 test1 name 列下的值
node1 :) alter table test1 clear column name;

#查看表中的数据
node1 :) select * from test1;
┌─id─┬─name─┬─loc──┐
│  1 │      │ 北京 │ 
│  3 │      │ 北京 │
└────┴──────┴──────┘
┌─id─┬─name─┬─loc──┐
│  2 │      │ 上海 │
└────┴──────┴──────┘
```

## 7.13 给列修改注释

- 示例：

```text
#修改表 test1 name 列的注释
node1 :) alter table test1 comment column name '姓名';

#查看表 test1描述
┌─name─┬─type───┬─default_type─┬─default_expression─┬─comment─┬...
│ id   │ UInt8  │              │                    │         │...
│ name │ String │              │                    │ 姓名    │...
│ loc  │ String │              │                    │         │...
└──────┴────────┴──────────────┴────────────────────┴─────────┴...
```

## 7.14 修改列类型

- 示例：

```text
#修改表 test1 name列类型为UInt8
node1 :) alter table test1 modify column name UInt8

#node1 :) desc test1;
┌─name─┬─type───┬─default_type─┬─default_expression─┬─comment─┬
│ id   │ UInt8  │              │                    │         │
│ name │ UInt8  │              │                    │ 姓名    │
│ loc  │ String │              │                    │         │
└──────┴────────┴──────────────┴────────────────────┴─────────┴
```



## 7.15 给表重命名

给表重新命名可以作用在任意的表引擎上。

- 给表重命名语法：

```text
RENAME TABLE [db11.]name11 TO [db12.]name12, [db21.]name21 TO [db22.]name22, ... [ON CLUSTER cluster]
```



示例：

```text
#创建库 testdb1
node1 :) create database testdb1;

#创建库 testdb2
node1 :) create database testdb2;

#使用库testdb1,并创建表 t1
node1 :) use testdb1;
node1 :) create table t1 (id UInt8 ,name String) engine = MergeTree() order by id ;

#将表 t1 重命名为test1
node1 :) rename table t1 to test1;

#将表test1 移动到testdb2库下，并重新命名为t2, testdb1 下没有表了
node1 :) rename table testdb1.test1 to testdb2.t2;
```



## 7.16 分区表的DDL操作

ClickHouse中只有MergeTree家族引擎下的表才能分区。这里说的分区表就是MergeTree家族表引擎对应的分区表。

### **1、查看分区信息**

- 示例：

```text
#在newdb中创建分区表 t_partition ,使用MergeTree引擎
node1 :) create table t_partition (id UInt8,name String,age UInt8,loc String) engine = MergeTree() order by id partition by loc;

#向表 t_partition 中插入以下数据：
node1 :) insert into t_partition values (1,'张三',18,'BJ'),(2,'李四',19,'GZ'),(3,'王五',20,'BJ'),(4,'马六',21,'GZ');

#查询表 t_partition 的分区信息
node1 :) select database,table,name,partition from system.parts where table = 't_partition';
┌─database─┬─table───────┬─name───────────────────────────────────┬─partition─┐
│ newdb    │ t_partition │ 8700fff36a8bf87b6ea3eedb16d04038_2_2_0 │ GZ        │
│ newdb    │ t_partition │ e35d0ca9d946a627c9fc98b8f80391ce_1_1_0 │ BJ        │
└──────────┴─────────────┴────────────────────────────────────────┴───────────┘

#也可以在ClickHouse节点上查看分区信息，路径为：/var/lib/clickhouse/data/newdb/t_partition/,信息如下：
```



![img](readme.assets/v2-6103ae350fe4babd380ae8002ecac368_1440w.png)

### **2、卸载分区**

将指定分区的数据移动到 detached 目录。服务器会忽略被分离的数据分区。只有当你使用 ATTACH 时，服务器才会知晓这部分数据。当执行操作以后，可以对 detached 目录的数据进行任意操作，例如删除文件，或者放着不管。

- 卸载分区语法：

```text
ALTER TABLE table_name DETACH PARTITION partition_expr
```



- 示例：

```text
#卸载 表 t_partition 中 ‘BJ’分区数据
node1 :) alter table t_partition detach partition 'BJ'

#查看表 t_partition中的数据
node1 :) select * from t_partition;
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘

#查看表 t_partition 中的分区信息
node1 :) select database,table,name,partition from system.parts where table = 't_partition';
┌─database─┬─table───────┬─name───────────────────────────────────┬─partition─┐
│ newdb    │ t_partition │ 8700fff36a8bf87b6ea3eedb16d04038_2_2_0 │ GZ        │
└──────────┴─────────────┴────────────────────────────────────────┴───────────┘

#查看路径/var/lib/clickhouse/data/newdb/t_partition/detached中数据，发现卸载的对应分区移动到此目录中
```



![img](readme.assets/v2-d2a90081063f34fd3156eb464f296e5c_1440w.png)

### **3、装载分区**

我们可以将已经卸载的分区重新装载到对应的表分区中。这里就是将detached目录中的数据重新移动到对应的表数据目录下。

也可以将卸载的分区数据加载到其他表中，但是这个表需要与原来的表具有相同的表结构及相同的分区字段。

- 装载分区数据语法：

```text
ALTER TABLE table_name ATTACH PARTITION partition_expr
```



- 示例：

```text
#将表 t_partition 对应的 ‘BJ’分区装载回来
node1 :) alter table t_partition attach partition 'BJ';

#查看表 t_partition 中的数据
node1 :) select * from t_partition;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘

#查看表 t_partition 分区信息
node1 :) select database,table,name,partition from system.parts where table = 't_partition';
```



![img](readme.assets/v2-15bf180851f3eaeb6d3926fa76b72803_1440w.png)



### **4、删除分区**

ClickHouse中的分区表，可以针对分区表删除某个分区，之后再导入当前分区的数据，以达到数据更新的目的。

执行删除分区命名是直接将对应分区数据删除，不会放入detached目录。该操作会将分区标记为不活跃的，然后在大约10分钟内删除全部数据。

- 删除分区语法：

```text
ALTER TABLE table_name DROP PARTITION partition_expr
```



- 示例

```text
#删除表 t_partition 中的 'BJ' 分区：
node1 :) alter table t_partition drop partition 'BJ';

#查询 t_partition 中的分区信息：
node1 :) select database,table,name,partition from system.parts where table = 't_partition';
┌─database─┬─table───────┬─name───────────────────────────────────┬─partition─┐
│ newdb    │ t_partition │ 8700fff36a8bf87b6ea3eedb16d04038_2_2_0 │ GZ        │
└──────────┴─────────────┴────────────────────────────────────────┴───────────┘
```



### **5、替换分区**

替换分区支持将table1表的分区数据复制到table2表，并替换table2表的已有分区。table1表中分区数据不会被删除，table1和table2表必须要有相同的表结构且分区字段相同。这个操作经常用作数据备份、表数据同步操作。

- 替换分区语法：

```text
ALTER TABLE table2 REPLACE PARTITION partition_expr FROM table1
```



- 示例：

```text
#创建表 table1 和table2 ，使用MergeTree表引擎，并且两表结构相同
node1 :) create table table1 (id UInt8,name String,age UInt8,loc String) engine = MergeTree() order by id partition by loc;

node1 :) create table table2 (id UInt8,name String,age UInt8,loc String) engine = MergeTree() order by id partition by loc;

#向table1中插入以下数据
node1 :) insert into table1 values (1,'张三',18,'BJ'),(2,'李四',19,'GZ'),(3,'王五',20,'BJ'),(4,'马六',21,'GZ');
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘

#向table2中插入以下数据
node1 :) insert into table2 values (5,'田七',22,'BJ'),(6,'赵八',23,'GZ'),(7,'李九',24,'BJ'),(8,'郑十',25,'GZ');
┌─id─┬─name─┬─age─┬─loc─┐
│  5 │ 田七 │  22 │ BJ  │
│  7 │ 李九 │  24 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │  GZ │
│  8 │ 郑十 │  25 │  GZ │
└────┴──────┴─────┴─────┘

#将table1表中’BJ’分区内的数据替换到table2中
node1 :) alter table table2 replace partition 'BJ' from table1;

#查看表 table2中的数据
node1 :) select * from table2;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │ GZ  │
│  8 │ 郑十 │  25 │ GZ  │
└────┴──────┴─────┴─────┘

#查看表 table1中的数据，没有变化，不会删除 ‘BJ’ 分区的数据
node1 :) select * from table1;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘
```



### **6、移动分区**

该操作将 table_source表的数据分区移动到 table_dest表，并删除table_source表的数据。

- 移动分区语法：

```text
ALTER TABLE table_source MOVE PARTITION partition_expr TO TABLE table_dest
```



- 示例：

```text
#创建表 table_source ,table_dest, 两表结构相同，都是MergeTree引擎表
node1 :) create table table_source (id UInt8,name String,age UInt8,loc String) engine = MergeTree() order by id partition by loc;

node1 :) create table table_dest (id UInt8,name String,age UInt8,loc String) engine = MergeTree() order by id partition by loc;

#向table_source 表中插入以下数据
node1 :) insert into table_source values (1,'张三',18,'BJ'),(2,'李四',19,'GZ'),(3,'王五',20,'BJ'),(4,'马六',21,'GZ');
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘

#向table_dest 表中插入以下数据：
node1 :) insert into table_dest values (5,'田七',22,'BJ'),(6,'赵八',23,'GZ'),(7,'李九',24,'BJ'),(8,'郑十',25,'GZ');
┌─id─┬─name─┬─age─┬─loc─┐
│  5 │ 田七 │  22 │ BJ  │
│  7 │ 李九 │  24 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │  GZ │
│  8 │ 郑十 │  25 │  GZ │
└────┴──────┴─────┴─────┘

#将表 table_source 中的分区‘BJ’的数据移动到 table_dest表中
node1 :) alter table table_source move partition 'BJ' to table table_dest;

#查看表 table_source中的数据
node1 :) select * from table_source;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘

#查看表 table_dest中的数据
node1 :) select * from table_dest;
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │ GZ  │
│  8 │ 郑十 │  25 │ GZ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  5 │ 田七 │  22 │ BJ  │
│  7 │ 李九 │  24 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
└────┴──────┴─────┴─────┘

#手动执行 optimize 命令，合并table_dest相同分区数据
node1 :) optimize table table_dest;

#查询表 table_dest中的数据
node1 :) select * from table_dest;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │ 张三 │  18 │ BJ  │
│  3 │ 王五 │  20 │ BJ  │
│  5 │ 田七 │  22 │ BJ  │
│  7 │ 李九 │  24 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │ GZ  │
│  8 │ 郑十 │  25 │ GZ  │
└────┴──────┴─────┴─────┘

#查看 table_source 表中的数据，分区‘BJ’被删除。
node1 :) select * from table_source;
┌─id─┬─name─┬─age─┬─loc─┐
│  2 │ 李四 │  19 │ GZ  │
│  4 │ 马六 │  21 │ GZ  │
└────┴──────┴─────┴─────┘
```



### **7、重置分区列**

重置指定分区的特定列的值，就是将指定分区下某列的数据清空，如果建表时使用了 DEFAULT 语句，该操作会将列的值重置为该默认值。

- 重置分区列语法：

```text
ALTER TABLE table_name CLEAR COLUMN column_name IN PARTITION partition_expr
```



- 示例：

```text
#针对之前的表 table_dest中的数据进行操作，清空当前表中 ‘BJ’分区中name列
node1 :) alter table table_dest clear column name in partition 'BJ';

#查看表 table_dest中的数据
node1 :) select * from table_dest;
┌─id─┬─name─┬─age─┬─loc─┐
│  1 │      │  18 │ BJ  │
│  3 │      │  20 │ BJ  │
│  5 │      │  22 │ BJ  │
│  7 │      │  24 │ BJ  │
└────┴──────┴─────┴─────┘
┌─id─┬─name─┬─age─┬─loc─┐
│  6 │ 赵八 │  23 │ GZ  │
│  8 │ 郑十 │  25 │ GZ  │
└────┴──────┴─────┴─────┘
```



## 7.17 临时表

**ClickHouse支持临时表，临时表具备以下特征：**

- 当会话结束或者链接中断时，临时表将随会话一起消失。
- 临时表仅能够使用Memory表引擎，创建临时表时不需要指定表引擎。
- 无法为临时表指定数据库。它是在数据库之外创建的，与会话绑定。
- 如果临时表与另一个表名称相同，那么当在查询时没有显式的指定db的情况下，将优先使用临时表。
- 对于分布式处理，查询中使用的临时表将被传递到远程服务器。

**创建一个临时表：**

```text
CREATE TEMPORARY TABLE [IF NOT EXISTS] table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2],
    ...
)
```



**注意：不需要指定表引擎，默认是Memory**

- 示例：

```text
#查看库 newdb下 表
node1 :) show tables;

SHOW TABLES
┌─name────────┐
│ t1          │
│ t2          │
│ t_log       │
│ t_stripelog │
│ t_tinylog   │
└─────────────┘
5 rows in set. Elapsed: 0.004 sec. 

#查询表 t_log表数据
node1 :) select * from t_log;

SELECT *
FROM t_log
┌─id─┬─name─┬─age─┐
│  1 │ 张三 │  18 │
│  2 │ 李四 │  19 │
└────┴──────┴─────┘
┌─id─┬─name─┬─age─┐
│  3 │ 王五 │  20 │
│  4 │ 马六 │  21 │
│  5 │ 田七 │  22 │
└────┴──────┴─────┘
5 rows in set. Elapsed: 0.004 sec. 

#创建临时表 t_log ，与当前库下的t_log同名
node1 :) create temporary table t_log(id UInt8 ,name String);

CREATE TEMPORARY TABLE t_log
(
    `id` UInt8,
    `name` String
)

Ok.

0 rows in set. Elapsed: 0.001 sec. 

#查询表 t_log的数据与结构，发现没有数据，这里查询的是临时表，结构如下：
node1 :) desc t_log;

DESCRIBE TABLE t_log
┌─name─┬─type───┬
│ id   │ UInt8  │
│ name │ String │
└──────┴────────┴
2 rows in set. Elapsed: 0.003 sec. 

#如果想要查询到库newdb下的t_log需要加上数据库名
node1 :) select * from newdb.t_log;

#切换库为default，同样还可以查询到表t_log，说明表不属于任何库
node1 :) use default;
node1 :) desc t_log;
DESCRIBE TABLE t_log
┌─name─┬─type───┬
│ id   │ UInt8  │
│ name │ String │
└──────┴────────┴
2 rows in set. Elapsed: 0.004 sec.

#退出客户端之后，重新登录，查询t_log不存在。
node1 :) select * from t_log;
Exception: Received from localhost:9000. DB::Exception: Table default.t_log doesn't exist.. 

#也可以不退出客户端直接删除临时表
node1 :) drop table t_log;

DROP TABLE t_log
Ok.
0 rows in set. Elapsed: 0.001 sec.
```



**注意：**在大多数情况下，临时表不是手动创建的，而是在使用外部数据进行查询或分布式时创建的,可以使用ENGINE = Memory的表代替临时表。

## 7.18 视图

ClickHouse中视图分为普通视图和物化视图，两者区别如图所示：

![img](https://pic4.zhimg.com/80/v2-d824bc9c1b0f05c602fea0367f5442ab_1440w.webp)



### **1、普通视图**

普通视图不存储数据，它只是一层select 查询映射，类似于表的别名或者同义词，能简化查询，对原有表的查询性能没有增强的作用，具体性能依赖视图定义的语句，当从视图中查询时，视图只是替换了映射的查询语句。普通视图当基表删除后不可用。

- 创建普通视图语法：

```text
CREATE [OR REPLACE] VIEW [IF NOT EXISTS] [db.]table_name [ON CLUSTER] AS SELECT ...
```



- 示例：

```text
#在库 newdb中创建表 personinfo
node1 :) create table personinfo(id UInt8,name String,age UInt8,birthday Date) engine = Log;

#向表 personinfo中插入如下数据：
node1 :) insert into personinfo values (1,'张三',18,'2022-06-01');
node1 :) insert into personinfo values (2,'李四',19,'2022-06-02');
node1 :) insert into personinfo values (3,'王五',20,'2022-06-03');
node1 :) insert into personinfo values (4,'马六',21,'2022-06-04');
node1 :) insert into personinfo values (5,'田七',22,'2022-06-05');

#查询表中的数据
node1 :) select * from personinfo;

SELECT *
FROM personinfo
┌─id─┬─name─┬─age─┬───birthday─┐
│  1 │ 张三 │  18 │ 2022-06-01 │
│  2 │ 李四 │  19 │ 2022-06-02 │
└────┴──────┴─────┴────────────┘
┌─id─┬─name─┬─age─┬───birthday─┐
│  3 │ 王五 │  20 │ 2022-06-03 │
│  4 │ 马六 │  21 │ 2022-06-04 │
│  5 │ 田七 │  22 │ 2022-06-05 │
└────┴──────┴─────┴────────────┘
5 rows in set. Elapsed: 0.004 sec.

#创建视图 person_view 映射查询子句
node1 :) create view person_view as select name,birthday from personinfo;
CREATE VIEW person_view AS
SELECT 
    name,
    birthday
FROM personinfo
Ok.
0 rows in set. Elapsed: 0.009 sec. 

#查询视图person_view中的数据结果
node1 :) select * from person_view;

SELECT *
FROM person_view
┌─name─┬───birthday─┐
│ 张三 │ 2022-06-01 │
│ 李四 │ 2022-06-02 │
└──────┴────────────┘
┌─name─┬───birthday─┐
│ 王五 │ 2022-06-03 │
│ 马六 │ 2022-06-04 │
│ 田七 │ 2022-06-05 │
└──────┴────────────┘
5 rows in set. Elapsed: 0.004 sec.

#删除视图 使用drop即可
node1 :) drop table person_view;

DROP TABLE person_view
Ok.
0 rows in set. Elapsed: 0.002 sec. 
```



### **2、物化视图**

**物化视图是查询结果集的一份持久化存储**，所以它与普通视图完全不同，而非常趋近于表。”查询结果集”的范围很宽泛，可以是基础表中部分数据的一份简单拷贝，也可以是多表join之后产生的结果或其子集，或者原始数据的聚合指标等等。

物化视图创建好之后，若**源表被写入新数据则物化视图也会同步更新**,POPULATE 关键字决定了物化视图的更新策略,若有POPULATE 则在创建视图的过程会将源表已经存在的数据一并导入，类似于 create table ... as,若无POPULATE 则物化视图在创建之后没有数据，只会在创建只有同步之后写入源表的数据，clickhouse 官方并不推荐使用populated，因为在创建物化视图的过程中同时写入的数据不能被插入物化视图。

物化视图是种特殊的数据表，**创建时需要指定引擎**，可以用show tables 查看。另外，物化视图不支持alter 操作。

产生物化视图的过程就叫做“物化”（materialization），广义地讲，物化视图是数据库中的预计算逻辑+显式缓存，典型的**空间换时间**思路，所以用得好的话，它可以避免对基础表的频繁查询并复用结果，从而显著提升查询的性能。

- 物化视图创建语法：

```text
CREATE MATERIALIZED VIEW [IF NOT EXISTS] [db.]table_name [ON CLUSTER] [TO[db.]name] [ENGINE = engine] [POPULATE] AS SELECT ...
```



- 示例：

```text
#在库 newdb 中创建物化视图 t_view1
node1 :) create materialized view  t_view1 engine = Log as select * from personinfo;

#查询 所有表
node1 :) show tables;

SHOW TABLES
┌─name───────────┐
│ .inner.t_view1 │
│ personinfo     │ 
└────────────────┘
2 rows in set. Elapsed: 0.004 sec. 

#向表 personinfo中插入如下数据：
node1 :) insert into personinfo values (1,'张三',18,'2022-06-01');
node1 :) insert into personinfo values (2,'李四',19,'2022-06-02');
node1 :) insert into personinfo values (3,'王五',20,'2022-06-03');
node1 :) insert into personinfo values (4,'马六',21,'2022-06-04');
node1 :) insert into personinfo values (5,'田七',22,'2022-06-05');

#查看物化视图 t_view1数据
node1 :) select * from t_view1;

SELECT *
FROM t_view1
┌─id─┬─name─┬─age─┬───birthday─┐
│  1 │ 张三 │  18 │ 2022-06-01 │
│  2 │ 李四 │  19 │ 2022-06-02 │
└────┴──────┴─────┴────────────┘
┌─id─┬─name─┬─age─┬───birthday─┐
│  3 │ 王五 │  20 │ 2022-06-03 │
│  4 │ 马六 │  21 │ 2022-06-04 │
│  5 │ 田七 │  22 │ 2022-06-05 │
└────┴──────┴─────┴────────────┘
5 rows in set. Elapsed: 0.004 sec. 

#创建物化视图 t_view2
node1 :) create materialized view  t_view2 engine = Log as select count(name) as cnt from personinfo;

#向表 personinfo中插入以下数据
node1 :) insert into personinfo values (6,'赵八',23,'2022-06-06'),(7,'孙九',22,'2022-06-07');

#查询物化视图表 t_view2数据,可以看到做了预计算，这里不能一条条插入，不然效果是每条数据都会生成一个结果。
node1 :) select * from t_view2;

SELECT *
FROM t_view2
┌─cnt─┐
│   2 │
└─────┘
1 rows in set. Elapsed: 0.004 sec. 

#删除物化视图
node1 :) drop table t_view2;
DROP TABLE t_view2
Ok.
0 rows in set. Elapsed: 0.001 sec.
```



**注意：**当创建好物化视图t_view1时，可以进入到/var/lib/clickhouse/data/newdb目录下看到%2Einner%2Et_view1目录，当物化视图中同步基表数据时，目录中有对应的列文件和元数据记录文件，与普通创建表一样，有目录结构。



# 八、DML

DML:Data Manipulation Language,数据操纵语言。ClickHouse中DML语言包含插入、更新、删除数据操作,DML操作仅适用MergeTree引擎,不能针对主键、分区键、排序键进行DML操作，DML操作不支持事务，一旦执行成功会立刻生效。

## 8.1 Insert 插入

insert 向表中插入数据。

- insert 语法：

```text
INSERT INTO [db.]table [(c1, c2, c3)] VALUES (v11, v12, v13), (v21, v22, v23), ...
或者
INSERT INTO [db.]table Select ...
```



- 示例：

```text
#创建表 t_insert ,这里使用MergeTree引擎
node1 :) create table t_insert (id UInt8 ,name String) engine = MergeTree() order by id ;

#向表 t_insert 中插入数据
node1 :) insert into t_insert values (1,'张三',18),(2,'李四',19);

#向表 t_insert 中插入数据
node1 :) insert into t_insert select * from t_insert;
```



## 8.2 update 更新

由于ClickHouse针对的是OLAP业务分析，Update操作在ClickHouse中不会经常使用。这种更新效率低下。

- update 更新操作语法：

```text
ALTER TABLE [db.]table UPDATE column1 = expr1 [, ...] WHERE filter_expr
```



- 示例：

```text
#创建表 t_update,使用MergeTree引擎
node1 :) create table t_update (id UInt8,name String,age UInt8) engine = MergeTree() order by id ;

#向表 t_update中插入如下数据
node1 :) insert into t_update values (1,'张三',18),(2,'李四',19)
┌─id─┬─name─┬─age─┐
│  1 │ 张三 │  18 │
│  2 │ 李四 │  19 │
└────┴──────┴─────┘

#更新姓名为 张三的年龄为 22
node1 :) alter table t_update update age = 22 where name = '张三';
┌─id─┬─name─┬─age─┐
│  1 │ 张三 │  22 │
│  2 │ 李四 │  19 │
└────┴──────┴─────┘
```



## 8.3 delete 删除

由于ClickHouse针对的是OLAP业务分析，Delete操作与Update操作一样在ClickHouse中不会经常使用。这种删除效率低下。

- delete 删除语法：

```text
ALTER TABLE [db.]table [ON CLUSTER cluster] DELETE WHERE filter_expr
```



- 示例：

```text
#创建表 t_delete,使用MergeTree引擎
node1 :) create table t_delete (id UInt8,name String,age UInt8) engine = MergeTree() order by id ;

#向表 t_delete中插入以下数据
node1 :) insert into t_update values (1,'张三',18),(2,'李四',19)
┌─id─┬─name─┬─age─┐
│  1 │ 张三 │  18 │
│  2 │ 李四 │  19 │
└────┴──────┴─────┘

#删除 张三 此条数据
node1 :) alter table t_delete delete where name = '张三';
┌─id─┬─name─┬─age─┐
│  2 │ 李四 │  19 │
└────┴──────┴─────┘
```



## 8.4 向表中导入导出数据

ClickHouse中支持多种数据格式数据导入和导出，支持格式有ORC,Parquet,Avro,Protobuf,xml,json,csv等，具体操作参照官网：**[https://clickhouse.tech/docs/en/sql-reference/statements/alter/update/](https://link.zhihu.com/?target=https%3A//clickhouse.tech/docs/en/sql-reference/statements/alter/update/)[1]**下面以向表导入导出CSV格式数据为例操作：

- 示例：

```text
#创建表 t_csv ，执行引擎为MergeTree
node1 :) create table t_csv (id UInt8,name String,age UInt8) engine = MergeTree order by id;

#在ClickHouse客户端准备数据文件 csvdata 写入以下数据
vim /root/csvdata
1,张三,18
2,李四,19
3,王五,20
4,马六,21
5,田七,22
#导入数据，在ClickHouse-client中执行导入数据命令
[root@node1 ~]# clickhouse-client --format_csv_delimiter="," --query="INSERT INTO newdb.t_csv FORMAT CSV" < /root/csvdata

注意：--format_csv_delimiter 指定分隔符

#进入ClickHouse客户端查看表 t_csv中的数据
node1 :) select * from t_csv;

┌─id─┬─name─┬─age─┐
│  1 │ 张三 │  18 │
│  2 │ 李四 │  19 │
│  3 │ 王五 │  20 │
│  4 │ 马六 │  21 │
│  5 │ 田七 │  22 │
└────┴──────┴─────┘

#导出数据，在ClickHouse-client中执行命令，将数据导入到result文件中
[root@node1 ~]# clickhouse-client --format_csv_delimiter="|" --query="select * from newdb.t_csv FORMAT CSV" > /root/result

#查看导出的结果数据
[root@node1 ~]# cat result 
1|"张三"|18
2|"李四"|19
3|"王五"|20
4|"马六"|21
5|"田七"|22
```

### **参考资料**

[1]

[https://clickhouse.tech/docs/en/sql-reference/statements/alter/update/:](https://link.zhihu.com/?target=https%3A//clickhouse.tech/docs/en/sql-reference/statements/alter/update/%3A) *[https://clickhouse.tech/docs/en](https://link.zhihu.com/?target=https%3A//clickhouse.tech/docs/en/sql-reference/statements/alter/update/%E3%80%82)*

# 九、spark 写入ck

[-](https://zhuanlan.zhihu.com/p/567666302)

## **Spark 写入 ClickHouse API**

SparkCore写入ClickHouse，可以直接采用写入方式。下面案例是使用SparkSQL将结果存入ClickHouse对应的表中。**在ClickHouse中需要预先创建好对应的结果表。**

## 9.1 导入依赖

```text
<!-- 连接ClickHouse需要驱动包-->
<dependency>
  <groupId>ru.yandex.clickhouse</groupId>
  <artifactId>clickhouse-jdbc</artifactId>
  <version>0.2.4</version>
  <!-- 去除与Spark 冲突的包 -->
  <exclusions>
    <exclusion>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </exclusion>
    <exclusion>
        <groupId>net.jpountz.lz4</groupId>
        <artifactId>lz4</artifactId>
    </exclusion>
</exclusions>
</dependency>

<!-- Spark-core -->
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-core_2.11</artifactId>
  <version>2.3.1</version>
</dependency>
<!-- SparkSQL -->
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-sql_2.11</artifactId>
  <version>2.3.1</version>
</dependency>
<!-- SparkSQL  ON  Hive-->
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-hive_2.11</artifactId>
  <version>2.3.1</version>
</dependency>
```



## 9.2 代码编写

```text
val session: SparkSession = SparkSession.builder().master("local").appName("test").getOrCreate()
val jsonList = List[String](
  "{\"id\":1,\"name\":\"张三\",\"age\":18}",
  "{\"id\":2,\"name\":\"李四\",\"age\":19}",
  "{\"id\":3,\"name\":\"王五\",\"age\":20}"
)

//将jsonList数据转换成DataSet
import session.implicits._
val ds: Dataset[String] = jsonList.toDS()

val df: DataFrame = session.read.json(ds)
df.show()

//将结果写往ClickHouse
val url = "jdbc:clickhouse://node1:8123/default"
val table = "test"
val properties = new Properties()
properties.put("driver", "ru.yandex.clickhouse.ClickHouseDriver")
properties.put("user", "default")
properties.put("password", "")
properties.put("socket_timeout", "300000")
df.write.mode(SaveMode.Append).option(JDBCOptions.JDBC_BATCH_INSERT_SIZE, 100000).jdbc(url, table, properties)
```



# 十、可视化工具

[-](https://zhuanlan.zhihu.com/p/568854000)



# 十一、TTL深入理解

[-](https://zhuanlan.zhihu.com/p/569863159)



TTL即Time To Live ，表示数据存活的时间。在MergeTree中，可以为某个列字段或整张表设置TTL。当时间到达时，如果是列字段级别的TTL，则会删除这列的数据；如果是表级别的TTL，则会删除整张表的数据；**如果同时设置了列级别和表级别的TTL，则会以先到期的那个为主。**

无论是列级别还是表级别的TTL，都需要依托某个DataTime或Date类型的字段，通过对这个时间字段的INTERVAL操作，来描述TTL的过期时间，例如：

```text
TTL time_col + INTERVAL 3 DAY
```

上述语句表示数据的存活时间是time_col时间的3天之后。

```text
TTL time_col + INTERVAL 1 MONTH
```

上述语句表示数据的存活时间是time_col时间的1个月之后。

INTERVAL 完整的操作包括：SECOND、MINUTE、HOUR、DAY、WEEK、MONTH、QUARTER、YEAR。



## 11.1 列级别TTL

如果想要设置列级别的TTL，在声明表字段的时候，为他们声明TTL表达式，主键字段不能被声明TTL，举例如下：

```text
#创建表t_mt2,指定gender 存活时间
CREATE TABLE t_mt2(
id UInt8,
name String,
age UInt8 ,
gender String  TTL create_time+INTERVAL 10 SECOND,
create_time DateTime
)engine=MergeTree
order by id;

注意：以上gender字段的存活时间为create_time取值基础上向后延续10秒，此ttl实现借助create_time时间字段。

#向表t_mt2中插入如下数据
node1 :) insert into t_mt2 values (1,'zs',18,'f',now());

注意：当经过10s后，正常我们可以执行optimize table t_mt2 final语句直接查看表中结果，发现列gender下的数据变成string类型的空。但是此ClickHouse版本有bug，需要重启ClickHouse后，再执行optimize table t_mt2 final语句，才能看到效果。经测试，在之前版本没有此问题。

#重启ClickHouse后，执行如下语句查看对应表中的数据
node1 :) optimize table t_mt2 final;
node1 :) select * from t_mt2;
```

![img](readme.assets/v2-c69340ab0b3bf04301773e14caa9cc7f_1440w.png)

如果想要修改列字段对的TTL,或是为已有字段添加TTL，则可以使用ALTER语句，如下：

```text
ALTER TABLE tbl MODIFY COLUMN col type TTL col_time + INTERVAL 1 DAY
```

举例如下：

```text
#给表 t_mt2中的age列，指定ttl过期时间为 5s
node1 :) alter table t_mt2 modify column age UInt8 ttl create_time+interval 5 second;

#查看t_mt2 建表语句，age列ttl 生效
node1 :) show create table t_mt2;
```

![img](readme.assets/v2-87064a22800b1cfe6ca4c36f0e04e7d1_1440w.webp)

## 11.2 表级别ttl

在ClickHouse中我们还可以对整张表设置TTL，需要在建表时在表参数中指定TTL表达式，当TTL触发时，满足过期时间的数据行将被整行删除。

举例：

```text
#创建表t_mt3,表级别指定TTL ，数据10s过期
CREATE TABLE t_mt3(
id UInt8,
name String,
age UInt8 ,
gender String,
create_time DateTime
)engine=MergeTree
order by id
TTL create_time+INTERVAL 10 SECOND;

#向表t_mt3中插入如下数据
node1 :) insert into t_mt3 values (1,'zs',18,'f',now());

#经过10s，执行optimize table t_mt3 final，再次查看表中数据被删除清空
node1 :) optimize table t_mt3 final;
```

我们也可以对表级别TTL进行修改，修改方法如下：

```text
ALTER TABLE tbl MODIFY TTL create_time +INTERVAL 3 DAY
```

举例如下：

```text
#修改表t_mt3 数据过期时间为1分钟
node1 :) alter table t_mt3 modify ttl create_time + interval 1 minute;

#查看t_mt3表的TTL
node1 :) show create table t_mt3;
```

![img](readme.assets/v2-1c42c0a64892755627b207f0674ca9dc_1440w.jpeg)

```text
#向表t_mt3中插入如下数据
node1 :) insert into t_mt3 values (1,'zs',18,'f',now());

#经过1分钟 执行optimize table t_mt3 final，数据被清空
node1 :) optimize table t_mt3 final;
```

**注意：无论是列级别TTL，还是表级别TTL，一旦设置后，目前没有取消的方法。**



# 十二、索引

[-](https://zhuanlan.zhihu.com/p/571660759)

## 12.1 一级索引

在MergeTree中PRIMARY KEY 主键并不用于去重，而是用于索引，加快查询速度,MergeTree会根据index_granularity间隔（默认8192行），为数据表生成一级索引并保存至primary.idx文件内，索引数据按照PRIMARY KEY 排序，相对于使用PRIMARY KEY 更常见的方式是通过ORDER BY 方式指定主键。

- **稀疏索引**

primary.idx文件内的一级索引采用稀疏索引实现。有稀疏索引就有稠密索引，二者区别如下：

![img](readme.assets/v2-4dd85002633e7c71a3269eba70483c89_1440w.jpeg)

在稠密索引中每一行索引标记都会对应到一行具体的数据记录。而在稀疏索引中每一行索引标记对应的是一段数据，而不是一行。

稀疏索引的优势显而易见，仅需要使用少量的索引标记就能够记录大量的数据区间位置信息，而且数据量越大优势越明显。在MergeTree系列引擎表中对应的primary.idx文件就是稀疏索引，由于稀疏索引占用空间小，所以primary.idx内的索引数据常驻内存。

- **索引粒度**

在ClickHouse MergeTree引擎中默认的索引粒度是8192，参数为index_granularity，一般我们不会修改此值，按照默认8192即可。我们可以通过以下sql语句查看每个MergeTree引擎表对应的index_granulariry的值：

```text
node1 :) show create table t_mt;
```



![img](readme.assets/v2-c2fe96ccbd200caa21e4d7db60a05fe5_1440w.jpeg)



索引粒度对于MergeTree表引擎非常重要，可以根据整个数据的长度，按照索引粒度对数据进行标注，然后抽取对应的数据形成索引。

- **索引形成过程**

表数据以index_granularity的粒度（默认8192）被标记成多个小区间，其中每个区间最多8192行数据，每个区间标记后形成一个MarkRange,通过start和end表示MarkRange的具体范围，数据文件也会按照index_granularity的间隔粒度生成压缩数据块。由于是稀疏索引，MergeTree需要间隔index_granularity行数据生成一条索引，同时对应一个索引编号，每个MarRange与一个索引编号对应，通过与start及end对应的索引编号的取值，可以得到对应的数值区间；索引编号对应的索引值会依据声明的主键字段获取，最终索引编号和索引值被写入primary.idx文件中保存。

假设现在有一份测试数据，共192行记录，其中主键ID为String类型，ID值从A000开始，后面依次为A001、A002...直到A192为止，假设我们设置MergeTree的索引粒度index_granularity=3,根据索引的生成规则，primary.idx文件内的索引数据如下：

![img](readme.assets/v2-b17a61f27943b5921fbb35f26cf88656_1440w.png)



根据索引数据，MergeTree将此数据片段划分成192/3=64个小的MarkRange,其中所有MarkRange的最大数值区间为[A000,+inf),划分的MarkRange如下：

![img](readme.assets/v2-443afd388a74d733dfa817bb7e7b2492_1440w.webp)

- **索引查询过程**

使用索引查询其实就是两个数值区间的交集判断，其中一个区间是有基于主键的查询条件转换而来的条件区间，而另一个区间是上图中**MarkRange**对应的**数值**区间。

整个索引查询的过程大致分为3个步骤：

**1、生成查询条件区间**

查询时首先将查询条件转换为条件区间，即便是单个值的查询条件也会转换成区间的形式，例如：

```text
WHERE ID='A003'
['A003','A003']

WHERE ID>'A000'
['A000',+inf]

WHERE ID<'A188'
(-inf,'A188']

WHERE ID like 'A006%'
('A006','A007']
```



**2、递归交集判断**

以递归的方式依次对MarkRange的数值区间与条件区间做交集判断，从最大的区间[A000,+inf)开:

- 如果不存在交集，则直接忽略掉整段MarkRange
- 如果存在交集，且MarkRange步长大于8(end-start),则将此区间进一步拆分成8个区间（由merge_tree_coarse_index_granularity指定，默认值为8），并重复此规则，继续做递归交集判断。
- 如果存在交集，且MarkRange不可再分解（步长小于8），则记录MarkRange并返回。

**3、合并MarkRange区间**

将最终匹配的MarkRange聚在一起，合并他们的范围。

当查询条件WHERE ID ='A003'的时候，最终读取[A000,A003)和[A003,A006]两个区间的数据即可，他们对应的MarkRange(start:0,end:2)范围，而无其他无用的区间都被裁剪过滤掉，因为MarkRange转换的数值区间是闭区间，所以会额外匹配到临近的一个区间，完整的逻辑图如下图所示：

![img](readme.assets/v2-2d33b5197421a4fbac7f9d1079b4aa79_1440w.webp)



## 12.2二级索引（跳数索引）

除了一级索引之外，MergeTree同样支持二级索引，二级索引又称为跳数索引，由数据的聚合信息构建而成，根据索引类型的不同，其聚合信息的内容也不同，跳数索引的目的与一级索引一样，也是帮助查询时减少数据扫描的范围。

跳数索引需要在Create语句内定义，完整语法如下：

```text
INDEX index_name expr TYPE index_type(...) GRANULARITY granularity
```



**对以上参数的解释如下：**

- index_name:定义的二级索引名称
- index_type:跳数索引类型，最常用就是minmax索引类型。minmax索引记录了一段数据内的最小和最大极值，其索引的作用类似分区目录，能够快速跳过无用的数据区间。
- granularity:定义聚合信息汇总的粒度。
- 与一级索引一样，如果在建表语句中声明了跳数索引，则会在路径“/var/lib/ClickHouse/data/DATABASE/TABLE/PARTITION/”目录下生成索引与标记文件（skp_idx.idx与skp_idx.mrk）。
- 在接触跳数索引时，很容易将index_granularity与granularity概念混淆，对于跳数索引而言，index_granularity定义了数据的粒度，而granularity定义了聚合信息汇总的粒度，也就是说，granularity定义了一行跳数索引能够跳过多少个index_granularity区间的数据。

**minmax跳数索引的生成规则**

minmax跳数索引聚合信息是在一个index_granularity区间内数据的最小和最大极值。首先，数据按照index_granularity粒度间隔将数据划分成n段，总共有[0~n-1]个区间（n=total_rows/index_granularity，向上取整），接着根据跳数索引从0区间开始，依次按index_granularity粒度从数据中获取聚合信息，每次向前移动1步，聚合信息逐步累加，最后当移动granularity次区间时，则汇总并生成一行跳数索引数据。

以下图为例：假设index_granularity=8192且granularity=3,则数据会按照index_granularity划分成n等份，MergeTree从第0段分区开始，依次获取聚合信息，当获取到第3个分区时（granularity=3），则汇总并生成第一行minmax索引（前3段minmax极值汇总后取值为[1,9]）。

![img](https://pic2.zhimg.com/80/v2-15fd7a1c6538af96b15d781e4a2e0dc9_1440w.webp)



**minmax跳数索引案例：**

```text
#删除表 t_mt
node1 :) drop table t_mt;

#重新创建t_mt表，包含二级索引
node1 :)CREATE TABLE t_mt
(
   id UInt8,
   name String,
   age UInt8,
   birthday Date,
   location String,
   INDEX a id TYPE minmax GRANULARITY 5
)
ENGINE = MergeTree
PARTITION BY toYYYYMM(birthday)
ORDER BY (id, age)
PRIMARY KEY id

#插入数据
insert into t_mt values (1,'张三',18,'2021-06-01','上海'), (2,'李四',19,'2021-02-10','北京'), (3,'王五',12,'2021-06-01','天津'), (1,'马六',10,'2021-06-18','上海'), (5,'田七',22,'2021-02-09','广州');

#查看数据分区路径
```



![img](readme.assets/v2-ac445fac265ebc414f4aadea068e0bb3_1440w.webp)



# 十三、Select语句

## 13.1 all 子句

`SELECT ALL` 和 `SELECT` 不带 `DISTINCT` 是一样的。

- 如果指定了 `ALL` ，则忽略它。
- 如果同时指定了 `ALL` 和 `DISTINCT` ，则会抛出异常。

`ALL` 也可以在聚合函数中指定，具有相同的效果（空操作）。例如：

```sql
SELECT sum(ALL number) FROM numbers(10);
```

等于

```sql
SELECT sum(number) FROM numbers(10);
```



## 13.2 array join

**ClickHouse的Array Join子句**

Array join 子句允许在数据表的内部，与数组类型的字段进行join操作，从而将一行数组展开为多行。

首先我们创建一张 MergeTree引擎表并加入数据，操作如下：

```text
#创建表 mr_tbl
node1 :) CREATE TABLE mr_tbl
(
    `id` UInt8,
    `name` String,
    `age` Int,
    `local` Array(String)
)
ENGINE = MergeTree()
ORDER BY id
#向表mr_tbl中插入数据
node1 :) insert into table mr_tbl values (1,'zs',18,['beijing','shanghai']),(2,'ls',19,['guangzhou','hangzhou']),(3,'ww',20,[]);

┌─id─┬─name─┬─age─┬─local────────────────────┐
│  1 │ zs   │  18 │ ['beijing','shanghai']   │
│  2 │ ls   │  19 │ ['guangzhou','hangzhou'] │
│  3 │ ww   │  20 │ []                       │
└────┴──────┴─────┴──────────────────────────┘
```

我们可以使用array join针对以上表数组字段一条膨胀成多条数据，类似Hive中的explode函数，在ClickHouse中没有explode函数，可以使用array join 达到同样效果。

在使用Array join时，一条select语句中只能存在一个Array join（使用嵌套子查询除外），目前支持INNER和LEFT两种JOIN策略：

### 13.2.1 INNER ARRAY JOIN

Array join 默认使用的就是INNER JOIN 策略，使用如下：

```text
node1 :) SELECT id,name,age,local FROM mr_tbl ARRAY JOIN local;
┌─id─┬─name─┬─age─┬─local─────┐
│  1 │ zs   │  18 │ beijing   │
│  1 │ zs   │  18 │ shanghai  │
│  2 │ ls   │  19 │ guangzhou │
│  2 │ ls   │  19 │ hangzhou  │
└────┴──────┴─────┴───────────┘
```

从以上查询结果来看，数据由原来的一行根据local列变成多行，并且排除掉了空数组对应的行。在使用Array Join时，如果我们在膨胀之后的数据结果中能够访问原有数组字段可以使用如下方式查询：

```text
node1 :) SELECT id,name,age,local ,v FROM mr_tbl ARRAY JOIN local AS v;
┌─id─┬─name─┬─age─┬─local────────────────────┬─v─────────┐
│  1 │ zs   │  18 │ ['beijing','shanghai']   │ beijing   │
│  1 │ zs   │  18 │ ['beijing','shanghai']   │ shanghai  │
│  2 │ ls   │  19 │ ['guangzhou','hangzhou'] │ guangzhou │
│  2 │ ls   │  19 │ ['guangzhou','hangzhou'] │ hangzhou  │
└────┴──────┴─────┴──────────────────────────┴───────────┘
```

### 13.2.2 LEFT ARRAY JOIN

Array Join 子句支持LEFT连接策略，Left array join不会排除空数组，执行如下语句并查看结果。

```text
node1 :) SELECT id,name,age,local FROM mr_tbl LEFT ARRAY JOIN local;
┌─id─┬─name─┬─age─┬─local─────┐
│  1 │ zs   │  18 │ beijing   │
│  1 │ zs   │  18 │ shanghai  │
│  2 │ ls   │  19 │ guangzhou │
│  2 │ ls   │  19 │ hangzhou  │
│  3 │ ww   │  20 │           │
└────┴──────┴─────┴───────────┘
```

当同时对多个数组字段进行Array join 操作时，**array join 对应的多个字段的数组长度必须相等**，查询的计算逻辑是按行合并并不是产生笛卡尔积，举例如下：

```text
#创建表 mr_tbl2
node1 :) CREATE TABLE mr_tbl2
(
    `id` UInt8,
    `name` String,
    `age` Int,
`local` Array(String),
`score` Array(UInt32)
)
ENGINE = MergeTree()
ORDER BY id

#向表mr_tbl2中插入以下数据
node1 :) insert into table mr_tbl2 values (1,'zs',18,['beijing','shanghai'],[100,200]),(2,'ls',19,['guangzhou','hangzhou'],[300,400]),(3,'ww',20,[],[]);

┌─id─┬─name─┬─age─┬─local────────────────────┬─score─────┐
│  1 │ zs   │  18 │ ['beijing','shanghai']   │ [100,200] │
│  2 │ ls   │  19 │ ['guangzhou','hangzhou'] │ [300,400] │
│  3 │ ww   │  20 │ []                       │ []        │
└────┴──────┴─────┴──────────────────────────┴───────────┘

#执行array join 语句，将数组中的数据一变多行
node1 :) select id,name,age,local,local2,score,score2 from mr_tbl2 left array join local as local2 ,score as score2;
```

![img](readme.assets/v2-12188ef3ddc830564bf43652a032e8ac_1440w.webp)



## 13.3 with子句

ClickHouse支持with子句以增强语句的表达，例如如下查询：

```text
node1 :) SELECT pow(pow(2,2),3)
┌─pow(pow(2, 2), 3)─┐
│                64 │
└───────────────────┘
```



我们可以通过使用with子句进行简化，提高可读性：

```text
node1 :) WITH pow(2,2) AS a SELECT power(a,3)
┌─pow(a, 3)─┐
│        64 │
└───────────┘
```



with的使用支持如下四种用法：

### 13.3.1 定义变量

可以通过with定义变量，这些变量在后续的查询子句中可以直接访问。

例如：

```text
node1 :) WITH 10 AS start
SELECT number
FROM system.numbers
WHERE number > start
LIMIT 5
┌─number─┐
│     11 │
│     12 │
│     13 │
│     14 │
│     15 │
└────────┘
```

### 13.3.2 调用函数

可以访问select子句中的列字段，并调用函数做进一步处理，处理之后的数据可以在select子句中继续使用。

例如：

```text
node1 :) WITH SUM(data_uncompressed_bytes) AS bytes
SELECT
    database,
    formatReadableSize(bytes) AS format
FROM system.columns
GROUP BY database
ORDER BY bytes DESC

┌─database─┬─format─────┐
│ datasets │ 7.40 GiB   │
│ system   │ 197.27 MiB │
│ default  │ 0.00 B     │
└──────────┴────────────┘
```

### 13.3 定义子查询

可以使用with定义子查询，例如，借助子查询可以得出各database未压缩数据大小与数据总和大小的比例排名：

```text
node1 :) WITH (
        SELECT SUM(data_uncompressed_bytes)
        FROM system.columns
    ) AS total_bytes
SELECT
    database,
    (SUM(data_uncompressed_bytes) / total_bytes) * 100 AS database_disk_usage
FROM system.columns
GROUP BY database
ORDER BY database_disk_usage DESC

┌─database─┬─database_disk_usage─┐
│ datasets │   97.31767735000648 │
│ system   │   2.682322649993527 │
│ default  │                   0 │
└──────────┴─────────────────────┘
```

注意：在with中使用子查询时智能返回一行数据，如果结果集大于一行则报错。

### 13.3.4 在子查询中重复使用with

在子查询中可以嵌套使用With子句，例如，在计算出各database未压缩数据大小与数据总和的比例之后，又进行取整函数操作：

```text
node1 :) WITH round(database_disk_usage) AS database_disk_usage_v1
SELECT
    database,
    database_disk_usage,
    database_disk_usage_v1
FROM
(
    WITH (
            SELECT SUM(data_uncompressed_bytes)
            FROM system.columns
        ) AS total_bytes
    SELECT
        database,
        (SUM(data_uncompressed_bytes) / total_bytes) * 100 AS database_disk_usage
    FROM system.columns
    GROUP BY database
    ORDER BY database_disk_usage DESC
)

┌─database─┬─database_disk_usage─┬─database_disk_usage_v1─┐
│ datasets │    97.2911778785499 │                     97 │
│ system   │  2.7088221214500954 │                      3 │
│ default  │                   0 │                      0 │
└──────────┴─────────────────────┴────────────────────────┘
```





## 13.4 From和Sample子句

[-](https://zhuanlan.zhihu.com/p/573410204)

### 13.4.1 From子句

From子句表示从何处读取数据，支持2种形式，由于From比较简单，这里不再举例，2种使用方式如下：

```text
SELECT clo1 FROM tbl;
SELECT rst FROM (SELECT sum(col1) as rst FROM tbl) 
```



from 关键字可以省略，此时会从虚拟表中取数，ClickHouse中没有dual虚拟表，它的虚拟表是system.one，例如，以下两种查询等价：

```text
SELECT 1;
SELECT 1 FROM system.one;
```



另外，FROM 子句后还可以跟上final修饰符，可以配合COllapsingMergeTree和VersionedCollapsingMergeTree等表引擎进行查询操作，强制在查询过程中合并，由于Final修饰符会降低查询性能，所以尽量避免使用Final修饰符。

### 13.4.1 Sample子句

Sample子句可以实现数据采样功能，使查询仅返回采样数据而非全部数据，从而减少查询负载。Sample采样机制是幂等机制，也就是说在数据不发生变化，使用相同的采样规则总是能够返回相同的数据。

sample子句只能用于MergeTree系列表引擎，并且要求在Create Table时声明sample by 抽样表达式。

例如，创建表 tbl 声明sample by抽样表达式：

```text
CREATE TABLE tbl(
id UInt32,
name String,
age UInt32,
birthday DATE
)ENGINE = MERGETREE()
PARTITION BY toYYYYMM(birthday)
ORDER BY (id,intHash32(age))
SAMPLE BY intHash32(age)
```



**以上创建sample by 采样表时注意：**

- Sample by 所声明的表达式必须同时包含在主键的声明内。
- Sample Key 必须是Int类型，虽然在建表不报错，但是数据查询时报错。

另外，建表时没有声明Sample by,在使用sample 采样时会报错。

Sample目前支持三种语法,前面导入的datasets.hits_v1创建时指定了SAMPLE BY ,建表语句如下：

```text
CREATE TABLE datasets.hits_v1(
    `WatchID` UInt64,
    `JavaEnable` UInt8,
    `Title` String,
  ... ...
 )
ENGINE = MergeTree()
PARTITION BY toYYYYMM(EventDate)
ORDER BY (CounterID, EventDate, intHash32(UserID))
SAMPLE BY intHash32(UserID)
```



下面就以表hits_v1为例，来讲解sample三种用法。

- **Sample factor**

Sample factor表示按因子系数采样，factor表示采样因子，取值0-1之间的小数，表示采样总体数据的比例。如果factor 设置为0或者1，则表示不采样。使用如下：

```text
#按10%的因子采样数据
SELECT CounterID FROM datasets.hits_v1 SAMPLE 0.1;
839889 rows in set. Elapsed: 0.114 sec. Processed 7.36 million rows, 88.30 MB (64.46 million rows/s., 773.46 MB/s.)
```



- **Sample rows**

Sample rows表示按照样本数量采样，其中rows表示大概采样多少行数据，是个近似值，取值必须大于1，如果rows行数大于表总数，效果等同于rows=1，即不采样。使用如下：

```text
node1 :) SELECT count() FROM datasets.hits_v1 SAMPLE 10000;
┌─count()─┐
│    9251 │
└─────────┘
```



- **SAMPLE factor OFFSET n**

SAMPLE factor OFFSET n 表示按因子系数和偏移量采样，其中factor表示采样因子，即采样总数据的百分比，n表示偏移多少数据后才开始采样，它们两个取值都是0~1之间的小数。使用如下：

```text
#偏移量0.5并按0.4的系数采样
node1 :) SELECT CounterID FROM datasets.hits_v1 SAMPLE 0.4 OFFSET 0.5;
3589194 rows in set.
```



![img](readme.assets/v2-824517be762c8bc87ae336a65072e4e9_1440w.webp)



偏移量0.5并按0.4的系数采样的采样为：从数据的二分之一处开始，按总数量的0.4采样数据。如果Sample比例采样出现了溢出，则数据会被自动截断，例如：

```text
node1 :) SELECT CounterID FROM datasets.hits_v1 SAMPLE 0.4 OFFSET 0.9;
892694 rows in set.
```



![img](readme.assets/v2-289c66bb9d5c5b6a0247e90af68cd128_1440w.webp)



## 13.5 Join子句



Join子句可以对左右两张表的数据进行连接，join语法包含连接精度和连接类型两部分。参照下图帮助大家理解：

![img](readme.assets/v2-25f242c092e167f94fb0cdb4eec80d1b_1440w.webp)

上图可知，连接精度分为ALL、ANY、ASOF三种，而连接类型分为外连接、内连接、交叉连接三种。我们对连接类型并不陌生，下面重点介绍连接精度。

为了方便测试，我们创建以下2张表：join_tbl1、join_tbl2，分别创建两张表，并插入数据：

```text
#创建join_tbl1表
CREATE TABLE join_tbl1
(
    `id` UInt8,
    `name` String,
    `time` DateTime
)
ENGINE = MergeTree()
ORDER BY id

#向表join_tbl1中插入数据
node1 :) insert into join_tbl1 values (1,'zs','2021-11-01 12:00:00'),(2,'ls','2021-11-01 12:30:00'),(3,'ww','2021-11-01 13:00:00'),(4,'zs','2021-11-01 13:30:00'),(5,'zs','2021-11-01 14:00:00');
```



查看数据

```
select * from join_tbl1

SELECT *
FROM join_tbl1

Query id: 4ce7b0f5-6174-4bdc-8153-6638ace51450

┌─id─┬─name─┬────────────────time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │
└────┴──────┴─────────────────────┘
```







```text
#创建join_tbl2表
CREATE TABLE join_tbl2
(
    `id` UInt8,
    `score` UInt8,
    `time` DateTime
)
ENGINE = MergeTree()
ORDER BY id;

#向表join_tbl2中插入数据
node1 :) insert into join_tbl2 values (1,100,'2021-11-01 11:55:00'),(1,200,'2021-11-01 12:01:00'),(2,300,'2021-11-01 13:10:00'),(2,400,'2021-11-01 14:30:00'),(6,500,'2021-11-01 15:00:00');
```

```
select * from join_tbl2

SELECT *
FROM join_tbl2

Query id: e650a4a0-8afc-4efd-8ed7-ccc352fdfbcb

┌─id─┬─score─┬────────────────time─┐
│  1 │   100 │ 2021-11-01 11:55:00 │
│  1 │   200 │ 2021-11-01 12:01:00 │
│  2 │    44 │ 2021-11-01 13:10:00 │
│  2 │   144 │ 2021-11-01 14:30:00 │
│  6 │   244 │ 2021-11-01 15:00:00 │
└────┴───────┴─────────────────────┘

5 rows in set. Elapsed: 0.015 sec. 
```

### 13.5.1 连接精度

连接精度决定了join查询在连接数据时所使用的策略，目前支持ALL、ANY和ASOF三种类型。

- **ALL**

如果左表内的一行数据在右表中有多行数据与之连接匹配，则返回右表中全部连接的数据，操作如下：

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
ALL INNER JOIN join_tbl2 AS b ON a.id = b.id

#结果返回了右表中所有与左表id相匹配的数据。


SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
ALL INNER JOIN join_tbl2 AS b ON a.id = b.id

Query id: 7e0d35b5-f90f-443f-856e-2c99ff7a0b10

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘

```

- **ANY**

如果左表内的一行数据在右表中有多行数据与之连接匹配，则仅返回右表中的第一行连接的数据。操作如下：

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
ANY INNER JOIN join_tbl2 AS b ON a.id = b.id

#查询结果如下，返回了右表中与左表id相连接的第一行数据
┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘

```



- **ASOF**

ASOF是一种模糊查询，它允许在连接键之后追加定义一个模糊连接的匹配条件asof_column(此条件要求不等式，大于或者小于)。举例如下：

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
ASOF INNER JOIN join_tbl2 AS b ON (a.id = b.id) AND (a.time >= b.time)

Query id: 3fcb46ad-9392-4d56-a6f0-c3f07e1eb7dc

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘

```

### 13.5.2 连接类型

连接类型与Mysql中的连接类型意思相同，这里就不再详解介绍，直接操作，操作如下：

- **INNER**

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
INNER JOIN join_tbl2 AS b ON a.id = b.id

Query id: a608d19f-fa54-4c07-8964-676e44f789d7

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘
```



- **LEFT OUTER JOIN**

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
LEFT JOIN join_tbl2 AS b ON a.id = b.id

Query id: 370d5cf7-9903-4d44-b525-390c61aef4cf

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    0 │     0 │ 1970-01-01 00:00:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    0 │     0 │ 1970-01-01 00:00:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    0 │     0 │ 1970-01-01 00:00:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘

```

- **RIGHT OUTER JOIN**

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
RIGHT JOIN join_tbl2 AS b ON a.id = b.id

Query id: f519a311-19c5-47be-97e7-bbfaf542d2f5

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘
┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  0 │      │ 1970-01-01 00:00:00 │    6 │   244 │ 2021-11-01 15:00:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘
```

- **FULL JOIN**

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
FULL OUTER JOIN join_tbl2 AS b ON a.id = b.id

Query id: 24c2a363-a627-4f2c-bc17-a76a9e8b19fa

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    0 │     0 │ 1970-01-01 00:00:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    0 │     0 │ 1970-01-01 00:00:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    0 │     0 │ 1970-01-01 00:00:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘
┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  0 │      │ 1970-01-01 00:00:00 │    6 │   244 │ 2021-11-01 15:00:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘

8 rows in set. Elapsed: 0.023 sec. 

```

- **CROSS**

CROSS JOIN 表示交叉连接，返回左表与右表两个数据集合的笛卡尔积，不需要指定JOIN key 连接键。

```text
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
CROSS JOIN join_tbl2 AS b

Query id: 14ad7eaa-8766-4204-a7e6-538140d401f2

┌─id─┬─name─┬────────────────time─┬─b.id─┬─score─┬──────────────b.time─┐
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  1 │ zs   │ 2021-11-01 12:00:00 │    6 │   244 │ 2021-11-01 15:00:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  2 │ ls   │ 2021-11-01 12:30:00 │    6 │   244 │ 2021-11-01 15:00:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  3 │ ww   │ 2021-11-01 13:00:00 │    6 │   244 │ 2021-11-01 15:00:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  4 │ zs   │ 2021-11-01 13:30:00 │    6 │   244 │ 2021-11-01 15:00:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    1 │   100 │ 2021-11-01 11:55:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    1 │   200 │ 2021-11-01 12:01:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    2 │    44 │ 2021-11-01 13:10:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    2 │   144 │ 2021-11-01 14:30:00 │
│  5 │ zs   │ 2021-11-01 14:00:00 │    6 │   244 │ 2021-11-01 15:00:00 │
└────┴──────┴─────────────────────┴──────┴───────┴─────────────────────┘
```



### 13.5.3 注意事项

在ClickHouse使用中，一般作为DWS或者DM层数据使用，很少会有JOIN操作。在使用JOIN查询时，为了优化JOIN查询性能，应该遵循左大右小的原则，即将数据量小的表放在右侧。这是因为在执行JOIN查询时，无论使用哪种JOIN连接方式，右表都会被全部加载到内存中与左表进行比较。

另外，join也没有缓存机制，每一次join查询就算是执行相同的sql查询，也会生成一次全新的查询计划。



## 13.6 explain

ClickHouse在版本20.6.3之后支持explain查看执行计划。explain基本语法如下：

```text
EXPLAIN [AST | SYNTAX | PLAN | PIPELINE] [setting = value, ...] SELECT ... [FORMAT ...]
```



- AST:用于查看语法树。
- SYNTAX:用于查询ClickHouse优化后的语法。
- PLAN:用于查看执行计划，默认值。
- PIPELINE:用于查看PIPELINE计划，相对于PLAN更加详细。

### 13.6.1 AST查看语法树**

```text
EXPLAIN AST
SELECT
    id,
    name,
    age,
    local
FROM mr_tbl

Query id: 72d078e7-175e-4e42-88a2-a6a8e084142d

┌─explain─────────────────────────────────────┐
│ SelectWithUnionQuery (children 1)           │
│  ExpressionList (children 1)                │
│   SelectQuery (children 2)                  │
│    ExpressionList (children 4)              │
│     Identifier id                           │
│     Identifier name                         │
│     Identifier age                          │
│     Identifier local                        │
│    TablesInSelectQuery (children 1)         │
│     TablesInSelectQueryElement (children 1) │
│      TableExpression (children 1)           │
│       TableIdentifier mr_tbl                │
└─────────────────────────────────────────────┘

12 rows in set. Elapsed: 0.008 sec. 
```



### 13.6.2 SYNTAX查看优化后语法，比较常用**

```text
EXPLAIN SYNTAX
SELECT
    a.id,
    a.name,
    a.time,
    b.id,
    b.score,
    b.time
FROM join_tbl1 AS a
CROSS JOIN join_tbl2 AS b

Query id: fbfe7db3-c758-4ac1-a2fb-483222d5541c

┌─explain───────────────────┐
│ SELECT                    │
│     id,                   │
│     name,                 │
│     time,                 │
│     b.id,                 │
│     score,                │
│     b.time                │
│ FROM join_tbl1 AS a       │
│ CROSS JOIN join_tbl2 AS b │
└───────────────────────────┘
```



![img](https://pic3.zhimg.com/80/v2-4c12727cad6e3c24ae2ce6050ba6001e_1440w.webp)



### 13.6.3 PLAN:用于查看执行计划，默认值**

**在使用PLAN时有一些设置：**

- header:打印计划中各个步骤的输出头，默认关闭，默认值0。
- description：打印计划中各个步骤的描述，默认开启，默认值1。
- indexes:打印计划中使用的索引，默认关闭，默认值0，支持MergeTree表引擎。
- actions：打印计划中各个步骤的详细信息，默认关闭，默认值0.
- json:打印计划步骤时使用json格式展示，默认关闭，默认值0，建议使用默认TSVRaw格式，避免不必要的开销。

```text
node1 :) EXPLAIN  PLAN  SELECT id ,name ,age FROM mr_tbl;
```



![img](readme.assets/v2-22ff7c2217bb12d85320b86d051cbacf_1440w.png)



```text
#设置一些属性后查询结果
node1 :) EXPLAIN PLAN header=1,description=1,indexes=1,actions=1,json=1 SELECT id ,name ,age FROM mr_tbl;
```



![img](readme.assets/v2-6dc4e4fbc0184db3645644bde823c99b_1440w.webp)



以上关于json中详细的字段解释可以参照官网解释：**[https://ClickHouse.com/docs/en/sql-reference/statements/explain/](https://link.zhihu.com/?target=https%3A//ClickHouse.com/docs/en/sql-reference/statements/explain/)[1]**

### 13.6.4 PIPELINE:用于查看PIPELINE计划，相当于是PLAN更详细的描述

**在执行pipeline是也可以设置一些参数：**

- header：打印计划中各个步骤的输出头，默认关闭，默认值0。
- graph：使用DOT图形语言描述管道图，默认关闭，默认值0。
- compact：如果graph开启，以紧凑模式打印管道图，默认开启，默认值1。

```text
node1 :) EXPLAIN PIPELINE header=1 SELECT sum(number) FROM numbers_mt(100000) GROUP BY number % 4;
```



![img](https://pic1.zhimg.com/80/v2-ed5264f9472c191659c4e23f1acc00cc_1440w.webp)



## 13.7 group by子句

Group By子句又称聚合查询，与MySQL或者Hive中的使用方式一样，但是需要注意一点在Select查询中如果有聚合查询，例如max,min等，与聚合查询出现的字段一定要出现在Group by中，否则语句报错。

ClickHouse中的Group by 还可以配合WITH ROLLUP、WITH CUBE、WITH TOTALS三种修饰符获取额外的汇总信息。

创建表mt_tbl2 并加载数据：

```text
#创建表mt_tbl2
CREATE TABLE mt_tbl2
(
    `province` String,
    `city` String,
    `item` String,
    `totalcount` UInt32
)
ENGINE = MergeTree()
ORDER BY (province, city)

#向表中插入如下数据：
node1 :)   insert into mt_tbl2 values ('北京','海淀','苹果手机',100),('上海','黄浦','小米手机',200),('北京','丰台','苹果手机',300),('北京','大兴','华为手机',400),('上海','嘉定','华为手机',500),('
北京','海淀','华为手机',600),('上海','黄浦','小米手机',700),('北京','大兴','苹果手机',800),('上海','嘉定','华为手机',900),('北京','海淀','小米手机',1000);
```

```
select * from mt_tbl2

SELECT *
FROM mt_tbl2

Query id: 9f9183f4-5dc8-4e8a-90c5-b4cdcc267624

┌─province─┬─city─┬─item─────┬─totalcount─┐
│ 
北京     │ 海淀 │ 华为手机 │        600 │
│ 上海     │ 嘉定 │ 华为手机 │        500 │
│ 上海     │ 嘉定 │ 华为手机 │        900 │
│ 上海     │ 黄浦 │ 小米手机 │        200 │
│ 上海     │ 黄浦 │ 小米手机 │        700 │
│ 北京     │ 丰台 │ 苹果手机 │        300 │
│ 北京     │ 大兴 │ 华为手机 │        400 │
│ 北京     │ 大兴 │ 苹果手机 │        800 │
│ 北京     │ 海淀 │ 苹果手机 │        100 │
│ 北京     │ 海淀 │ 小米手机 │       1000 │
└──────────┴──────┴──────────┴────────────┘
```



### 13.7.1 WITH ROLLUP

ROLLUP 能够按照聚合键从右向左上卷数据，基于聚合函数依次生成分组小计和总计。操作如下：

```text
SELECT
    province,
    city,
    item,
    sum(totalcount) AS total
FROM mt_tbl2
GROUP BY
    province,
    city,
    item
    WITH ROLLUP

Query id: c2b441e1-7640-496d-91b4-cab0073f6bf5

┌─province─┬─city─┬─item─────┬─total─┐
│ 北京     │ 海淀 │ 苹果手机 │   100 │
│ 上海     │ 嘉定 │ 华为手机 │  1400 │
│ 北京     │ 大兴 │ 华为手机 │   400 │
│ 
北京     │ 海淀 │ 华为手机 │   600 │
│ 上海     │ 黄浦 │ 小米手机 │   900 │
│ 北京     │ 大兴 │ 苹果手机 │   800 │
│ 北京     │ 海淀 │ 小米手机 │  1000 │
│ 北京     │ 丰台 │ 苹果手机 │   300 │
└──────────┴──────┴──────────┴───────┘
┌─province─┬─city─┬─item─┬─total─┐
│ 北京     │ 大兴 │      │  1200 │
│ 上海     │ 嘉定 │      │  1400 │
│ 
北京     │ 海淀 │      │   600 │
│ 北京     │ 丰台 │      │   300 │
│ 上海     │ 黄浦 │      │   900 │
│ 北京     │ 海淀 │      │  1100 │
└──────────┴──────┴──────┴───────┘
┌─province─┬─city─┬─item─┬─total─┐
│ 北京     │      │      │  2600 │
│ 上海     │      │      │  2300 │
│ 
北京     │      │      │   600 │
└──────────┴──────┴──────┴───────┘
┌─province─┬─city─┬─item─┬─total─┐
│          │      │      │  5500 │
└──────────┴──────┴──────┴───────┘

18 rows in set. Elapsed: 0.036 sec. 
```



![img](https://pic4.zhimg.com/80/v2-4b249c675fad4d43d6bd5330bea87b6b_1440w.webp)

### 13.7.2 WITH CUBE

CUBE 是立方体意思，WITH CUBE会基于聚合键之间所有的组合生成小计信息。操作如下：

```text
node1 :) select province,city,item,sum(totalcount) as total from mt_tbl2 group by province,city,item with cube;
```



![img](readme.assets/v2-df863aa21dc59da7476c5fb1871f06ec_1440w.webp)



![img](https://pic3.zhimg.com/80/v2-10bca80d64000ddcf224dceb62ec60be_1440w.webp)

### 13.7.3 WITH TOTALS

WITH TOTALS 会基于聚合键生成结果外，还会附带一行Totals汇总统计。操作如下：

```text
node1 :) select province,city,item,sum(totalcount) as total from mt_tbl2 group by province,city,item with totals;
```



![img](readme.assets/v2-94503ea5737ba3d7c7c20dd02f7a8e9a_1440w.webp)



## 13.8 having & order by

### 13.8.1 HAVING子句

ClickHouse也支持Having子句，需要与group by 同时出现，不能单独使用，它能够在聚合计算之后实现二次过滤数据。操作如下：

```text
node1 :) select province,city,item,sum(totalcount) as total from mt_tbl2 group by province,city,item having total >1000;
┌─province─┬─city─┬─item─────┬─total─┐
│ 上海     │ 嘉定 │ 华为手机 │  1400 │
└──────────┴──────┴──────────┴───────┘
```

### 13.8.2 ORDER BY 子句

Order by 子句通过声明排序键来指定查询数据返回时的顺序。在MergeTree表引擎中也有Order by 参数用于指定排序键。在MergeTree表引擎中指定order by 后，数据在各个分区内按照其定义的规则排序，这是一种分区内的局部排序，如果在查询时数据跨越了多个分区，则他们返回的顺序是无法预知的，每一次查询返回的顺序都有可能不同。这种情况下，如果希望数据总是能够按照期望的顺序返回，就需要借助Order by 子句来指定全局排序。



## 13.9 limit by & limit

### 13.9.1 LIMIT BY子句

LIMIT BY 子句运行在Order by 之后和LIMIT 之前，能够按照指定分组，最多返回前n行数据，如果数据总行少于n行，则按实际数量返回，常用于TOPN的查询场景，功能类似Hive中的开窗函数。

LIMIT BY 的常规语法如下：

```text
LIMIT n BY express
```

n指的是获取几条数据；express通常是一到多个字段，即按照express分组获取每个分组的前n条数据。

用法示例如下：

```text
#目前有表mt_tbl2，数据如下：
node1 :) select * from mt_tbl2;
```

![img](https://pic3.zhimg.com/80/v2-6adb81c5031db1a7ea2da32b57e2688a_1440w.webp)

```text
#查询表mt_tbl2中每个省份对应的totalcount top2最大值
node1 :) select province,totalcount from mt_tbl2 order by totalcount desc limit 2 by province;
```

![img](https://pic4.zhimg.com/80/v2-73b406dbbbd71eadd4d49858be30076b_1440w.webp)

除了常规语法外，LIMIT BY 也支持跳过offset偏移量获取数据，具体语法如下：

```text
#按照express分组，获取跳过y行后的top n行数据。
LIMIT n offset y BY express
#简化为
LIMIT y,n BY express
```

举例：获取表mt_tbl2中每个省份第二、第三大销售额

```text
node1 :) select province,totalcount from mt_tbl2 order by totalcount desc limit 2 offset 1 by province;
```

![img](https://pic4.zhimg.com/80/v2-053fa2f9803c7f99818492fe180a41f3_1440w.webp)

### 13.9.2 LIMIT 子句

LIMIT 子句用于返回指定的前n行数据，常用于分页场景，它的三种语法形式如下：

```text
#返回前n行数据
LIMIT n
#指定从第m行开始返回前n行数据
LIMIT n OFFSET m
#指定从第m行开始返回前n行数据简化写法
LIMIT m,n
```

# 十四、数据字典

## 14.1 简单介绍

数据字典是ClickHouse提供一种非常简单、实用的存储媒介，他以键值和属性映射的形式定义数据。字典中的数据会主动或者被动加载到内存并支持动态更新。由于字典数据常驻内存的特性，所以非常适合保存常量或经常使用的维度表数据，以避免不必要的JOIN查询。

数据字典分为内置与扩展两种形式，内置字典是ClickHouse默认自带的字典，外部字典是用户通过自定义配置实现的字典。ClickHouse目前只有一种内置字典：Yandex.Metrica字典，这个字典是ClickHouse自家产品上的字典，设计目的是快速存取地理数据，但是ClickHouse没有将地理数据公布出来，内置字典默认是禁用状态，想要使用内置字典还需要模拟数据然后开启，功能十分有限。在使用ClickHouse 字典时，外部扩展字典才是更加常用的功能。

在正常情况下，字典中的数据只能通过字典函数访问，ClickHouse设置了一类字典函数，专门用于字典数据的取用。我们也可以通过字典表将字典数据挂载到一张代理的数据表下，从而访问字典中的数据，也可以实现数据表与字典数据的join查询。

![img](readme.assets/v2-84d8d4c17e4126062efe45d639cbb1df_1440w.webp)

## 14.2 字典创建

在ClickHouse20版本之前创建外部数据字典需要创建大量的xml文件来实现，在ClickHouse20.1版本之后引入了“Create dictionary”语句创建数据字典，创建字典表举例操作如下：

```text
#创建新的库并使用
create database dic_test_db;
use dic_test_db;

#创建普通MergeTree表，描述地理位置表
CREATE TABLE loc_info(
uuid String,
local_id UInt64,
local_name String
)engine=MergeTree()
order by uuid;

#创建 数据字典表
CREATE DICTIONARY dic_loc_info(
local_id UInt64,
local_name String
)PRIMARY KEY local_id
SOURCE(ClickHouse(
 HOST 'node1'
 PORT 9000
 USER 'default'
 TABLE 'loc_info'
 PASSWORD ''
 DB 'dic_test_db'
 ))
LIFETIME (MIN 1 MAX 10)
LAYOUT(HASHED());
注意：以上创建字典表的参数解释如下：
HOST:指定ClickHouse节点名称
PORT:指定ClickHouse端口，默认9000
USER:连接ClickHouse用户名
PASSWORD:连接ClickHouse用户名对应密码
TABLE:此字典表映射的表名，字典表中的列名与映射表中列名一致。
DB:字典表映射表所在的库
LIFETIME:字典的自动更新频率
LAYOUT:字典的类型，决定了数据在内存中以何种结构组织和存储。目前扩展字典共拥有7种类型。

#创建普通用户表
CREATE TABLE person_info(
person_id String,
name String,
age UInt8,
loc_id UInt64
)engine=MergeTree()
order by person_id;

#向表loc_info中插入以下数据：
insert into loc_info values ('uuid1',100,'北京'),('uuid2',101,'上海'),('uuid3',102,'广州');

#向表 person_info中插入以下数据：
insert into person_info values ('1','张三',18,102),('2','李四',19,101),('1','王五',20,100);
```



## 14.3 元数据查询

通过system.dictionaries系统表可以查询扩展字典的元数据信息。查询语句如下：

```text
select name,type,key,attribute.names,attribute.types,source from system.dictionaries;
```

![img](readme.assets/v2-677da4555cd9378956b1e39cbeea629f_1440w.png)

_**注意**_*：以上查询字段的意义如下*

***name**__:字典的名称，使用字典函数时需要通过字典名称访问数据。*

***type**__:字典所属类型。*

***key**__:字典的key值，数据通过key值定位。*

***attribute.names**__:属性名称，以数组形式保存。*

***attribute.types**__:属性类型，以数组形式保存。*

***source**__:数据源信息。*

## 14.4 数据查询

字典数据使用时可以通过字典函数获取，如下：“dictGetString”就是字典函数，查询使用字典如下：

```text
#使用dic查询数据
select person_id,name,age,loc_id,dictGetString('dic_test_db.dic_loc_info','local_name',loc_id) as local_name from person_info;
```



![img](readme.assets/v2-46565df4935066e84b4097bcfaea5ae0_1440w.jpeg)

dictGet('字典名称','获取的值','查询的值(参数)')，如果字典类型是复合型key（参照字典类型complex_key_hashed/complex_key_cache）对应字典类型,在使用dicGet时需要使用元组作为参数，例如：dicGetString('字典名称','获取的值',tuple(col1,col2))。

除了dictGetString外，ClickHouse还提供了一系列以dictGet为前缀的字典函数，具体如下：

- **获取整形数据的函数：**

dictGetUInt8,dictGetUInt16,dictGetUInt32,dictGetUInt64,dictGetInt8,dictGetInt16,dictGetInt32,dictGetInt64。

- **获取浮点数据的函数：**

dictGetFloat32,dictGetFloat64

- **获取日期数据的函数：**

dictGetDate,dictGetFloat64

- **获取字符串数据的函数：**

dictGetString,dictGetUUID

我们在使用时，也可以直接使用dictGet(xx)传入对应的参数即可。

## 14.5 字典删除

删除字典语法如下：

```text
DROP DICTIONARY dic_name;

#将dic_loc_info删除
node1 :) drop dictionary dic_loc_info;
```



## 14.6 数据字典类型

[-](https://zhuanlan.zhihu.com/p/580231701)

在创建字典表语句中使用“layout”来指定字典的类型，目前扩展字典支持7种类型，分别为flat、hashed、range_hashed、cache、complex_key_hashed、complex_key_cache、ip_trie，不同的字典类型决定了数据在内存中以何种结构组织和存储。

扩展字典根据使用时传入的条件不同，可以划分为两类：

- 支持单个数值型条件（条件类型必须是UInt64）

flat、hashed、range_hashed、cache

- 支持复合条件

complex_key_hashed、complex_key_cache、ip_trie

## 14.7 数据字典源数据

### 14.7.1 文件数据源

ClickHouse中的字典还可以映射本地文件数据。操作如下：

1、创建本地csv文件

在本地创建的csv文件需要放在“/var/lib/ClickHouse/user_files”路径下，在此目录下创建organization.csv文件，写入如下内容：

```text
1,"a0001","研发部"
2,"a0002","产品部"
3,"a0003","数据部"
4,"a0004","测试部"
5,"a0005","运维部"
6,"a0006","规划部"
7,"a0007","市场部"
```



2、创建字典表

```text
#在ClickHouse中创建字典表
create dictionary org_dic(
id UInt64,
code String,
name String
)
primary key id
SOURCE(FILE(path '/var/lib/ClickHouse/user_files/organization.csv' format 'CSV'))
LAYOUT(FLAT())
LIFETIME(30);

注意：SOURCE(FILE(path '文件名'，' 文件格式'))，文件格式支持CSV和TabSeparated。

#查询使用字典表
node1 :) select dictGet('dic_test_db.org_dic','name',toUInt64(2)) as name;
┌─name───┐
│ 产品部 │
└────────┘
```

### 14.2 MySQL数据源

ClickHouse中还支持从MySQL中提取数据到字典表。具体操作如下：

1、在MySQL中创建表organization并插入数据

```text
#在mysql中创建库、表及插入数据
mysql> create database mytest;
mysql> use mytest;
mysql> create table organization(id int,code varchar(255),name varchar(255));

#插入数据
insert into mytest.organization values(1,"a0001","org1");
insert into mytest.organization values(2,"a0002","org2");
insert into mytest.organization values(3,"a0003","org3");
insert into mytest.organization values(4,"a0004","org4");
insert into mytest.organization values(5,"a0005","org5");
insert into mytest.organization values(6,"a0006","org6");
insert into mytest.organization values(7,"a0007","org7");

#查询数据
mysql> select * from organization;
```



![img](readme.assets/v2-4bef8a1c71b055a82b3bff6ce96956ec_1440w.webp)

**2、在ClickHouse中创建字典表**

```text
#在ClickHouse中创建flat字典表
create dictionary org_dic2(
id UInt64,
code String,
name String
)
primary key id
SOURCE(MYSQL(
    port 3306
    user 'root'
    password '123456'
    host 'node2'
    db 'mytest'
    table 'organization'
))
LAYOUT(FLAT())
LIFETIME(MIN 3 MAX 5);


#使用字典表
node1 :)  select dictGet('dic_test_db.org_dic2','name',toUInt64(2)) as name;
┌─name─┐
│ org2 │
└──────┘
```



**注意：**ClickHouse字典表除了支持将MySQL、ClickHouse表作为数据来源，还可以从MongoDB中读取数据。



# 十五、MaterializeMySQL物化引擎

在ClickHouse基础课程中我们知道可以使用两种方式通过ClickHouse可以操作MySQL数据库，分别使用使用 MySQL数据库引擎和MySQL表引擎。

## 15.1 MySQL数据库引擎

使用MySQL数据库引擎将远程MySQL服务器的表映射到ClickHouse中，允许对表进行**Insert插入**和**Select查询**，方便ClickHouse与MySQL之间进行数据交换。MySQL数据库引擎不会将MySQL的数据真正同步到ClickHouse存储中，ClickHouse就像一个壳子，可以将MySQL的表映射成ClickHouse表，使用ClickHouse查询MySQL中的数据，在MySQL中进行的CRUD操作，可以同时映射到ClickHouse中。

MySQL数据库引擎会将对其的查询转换为MySQL语法并发送到MySQL服务器中，因此可以执行诸如SHOW TABLES或SHOW CREATE TABLE之类的操作，但是**不允许创建表、修改表、删除数据、重命名**操作。

MySQL数据库引擎语法如下：

```text
CREATE DATABASE [IF NOT EXISTS] db_name [ON CLUSTER cluster]
ENGINE = MySQL('host:port', ['database' | database], 'user', 'password')
```



## 15.2 MySQL表引擎

ClickHouse MySQL数据库引擎可以将MySQL某个库下的表映射到ClickHouse中，使用ClickHouse对数据进行操作。ClickHouse同样支持MySQL表引擎，即映射一张MySQL中的表到ClickHouse中，使用ClickHouse进行数据操作，与MySQL数据库引擎一样，这里映射的表只能做查询和插入操作，不支持删除和更新操作。

MySQL表引擎语法如下：

```text
CREATE TABLE [IF NOT EXISTS] [db.]table_name [ON CLUSTER cluster]
(
    name1 [type1] [DEFAULT|MATERIALIZED|ALIAS expr1] [TTL expr1],
    name2 [type2] [DEFAULT|MATERIALIZED|ALIAS expr2] [TTL expr2],
    ...
) ENGINE = MySQL('host:port', 'database', 'table', 'user', 'password'[, replace_query, 'on_duplicate_clause']);
```



实际上以上两种方式都是将ClickHouse中的SQL转换为MySQL语法并发送到MySQL服务器中执行，数据也没有存储在ClickHouse中。

## 15.3 MaterializeMySQL物化引擎

ClickHouse在20.8.2版本之后增加了MaterializeMySQL物化引擎，该引擎可以将MySQL中某个库下的所有表数据**全量及增量实时同步**到ClickHouse中，**通过ClickHouse对MySQL中的数据进行高效的OLAP数据分析，降低线上MySQL的负载，将OLTP与OLAP业务完美结合。**

MaterializeMySQL物化引擎实时同步MySQL中数据原理是将ClickHouse作为MySQL副本，读取MySQL binlog日志实时物化MySQL数据，在ClickHouse中会针对MySQL映射库下的每一张表都会创建一张ReplacingMergeTree表引擎。

**MaterializeMySQL物化引擎特点如下：**

- 支持MySQL库级别的数据同步，不支持表级别。
- MySQL库映射到ClickHouse中自动创建ReplacingMergeTree引擎表。
- 支持全量和增量同步，首次创建数据库引擎时进行一次全量复制，之后通过监控binlog变化进行增量数据同步。
- 支持MySQL5.6、5.7、5.8版本。
- 兼容支持MySQL中Insert、update、delete、alter、create、drop、truncate等大部分常用的DDL操作，不支持修改表名、修改列操作。支持添加列、删除列。
- 支持MySQL复制为GTID操作。从MySQL 5.6.5 开始新增了一种基于 GTID 的复制方式。通过 GTID 保证了每个在主库上提交的事务在集群中有一个唯一的ID。这种方式强化了数据库的主备一致性，故障恢复以及容错能力。

### **1、开启MySQL binlog**

使用MaterializeMySQL物化引擎首先需要开启MySQL binlog，开启步骤如下：

**1.1、登录mysql查看MySQL是否开启binlog日志**

```text
[root@node2 ~]# mysql -u root -p123456
mysql> show variables like 'log_%';
```



![img](readme.assets/v2-19a9da120b9d8f2151073465f8fd3a14_1440w.webp)

**1.2、开启mysql binlog日志**

在/etc/my.cnf文件中[mysqld]下写入以下内容：

```text
[mysqld]
#随机指定一个不能和其他集群中机器重名的字符串
server-id=123
#配置binlog日志目录，配置后会自动开启binlog日志，并写入该目录
log-bin=/var/lib/mysql/mysql-bin
#设置binglog格式为Row
binlog_format=ROW 
#开启GTID
gtid-mode=on
#设置为主从强一致性
enforce-gtid-consistency=1
```



**1.3、重启mysql 服务，重新查看binlog日志情况**

```text
[root@node2 ~]# service mysqld restart
[root@node2 ~]# mysql -u root -p123456
mysql> show variables like 'log_%';
```



![img](readme.assets/v2-148f546935cc39d1db57c62127c61a0d_1440w.webp)



### **2、使用MaterializedMySQL物化引擎**

MaterializedMySQL物化引擎使用语法如下：

```text
CREATE DATABASE [IF NOT EXISTS] db_name [ON CLUSTER cluster]
ENGINE = MaterializedMySQL('host:port', ['database' | database], 'user', 'password') [SETTINGS ...]
```



以上参数解释如下：

- host:port:MySQL服务器的端口。
- database:MySQL数据库名称。
- user:登录MySQL用户名。
- password：登录MySQL密码。

使用MaterializeMySQL物化引擎步骤如下：

**2.1、在MySQL中创建表person_info并插入数据**

```text
#创建数据库 ck_db并使用
mysql> create database ck_db;
mysql> use ck_db;

#创建表person_info,必须指定主键，否则后期无法映射物化表
mysql> create table person_info(id int,name varchar(255),age int,primary key (id));

#插入以下数据
mysql> insert into person_info values (1,"zs",18),(2,"ls",19),(3,"ww",20);
```



![img](https://pic3.zhimg.com/80/v2-6682afcc041b9e72c7e5637f95c96486_1440w.webp)



**2.2、在ClickHouse中开启Mysql物化引擎**

由于MySQL物化引擎目前是实验阶段，在目前的ClickHouse版本中想要使用MySQL物化引擎，必须先设置参数开启。

```text
set allow_experimental_database_materialized_mysql=1;
```



**2.3、在ClickHouse中创建MySQL物化引擎**

```text
#创建MaterializeMySQL物化引擎
CREATE DATABASE mysql_ck_db ENGINE = MaterializedMySQL('node2:3306', 'ck_db', 'root', '123456')

#在ClickHouse中使用mysql_ck_db库，并查询表
node1 :) use mysql_ck_db;

node1 :) show tables;
┌─name────────┐
│ person_info │
└─────────────┘

node1 :) select * from person_info;
┌─id─┬─name─┬─age─┐
│  1 │ zs   │  18 │
└────┴──────┴─────┘
┌─id─┬─name─┬─age─┐
│  2 │ ls   │  19 │
│  3 │ ww   │  20 │
└────┴──────┴─────┘

node1 :) show create table person_info;
```



![img](readme.assets/v2-53b2aa4391e1f57bfa4ffc4aeb6a2566_1440w.webp)

注意：以上partition id按照长度为4294967进行分段分区。

**测试向mysql表person_Info中插入数据**

当向mysql表person_info中插入数据时，MaterializeMySQL物化引擎同步数据，操作如下：

```text
#向mysql 表person_info中插入如下数据
mysql> insert into person_info values (4,"a1",21),(5,"a2",22),(6,"a3",23);
```



![img](https://pic3.zhimg.com/80/v2-3d7509fa2089da3e97f9da5fc6b4eb5a_1440w.webp)

```text
#在ClickHouse中查询物化表person_info数据,数据实时变化
node1 :) select * from person_info;
┌─id─┬─name─┬─age─┐
│  1 │ zs   │  18 │
│  4 │ a1   │  21 │
│  5 │ a2   │  22 │
└────┴──────┴─────┘
┌─id─┬─name─┬─age─┐
│  2 │ ls   │  19 │
│  3 │ ww   │  20 │
│  6 │ a3   │  23 │
└────┴──────┴─────┘
```

- **测试更新mysql表数据**

当对mysql表person_info进行修改时，MaterializeMySQL物化引擎同步数据，操作如下：

```text
#更新mysql表中id=1对应的“zs”为“zhangsan”
mysql> update person_info set name = "zhangsan" where id =1;
```



![img](https://pic3.zhimg.com/80/v2-f67c303e68a434fce02522b9b263b2a2_1440w.webp)



```text
#在ClickHouse中查询物化表person_info数据，数据实时更新
node1 :) select * from person_info;
```



![img](https://pic1.zhimg.com/80/v2-811d62ed93803807ce9cebdd4cafcbbc_1440w.webp)



- **测试删除mysql表数据**

当对mysql表person_info进行删除时，MaterializeMySQL物化引擎同步数据，操作如下：

```text
#删除mysql表中id=6对应的数据
mysql> delete from person_info  where id =6;
```



![img](https://pic1.zhimg.com/80/v2-1a7ea10c7ff9f7ea7d1ac9be33dc6854_1440w.webp)



```text
#在ClickHouse中查询物化表person_info数据，数据实时更新
node1 :) select * from person_info;
```



![img](https://pic1.zhimg.com/80/v2-df9ae87a20365d4fdec2eecefff6bd58_1440w.webp)

- **测试删除、增加列**

当对mysql表person_info增加和删除列时，MaterializeMySQL物化引擎同步列信息，操作如下：

```text
#将mysql中person_info列age删除，增加score列
mysql> alter table person_info drop column age;
mysql> alter table person_info add column score int;
```



![img](https://pic2.zhimg.com/80/v2-7164feb90998af643ecafe5ea2999255_1440w.webp)



```text
#在ClickHouse中查询person_info物化表
node1 :) select * from person_info;
```



![img](https://pic4.zhimg.com/80/v2-3e450639c0d024b505752c610c9b99a3_1440w.webp)

- **测试删除表**

当对mysql表person_info进行删除时，MaterializeMySQL物化引擎同步删除，操作如下：

```text
#将mysql中person_info表删除
mysql> drop table person_info;

#在ClickHouse中默认也删除了person_info
```



通过以上测试我们发现，在mysql中对表的增、删、改、删除等操作在ClickHouse中是实时同步的，本质原因是MaterializedMySQL物化引擎会默认为每一张表生成ReplacingMergeTree引擎表，这个引擎表中实际上还有2个额外字段“_sign”和“_version”，当ClickHouse遇到删除数据的binlog操作时，在ClickHouse对应的ReplacingMergeTree引擎表中只是标记性删除，并非物理上的实际删除，随着删除日志的增多，查询过滤会有一定的负担。

对以上内容操作如下：

```text
#创建表person_info
mysql> create table person_info(id int,name varchar(255),age int,primary key (id));

#插入以下数据
mysql> insert into person_info values (1,"zs",18),(2,"ls",19),(3,"ww",20);

#在mysql中对id=1的数据进行删除，对id=2的数据进行修改
mysql> delete from person_info where id=1;
mysql> update person_info set name = "a1" where id=2;

#在ClickHouse中查询对应的person_info物化视图表
node1 :) select id,name,age,_sign,_version from person_info;
```



![img](https://pic3.zhimg.com/80/v2-1aa08fecbc11db8f171795949b808b62_1440w.webp)

**注意：对MaterializedMySQL物化引擎表进行查询时，底层实际上是对对应的ReplacingMergeTree表进行查询，不需要指定额外的final修饰符，实际上查询直接过滤出“_sign”不等于-1且“_version”最大的结果数据。**

# 十六、Projection投影

**ClickHouse的Projection投影**

在使用ClickHouse MergeTree引擎时，如果某张MergeTree表建表排序规则如下：

```text
order by A,B,C
```



那么通常过滤查询Where A很快，但是Where C会慢一些。

此外，我们在使用物化视图时，经常基于一张底表构建许多物化视图，以帮助更进一步提升查询性能、降低数据分析开销，例如：

```text
#创建普通MergeTree 表作为底表
create table personinfo(id UInt32,name String,age UInt32,score UInt32) engine = MergeTree() order by id;

#基于以上底表创建统计平均年龄物化视图表
create materialized view  t_view1 engine = Log as select sum(age)/count(name) as avg_age from personinfo;

#基于以上底表创建统计每人总分数物化视图表
create materialized view  t_view2 engine = Log as select name,sum(score) as total_score from personinfo group by name;

#向表personinfo中插入以下数据
insert into personinfo values (1,'zs',18,100),(2,'ls',19,200),(3,'ww',20,300),(4,'zs',18,400),(5,'ls',19,500),(6,'ww','20',600);

#查询t_view1与t_view2两张物化视图结果
node1 :) select * from t_view1;
┌─avg_age─┐
│      19 │
└─────────┘

node1 :) select * from t_view2;
┌─name─┬─total_score─┐
│ ls   │         700 │
│ ww   │         900 │
│ zs   │         500 │
└──────┴─────────────┘
```



以上构建的物化视图实际上在ClickHouse中相当于是独立的表，也会单独存储在数据目录“/var/lib/ClickHouse/data/${DB}”中：

![img](readme.assets/v2-bbfee15a9660dd9a3afc56e375888fee_1440w.png)

既然物化视图也是独立的表，那么就存在与原表数据一致性问题，如果物化视图很多，维护起来也是问题。

目前使用ClickHouse我们遇到以上不完美的地方，总结下来就是：

- **MergeTree只支持一种排序规则**
- **物化视图不够智能**

## 16.1 Projection投影介绍

ClickHouse Projection功能的出现完美解决了以上两点不完美。Projection(投影)指一组列的组合，可以按照与原表不同的排序存储，并且支持聚合函数查询，可以将Projection看成一种更加智能的物化视图，与物化视图一样本质也是用空间换时间，其具备以下特点：

- **part-level存储：**

相比普通物化视图是一张独立的表，Projection 物化的数据就保存在原表的分区目录中，支持明细数据的普通Projection和预聚合Projection。

- **无感使用，自动命中：**

可以对一张 MergeTree 创建多个 Projection ，当执行 Select 语句的时候，能根据查询范围，自动匹配最优的 Projection 提供查询加速。如果没有命中 Projection,就直接查询底表。

- **数据同源、同生共死：**

**因为物化的数据保存在原表的分区，所以数据的更新、合并都是同源的，也就不会出现不一致的情况了。**

## 16.2 创建Projection投影测试

下面我们通过案例来测试Projection的使用性能，示例如下：

```text
#向MySQL 库ck_db中导入 song表,数据量为17万左右，在ClickHouse库mysql_ck_db中会有对应的物化引擎表，这时在ClickHouse默认default库中执行如下语句，将song表数据导入到default.song_info表中
node1 :) create table song_info engine=MergeTree() order by source_id partition by status as select * from mysql_ck_db.song;

#没有Projection的时候，查询非主键name列
node1 :) select name from song_info where name = '独家女孩';
1 rows in set. Elapsed: 0.017 sec. Processed 177.31 thousand rows, 4.16 MB (10.68 million rows/s., 250.61 MB/s.)
```



针对name列创建一个Projection，为特定的where字段加速，按照查询的需求生成有别于主键的另一种排序规则：

```text
ALTER TABLE song_info ADD PROJECTION p1
( 
    SELECT 
      name,album
    ORDER BY name
) 
```



注意：只有在创建Projection之后，再被写入的数据才会自动物化，对于历史数据需要执行如下命令手动触发物化：

```text
ALTER TABLE song_info MATERIALIZE PROJECTION p1;
```



我们可以执行如下命令查询物化是否完成：

```text
SELECT
    table,
    mutation_id,
    command,
    is_done
FROM system.mutations AS m
where table = 'song_info';
```



![img](readme.assets/v2-726aa5a83d67b7e952131c53fdd6eff7_1440w.png)

以上完成后，我们可以进入到song_info数据目录：“/var/lib/ClickHouse/data/default/song_info/”，发现生成新的数据目录：

![img](readme.assets/v2-0b3015943dd019bdbb02216733ebca53_1440w.webp)



编辑且每个分区对应的新目录中多了一个p1.proj子目录，进入到此子目录，我们发现与MergeTree表存储格式一样，如下：

![img](readme.assets/v2-59abc37a6b485521316aecf4cab2d170_1440w.webp)

有了p1 projection后我们执行同样查询，首先需要设置参数开启projection功能：

```text
SET allow_experimental_projection_optimization = 1;
```



开启之后，重新执行原有的SQL，我们发现由原来扫码4.16M数据变成了扫描407Kb数据：

```text
node1 :) select name from song_info where name = '独家女孩';
1 rows in set. Elapsed: 0.011 sec. Processed 17.08 thousand rows, 407.83 KB (1.60 million rows/s., 38.23 MB/s.)
```



## 16.3 explain验证是否使用Projection优化

我们可以执行如下命令来查看执行的SQL是否使用了Projection功能：

```text
node1 :)explain select name from song_info where name = '独家女孩';
```



![img](readme.assets/v2-15a4b7f0f0724be8dddf15cf4ffb0167_1440w.png)

如上，如果看到“MergeTree(with 0 projection p1)”代表使用了Projection优化。

## 16.4 Projection支持预聚合

projection同样支持预聚合，在没有优化的情况下，以下查询会全表扫描：

```text
node1 :) select source,count(*) from song_info group by source;
7 rows in set. Elapsed: 0.011 sec. Processed 177.31 thousand rows, 886.58 KB (16.12 million rows/s., 80.60 MB/s.)
```



创建另外一个Projection：

```text
ALTER TABLE song_info ADD PROJECTION agg_p2
   ( 
     SELECT
         source, 
         count(*)
       GROUP BY source
   )
```



由于历史数据存在，需要手动触发下物化：

```text
ALTER TABLE song_info MATERIALIZE PROJECTION agg_p2;
```



执行完成之后，再次执行相同查询：

```text
node1 :) select source,count(*) from song_info group by source;
7 rows in set. Elapsed: 0.009 sec.
```



可以通过以上看出速度快了很多。

## 16.5 删除Projection投影

我们可以执行如下命令删除Projection：

```text
node1 :) ALTER TABLE song_info DROP PROJECTION p1;
node1 :) ALTER TABLE song_info DROP PROJECTION agg_p2;
```



## 16.6 Projection投影总结

在使用Projection时，查询使用Projection的匹配规则如下：

- 设置SET allow_experimental_projection_optimization = 1
- 返回的数据行小于基表总数
- 查询覆盖的分区part超过一半
- Where必须是Projection定义中Group By的子集
- Group By必须是Projection定义中Group By的子集
- Select 必须是Projection定义中Select的子集
- 匹配多个Projection的时候，自动选取读取part最少的

利用Projection，我们只需要面对一张表查询就行，即拥有了原来物化视图的性能，又免去了维护成本与数据一致性的问题，相信未来可以使用Projection替换物化视图。

![img](readme.assets/v2-8c73156c85a01b768ee0068f4b270928_1440w.webp)



# 十七 ClickHouse优化

[-](https://zhuanlan.zhihu.com/p/582366185)

## 17.1 表优化

### **1、日期字段避免使用String存储**

在Hive中对于日期数据我们经常使用String类型存储，但是在ClickHouse中建表时针对日期类型数据存储建议使用日期类型存储，不使用String类型存储，因为在使用到日期时日期类型可以直接处理，String类型的日期数据还需要使用函数进行处理，执行效率低。例如：

```text
select toDateTime('2021-12-31 17:22:23'),toTypeName(toDateTime('2021-12-31 17:22:23'))
```



![img](readme.assets/v2-145e3ac5dbf4c08ab7398c9ac8da57d2_1440w.png)

### **2、Nullable值处理**

在ClickHouse表中数据存储时，对于一些列尽量不使用Nullable类型存储，因为此类型需要单独创建额外的文件来存储NULL的标记并且Nullable类型列无法被索引，会拖累性能，在数据存储时如果有空值时，我们可以选择在业务中没有意义的值来替代NULL值。

### **3、分区和索引**

ClickHouse中一般选择按天分区，可以指定tuple()指定多个列为组合分区。如果不按天分区，每个分区数据量控制在800~1000万为宜。

建表时通过order by 指定索引列，可以指定tuple(),指定多个列为索引列，指定索引列时最好满足高基列在前、查询频率大的列在前的原则。基数过大的列不适合作为索引列，因为如果某列基数特别大，这种情况有索引和没索引效果一样。

### **4、建表指定TTL**

如果表不是必须保存全量历史数据，建议指定TTL，以免去手动清除过期数据的麻烦。

## 17.2 写入查询优化

### **1、避免小批量数据写入**

尽量避免单条和小批量插入、删除操作，会产生大量小分区文件，给后台Merge带来压力。

### **2、count优化**

在ClickHouse中向查询数据总条数时，使用count() 代替count(列)查询，因为使用count()查询会自动寻找数据目录中的“count.txt”文件读取数据总条目，性能极高。如果使用count(列)相当于扫描全表读取总数据量。

```text
node1 :) explain plan select count() from person_info;
```



![img](readme.assets/v2-bd1ae4386e64c8f57c6bcce47558dd7e_1440w.webp)



```text
node1 :) explain plan select count(name) from person_info;
```



### **3、避免使用select \***

数据量太大时应避免使用select * 查询，这种查询会将表中所有字段都查询出来，IO消耗大，查询字段越少消耗的IO资源就越少，性能就会越高。

### **4、避免构建虚拟列**

如果非必要尽量避免在查询时构建虚拟列，虚拟列非常消耗资源，造成性能浪费，可以考虑在前端进行处理或者在表中构建实际的列进行额外存储。

```text
#避免使用虚拟列 ，以下count1/count2就是虚拟列
select id,name,count1,count2,count1/count2 as new_col from tbl;
```



### **5、使用uniqCombined代替count(distinct)**

使用uniqCombined替代distinct性能可提升10倍以上，uniqCombined 底层采用类似HyperLogLog算法实现，如能接收2%左右的数据误差，可直接使用这种去重方式提升查询性能。

```text
node1 :) select count(distinct WatchID) from datasets.hits_v1;
```



![img](readme.assets/v2-0701aef42699e0870fb24d270abb0151_1440w.png)

```text
node1 :) select uniqCombined(WatchID) from datasets.hits_v1;
```



![img](readme.assets/v2-f11471f81a783fa7b5cf70cf4a6d5c2a_1440w.png)

### **6 、使用物化视图**

对于一些确定的数据模型，可以将统计指标通过物化视图的方式进行构建，这样可避免数据查询时重复计算的过程，同样在后期也可以构建Projection投影来替代物化视图。

### **7、Join关联相关**

当多表关联查询时，查询的数据仅来源于一张表时，可考虑用IN代替JOIN，速度会更快。

```text
node1 :) select count(distinct a.CounterID) as cnt  from hits_v1 as a  join visits_v1  as b on a.CounterID = b.CounterID
```



![img](readme.assets/v2-2328ac1b93ff5e8c10c918845847c248_1440w.png)

```text
node1 :) select count(distinct CounterID) as cnt from hits_v1 where CounterID in (select  CounterID from visits_v1);
```



![img](readme.assets/v2-e79e60bc5d5112c755acf8e6a0da2f52_1440w.png)

此外，多表关联时，将小表放在右侧，因为右表自动会被加载到内存中与左表进行关联。

### **8、分布式表使用global**

对分布式表使用join 或者 in时，ClickHouse会将当前SQL分发到各个ClickHouse节点上执行，例如有如下SQL:

```text
select a.id,a.name,b.score from a join b on a.id = b.id
```



如果以上a表和b表都是分布式表，ClickHouse集群有3个节点，那么上面SQL会分发到ClickHouse所有节点执行，b表会在每个节点上收集其他节点对应b表数据并放在内存，这样的话，每个ClickHouse节点都会从对应的3台节点上将b表数据进行汇集。

如果使用global关键字，执行如下SQL：

```text
select a.id,a.name,b.score from a global join b on a.id = b.id
```



这样执行SQL的话，相当于在当前写SQL节点会将查询得到b表所有数据，然后统一分发到其他ClickHouse各个节点上，然后每个节点在执行与a表关联。这样使用global就减少了集群之间查询次数。假设b表有N个分片分布在N个ClickHouse节点上，不使用global时，每个节点获取b表全量数据需要执行N的平方次查询，使用global时只需要执行N次查询即可。

所以在使用分布式表进行join或者in时，可以优先考虑使用global，使用用法如下：

```text
select a.id,a.name,b.score from a global join b on a.id = b.id
select a.id,a.name from a global  where a.id global in (select id from b)
```



### **9、避免使用final**

ClickHouse中我们可以使用ReplacintMergeTree来对数据进行去重，这个引擎可以在数据主键相同时根据指定的字段保留一条数据，ReplacingMergeTree只是在一定程度上解决了数据重复问题，由于自动分区合并机制在后台定时执行，所以并不能完全保障数据不重复。我们需要在查询时在最后执行final关键字，final执行会导致后台数据合并，查询时如果有final效率将会极低，我们应当避免使用final查询，那么不使用final我们可以通过自己写SQL方式查询出想要的数据，举例如下：

```text
#创建replacingMergeTree 表t_replacing_mt
create table t_replacing_mt(
id UInt8,
name String,
age UInt8
) engine = ReplacingMergeTree(age)
order by id;

#向表中插入以下数据
insert into t_replacing_mt values (1,'张三',18),(2,'李四',19),(3,'王五',20);
```



![img](readme.assets/v2-083d630061f0198bc8c7cdab6914f741_1440w.webp)

```text
#继续向表中插入如下数据
insert into t_replacing_mt values (1,'张三',20),(2,'李四',15);
```



![img](readme.assets/v2-257f1286ef28808c8947cf544888a640_1440w.jpeg)

```text
#通过final查询最终结果
node1 :) select * from t_replacing_mt final;
```

![img](readme.assets/v2-67898c5351af2bc7cf530c037a94f7e7_1440w.jpeg)

下面我们不使用final，通过自己写SQL方式现在查询最终合并数据，操作如下：

```text
#重新删除表t_replacing_mt，重建、并加载如下数据
drop table t_replacing_mt;

create table t_replacing_mt(
id UInt8,
name String,
age UInt8
) engine = ReplacingMergeTree(age)
order by id;

insert into t_replacing_mt values (1,'张三',18),(2,'李四',19),(3,'王五',20);

#继续向表中插入如下数据
insert into t_replacing_mt values (1,'张三',20),(2,'李四',15);

#自己写SQL方式实现查询去重后的数据，这样避免使用final查询，效率提高
SELECT
    id,
    argMax(name, age) AS namex,
    max(age) AS agex
FROM t_replacing_mt
GROUP BY id
```



![img](readme.assets/v2-e8bbafeca69a9d6d85ac9d07455744e9_1440w.webp)

**注意：argMax(arg,val)函数意思是找到val最大值对应的arg值，如果val有多个相同最大值，则遇到的第一条对应的arg值输出。**

我们还可以针对ReplacingMergeTree表加上一个时间字段，通过自己写SQL方式实现数据更新来避免使用CollapsingMergeTree引擎进行数据更新。当有数据更新时，我们插入这条更新的数据，时间对应的是最新时间，查询时找到最大时间对应的数据即可，不必再创建CollapsingMergeTree引擎使用final语句进行更新数据，具体操作类似以上SQL操作。





# 十八、用户配置

**ClickHouse用户配置**

在之前使用ClickHouse过程中为了简便我们使用的default默认用户，默认无密码登录模式，这显然不符合生产环境的要求。我们可以通过“/etc/clickhoue-server/user.xml”来配置用户相关配置项，此文件可以配置系统参数设置、用户定义、权限、以及熔断机制等。

## 18.1 用户Profile

用户Profile类似用户角色，可以预先在user.xml中为ClickHouse定义多组profile角色，并为每组角色定义不同的配置项以实现配置的复用，例如：

```text
<yandex>
 <!-- 配置profiles -->
 <profiles>
  <default><!-- 自定义角色，默认名称 -->
   <max_memory_usage>10000000000</max_memory_usage>
   <use_uncompressed_cache>0</use_uncompressed_cache>
  </default>
  <test1><!-- 自定义角色，默认名称 -->
   <distributed_product_mode>allow</distributed_product_mode>
  </test1>
  ... ...
 </profiles>
 ... ...
</yandex>
```



**注意：**

- max_memory_usage：在单个ClickHouse服务进程中，运行一次查询限制使用的最大内存用量，默认值为10G
- use_uncompressed_cache：是否使用未压缩块的缓存，填写0或1。默认情况下，0（禁用）。

distributed_product_mode:当查询时包含分布式表join时，更改分布式子查询行为。allow允许使用子查询。

以上配置中我们定义了“default”和“test1”两组profile角色，在user.xml中配置用户时，我们就可以使用对应的角色。

注意：在标签配置中，名称为“default”的profile将作为默认的配置被加载，所以必须有“default”角色。

## 18.2 配置约束

constraints标签可以设置一组约束条件，保证profile角色内的参数值不会被随意修改，约束条件有如下三种规则：

- Min:最小值约束，在设置相应参数的时候，取值不能小于该阈值。
- Max：最大值约束，在设置相应参数的时候，取值不能大于该阈值。
- Readonly：只读约束，该参数值不允许被修改。

例如，修改user.xml中profile角色，如下：

```text
<yandex>
 <!-- 配置profiles -->
 <profiles>
  <default><!-- 自定义角色，默认名称 -->
   <max_memory_usage>10000000000</max_memory_usage>
    <distributed_product_mode>allow</distributed_product_mode>
   <constraints><!--配置约束-->
    <max_memory_usage>
     <min>5000000000</min>
     <max>20000000000</max>
    </max_memory_usage>
    <distributed_product_mode>
     <readonly/>
    </distributed_product_mode>
   </constraints>
  </default>
  ... ...
 </profiles>
 ... ...
</yandex>
```



从上面的配置我们可以看出，在default默认的profile角色内给两组参数设置了约束，配置完成之后可以直接登录ClickHouse，尝试修改对应的值：

```text
#修改使用最大内存
node1 :) set max_memory_usage=50;
 DB::Exception: Setting max_memory_usage shouldn't be less than 5000000000. (SETTING_CONSTRAINT_VIOLATION)

#修改distributed_product_mode的参数值
node1 :) set distributed_product_mode='deny';
 DB::Exception: Setting distributed_product_mode should not be changed. (SETTING_CONSTRAINT_VIOLATION)
```



以上我们可以看到配置约束成功阻止了预期外的修改。

**注意：在default中默认定义的constraints约束，将作为默认全局约束，自动被其他profile继承。**

## 18.3 用户定义

在user.xml中可以使用users标签配置自定义用户，我们可以在user.xml中看到标签下有default用户，我们之前使用默认就是此用户。下面我们配置一个新的用户“zhangsan”,user.xml配置如下：

```text
<yandex>
... ...
 <users>
  <default>
   ... ...
  </default>
  <zhangsan>
   <!-- 设置密码 -->
   <password>123456</password>
   
   <!-- 设置用户允许登录的网络地址 -->
   <networks>
    <!-- 表示任意节点登录 -->
    <ip>::/0</ip>
   </networks>
   
   <!-- 用户使用的profile 角色 -->
   <profile>default</profile>
   
   <!-- 设置熔断机制 -->
   <quota>default</quota>
   
  </zhangsan>
 </users>
</yandex>
```

以上配置完成后，不需重启ClickHouse，使用用户zhangsan登录ClickHouse，操作如下：

```text
[root@node1 ~]# ClickHouse-client -u zhangsan --password 123456
```



## 18.4 访问权限

访问权限是整个权限体系的第一层防护，可以分为两类权限。

### **1、网络访问权限**

网络权限使用networks标签设置，用户限制某个用户登录的客户端地址，例如,修改users.xml文件：

```text
<zhangsan>
 <!-- 设置用户允许登录的网络地址 -->
 <networks>
  <!-- 限制在此ip登录 -->
  <ip>192.168.179.5</ip>
 </networks>
</zhangsan>
```



修改之后，在node1(192.168.179.4)上登录zhangsan用户登录不上。

```text
[root@node1 logs]# ClickHouse-client -u zhangsan --password 123456
ClickHouse client version 21.9.4.35 (official build).
Connecting to localhost:9000 as user zhangsan.
Code: 516. DB::Exception: Received from localhost:9000. DB::Exception: zhangsan: Authentication failed: password is incorrect or there is no user with such name. (AUTHENTICATION_FAILED)
```



### **2、数据库与字典访问权限**

当客户端连入ClickHouse之后，可以进一步限制某个用户数据库和字典的访问权限，可以通过<allow_database>和<allow_dictionaries>标签进行设置，如果不进行任何定义则表示不进行限制。

现在针对用户zhangsan只能使用default库，在users.xml中的配置如下：

```text
<zhangsan>
 <!-- 设置用户访问数据库 -->
 <allow_databases>
  <database>default</database>
  <database>datasets</database>
 </allow_databases>
</zhangsan>
```



配置完成之后，使用zhangsan用户重新登录ClickHouse，查看数据库：

```text
node1 :) show databases;
┌─name─────┐
│ datasets │
│ default  │
└──────────┘
```



## 18.5 查询权限

查询权限是整个权限体系的第二层防护，它决定了一个用户能够执行的查询语句，查询权限可以分为以下四类：

- 读权限：包括SELECT、EXISTS、SHOW和DESCRIBE查询。
- 写权限：包括INSERT和OPTIMIZE查询。
- 设置权限：包括SET查询。
- DDL权限：包括CREATE、DROP、ALTER、RENAME、ATTACH、DETACH和TRUNCATE查询。

上述四类权限，可以通过以下两项配置标签控制：

**1、readonly**

读权限、写权限和设置权限均由此标签控制，它有三种取值：

- 当取值为0时，不进行任何限制（默认值）
- 当取值为1时，只拥有读权限（只能执行SELECT、EXISTS、SHOW和DESCRIBE）
- 当取值为2时，拥有读权限和设置权限（在读权限基础上，增加了SET查询）

**2、allow_ddl**

DDL权限由此标签控制，它有两种取值：

- 当取值为0时，不允许DDL查询。
- 当取值为1时，允许DDL查询（默认值）。

此外，需要注意的是readonly和allow_ddl需要定义在用户使用的profiles角色中。

查询权限测试如下：

首先，在users.xml中增加新角色normal，并配置只有read读权限，配置如下：

```text
<yandex>
 <!-- 配置profiles -->
 <profiles>
        ... ...
        <!-- 自定义角色normal,只有read读权限 -->
        <normal>
             <readonly>1</readonly>             
             <allow_ddl>0</allow_ddl>
        </normal>
  ... ...
 </profiles>
 ... ...
</yandex>
```



其次在users.xml中给zhangsan配置角色为normal：

```text
<!-- 用户使用的profile 角色 -->
<zhangsan>
... ...
 <profile>normal</profile>
</zhangsan>
```



配置完成之后，使用用户zhangsan登录ClickHouse，做如下操作：

```text
#创建数据库
node1 :) create database aaa;
 DB::Exception: zhangsan: Not enough privileges. To execute this query it's necessary to have grant CREATE DATABASE ON aaa.*. (ACCESS_DENIED)

#查询表person_info中数据
node1 :) select * from person_info;
┌─id─┬─name─┬─age─┬─gender─┬─loc─────┐
│  1 │ zs   │  18 │ m      │ beijing │
│  3 │ ww   │  20 │ m      │ beijing │
└────┴──────┴─────┴────────┴─────────┘
┌─id─┬─name─┬─age─┬─gender─┬─loc──────┐
│  2 │ ls   │  19 │ f      │ shanghai │
│  4 │ ml   │  21 │ m      │ shanghai │
└────┴──────┴─────┴────────┴──────────┘

#向表person_info中插入数据
node1 :) insert into person_info values (5,'xx',10,'f','beijing');
DB::Exception: zhangsan: Cannot execute query in readonly mode. (READONLY)
```



通过以上测试我们发现权限已经生效。

## 18.5 数据行级权限

数据权限是整个权限体系中的第三层防护，决定了一个用户能够看到什么数据。数据权限使用database标签定义，是用户定义中的一项选填设置，database通过定义用户级别的查询过滤器来实现数据的行级粒度权限，定义规则如下：

```text
<databases>
 <database_name><!-- 数据库名称 -->
  <table_name><!-- 表名称 -->
   <filter>id < 10</filter><!-- 数据过滤条件 -->
  </table_name>
 </database_name>
</databases>
```



以上标签中database_name是数据库名称，table_name是表名称，而filter则是权限过滤的关键，等同于定义了一条where条件，支持组合条件。

下面测试数据权限，给用户zhangsan限制读取表person_info数据中id<=2 或者id=4的数据，配置users.xml如下：

```text
<zhangsan>
 ... ...
 <databases>
  <default><!-- 数据库名称 -->
   <person_info><!-- 表名称 -->
    <!-- 过滤条件id <=2 or id =4 -->
    <filter>id&lt;=2 or id=4 </filter> <!-- 数据过滤条件 -->   </person_info>
  </default>
 </databases>
</zhangsan>
```



**注意：**

- < 表示小于号

- 表示大于号

  

以上配置完成之后，使用zhangsan登录ClickHouse查询数据，结果如下：

```text
node1 :) select * from person_info;
┌─id─┬─name─┬─age─┬─gender─┬─loc─────┐
│  1 │ zs   │  18 │ m      │ beijing │
└────┴──────┴─────┴────────┴─────────┘
┌─id─┬─name─┬─age─┬─gender─┬─loc──────┐
│  2 │ ls   │  19 │ f      │ shanghai │
│  4 │ ml   │  21 │ m      │ shanghai │
└────┴──────┴─────┴────────┴──────────┘
```

可见配置的数据行级权限生效。

# 十九、数据备份

[-](https://zhuanlan.zhihu.com/p/584140763)

**ClickHouse数据备份**

在ClickHouse中存在副本，主要作用是保证数据的高可用，副本不能防止数据误删这类操作，ClickHouse提供了多种数据备份的方法来防止数据误删。

## 19.1 导出文件备份

如果数据的体量较小，可以通过dump的形式将数据导出为本地文件，具体操作如下：

```text
#在ClickHouse中创建表 test_backup
create table test_backup(
id UInt64,
name String,
age UInt8,
loc String
)engine=MergeTree()
order by id 
partition by loc 

#向表test_backup中插入如下数据
node1 :) insert into test_backup values (1,'zs',18,'beijing'),(2,'ls',19,'shanghai'),(3,'ww',20,'guangzhou'),(4,'ml',21,'beijing'),(5,'tq',22,'shanghai'),(6,'gb',23,'guangzhou');
```



![img](https://pic1.zhimg.com/80/v2-804ebe06c420b67630e138e3bb98d96c_1440w.webp)

执行导出数据命令：

```text
#在node1节点创建导出数据目录“/software/data/”
[root@node1 ~]# mkdir -p /software/data/

#执行导出数据命令
[root@node1 ~]# ClickHouse-client --query="select * from test_backup" > /software/data/test_backup.tsv
注意：以上命令执行完成之后，会在“/software/data”目录下生成test_backup.tsv文件，数据默认按照tab隔开。
```

数据备份完成之后，需要还原到表时，可以执行如下命令：

```text
#首先将表test_backup数据全部删除
node1 :) alter table test_backup delete where id >0;

#执行如下命令，将备份数据再次导入到表test_backup
[root@node1 data]# cat /software/data/test_backup.tsv |ClickHouse-client --query "Insert into test_backup format TSV"
注意：TSV格式必须大写。

#导入之后，查询表test_backup中的数据
node1 :) select * from test_backup;
```



![img](readme.assets/v2-fe6b79891bc8879cd461546e5d32f139_1440w.jpeg)

## 19.2 通过快照表备份

快照表实质上就是普通的数据表，使用快照表备份时首先需要建立一张与原表结构相同的数据表，然后再使用“Insert into select ”句式将原有数据表写入到备份表。

假设test_backup表需要在1号这天做备份，步骤如下：

```text
#创建备份表，备份表名称以当天日期结尾，只是创建了表
node1 :) create table test_backup_01 as test_backup;

#备份数据
node1 :) insert into test_backup_01 select * from test_backup;

#查询test_backup_01表中的数据
node1 :) select * from test_backup_01;
```



![img](readme.assets/v2-c6c5a42da25f9c01d9fcb429dba7fd8a_1440w.jpeg)

如果考虑到容灾问题，也可以将备份表放置在不同的ClickHouse节点上，此时备份数据的SQL如下：

```text
insert into test_backup_01 select * from remote('node1:9000','default','test_backup','default','')
```

**注意：remote中参数意义为remote(远程ClickHouse集群,连接ClickHouse库，备份的表，登录用户，用户密码)**

测试如下：

```text
#在node2节点登录ClickHouse，创建test_backup_01表
node2 :)create table test_backup_01(
id UInt64,
name String,
age UInt8,
loc String
)engine=MergeTree()
order by id 
partition by loc ;

#然后在node2执行如下备份数据命令：
node2 :) insert into test_backup_01 select * from remote('node1:9000','default','test_backup','default','');

#在node2查询表test_backup_01表数据
node2 :) select * from test_backup_01;
```

![img](readme.assets/v2-ea775ee9d3698bd5bd28d179d6f82fc6_1440w.jpeg)

## 19.3 按分区备份

基于数据分区的备份，ClickHouse目前提供了FREEZE与FETCH两种方式。

### **1、使用FREEZE备份**

FREEZE的完整语法如下：

```text
ALTER TABLE tb_name FREEZE PARTITION partition_expr
```

分区在备份之后，会统一保存到“/var/lib/ClickHouse/shadow/N”目录下，其中N是一个自增的整数，代表备份的次数（FREEZE执行的次数），具体次数由shadow子目录下的increment.txt文件记录。

使用FREEZE对分区进行备份测试：

在node1上对表test_backup分区“beijing”进行备份，具体操作如下：

```text
#在node1中执行如下命令
node1 :) ALTER TABLE test_backup FREEZE PARTITION 'beijing';
```

以上命令执行完成之后，可以在“/var/lib/ClickHouse/shadow/1”下看到对应的数据。

![img](readme.assets/v2-dedfbdafb7436caac9cafda34d78e061_1440w.png)

在对应的数字目录中，目录结构为：/store/[database]/[table]/[partition_folder],如下：

![img](readme.assets/v2-e6f973b07713f5a32d80b1e280f32dfa_1440w.png)

对于备份的数据还原，需要借助ATTACH装载分区的方式实现，需要将shadow子目录下的分区文件复制到相应数据表的detached目录下，然后再使用ATTACH语句加载。

下面将备份的分区还原到表test_backup中，操作如下：

```text
#将表test_backup表“beijing”分区删除
node1 :) alter table test_backup drop partition 'beijing';

#将备份的分区数据放入表test_backup数据目录下的detached目录下
[root@node1 ~]# cp -r /var/lib/ClickHouse/shadow/1/store/5eb/5eb930e9-8a99-4def-9eb9-30e98a997def/* /var/lib/ClickHouse/data/default/test_backup/detached/

#查看detached目录下的数据
[root@node1 detached]# ls /var/lib/ClickHouse/data/default/test_backup/detached
e8ccea18d4053730ef65315d1781f8c6_14_14_0

#修改分区数据对应的拥有者权限为ClickHouse
[root@node1 detached]# chown -R ClickHouse:ClickHouse e8ccea18d4053730ef65315d1781f8c6_14_14_0/

#给表test_backup表添加分区
node1 :) alter table test_backup attach partition 'beijing';

#查询表test_backup表中数据，数据还原成功
node1 :) select * from test_backup;
```



![img](readme.assets/v2-0b5b7b8b09cc65432d5ff544143eff19_1440w.webp)

### **2、使用FETCH备份**

FETCH 只支持ReplicatedMergeTree系列的表引擎，完整语法如下：

```text
ALTER TABLE tb_name FETCH PARTITION partition_id FROM zk_path
```



其工作原理与ReplicatedMergeTree同步数据的原理类似，FETCH通过指定的zk_path找到ReplicatedMergeTree的所有副本实例，然后选择一个最合适的副本，下载相应的分区数据。

FETCH备份分区数据案例如下：

```text
#在ClickHouse中创建ReplicatedMergeTree表 test_fetch
#在node1节点执行建表语句
Create table test_fetch(
id UInt32,
name String,
age UInt32,
gender String,
loc String
) engine = ReplicatedMergeTree('/ClickHouse/tables/01/test_fetch','node1')
partition by loc
order by id;

#在node2节点执行建表语句
Create table test_fetch(
id UInt32,
name String,
age UInt32,
gender String,
loc String
) engine = ReplicatedMergeTree('/ClickHouse/tables/01/test_fetch','node2')
partition by loc
order by id;

#在node1向表test_fetch中插入数据
insert into test_fetch values (1,'zs',18,'m','beijing'),(2,'ls',19,'f','shanghai'),(3,'ww',20,'m','beijing'),(4,'ml',21,'m','shanghai')
```



将表test_fetch “beijing”分区数据进行备份，在node1节点上执行如下命令：

```text
ALTER TABLE test_fetch FETCH PARTITION 'beijing' FROM '/ClickHouse/tables/01/test_fetch'
```



执行以上命令之后，将对应分区的数据下载到当前节点本地“/var/lib/ClickHouse/data/{database}/{table}/detached”目录下：

![img](readme.assets/v2-53f0f7252e740df5398ff549d5e3fe12_1440w.webp)

与FREEZE一样，对分区数据进行还原时，需要借助ATTACH装载分区来实现，具体操作如下：

```text
#在node1上删除表test_fetch “beijing”分区数据
node1 :) alter table test_fetch drop partition 'beijing';

#由于默认在detached中的数据权限为ClickHouse，不需要修改所有者，直接执行添加分区命令恢复备份数据即可
node1 :) alter table test_fetch attach partition 'beijing';

#查询表test_fetch中的数据
node1 :) select * from test_fetch;
```



![img](readme.assets/v2-9cd77da8f2d8e38fa84eb45cda3527a6_1440w.webp)

FREEZE和FETCH虽然都能实现分区文件的备份，但不会备份数据表的元数据（也就是建表语句），为了做到万无一失的备份，还需要对元数据进行备份，元数据在“/var/lib/ClickHouse/metadata/{database}”目录下，找到对应表的sql文件单独保存即可。

# 二十、服务监控

## 20.1 系统表

在ClickHouse System库下的系统表中最要有三张表支撑了ClickHouse运行指标的查询，分别是：metrics、events和asynchronous_metrice。

### **1、metrics**

metrics表用于统计ClickHouse服务在运行时，当前正在执行的概要信息，包括正在执行的查询总次数、正在发生的合并操作总次数等，查询方式如下：

```text
node1 :) select * from system.metrics limit 5;
```



![img](https://pic4.zhimg.com/80/v2-679bd98a4e581cd9320cb5403128d78f_1440w.webp)

### **2、events**

events统计ClickHouse在运行过程中已经执行过的概要信息，包括总的查询次数、总的SELECT查询次数等，该系统表的查询方法如下：

```text
node1 :) select event,value from system.events limit 5;
```



![img](https://pic1.zhimg.com/80/v2-82159b401b055bdc6b700cea5a1b74f4_1440w.webp)

### **3、asynchronous_metrics**

asynchronous_metrics用于统计ClickHouse服务运行过程中当前正在后台异步运行的概要信息，包括当前分配的内存、执行队列中的任务数量等。该系统表的查询方法如下：

```text
node1 :) select * from system.asynchronous_metrics limit 5;
```



![img](https://pic2.zhimg.com/80/v2-e0b3cee90fa798eea0f9d3568fc12d21_1440w.webp)

## 20.2 查询日志

查询日志重要的目前有3种类型，分别从不同角度记录了ClickHouse的操作行为，ClickHouse会为每种类型的查询日志自动生成相应的系统表以供查询。

### **1、query_log**

query_log是最常用的查询日志，记录了ClickHouse服务中所有已经执行的查询记录。查询方式如下：

```text
node1 :) select type,concat(substr(query,1,20),'...') as query,read_rows,query_duration_ms as duration from system.query_log limit 5;
```



![img](https://pic4.zhimg.com/80/v2-11f72f6f10209e5ba1958105786340af_1440w.webp)

### **2、query_thread_log**

query_thread_log系统表记录了所有线程的执行查询信息。查询方式如下：

```text
node1 :) select thread_name,concat(substr(query,1,20),'...') as query,query_duration_ms as duration,memory_usage as memory from system.query_thread_log limit 6;
```



![img](https://pic4.zhimg.com/80/v2-8af55d71f6d5011d148a31bc3be75037_1440w.webp)

### **3、part_log**

part_log日志记录了MergeTree系列表引擎的分区操作日志。查询方式如下：

```text
node1 :) select event_type as type ,table ,partition_id,event_date from system.part_log desc limit 5;
```



![img](https://pic3.zhimg.com/80/v2-0bb04830c7e32ce640039d7a754d68d2_1440w.webp)

除了上面介绍的系统表和查询日志之外，ClickHouse还能够与第三方监控系统集成。

# 二十一、数据类型

## 21.1 int类型

创建表时，可以为整数设置类型参数 (例如. `TINYINT(8)`, `SMALLINT(16)`, `INT(32)`, `BIGINT(64)`), 但 ClickHouse 会忽略它们.

**整型范围**

- `Int8` — [-128 : 127]
- `Int16` — [-32768 : 32767]
- `Int32` — [-2147483648 : 2147483647]
- `Int64` — [-9223372036854775808 : 9223372036854775807]
- `Int128` — [-170141183460469231731687303715884105728 : 170141183460469231731687303715884105727]
- `Int256` — [-57896044618658097711785492504343953926634992332820282019728792003956564819968 : 57896044618658097711785492504343953926634992332820282019728792003956564819967]

别名：

- `Int8` — `TINYINT`, `BOOL`, `BOOLEAN`, `INT1`.
- `Int16` — `SMALLINT`, `INT2`.
- `Int32` — `INT`, `INT4`, `INTEGER`.
- `Int64` — `BIGINT`.

**无符号整型范围**

- `UInt8` — [0 : 255]
- `UInt16` — [0 : 65535]
- `UInt32` — [0 : 4294967295]
- `UInt64` — [0 : 18446744073709551615]
- `UInt128` — [0 : 340282366920938463463374607431768211455]
- `UInt256` — [0 : 115792089237316195423570985008687907853269984665640564039457584007913129639935]

## 21.2 UUID

UUID类型值的示例如下:

```text
61f0c404-5cb3-11e7-907b-a6006ad3dba0
```



如果在插入新记录时未指定UUID列的值，则UUID值将用零填充:

```text
00000000-0000-0000-0000-000000000000
```

如何生成

要生成UUID值，ClickHouse提供了 [generateuidv4](https://clickhouse.com/docs/zh/sql-reference/functions/uuid-functions) 函数。

```
这个例子演示了创建一个具有UUID类型列的表，并在表中插入一个值。

CREATE TABLE t_uuid (x UUID, y String) ENGINE=TinyLog

INSERT INTO t_uuid SELECT generateUUIDv4(), 'Example 1'

SELECT * FROM t_uuid

┌────────────────────────────────────x─┬─y─────────┐
│ 417ddc5d-e556-4d27-95dd-a34d84e46a50 │ Example 1 │
└──────────────────────────────────────┴───────────┘

示例2

在这个示例中，插入新记录时未指定UUID列的值。

INSERT INTO t_uuid (y) VALUES ('Example 2')

SELECT * FROM t_uuid

┌────────────────────────────────────x─┬─y─────────┐
│ 417ddc5d-e556-4d27-95dd-a34d84e46a50 │ Example 1 │
│ 00000000-0000-0000-0000-000000000000 │ Example 2 │
└──────────────────────────────────────┴───────────┘

```



## 21.3 Date32

以本机字节顺序存储为有符号 32 位整数，值表示自 1970-01-01 以来的天数（0 表示 1970-01-01，负值表示 1970 年之前的天数

```
创建一个带有 -type 列的表Date32并向其中插入数据：

CREATE TABLE new
(
    `timestamp` Date32,
    `event_id` UInt8
)
ENGINE = TinyLog;

INSERT INTO new VALUES (4102444800, 1), ('2100-01-01', 2);
SELECT * FROM new;

┌──timestamp─┬─event_id─┐
│ 2100-01-01 │        1 │
│ 2100-01-01 │        2 │
└────────────┴──────────┘
```

## 21.4 date64

此类型允许以日期（date）加时间（time）的形式来存储一个时刻的时间值，具有定义的亚秒精度

时间刻度大小（精度）：10-精度 秒

值的范围: [1900-01-01 00:00:00, 2299-12-31 23:59:59.99999999] (注意: 最大值的精度是8)。

**示例**

**1.** 创建一个具有 `DateTime64` 类型列的表，并向其中插入数据:

```sql
CREATE TABLE dt
(
    `timestamp` DateTime64(3, 'Asia/Istanbul'),
    `event_id` UInt8
)
ENGINE = TinyLog
```

- 时间刻度的分辨率由precision参数确定

```sql
INSERT INTO dt Values (1546300800000, 1), ('2019-01-01 00:00:00', 2)
SELECT * FROM dt
┌───────────────timestamp─┬─event_id─┐
│ 2019-01-01 03:00:00.000 │        1 │
│ 2019-01-01 00:00:00.000 │        2 │
└─────────────────────────┴──────────┘
```



- 将日期时间作为integer类型插入时，它会被视为适当缩放的Unix时间戳(UTC)。`1546300800000` （精度为3）表示 `'2019-01-01 00:00:00'` UTC. 不过，因为 `timestamp` 列指定了 `Asia/Istanbul` （UTC+3）的时区，当作为字符串输出时，它将显示为 `'2019-01-01 03:00:00'`
- 当把字符串作为日期时间插入时，它会被赋予时区信息。 `'2019-01-01 00:00:00'` 将被认为处于 `Asia/Istanbul` 时区并被存储为 `1546290000000`.

**2.** 过滤 `DateTime64` 类型的值

```sql
SELECT * FROM dt WHERE timestamp = toDateTime64('2019-01-01 00:00:00', 3, 'Asia/Istanbul')
```



```text
┌───────────────timestamp─┬─event_id─┐
│ 2019-01-01 00:00:00.000 │        2 │
└─────────────────────────┴──────────┘
```



与 `DateTime` 不同, `DateTime64` 类型的值不会自动从 `String` 类型的值转换过来

**3.** 获取 `DateTime64` 类型值的时区信息:

```sql
SELECT toDateTime64(now(), 3, 'Asia/Istanbul') AS column, toTypeName(column) AS x
```



```text
┌──────────────────column─┬─x──────────────────────────────┐
│ 2019-10-16 04:12:04.000 │ DateTime64(3, 'Asia/Istanbul') │
└─────────────────────────┴────────────────────────────────┘
```



**4.** 时区转换

```sql
SELECT
toDateTime64(timestamp, 3, 'Europe/London') as lon_time,
toDateTime64(timestamp, 3, 'Asia/Istanbul') as mos_time
FROM dt
```



```text
┌───────────────lon_time──┬────────────────mos_time─┐
│ 2019-01-01 00:00:00.000 │ 2019-01-01 03:00:00.000 │
│ 2018-12-31 21:00:00.000 │ 2019-01-01 00:00:00.000 │
└─────────────────────────┴─────────────────────────┘
```



## 21.5 低基数类型

语法

```sql
LowCardinality(data_type)
```



**参数**

- `data_type` — [String](https://clickhouse.com/docs/zh/sql-reference/data-types/string), [FixedString](https://clickhouse.com/docs/zh/sql-reference/data-types/fixedstring), [Date](https://clickhouse.com/docs/zh/sql-reference/data-types/date), [DateTime](https://clickhouse.com/docs/zh/sql-reference/data-types/datetime)，包括数字类型，但是[Decimal](https://clickhouse.com/docs/zh/sql-reference/data-types/decimal)除外。对一些数据类型来说，`LowCardinality` 并不高效，详查[allow_suspicious_low_cardinality_types](https://clickhouse.com/docs/zh/operations/settings/settings#allow_suspicious_low_cardinality_types)设置描述。

描述

`LowCardinality` 是一种改变数据存储和数据处理方法的概念。 ClickHouse会把 `LowCardinality` 所在的列进行[dictionary coding](https://en.wikipedia.org/wiki/Dictionary_coder)。对很多应用来说，处理字典编码的数据可以显著的增加[SELECT](https://clickhouse.com/docs/zh/sql-reference/statements/select)查询速度。

使用 `LowCarditality` 数据类型的效率依赖于数据的多样性。如果一个字典包含少于10000个不同的值，那么ClickHouse可以进行更高效的数据存储和处理。反之如果字典多于10000，效率会表现的更差。

当使用字符类型的时候，可以考虑使用 `LowCardinality` 代替[Enum](https://clickhouse.com/docs/zh/sql-reference/data-types/enum)。 `LowCardinality` 通常更加灵活和高效。

例子

创建一个 `LowCardinality` 类型的列：

```sql
CREATE TABLE lc_t
(
    `id` UInt16,
    `strings` LowCardinality(String)
)
ENGINE = MergeTree()
ORDER BY id
```



## 21.6 域

ClickHouse 支持表示地理对象的数据类型——位置、土地等。

`Point`由其 X 和 Y 坐标表示，存储为[元组](https://clickhouse.com/docs/en/sql-reference/data-types/tuple)（[Float64](https://clickhouse.com/docs/en/sql-reference/data-types/float)、[Float64](https://clickhouse.com/docs/en/sql-reference/data-types/float)）。

**例子**

询问：

```sql
CREATE TABLE geo_point (p Point) ENGINE = Memory();
INSERT INTO geo_point VALUES((10, 10));
SELECT p, toTypeName(p) FROM geo_point;

┌─p───────┬─toTypeName(p)─┐
│ (10,10) │ Point         │
└─────────┴───────────────┘
```

[戒指]

`Ring`是一个没有孔的简单多边形，存储为点数组：[Array](https://clickhouse.com/docs/en/sql-reference/data-types/array) ( [Point](https://clickhouse.com/docs/zh/sql-reference/data-types/geo#point-data-type) )。

**例子**

询问：

```sql
CREATE TABLE geo_ring (r Ring) ENGINE = Memory();
INSERT INTO geo_ring VALUES([(0, 0), (10, 0), (10, 10), (0, 10)]);
SELECT r, toTypeName(r) FROM geo_ring;
┌─r─────────────────────────────┬─toTypeName(r)─┐
│ [(0,0),(10,0),(10,10),(0,10)] │ Ring          │
└───────────────────────────────┴───────────────┘
```

**多边形**

`Polygon`是一个带有孔的多边形，存储为环数组：[Array](https://clickhouse.com/docs/en/sql-reference/data-types/array) ( [Ring](https://clickhouse.com/docs/zh/sql-reference/data-types/geo#ring-data-type) )。外部数组的第一个元素是多边形的外部形状，后面的所有元素都是孔。

**例子**

这是一个只有一个孔的多边形：

```sql
CREATE TABLE geo_polygon (pg Polygon) ENGINE = Memory();
INSERT INTO geo_polygon VALUES([[(20, 20), (50, 20), (50, 50), (20, 50)], [(30, 30), (50, 50), (50, 30)]]);
SELECT pg, toTypeName(pg) FROM geo_polygon;
┌─pg────────────────────────────────────────────────────────────┬─toTypeName(pg)─┐
│ [[(20,20),(50,20),(50,50),(20,50)],[(30,30),(50,50),(50,30)]] │ Polygon        │
└───────────────────────────────────────────────────────────────┴────────────────┘
```

**多重多边形**

`MultiPolygon`由多个多边形组成，并存储为多边形数组：[Array](https://clickhouse.com/docs/en/sql-reference/data-types/array) ( [Polygon](https://clickhouse.com/docs/zh/sql-reference/data-types/geo#polygon-data-type) )。

**例子**

该多边形由两个独立的多边形组成 - 第一个没有孔，第二个有一个孔：

```sql
CREATE TABLE geo_multipolygon (mpg MultiPolygon) ENGINE = Memory();
INSERT INTO geo_multipolygon VALUES([[[(0, 0), (10, 0), (10, 10), (0, 10)]], [[(20, 20), (50, 20), (50, 50), (20, 50)],[(30, 30), (50, 50), (50, 30)]]]);
SELECT mpg, toTypeName(mpg) FROM geo_multipolygon;
```

结果：

```text
┌─mpg─────────────────────────────────────────────────────────────────────────────────────────────┬─toTypeName(mpg)─┐
│ [[[(0,0),(10,0),(10,10),(0,10)]],[[(20,20),(50,20),(50,50),(20,50)],[(30,30),(50,50),(50,30)]]] │ MultiPolygon    │
└─────────────────────────────────────────────────────────────────────────────────────────────────┴─────────────────┘
```



## 21.7 Map

`Map(key, value)` 可以存储 `key:value` 键值对类型的数据。

**参数**

- `key` — 键值对的key，类型可以是：[String](https://clickhouse.com/docs/zh/sql-reference/data-types/string), [Integer](https://clickhouse.com/docs/zh/sql-reference/data-types/int-uint), [LowCardinality](https://clickhouse.com/docs/zh/sql-reference/data-types/lowcardinality), 或者 [FixedString](https://clickhouse.com/docs/zh/sql-reference/data-types/fixedstring).
- `value` — 键值对的value，类型可以是：[String](https://clickhouse.com/docs/zh/sql-reference/data-types/string), [Integer](https://clickhouse.com/docs/zh/sql-reference/data-types/int-uint), [Array](https://clickhouse.com/docs/zh/sql-reference/data-types/array), [LowCardinality](https://clickhouse.com/docs/zh/sql-reference/data-types/lowcardinality), 或者 [FixedString](https://clickhouse.com/docs/zh/sql-reference/data-types/fixedstring).

使用 `a['key']` 可以从 `a Map('key', 'value')` 类型的列中获取到对应的值，这是一个线性复杂度的查询。

**示例**

示例表:

```sql
CREATE TABLE table_map (a Map(String, UInt64)) ENGINE=Memory;
INSERT INTO table_map VALUES ({'key1':1, 'key2':10}), ({'key1':2,'key2':20}), ({'key1':3,'key2':30});
```



查询 `key2` 的所有值:

```sql
SELECT a['key2'] FROM table_map;
```



查询结果:

```text
┌─arrayElement(a, 'key2')─┐
│                      10 │
│                      20 │
│                      30 │
└─────────────────────────┘
```



如果在 `Map()` 类型的列中，查询的 `key` 值不存在，那么根据 `value` 的类型，查询结果将会是数字0，空字符串或者空数组。

```sql
INSERT INTO table_map VALUES ({'key3':100}), ({});
SELECT a['key3'] FROM table_map;
```



查询结果:

```text
┌─arrayElement(a, 'key3')─┐
│                     100 │
│                       0 │
└─────────────────────────┘
┌─arrayElement(a, 'key3')─┐
│                       0 │
│                       0 │
│                       0 │
└─────────────────────────┘
```

**将Tuple类型转换成Map类型**

您可以使用 [CAST](https://clickhouse.com/docs/zh/sql-reference/functions/type-conversion-functions#type_conversion_function-cast) 方法将 `Tuple()` 转换成 `Map()` ：

```sql
SELECT CAST(([1, 2, 3], ['Ready', 'Steady', 'Go']), 'Map(UInt8, String)') AS map;
```



```text
┌─map───────────────────────────┐
│ {1:'Ready',2:'Steady',3:'Go'} │
└───────────────────────────────┘
```

**Map.keys 和 Map.values 单独使用**

为了更好使用 `Map` 类型，在一定的场景下，可以单独使用 `keys` 或者 `values`，而不需要将整个列的数据都读取出来。

**示例**

查询:

```sql
CREATE TABLE t_map (`a` Map(String, UInt64)) ENGINE = Memory;
INSERT INTO t_map VALUES (map('key1', 1, 'key2', 2, 'key3', 3));
SELECT a.keys FROM t_map;
SELECT a.values FROM t_map;
```



结果:

```text
┌─a.keys─────────────────┐
│ ['key1','key2','key3'] │
└────────────────────────┘
┌─a.values─┐
│ [1,2,3]  │
└──────────┘
```



## 21.8 布尔-无

在此之前的版本，没有单独的类型来存储布尔值。可以使用 UInt8 类型，取值限制为 0 或 1。



## 21.9 精度浮点数

Decimal(P,S),Decimal32(S),Decimal64(S),Decimal128(S)

有符号的定点数，可在加、减和乘法运算过程中保持精度。对于除法，最低有效数字会被丢弃（不舍入）。

- P - 精度。有效范围：[1:38]，决定可以有多少个十进制数字（包括分数）。
- S - 规模。有效范围：[0：P]，决定数字的小数部分中包含的小数位数。

对于不同的 P 参数值 Decimal 表示，以下例子都是同义的： -P从[1:9]-对于Decimal32(S) -P从[10:18]-对于Decimal64(小号) -P从[19:38]-对于Decimal128（S)

**十进制值范围**

- Decimal32(S) - ( -1 * 10^(9 - S),1*10^(9-S) )
- Decimal64(S) - ( -1 * 10^(18 - S),1*10^(18-S) )
- Decimal128(S) - ( -1 * 10^(38 - S),1*10^(38-S) )

例如，Decimal32(4) 可以表示 -99999.9999 至 99999.9999 的数值，步长为0.0001。

**内部表示方式**

数据采用与自身位宽相同的有符号整数存储。这个数在内存中实际范围会高于上述范围，从 String 转换到十进制数的时候会做对应的检查。

由于现代CPU不支持128位数字，因此 Decimal128 上的操作由软件模拟。所以 Decimal128 的运算速度明显慢于 Decimal32/Decimal64。

**运算和结果类型**

对Decimal的二进制运算导致更宽的结果类型（无论参数的顺序如何）。

- `Decimal64(S1) <op> Decimal32(S2) -> Decimal64(S)`
- `Decimal128(S1) <op> Decimal32(S2) -> Decimal128(S)`
- `Decimal128(S1) <op> Decimal64(S2) -> Decimal128(S)`

精度变化的规则：

- 加法，减法：S = max(S1, S2)。
- 乘法：S = S1 + S2。
- 除法：S = S1。

对于 Decimal 和整数之间的类似操作，结果是与参数大小相同的十进制。

未定义Decimal和Float32/Float64之间的函数。要执行此类操作，您可以使用：toDecimal32、toDecimal64、toDecimal128 或 toFloat32，toFloat64，需要显式地转换其中一个参数。注意，结果将失去精度，类型转换是昂贵的操作。

Decimal上的一些函数返回结果为Float64（例如，var或stddev）。对于其中一些，中间计算发生在Decimal中。对于此类函数，尽管结果类型相同，但Float64和Decimal中相同数据的结果可能不同。

**溢出检查**

在对 Decimal 类型执行操作时，数值可能会发生溢出。分数中的过多数字被丢弃（不是舍入的）。整数中的过多数字将导致异常。

```text
SELECT toDecimal32(2, 4) AS x, x / 3

┌──────x─┬─divide(toDecimal32(2, 4), 3)─┐
│ 2.0000 │                       0.6666 │
└────────┴──────────────────────────────┘

SELECT toDecimal32(4.2, 8) AS x, x * x

DB::Exception: Scale is out of bounds.

SELECT toDecimal32(4.2, 8) AS x, 6 * x

DB::Exception: Decimal math overflow.
```



检查溢出会导致计算变慢。如果已知溢出不可能，则可以通过设置`decimal_check_overflow`来禁用溢出检查，在这种情况下，溢出将导致结果不正确：

```text
SET decimal_check_overflow = 0;
SELECT toDecimal32(4.2, 8) AS x, 6 * x

┌──────────x─┬─multiply(6, toDecimal32(4.2, 8))─┐
│ 4.20000000 │                     -17.74967296 │
└────────────┴──────────────────────────────────┘
```



溢出检查不仅发生在算术运算上，还发生在比较运算上：

```text
SELECT toDecimal32(1, 8) < 100

DB::Exception: Can't compare.
```

## 21.10 enum

包括 `Enum8` 和 `Enum16` 类型。`Enum` 保存 `'string'= integer` 的对应关系。在 ClickHouse 中，尽管用户使用的是字符串常量，但所有含有 `Enum` 数据类型的操作都是按照包含整数的值来执行。这在性能方面比使用 `String` 数据类型更有效。

- `Enum8` 用 `'String'= Int8` 对描述。
- `Enum16` 用 `'String'= Int16` 对描述。

**用法示例**

创建一个带有一个枚举 `Enum8('hello' = 1, 'world' = 2)` 类型的列：

```text
CREATE TABLE t_enum
(
    x Enum8('hello' = 1, 'world' = 2)
)
ENGINE = TinyLog
```



这个 `x` 列只能存储类型定义中列出的值：`'hello'`或`'world'`。如果您尝试保存任何其他值，ClickHouse 抛出异常。

```sql
INSERT INTO t_enum VALUES ('hello'), ('world'), ('hello')
```



```response
Ok.

3 rows in set. Elapsed: 0.002 sec.
```



```sql
INSERT INTO t_enum VALUES('a')
```



```response
Exception on client:
Code: 49. DB::Exception: Unknown element 'a' for type Enum8('hello' = 1, 'world' = 2)
```



当您从表中查询数据时，ClickHouse 从 `Enum` 中输出字符串值。

```sql
SELECT * FROM t_enum
```



```response
┌─x─────┐
│ hello │
│ world │
│ hello │
└───────┘
```

如果需要看到对应行的数值，则必须将 `Enum` 值转换为整数类型。

```sql
SELECT CAST(x, 'Int8') FROM t_enum
```



```response
┌─CAST(x, 'Int8')─┐
│               1 │
│               2 │
│               1 │
└─────────────────┘
```

在查询中创建枚举值，您还需要使用 `CAST`。

```sql
SELECT toTypeName(CAST('a', 'Enum8(\'a\' = 1, \'b\' = 2)'))
```



```response
┌─toTypeName(CAST('a', 'Enum8(\'a\' = 1, \'b\' = 2)'))─┐
│ Enum8('a' = 1, 'b' = 2)                              │
└──────────────────────────────────────────────────────┘
```

**规则及用法**

`Enum8` 类型的每个值范围是 `-128 ... 127`，`Enum16` 类型的每个值范围是 `-32768 ... 32767`。所有的字符串或者数字都必须是不一样的。允许存在空字符串。如果某个 Enum 类型被指定了（在表定义的时候），数字可以是任意顺序。然而，顺序并不重要。

`Enum` 中的字符串和数值都不能是 [NULL](https://clickhouse.com/docs/zh/sql-reference/data-types/enum)。

`Enum` 包含在 [可为空](https://clickhouse.com/docs/zh/sql-reference/data-types/nullable) 类型中。因此，如果您使用此查询创建一个表

```sql
CREATE TABLE t_enum_nullable
(
    x Nullable( Enum8('hello' = 1, 'world' = 2) )
)
ENGINE = TinyLog
```



不仅可以存储 `'hello'` 和 `'world'` ，还可以存储 `NULL`。

```sql
INSERT INTO t_enum_nullable Values('hello'),('world'),(NULL)
```



在内存中，`Enum` 列的存储方式与相应数值的 `Int8` 或 `Int16` 相同。

当以文本方式读取的时候，ClickHouse 将值解析成字符串然后去枚举值的集合中搜索对应字符串。如果没有找到，会抛出异常。当读取文本格式的时候，会根据读取到的字符串去找对应的数值。如果没有找到，会抛出异常。

当以文本形式写入时，ClickHouse 将值解析成字符串写入。如果列数据包含垃圾数据（不是来自有效集合的数字），则抛出异常。Enum 类型以二进制读取和写入的方式与 `Int8` 和 `Int16` 类型一样的。

隐式默认值是数值最小的值。

在 `ORDER BY`，`GROUP BY`，`IN`，`DISTINCT` 等等中，Enum 的行为与相应的数字相同。例如，按数字排序。对于等式运算符和比较运算符，Enum 的工作机制与它们在底层数值上的工作机制相同。

枚举值不能与数字进行比较。枚举可以与常量字符串进行比较。如果与之比较的字符串不是有效Enum值，则将引发异常。可以使用 IN 运算符来判断一个 Enum 是否存在于某个 Enum 集合中，其中集合中的 Enum 需要用字符串表示。

大多数具有数字和字符串的运算并不适用于Enums；例如，Enum 类型不能和一个数值相加。但是，Enum有一个原生的 `toString` 函数，它返回它的字符串值。

Enum 值使用 `toT` 函数可以转换成数值类型，其中 T 是一个数值类型。若 `T` 恰好对应 Enum 的底层数值类型，这个转换是零消耗的。

Enum 类型可以被 `ALTER` 无成本地修改对应集合的值。可以通过 `ALTER` 操作来增加或删除 Enum 的成员（只要表没有用到该值，删除都是安全的）。作为安全保障，改变之前使用过的 Enum 成员将抛出异常。

通过 `ALTER` 操作，可以将 `Enum8` 转成 `Enum16`，反之亦然，就像 `Int8` 转 `Int16`一样。



## 21.11 固定字符串

固定长度 N 的字符串（N 必须是严格的正自然数）。

您可以使用下面的语法对列声明为`FixedString`类型：

```sql
<column_name> FixedString(N)
```



其中`N`表示自然数。

**当数据的长度恰好为N个字节时，`FixedString`类型是高效的。 在其他情况下，这可能会降低效率。**

可以有效存储在`FixedString`类型的列中的值的示例：

- 二进制表示的IP地址（IPv6使用`FixedString(16)`）
- 语言代码（ru_RU, en_US … ）
- 货币代码（USD, RUB … ）
- 二进制表示的哈希值（MD5使用`FixedString(16)`，SHA256使用`FixedString(32)`）

请使用[UUID](https://clickhouse.com/docs/zh/sql-reference/data-types/uuid)数据类型来存储UUID值，。

当向ClickHouse中插入数据时,

- 如果字符串包含的字节数少于`N’,将对字符串末尾进行空字节填充。
- 如果字符串包含的字节数大于`N`,将抛出`Too large value for FixedString(N)`异常。

当做数据查询时，ClickHouse不会删除字符串末尾的空字节。 如果使用`WHERE`子句，则须要手动添加空字节以匹配`FixedString`的值。 以下示例阐明了如何将`WHERE`子句与`FixedString`一起使用。

考虑带有`FixedString（2)`列的表：

```text
┌─name──┐
│ b     │
└───────┘
```



查询语句`SELECT * FROM FixedStringTable WHERE a = 'b'` 不会返回任何结果。请使用空字节来填充筛选条件。

```sql
SELECT * FROM FixedStringTable
WHERE a = 'b\0'
```



```text
┌─a─┐
│ b │
└───┘
```



这种方式与MySQL的`CHAR`类型的方式不同（MySQL中使用空格填充字符串，并在输出时删除空格）。

请注意，`FixedString(N)`的长度是个常量。仅由空字符组成的字符串，函数[length](https://clickhouse.com/docs/zh/sql-reference/functions/array-functions#array_functions-length)返回值为`N`,而函数[empty](https://clickhouse.com/docs/zh/sql-reference/functions/string-functions#empty)的返回值为`1`。

## 21.12  float

类型与以下 C 语言中类型是相同的：

- `Float32` - `float`
- `Float64` - `double`

我们建议您尽可能以整数形式存储数据。例如，将固定精度的数字转换为整数值，例如货币数量或页面加载时间用毫秒为单位表示

使用浮点数

对浮点数进行计算可能引起四舍五入的误差。

```sql
SELECT 1 - 0.9
```



```text
┌───────minus(1, 0.9)─┐
│ 0.09999999999999998 │
└─────────────────────┘
```



- 计算的结果取决于计算方法（计算机系统的处理器类型和体系结构）
- 浮点计算结果可能是诸如无穷大（`INF`）和«非数字»（`NaN`）。对浮点数计算的时候应该考虑到这点。
- 当一行行阅读浮点数的时候，浮点数的结果可能不是机器最近显示的数值。

NaN和Inf与标准SQL相比，ClickHouse 支持以下类别的浮点数：

- `Inf` – 正无穷

```sql
SELECT 0.5 / 0
```



```text
┌─divide(0.5, 0)─┐
│            inf │
└────────────────┘
```



- `-Inf` – 负无穷

```sql
SELECT -0.5 / 0
```



```text
┌─divide(-0.5, 0)─┐
│            -inf │
└─────────────────┘
```



- `NaN` – 非数字

```text
SELECT 0 / 0

┌─divide(0, 0)─┐
│          nan │
└──────────────┘
```

可以在 [ORDER BY 子句](https://clickhouse.com/docs/zh/sql-reference/data-types/float) 查看更多关于 `NaN` 排序的规则。



## 21.13 IP

### 21.13.1 IPv4

`IPv4`是与`UInt32`类型保持二进制兼容的Domain类型，其用于存储IPv4地址的值。它提供了更为紧凑的二进制存储的同时支持识别可读性更加友好的输入输出格式。

基本使用

```sql
CREATE TABLE hits (url String, from IPv4) ENGINE = MergeTree() ORDER BY url;

DESCRIBE TABLE hits;
┌─name─┬─type───┬─default_type─┬─default_expression─┬─comment─┬─codec_expression─┐
│ url  │ String │              │                    │         │                  │
│ from │ IPv4   │              │                    │         │                  │
└──────┴────────┴──────────────┴────────────────────┴─────────┴──────────────────┘

# 同时您也可以使用`IPv4`类型的列作为主键：
CREATE TABLE hits (url String, from IPv4) ENGINE = MergeTree() ORDER BY from;

#在写入与查询时，`IPv4`类型能够识别可读性更加友好的输入输出格式：
INSERT INTO hits (url, from) VALUES ('https://wikipedia.org', '116.253.40.133')('https://clickhouse.com', '183.247.232.58')('https://clickhouse.com/docs/en/', '116.106.34.242');

SELECT * FROM hits;
┌─url────────────────────────────────┬───────────from─┐
│ https://clickhouse.com/docs/en/ │ 116.106.34.242 │
│ https://wikipedia.org              │ 116.253.40.133 │
│ https://clickhouse.com          │ 183.247.232.58 │
└────────────────────────────────────┴────────────────┘

同时它提供更为紧凑的二进制存储格式：
SELECT toTypeName(from), hex(from) FROM hits LIMIT 1;

┌─toTypeName(from)─┬─hex(from)─┐
│ IPv4             │ B7F7E83A  │
└──────────────────┴───────────┘


# 不可隐式转换为除`UInt32`以外的其他类型类型。如果要将`IPv4`类型的值转换成字符串，你可以使用`IPv4NumToString()`显示的进行转换：

SELECT toTypeName(s), IPv4NumToString(from) as s FROM hits LIMIT 1;

┌─toTypeName(IPv4NumToString(from))─┬─s──────────────┐
│ String                            │ 183.247.232.58 │
└───────────────────────────────────┴────────────────┘


或可以使用`CAST`将它转换为`UInt32`类型:
SELECT toTypeName(i), CAST(from as UInt32) as i FROM hits LIMIT 1;
┌─toTypeName(CAST(from, 'UInt32'))─┬──────────i─┐
│ UInt32                           │ 3086477370 │
└──────────────────────────────────┴────────────┘
```



### 21.13.2 IPv6

`IPv6`是与`FixedString(16)`类型保持二进制兼容的Domain类型，其用于存储IPv6地址的值。它提供了更为紧凑的二进制存储的同时支持识别可读性更加友好的输入输出格式。

基本用法

```sql
CREATE TABLE hits (url String, from IPv6) ENGINE = MergeTree() ORDER BY url;

DESCRIBE TABLE hits;
```



```text
┌─name─┬─type───┬─default_type─┬─default_expression─┬─comment─┬─codec_expression─┐
│ url  │ String │              │                    │         │                  │
│ from │ IPv6   │              │                    │         │                  │
└──────┴────────┴──────────────┴────────────────────┴─────────┴──────────────────┘
```



同时您也可以使用`IPv6`类型的列作为主键：

```sql
CREATE TABLE hits (url String, from IPv6) ENGINE = MergeTree() ORDER BY from;
```

在写入与查询时，`IPv6`类型能够识别可读性更加友好的输入输出格式：

```sql
INSERT INTO hits (url, from) VALUES ('https://wikipedia.org', '2a02:aa08:e000:3100::2')('https://clickhouse.com', '2001:44c8:129:2632:33:0:252:2')('https://clickhouse.com/docs/en/', '2a02:e980:1e::1');

SELECT * FROM hits;
```



```text
┌─url────────────────────────────────┬─from──────────────────────────┐
│ https://clickhouse.com          │ 2001:44c8:129:2632:33:0:252:2 │
│ https://clickhouse.com/docs/en/ │ 2a02:e980:1e::1               │
│ https://wikipedia.org              │ 2a02:aa08:e000:3100::2        │
└────────────────────────────────────┴───────────────────────────────┘
```

同时它提供更为紧凑的二进制存储格式：

```sql
SELECT toTypeName(from), hex(from) FROM hits LIMIT 1;
```

```text
┌─toTypeName(from)─┬─hex(from)────────────────────────┐
│ IPv6             │ 200144C8012926320033000002520002 │
└──────────────────┴──────────────────────────────────┘
```

不可隐式转换为除`FixedString(16)`以外的其他类型类型。如果要将`IPv6`类型的值转换成字符串，你可以使用`IPv6NumToString()`显示的进行转换：

```sql
SELECT toTypeName(s), IPv6NumToString(from) as s FROM hits LIMIT 1;
```



```text
┌─toTypeName(IPv6NumToString(from))─┬─s─────────────────────────────┐
│ String                            │ 2001:44c8:129:2632:33:0:252:2 │
└───────────────────────────────────┴───────────────────────────────┘
```

或使用`CAST`将其转换为`FixedString(16)`：

```sql
SELECT toTypeName(i), CAST(from as FixedString(16)) as i FROM hits LIMIT 1;
```

```text
┌─toTypeName(CAST(from, 'FixedString(16)'))─┬─i───────┐
│ FixedString(16)                           │  ��� │
└───────────────────────────────────────────┴─────────┘
```



## 21.14 嵌套数据结构

嵌套数据结构类似于嵌套表。嵌套数据结构的参数（列名和类型）与 CREATE 查询类似。每个表可以包含任意多行嵌套数据结构。

示例:

```sql
CREATE TABLE test.visits
(
    CounterID UInt32,
    StartDate Date,
    Sign Int8,
    IsNew UInt8,
    VisitID UInt64,
    UserID UInt64,
    ...
    Goals Nested
    (
        ID UInt32,
        Serial UInt32,
        EventTime DateTime,
        Price Int64,
        OrderID String,
        CurrencyID UInt32
    ),
    ...
) ENGINE = CollapsingMergeTree(StartDate, intHash32(UserID), (CounterID, StartDate, intHash32(UserID), VisitID), 8192, Sign)
```



上述示例声明了 `Goals` 这种嵌套数据结构，它包含访客转化相关的数据（访客达到的目标）。在 ‘visits’ 表中每一行都可以对应零个或者任意个转化数据。

只支持一级嵌套。嵌套结构的列中，若列的类型是数组类型，那么该列其实和多维数组是相同的，所以目前嵌套层级的支持很局限（MergeTree 引擎中不支持存储这样的列）

大多数情况下，处理嵌套数据结构时，会指定一个单独的列。为了这样实现，列的名称会与点号连接起来。这些列构成了一组匹配类型。在同一条嵌套数据中，所有的列都具有相同的长度。

示例:

```sql
SELECT
    Goals.ID,
    Goals.EventTime
FROM test.visits
WHERE CounterID = 101500 AND length(Goals.ID) < 5
LIMIT 10
```



```text
┌─Goals.ID───────────────────────┬─Goals.EventTime───────────────────────────────────────────────────────────────────────────┐
│ [1073752,591325,591325]        │ ['2014-03-17 16:38:10','2014-03-17 16:38:48','2014-03-17 16:42:27']                       │
│ [1073752]                      │ ['2014-03-17 00:28:25']                                                                   │
│ [1073752]                      │ ['2014-03-17 10:46:20']                                                                   │
│ [1073752,591325,591325,591325] │ ['2014-03-17 13:59:20','2014-03-17 22:17:55','2014-03-17 22:18:07','2014-03-17 22:18:51'] │
│ []                             │ []                                                                                        │
│ [1073752,591325,591325]        │ ['2014-03-17 11:37:06','2014-03-17 14:07:47','2014-03-17 14:36:21']                       │
│ []                             │ []                                                                                        │
│ []                             │ []                                                                                        │
│ [591325,1073752]               │ ['2014-03-17 00:46:05','2014-03-17 00:46:05']                                             │
│ [1073752,591325,591325,591325] │ ['2014-03-17 13:28:33','2014-03-17 13:30:26','2014-03-17 18:51:21','2014-03-17 18:51:45'] │
└────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────┘
```



所以可以简单地把嵌套数据结构当做是所有列都是相同长度的多列数组。

SELECT 查询只有在使用 ARRAY JOIN 的时候才可以指定整个嵌套数据结构的名称。更多信息，参考 «ARRAY JOIN 子句»。示例：

```sql
SELECT
    Goal.ID,
    Goal.EventTime
FROM test.visits
ARRAY JOIN Goals AS Goal
WHERE CounterID = 101500 AND length(Goals.ID) < 5
LIMIT 10
```



```text
┌─Goal.ID─┬──────Goal.EventTime─┐
│ 1073752 │ 2014-03-17 16:38:10 │
│  591325 │ 2014-03-17 16:38:48 │
│  591325 │ 2014-03-17 16:42:27 │
│ 1073752 │ 2014-03-17 00:28:25 │
│ 1073752 │ 2014-03-17 10:46:20 │
│ 1073752 │ 2014-03-17 13:59:20 │
│  591325 │ 2014-03-17 22:17:55 │
│  591325 │ 2014-03-17 22:18:07 │
│  591325 │ 2014-03-17 22:18:51 │
│ 1073752 │ 2014-03-17 11:37:06 │
└─────────┴─────────────────────┘
```



不能对整个嵌套数据结构执行 SELECT。只能明确列出属于它一部分列。

对于 INSERT 查询，可以单独地传入所有嵌套数据结构中的列数组（假如它们是单独的列数组）。在插入过程中，系统会检查它们是否有相同的长度。

对于 DESCRIBE 查询，嵌套数据结构中的列会以相同的方式分别列出来。

ALTER 查询对嵌套数据结构的操作非常有限。



## 21.15 空

允许用特殊标记 ([NULL](https://clickhouse.com/docs/zh/sql-reference/data-types/nullable)) 表示«缺失值»，可以与 `TypeName` 的正常值存放一起。例如，`Nullable(Int8)` 类型的列可以存储 `Int8` 类型值，而没有值的行将存储 `NULL`。

对于 `TypeName`，不能使用复合数据类型 [阵列](https://clickhouse.com/docs/zh/sql-reference/data-types/array) 和 [元组](https://clickhouse.com/docs/zh/sql-reference/data-types/tuple)。复合数据类型可以包含 `Nullable` 类型值，例如`Array(Nullable(Int8))`。

`Nullable` 类型字段不能包含在表索引中。

除非在 ClickHouse 服务器配置中另有说明，否则 `NULL` 是任何 `Nullable` 类型的默认值。

**存储特性**

要在表的列中存储 `Nullable` 类型值，ClickHouse 除了使用带有值的普通文件外，还使用带有 `NULL` 掩码的单独文件。 掩码文件中的条目允许 ClickHouse 区分每个表行的 `NULL` 和相应数据类型的默认值。 由于附加了新文件，`Nullable` 列与类似的普通文件相比消耗额外的存储空间。

**!!! 注意点 "注意点" 使用 `Nullable` 几乎总是对性能产生负面影响，在设计数据库时请记住这一点**

掩码文件中的条目允许ClickHouse区分每个表行的对应数据类型的«NULL»和默认值由于有额外的文件，«Nullable»列比普通列消耗更多的存储空间

**用法示例**

```sql
CREATE TABLE t_null(x Int8, y Nullable(Int8)) ENGINE TinyLog
INSERT INTO t_null VALUES (1, NULL), (2, 3)
SELECT x + y FROM t_null
┌─plus(x, y)─┐
│       ᴺᵁᴸᴸ │
│          5 │
└────────────┘
```



## 21.16 字符串

字符串可以任意长度的。它可以包含任意的字节集，包含空字节。因此，字符串类型可以代替其他 DBMSs 中的 VARCHAR、BLOB、CLOB 等类型。

编码

ClickHouse 没有编码的概念。字符串可以是任意的字节集，按它们原本的方式进行存储和输出。 若需存储文本，我们建议使用 UTF-8 编码。至少，如果你的终端使用UTF-8（推荐），这样读写就不需要进行任何的转换了。 同样，对不同的编码文本 ClickHouse 会有不同处理字符串的函数。 比如，`length` 函数可以计算字符串包含的字节数组的长度，然而 `lengthUTF8` 函数是假设字符串以 UTF-8 编码，计算的是字符串包含的 Unicode 字符的长度。

# 二十二、运算符

该 `IN`, `NOT IN`, `GLOBAL IN`，和 `GLOBAL NOT IN` 运算符是单独考虑的，因为它们的功能相当丰富。

`IN` 运算符假定运算的结果 [NULL](https://clickhouse.com/docs/zh/sql-reference/syntax#null-literal) 总是等于 `0`，无论是否 `NULL` 位于操作员的右侧或左侧。 `NULL` 值不包含在任何数据集中，彼此不对应，并且在以下情况下无法进行比较 [transform_null_in=0](https://clickhouse.com/docs/zh/operations/settings/settings#transform_null_in).

下面是一个例子 `t_null` 表:

```text
┌─x─┬────y─┐
│ 1 │ ᴺᵁᴸᴸ │
│ 2 │    3 │
└───┴──────┘
```

运行查询 `SELECT x FROM t_null WHERE y IN (NULL,3)` 为您提供以下结果:

```text
┌─x─┐
│ 2 │
└───┘
```

## 22.1 分布式子查询

带子查询的IN-s有两个选项（类似于连接）：normal `IN` / `JOIN` 和 `GLOBAL IN` / `GLOBAL JOIN`. 它们在分布式查询处理的运行方式上有所不同。

使用时 `GLOBAL IN` / `GLOBAL JOINs`，首先所有的子查询都运行 `GLOBAL IN` / `GLOBAL JOINs`，并将结果收集在临时表中。 然后将临时表发送到每个远程服务器，其中使用此临时数据运行查询。

对于非分布式查询，请使用常规 `IN` / `JOIN`.



# 二十三、 golang 客户端

推荐使用gorm 底层也是clickhouse-go

或者直接使用clickhouse-go 自己写sql

clickhouse-go 性能更好些

[-](https://github.com/ClickHouse/clickhouse-go)

```
package main

import (
	"fmt"
	ck "gorm.io/driver/clickhouse"
	"gorm.io/gorm"
	"time"
)

type TVersionCollapsingMt struct {
	ID         uint8  `json:"id" gorm:"column:id"`
	Name       string `json:"name" gorm:"column:name"`
	Loc        string `json:"loc" gorm:"column:loc"`
	LoginTimes string `json:"login_times" gorm:"column:login_times"`
	TotalDur   uint8  `json:"total_dur" gorm:"column:total_dur"`
	Sign       int8   `json:"sign" gorm:"column:sign"`
	Version    uint8  `json:"version" gorm:"column:version"`
}

func (m *TVersionCollapsingMt) TableName() string {
	return "t_version_collapsing_mt"
}

func main() {
	//ck_go()
	gorms()
}

func gorms() {
	dsn := "clickhouse://admin:123456@0.0.0.0:9090,0.0.0.0:8123/test?dial_timeout=200ms&max_execution_time=60s&debug=true"
	db, err := gorm.Open(ck.Open(dsn), &gorm.Config{})
	if err != nil {
		fmt.Println(err)
		panic("failed to connect database")
	}

	cl, err := db.DB()
	cl.SetConnMaxLifetime(3 * time.Hour)
	cl.SetMaxIdleConns(3)
	cl.SetMaxOpenConns(5)

	// Auto Migrate
	//db.AutoMigrate(&User{})
	// Set table options
	//db.Set("gorm:table_options", "ENGINE=Distributed(cluster, default, hits)").AutoMigrate(&User{})

	// Set table cluster options
	//db.Set("gorm:table_cluster_options", "on cluster default").AutoMigrate(&User{})

	// Insert
	//db.Create(&User{Name: "Angeliz", Age: 18})

	var data = []*TVersionCollapsingMt{}
	// Select
	if err = db.
		Model(&TVersionCollapsingMt{}).Where("id > ?", 0).
		Find(&data).Error; nil != err {
		fmt.Println(err)
	}

	fmt.Println(data[0])
	//// Batch Insert
	//user1 := User{Age: 12, Name: "Bruce Lee"}
	//user2 := User{Age: 13, Name: "Feynman"}
	//user3 := User{Age: 14, Name: "Angeliz"}
	//var users = []User{user1, user2, user3}
	//db.Create(&users)
	// ...
}

```























































































