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
