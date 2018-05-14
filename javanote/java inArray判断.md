## java inArray判断

可以使用Arrays类中binarySearch(Object[] a, Object key) 方法来查找是否存在某个值，如果某个值存在则返回值大于0，反之返回值则小于0。

- 优点：使用二分查找法，效率快捷。
- 缺点：查询的数组必须是有序的，如果不是有序的话，使用此方法是没有用的。

```
String[] array = {"1","2","3","4"};
int index = Arryas.binarySearch(array,"2");
System.out.println("index:" + index); //--- index:1
index = Arryas.binarySearch(array,"0");
System.out.println("index:" + index); //--- index:-1
index = Arryas.binarySearch(array,"5");
System.out.println("index:" + index); //--- index:-5
```

使用Arrays类中asList()方法将数组转化为List()列表，在使用contains()方法判断数组中是否存在某个值。

- 优点：数组可以是乱序的，没有顺序的要求。
- 缺点：查询效率上可能稍慢，但应该不会影响大局。

```
String[] array = {"1","2","3","4"};
boolean flag = Arrays.asList(array).contains("2");
System.out.println("flag:" + flag);//--- flag:true
flag = Arrays.asList(array).contains("0");
System.out.println("flag:" + flag);//--- flag:false
flag = Arrays.asList(array).contains("5");
System.out.println("flag:" + flag);//--- flag:false
```