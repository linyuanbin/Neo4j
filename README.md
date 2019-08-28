# Neo4j
[Neo4J官网](https://neo4j.com/)
# 首先
* Neo4j是一个世界领先的开源**图形数据库**。 它是由Neo技术使用Java语言完全开发的。
（图数据库的数据存储其实可以看成都是以**节点**（Node）和**边**（relationship）的形式存在的）

# 为什么要选择图数据库
* 简单地说，我们可以说图数据库主要用于存储更多的连接数据。处理复杂节点之间的关联关系。
（例如：类似于Facebook这样一个社交网络，存在的复杂的人物社交关系圈）

# Neo4j的特点
* SQL就像简单的查询语言Neo4j CQL
* 它遵循属性图数据模型
* 它通过使用Apache Lucence支持索引
* 它支持UNIQUE约束
* 它它包含一个用于执行CQL命令的UI：Neo4j数据浏览器
* 它支持完整的ACID（原子性，一致性，隔离性和持久性）规则
* 它采用原生图形库与本地GPE（图形处理引擎）
* 它支持查询的数据导出到JSON和XLS格式
* 它提供了REST API，可以被任何编程语言（如Java，Spring，Scala等）访问
* 它提供了可以通过任何UI MVC框架（如Node JS）访问的Java脚本
* 它支持两种Java API：Cypher API和Native Java API来开发Java应用程序

# Neo4j的优点
* 它很容易表示连接的数据
* 检索/遍历/导航更多的连接数据是非常容易和快速的
* 它非常容易地表示半结构化数据
* Neo4j CQL查询语言命令是人性化的可读格式，非常容易学习
* 它使用简单而强大的数据模型
* 它不需要复杂的连接来检索连接的/相关的数据，因为它很容易检索它的相邻节点或关系细节没有连接或索引

## *属性图模型规则*
* 表示节点，关系和属性中的数据
* 节点和关系都包含属性
* 关系连接节点
* 属性是键值对（K:V）
* 节点用圆圈表示，关系用方向键表示。(在创建关系时是必须指名方向的，图数据库是有向图)
* 关系具有方向：单向和双向。
* 每个关系包含“开始节点”或“从节点”和“到节点”或“结束节点”

![结构图](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEb3677347e7cd4e969ab589a6da7156c6/724 '结构图')

![节点属性图](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEa249227c4c657732f448f59fcf5523ba/356 '节点属性图')


![节点关系图](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCE39eda98631ea3e16cd30c0208bee6067/358 '节点关系图')


## 上手
#### 环境搭建
* 环境搭建 http://www.neo4j.org/download  按照指引完成安装
        （访问：http：// localhost：7474 /  登录就可以看到Neo4j UI 界面）

#### Neo4j图数据库主要有以下构建块 ：
* 节点
* 属性
* 关系
* 标签
* 数据浏览器

创建节点时，节点需要一个节点名称（nodeName），同时可以有多个标签（labelName），节点名称是作为数据结构存储用的命名，标签名称才是在CQL查询中用的名称（可以看成表名，但是Neo4J中没有表的概念）

## 操作Neo4J (Neo4j CQL)
#### 1.创建一个节点：

```
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```

<div align=right><img src="https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEf015a4a63b2cd1b031901ba57ebc7fd0/385"></div>

![CQL操作](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEdcca4b342e18c344f80d486f14321af7/393)

#### 2.查询

Match 和return的组合语句来查询并返回指定的结果；

'Match(n:EmpP) where ....  return n.name limit 10'

##### 这里需要**注意**几点：
1. MATCH先指定需要建立关系的节点
2. CREATE创建关系时是有一个方向箭头的，必须指定从一个节点指向另一个节点，否则报错
3. 创建节点时是先节点名称后标签名称，但是关系创建这里是先标签名称后节点名称

#### 3.创建关系
节点的操作基本都比较书面化，基本一看就会，下面看看如何创建关系（边的创建）

```
MATCH (x:<node1-label-name>),(y:<node2-label-name>)
WHERE x.name = 'A' AND y.name = 'B'
CREATE  
	(x)-[r:<relationship-label-name>]->(y)
RETURN r
```
#### 4.关系的查询
```
MATCH (neo)-[:PARTNER]-(partner)
等同于
MATCH (neo)-[:PARTNER]->(partner) and MATCH (neo)<-[:PARTNER]-(partner)
```

#### 5.WITH关键字
```
match (na:company)-[re]->(nb:company) where na.id = '12399145' WITH na,re,nb match (nb:company)-[re2]->(nc:company) return na,re,nb,re2,nc
可以总结为With可以将前面查询的结果作为后面查询的条件了使用
```
* 其他基本CQL基本大同小异
```
基本命令总结：
1.match子句:
MATCH 
(
   <node-name>:<label-name>
)

2.return 子句：
RETURN 
   <node-name>.<property1-name>,
   ........
   <node-name>.<propertyn-name>

3.查询语句：
MATCH (dept: Dept)
RETURN dept.deptno,dept.dname //dept看成是一个别名可以随意取，Dept是创建时的标签名

4.创建包含多个标签的节点：
CREATE (m:Movie:Cinema:Film:Picture) //Movie:Cinema:Film:Picture 都是标签名

5.Where子句使用
MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
RETURN emp

6.Delete子句（delete删除的是节点和关系）
MATCH (e: Employee) DELETE e //删除节点
<!--DELETE <node1-name>,<node2-name>,<relationship-name> 删除节点以及关联节点和关系-->
MATCH (cc: CreditCard)-[rel]-(c:Customer) 
DELETE cc,c,rel

7.remove删除（删除的是节点和关系的标签以及节点和关系的属性）
MATCH (dc:DebitCard) 
REMOVE dc.cvv
RETURN dc

8.SET子句（1.向现有节点或关系添加新属性 2.添加或更新属性值）
MATCH (dc:DebitCard)
SET dc.atm_pin = 3456
RETURN dc

9.Sorting排序
MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno
ORDER BY emp.name DEAC

10.UNION子句
MATCH (cc:CreditCard)
RETURN cc.id as id,cc.number as number,cc.name as name,
   cc.valid_from as valid_from,cc.valid_to as valid_to
UNION
MATCH (dc:DebitCard)
RETURN dc.id as id,dc.number as number,dc.name as name,
   dc.valid_from as valid_from,dc.valid_to as valid_to
如果用UNION ALL则不会进行结果去重

11.LIMIT和SKIP子句
MATCH (emp:Employee)
RETURN emp
LIMIT 2

MATCH (emp:Employee)
RETURN emp
SKIP 2



12.索引
CREATE INDEX ON :Person(firstname) //如果一个节点有Label("Person")并且有Property(“firstname”)，那么这个Node会被添加到index中
//DROP INDEX ON :Person(firstname)
CREATE INDEX ON :Person(age, country)
//DROP INDEX ON :Person(age, country)

//为Label名称为”Movie“以及”Book“的Nodes的Property(“title”)及Property(“description”)创建全文索引，索引名称为”titlesAndDescriptions“。
CALL db.index.fulltext.createNodeIndex("titlesAndDescriptions",["Movie", "Book"],["title", "description"])
```

#### 索引的使用：
```
单属性索引也自动用于WHERE子句中索引属性的不等（范围）比较。复合索引目前无法支持范围比较
MATCH (person:Person)
WHERE person.firstname > 'B'
RETURN person

在IN对谓词person.age和person.country下面的查询将使用综合指数Person(age, country)，如果它存在
MATCH (person:Person)
WHERE person.age IN [10, 20, 35] AND person.country IN ['Sweden', 'USA', 'UK']
RETURN person

以下查询中的STARTS WITH谓词person.firstname将使用Person(firstname)索引（如果存在）。复合索引目前无法支持STARTS WITH。
MATCH (person:Person)
WHERE person.firstname STARTS WITH 'And'
RETURN person

以下查询中的ENDS WITH谓词person.firstname将使用Person(firstname)索引（如果存在）。Person(firstname)将搜索存储在索引中的所有值，
并'hn'返回以结尾的条目。这意味着，虽然搜索将不使用进行优化，以查询的程度=，IN，>，<或STARTS WITH，它仍比在第一位置不使用索引更快。复合索引目前无法支持ENDS WITH。
MATCH (person:Person)
WHERE person.firstname ENDS WITH 'hn'
RETURN person

以下查询中的CONTAINS谓词person.firstname将使用Person(firstname)索引（如果存在）。Person(firstname)将搜索存储在索引中的所有值，并'h'返回包含的条目。
这意味着，虽然搜索将不使用进行优化，以查询的程度=，IN，>，<或STARTS WITH，它仍比在第一位置不使用索引更快。复合索引目前无法支持CONTAINS。
MATCH (person:Person)
WHERE person.firstname CONTAINS 'h'
RETURN person

如果索引具有点值的属性，则索引将用于空间距离搜索以及范围查询。
MATCH (p:Person)
WHERE distance(p.location, point({ x: 1, y: 2 }))< 2
RETURN p.location
即使对于2D和3D空间Point类型，在有界范围上进行索引搜索的能力也是有效的。
MATCH (person:Person)
WHERE point({ x: 1, y: 5 })< person.location < point({ x: 2, y: 6 })
RETURN person

全文检索索引
db.index.fulltext.createNodeIndex
创建 ： CALL db.index.fulltext.createNodeIndex("titlesAndDescriptions",["Movie", "Book"],["title", "description"])
使用： CALL db.index.fulltext.queryNodes("titlesAndDescriptions", "matrix") YIELD node, score
        RETURN node.title, node.description, score
db.index.fulltext.createRelationshipIndex 
```

##### 节点、属性、关系存储结构图：
* Node和Relationship 的 Property 是用一个 Key-Value 的双向列表来保存的； Node 的 Relatsionship 
是用一个双向列表来保存的，通过关系，可以方便的找到关系的 from-to Node. Node 节点保存第1个属性和第1个关系ID。
![](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCE5bb51b6754c3d6296411afefc675f9c8/745)
![](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCE956276019bed4c7f116a19ff0aec90ba/738)
![节点存储结构](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEd7b290b04bfd4cc98bef1f0e8086a3ba/741)
![关系存储结构](https://note.youdao.com/yws/public/resource/a86b10a2cce90c7a8f3e6fb3e8734921/xmlnote/WEBRESOURCEeb4104ba60adacf871cb253e78cc4e28/743)

##### **复合索引**失效场景：
* 存在检查： exists(n.prop)
* 范围搜索： n.prop > value
* 前缀搜索： STARTS WITH
* 后缀搜索： ENDS WITH
* 子串搜索： CONTAINS

#### 优势的思考：
* 之所以在一些场景中选用Neo4J这样的图数据库，主要是为了解决复杂的关联关系，在图里面关系是节点之间的事，并不像Mysql中通过表的关联，
在图里面更加灵活，可以随意修改不同节点之间的关联关系，操作的是针对的某个节点而不像Mysql中是对整个数据表进行操作。

#### 下面这些情况<font color=red>不利于</font>图数据库的使用：
* 记录大量基于事件的数据（例如日志条目或传感器数据）
* 对大规模分布式数据进行处理，类似于Hadoop
* 二进制数据存储
* 适合于保存在关系型数据库中的结构化数据
