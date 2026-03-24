# ImmutableList
不可变list
```Java
List<Item> itemList = ImmutableList.of(item1, item2);
```

# ImmutableMap
不可变Map
```Java
Map<String, String> map = ImmutableMap.of(k1, v1, k2, v2);
```

# ArrayListMultimap
当key重复时,value不会被覆盖.就是说value有几个,key就有几个.

# Maps
## uniqueIndex
```Java
list转map,但是key值必须是唯一的,不然报异常.
Map<Long, Class> positionSrMap = Maps.uniqueIndex(list, Class::getId);
```



