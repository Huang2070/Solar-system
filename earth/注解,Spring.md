深入理解Java：注解（Annotation）基本概念
https://www.cnblogs.com/peida/archive/2013/04/23/3036035.html


注解（Annotation）自定义注解入门
https://www.cnblogs.com/peida/archive/2013/04/24/3036689.html


注解（Annotation）--注解处理器
https://www.cnblogs.com/peida/archive/2013/04/26/3038503.html


# 写一个注解例子
``` Java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ExampleAnnotation {

    String param1() default "";

    String param2() default "";
}
```