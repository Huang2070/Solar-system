* xml文件的注释中不能有"--"字符
* 在&字符后面加上amp; 就不会报错了
  
---
  
xml中的转义
```xml
<：& lt
>：& gt
&：& amp
```
---

标明是纯文本，没有这个的话 < > & 字符是不能直接存入XML的，需要转义，而用这个标记则不需要转义而将这些符号存入XML文档。
可以避免未预料的特殊符号导致XML解析出错。
```xml
<![CDATA[ ]]>
```

