## http请求,返回的实体类中变量如果为null,不返回

### 引入jar包
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.15.2</version>
</dependency>
```
### 实体类加注解@JsonInclude
```Java
@JsonInclude(JsonInclude.Include.NON_NULL)
Public class ClassA {
    ...
}
```

*** 

## 复制实体类
* org.springframework.beans.BeanUtils#copyProperties(Object source, Object target);

*** 

## 成员内部类的写法
```Java
//在外部类里定义
@Data
public class OuterClass {
    private String a;
    private String b;

    @Data
    public class InnerClass {
        private String c;
        private String d;
    }
}

//new成员类的方式
OuterClass aaa = new OuterClass();
OuterClass.InnerClass bbb = aaa.new InnerClass();
```



