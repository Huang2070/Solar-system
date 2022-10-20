https://www.cnblogs.com/zhangboyu/p/7580262.html

- [前言](#前言)
- [创建Optional实例](#创建optional实例)
- [访问Optional对象的值](#访问optional对象的值)
  - [直接取值](#直接取值)
  - [取值时设置默认值.](#取值时设置默认值)
    - [orElse() 和 orElseGet() 的不同之处](#orelse-和-orelseget-的不同之处)
  - [取值时设置异常](#取值时设置异常)
- [转换Optional包装的值](#转换optional包装的值)
- [过滤值](#过滤值)
- [应该怎么用](#应该怎么用)


# 前言
Optional 类主要解决的问题是臭名昭著的空指针异常（NullPointerException）.

本质上，这是一个包含有可选值的包装类，这意味着 Optional 类既可以含有对象也可以为空。

Optional 是 Java 实现函数式编程的强劲一步，并且帮助在范式中实现。但是 Optional 的意义显然不止于此。


我们从一个简单的用例开始。在 Java 8 之前，任何访问对象方法或属性的调用都可能导致 NullPointerException：
```Java
String isocode = user.getAddress().getCountry().getIsocode().toUpperCase();
```
在这个小示例中，如果我们需要确保不触发异常，就得在访问每一个值之前对其进行明确地检查：
```Java
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        Country country = address.getCountry();
        if (country != null) {
            String isocode = country.getIsocode();
            if (isocode != null) {
                isocode = isocode.toUpperCase();
            }
        }
    }
}
```
你看到了，这很容易就变得冗长，难以维护。

# 创建Optional实例
```Java
//创建一个包含值为空的optional对象,访问emptyOpt变量的值会导致 NoSuchElementException。
Optional<User> emptyOpt = Optional.empty();
emptyOpt.get();

//可以使用of()和ofNullable()方法创建包含值的Optional。两个方法的不同之处在于如果你把null值作为参数传递进去，of()方法会抛出 NullPointerException, 应该明确对象不为null的时候使用of().
Optional<User> opt = Optional.of(user);

//如果对象即可能是null也可能是非null，就应该使用ofNullable()方法.
Optional<User> opt = Optional.ofNullable(user);
```


# 访问Optional对象的值

## 直接取值
```Java
//从 Optional 实例中取回实际值对象的方法之一是使用 get() 方法.
String name = "John";
Optional<String> opt = Optional.ofNullable(name);
String str = opt.get();
```
注意: get方法会在值为 null 的时候抛出异常:   
java.util.NoSuchElementException: No value present.  
要避免异常，你可以选择首先验证是否有值.

```Java
//检查是否有值的另一个选择是 ifPresent() 方法。
if(opt.isPresent()) {
    //不为null判断
}

//该方法除了执行检查，还接受一个Consumer(消费者)参数，如果对象不是空的，就会执行传入的Lambda表达式：
opt.ifPresent( u -> assertEquals(user.getEmail(), u.getEmail()));
//这个例子中, 只有opt中值不为空时才执行断言.
```

## 取值时设置默认值.
```Java
//第一个方法是 orElse()，它的工作方式非常直接，如果有值则返回该值，否则返回传递给它的参数值.
User user = null;
User user2 = new User("anna@gmail.com", "1234");
User result = Optional.ofNullable(user).orElse(user2);

//第二个同类型的API是 orElseGet() —— 其行为略有不同。这个方法会在有值的时候返回值，如果没有值，它会执行作为参数传入的 Supplier(供应者) 函数式接口，并将返回其执行结果.
User result = Optional.ofNullable(user).orElseGet( () -> user2);
```

### orElse() 和 orElseGet() 的不同之处
1.当Optional里对象为空而返回默认对象时，行为并无差异。都会执行传入的函数.

2.当Optional里对象不为空时, 两个方法都会返回Optional里的非空值。不过，orElse() 方法仍然会执行传入的函数。与之相反，orElseGet() 方法不会执行。
```Java
@Test
public void givenPresentValue_whenCompare_thenOk() {
    User user = new User("john@gmail.com", "1234");

    logger.info("Using orElse");
    User result = Optional.ofNullable(user).orElse(createNewUser());

    logger.info("Using orElseGet");
    User result2 = Optional.ofNullable(user).orElseGet(() -> createNewUser());
}

//输出: 
Using orElse
Creating New User
Using orElseGet
```

## 取值时设置异常
```Java
//如果user值为null，会抛出IllegalArgumentException
User result = Optional.ofNullable(user).orElseThrow( () -> new IllegalArgumentException());
```
这个方法让我们有更丰富的语义，可以决定抛出什么样的异常，而不总是抛出 NullPointerException。


# 转换Optional包装的值
```Java
//map取出Optional包装值的内容,然后重新包装放进Optional里.
User user = new User("anna@gmail.com", "1234");
String email = Optional.ofNullable(user)
    .map(u -> u.getEmail()).orElse("default@gmail.com");
```

# 过滤值
filter() 接受一个逻辑语句参数，返回测试结果为 true 的值。如果测试结果为 false，会返回一个空的 Optional。
```Java
User user = new User("anna@gmail.com", "1234");
Optional<User> result = Optional.ofNullable(user)
    .filter(u -> u.getEmail() != null && u.getEmail().contains("@"));
```


# 应该怎么用
* Optional 不是 Serializable。因此，它不应该用作类的字段。
* Optional 主要用作返回类型。在获取到这个类型的实例后，如果它有值，你可以取得这个值，否则可以进行一些替代行为。
* Optional 类有一个非常有用的用例，就是将其与流或其它返回 Optional 的方法结合，以构建流畅的API。
```Java
//使用 Stream 返回 Optional 对象的 findFirst() 方法.
List<User> users = new ArrayList<>();
User user = users.stream().findFirst().orElse(new User("default", "1234"));
```