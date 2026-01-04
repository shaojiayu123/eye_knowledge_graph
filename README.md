# eye_knowledge_graph

## neo4j导入csv格式数据语句
### 0. 在开始导入前安装APOC
### 1. 之后先将csv文件放入neo4j目录下的import文件夹中
### 2. 进入neo4j界面输入下面命令
#### （1）导入节点文件
```python
LOAD CSV WITH HEADERS FROM 'file:///nodes.csv' AS row
CREATE (n {id: row.`id:ID`, name: row.name})
WITH n, row
CALL apoc.create.addLabels(n, [split(row.`:LABEL`, ";")[0]]) YIELD node
RETURN count(n);
```
#### （2）导入关系文件
```python
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
MATCH (start {id: row.`:START_ID`}), (end {id: row.`:END_ID`})
CALL apoc.create.relationship(start, row.`TYPE`, {}, end) YIELD rel
RETURN count(rel);
```
