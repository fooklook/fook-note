## mysql递归查询

### mysql查询所有子节点

在Oracle 中有一个 Hierarchical Queries 通过CONNECT BY 可以方便的查了所有当前节点下的所有子节点。在MySQL的目前版本中还没有对应的功能。

在MySQL中如果是有限的层次，比如事先如果可以确定这个树的最大深度是4, 那么所有节点为根的树的深度均不会超过4，则可以直接通过left join 来实现。

但有时无法控制树的深度。这时就需要在MySQL中用存储过程来实现或在你的程序中来实现这个递归。

表结构：
```mysql
create table treeNodes
(
    id int primary key,
    nodename varchar(20),
    pid int
);
```
插入
```mysql
INSERT INTO `treeNodes` VALUES ('1', 'A', '0'); 
INSERT INTO `treeNodes` VALUES ('2', 'B', '1'); 
INSERT INTO `treeNodes` VALUES ('3', 'C', '1'); 
INSERT INTO `treeNodes` VALUES ('4', 'D', '2'); 
INSERT INTO `treeNodes` VALUES ('5', 'E', '2'); 
INSERT INTO `treeNodes` VALUES ('6', 'F', '3'); 
INSERT INTO `treeNodes` VALUES ('7', 'G', '6'); 
INSERT INTO `treeNodes` VALUES ('8', 'H', '0'); 
INSERT INTO `treeNodes` VALUES ('9', 'I', '8'); 
INSERT INTO `treeNodes` VALUES ('10', 'J', '8'); 
INSERT INTO `treeNodes` VALUES ('11', 'K', '8'); 
INSERT INTO `treeNodes` VALUES ('12', 'L', '9'); 
INSERT INTO `treeNodes` VALUES ('13', 'M', '9'); 
INSERT INTO `treeNodes` VALUES ('14', 'N', '12'); 
INSERT INTO `treeNodes` VALUES ('15', 'O', '12'); 
INSERT INTO `treeNodes` VALUES ('16', 'P', '15'); 
INSERT INTO `treeNodes` VALUES ('17', 'Q', '15');
```

### 利用函数来得到所有子节点号

创建一个function getChildLst, 得到一个由所有子节点号组成的字符串。

```mysql
delimiter //
CREATE FUNCTION `getChildLst`(rootId INT)
    RETURNS varchar(1000)
    BEGIN
    DECLARE sTemp VARCHAR(1000);
    DECLARE sTempChd VARCHAR(1000);
    SET sTemp = '$';
    SET sTempChd =cast(rootId as CHAR);
    WHILE sTempChd is not null DO
        SET sTemp = concat(sTemp,',',sTempChd);
        SELECT group_concat(id) INTO sTempChd FROM treeNodes where FIND_IN_SET(pid,sTempChd)>0;
    END WHILE;
    RETURN sTemp;
   END
//
```
查询
```mysql
select getChildLst(1);
+-----------------+
| getChildLst(1)  |
+-----------------+
| $,1,2,3,4,5,6,7 |
+-----------------+
//子节点展示
select * from treeNodes where FIND_IN_SET(id, getChildLst(1));
+----+----------+------+
| id | nodename | pid  |
+----+----------+------+
|  1 | A        |    0 |
|  2 | B        |    1 |
|  3 | C        |    1 |
|  4 | D        |    2 |
|  5 | E        |    2 |
|  6 | F        |    3 |
|  7 | G        |    6 |
+----+----------+------+
//查询
```

### mysql查询所有子节点

表结构如上

```mysql
delimiter // 
CREATE FUNCTION `getParLst`(rootId INT)
RETURNS varchar(1000) 
BEGIN
	DECLARE sTemp VARCHAR(1000);
	DECLARE sTempPar VARCHAR(1000); 
	SET sTemp = ''; 
	SET sTempPar =rootId; 
	
	#循环递归
	WHILE sTempPar is not null DO 
		#判断是否是第一个，不加的话第一个会为空
		IF sTemp != '' THEN
			SET sTemp = concat(sTemp,',',sTempPar);
		ELSE
			SET sTemp = sTempPar;
		END IF;

		SET sTemp = concat(sTemp,',',sTempPar); 
		SELECT group_concat(pid) INTO sTempPar FROM treenodes where pid<>id and FIND_IN_SET(id,sTempPar)>0; 
	END WHILE;	
RETURN sTemp; 
END
//
```
查询
```mysql
select * from treeNodes where FIND_IN_SET(id,getParList(15));
+----+----------+------+
| id | nodename | pid  |
+----+----------+------+
|  8 | H        |    0 |
|  9 | I        |    8 |
| 12 | L        |    9 |
| 15 | D        |   12 |
+----+----------+------+
```