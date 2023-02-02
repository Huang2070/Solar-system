# pair

在 Apache Commons 库中，org.apache.commons.lang3.tuple包中提供 Pair 抽象类，不能被直接实例化。其有两个子类，分别代表可变与不可变配对：和 MutablePair和ImmutablePair。两者都实现了访问 key/value 以及 setter和getter方法.

```Java
//两种方式初始化
ImmutablePair<Integer, String> pair = new ImmutablePair<>(2, "two");
MutablePair<Integer, String> pair = MutablePair.of(2, "two");

//两种方式获取key
Integer key = pair.getKey();
Integer key = pair.getLeft();
//两种方式获取value
String value = pair.getValue();
String value = pair.getRight();
```


# org.apache.commons.lang3.StringUtils
StringUtils.isBlank():是否为空，不管是null还是空字符串都是true。

StringUtils.isNotBlank():是否不为空，上一个方法取反。

StringUtils.isAnyBlank():同时验证多个字符串是否为空，任意为空返回true。

StringUtils.isNoneBlank():都不为空，上一个取反。

StringUtils.isAllBlank():所有都是空。