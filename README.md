jSQL
====

a SQL like database using javascript, also has a syntax similar wrapper for python(tornado).

version: v0.5.x

### When Should I Use jSQL?

本篇所提及的jSQL均指的JavaScript版本的jSQL，jSQL(JavaScript)一般擅长于如下应用场景：

- 前端一次性从后端拉取所有数据，数据的排序、筛选、查询均由前端实现
- 前端定期的从服务端请求数据，并对已得到的数据做排序、筛选查询展示

jSQL不擅长如下应用场景：

- 前端第一次只获取第一页的数据，每次分页都需要想后端进行接口请求
- 数据量虽然一次请求但是数据量巨大（实际情况中应该不常遇到）

### Why Should I Use jSQL?

使用jSQL可以让你更加专注于业务需求和逻辑，通过jSQL可以将排序、筛选等数据查询的底层操作同业务逻辑完美分离。从而提高代码的可维护性和开发效率。jSQL不仅可以用于PC端，也可以在移动手持设备的应用中使用。除了JavaScript版本的jSQL，如果你喜欢jSQL的使用方式和API，同时你又是一个Python后端开发人员且不喜欢庞大的ORM，同样有短小精干的Python-jSQL可以让你在Python框架中方便的对MySQL进行操作。jSQL不是一个库，它是一个使得数据的查询操作变得更方便快捷高效的解决思路和思想的抽象。

## API Document

jSQL是一个用SQL的思想和API实现JavaScript数据操作（查询，过滤）的方法库（同时有基于Tornado DB封装的类似API的后端SQL查询封装），它通过链式调用的方式简化数据查询和操作，同时支持两种形式的数据源。

- `Object`
- `Array`

Object类型：

	{
		key: {column: value},
		key: {column: value}
	}

Object的key对应数据表的唯一索引，cloumn映射为数据表的字段，value对应数据表相应字段的值。

Array类型：

	[
		{column: value},
		{column: value}
	]

Array的每个item对应数据表的一个记录，cloumn对应字段名，value对应字段值，同Object类型不同的是，索引key是根据指定的条件或者自动生成的。

`create(dbname, db /*, indexList */)`

在内存中创建一个数据库（指定indexList则根据指定字段生成唯一键）

`use(dbname)`

使用指定数据库作为当前操作的数据库，并重置缓冲区(buffer)

`drop(dbname)`

丢弃指定数据库，同时从内存中抹除

`dbs()`

获取当前内存中存在的所有数据库名称

`db()`

获取当前操作的数据库名称

`select(key)`

指定获取的数据字段名称

`count()`

获取当前操作缓冲区的数据条数

`total(scope|function)`

返回当前操作数据库中含有某字段（或者符合传入条件）的数据总条数。

`orderby(field, /* callback, */ order)`

根据指定字段的值（或者处理后的值）和排序规则对当前缓冲区的数据进行排序。

`where(fn|[fn, …])`

返回当前缓冲区中符合指定条件的数据集合（如果传入为一组fn，则为并集）。

`iterate(fn)`

根据指定的方法对当前缓冲区中的数据进行遍历。

`findAll(fn)`

以Object类型返回当前缓冲区中的数据集合

`find(key)`

获取缓冲区中指定key的数据。

`listAll()`

以Array类型返回当前缓冲区中的数据集合。

`update(key, data)`

根据指定数据对指定key的数据进行更新操作。

`insert(item, key /*, from index */)`

（在指定位置）插入一条数据。

`save(data)`

向当前操作的数据库中追加一条数据。

`saveAll(data)`

强当前操作的数据库中追加一批数据。

`delete()`

从当前操作的数据库中删除缓冲区中的数据。

`limit(start, end)`

返回当前缓冲区指定区域的数据（也可以使用`start:end`）。

`keys()`

返回当前缓冲区中所有数据的键名集合。

`first(fn)`

返回满足条件的第一条数据。

`last(fn)`

返回满足条件的最后一条数据。

`rebase()`

重置缓冲区数据。

`noConflict()`

避免命名空间冲突。

### 闭合操作

所谓闭合操作即执行到此将重置缓冲区，是链式查询的最终状态。

- `count()`
- `find()`
- `findAll()`
- `listAll()`
- `insert()`

除此之外，`use()`也会触发缓冲区重置的操作。

### TODO

- 数据的追加和删除操作。
- 索引的实现和优化。
- 性能测试和优化。
- 功能测试和优化。
- 单元测试。
- 封版应用到列表页实践。