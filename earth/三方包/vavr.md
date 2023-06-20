# 说明
手册: https://blog.csdn.net/ssehs/article/details/105831916
Vavr(以前称为Javaslang)是一个Java8+的函数式编程库，它提供了持久的数据类型和函数控制结构。

# Tuple
Tuple即元组.
Java缺少元组的一般概念。Tuple将固定数量的元素组合在一起，这样它们就可以作为一个整体传递。与数组或列表不同，tuple可以包含不同类型的对象，但它们也是不可变的。

元组的类型有Tuple1、Tuple2、Tuple3等等。目前有8个元素的上限。要访问元组t的元素，可以使用方法t._1访问第一个元素t._2访问第二个，依此类推。

```Java
//创建元祖
Tuple2<String, Integer> tuple2Example = Tuple2.of("Java", 8);

//访问第一个元素
String s = tuple2Example._1;

//访问第二个元素
String i = tuple2Example._2;
```