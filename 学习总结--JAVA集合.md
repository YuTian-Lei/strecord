# JAVA集合

## 集合包

![](C:\Users\lenovo\Desktop\学习总结\img\aHR0cHM6Ly9pbWFnZXMwLmNuYmxvZ3MuY29tL2Jsb2cvNDk3NjM0LzIwMTMwOS8wODE3MTAyOC1hNWUzNzI3NDFiMTg0MzE1OTFiYjU3N2IxZTFjOTVlNi5qcGc.png)

![](C:\Users\lenovo\Desktop\学习总结\img\JAVA集合.png)

## 同步容器

```Java
	public static void main(String[] args) {
	  Collection collection = Collections.synchronizedCollection(Lists.newArrayList());
	  List list = Collections.synchronizedList(Lists.newArrayList());
	  Set set = Collections.synchronizedSet(Sets.newHashSet());
	  Map map = Collections.synchronizedMap(Maps.newHashMap());	  
	}
```

## JUC下的并发容器

![](C:\Users\lenovo\Desktop\学习总结\img\JUC集合.png)