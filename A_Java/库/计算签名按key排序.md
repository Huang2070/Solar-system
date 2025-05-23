# 需求
1.根据参数的key值升序排列, 以key=value&key1=value1的形式拼接起来, 得到basestr字符串。

2.将basestr字符串加上secret,进行MD5值（MD5(basestr+ secret)）计算得到sign值xx。


# 写法
## 通过SortedMap接口构建接口
```Java
public static String getSign(Map<String, Object> param) {

    //所有参与传参的参数按照accsii排序（升序）
    TreeMap<String, Object> parameters = new TreeMap<>(param);

    StringBuilder stringBuilder = new StringBuilder();
    for (Map.Entry<String, Object> entry : parameters.entrySet()) {
        String key = entry.getKey();
        Object value = entry.getValue();

        if (!StringUtils.isAnyBlank(value.toString(), key) && !"sign".equals(key)) {
            stringBuilder.append(key).append("=").append(value).append("&");
        }
    }

    String orderParamStr = stringBuilder.substring(0, stringBuilder.length() - 1);

    String md5Str = orderParamStr + secret;
    return SignUtil.md5(md5Str);
}
```



# 函数式参数排序方法 
```Java
//params是Map<String, String>类型的参数键值对, 获取key=value的list,通过&拼接
String concatStr = params.entrySet().stream()
    //过滤value是null的字段
    .filter(paramEntry -> Objects.nonNull(paramEntry.getValue()))
    //过滤签名字段
    .filter(e -> !Objects.equals(e.getKey(), "sign"))
    //排序
    .sorted(Map.Entry.comparingByKey())
    //拼接
    .map(e -> e.getKey() + "=" + e.getValue()).collect(Collectors.joining("&"));
```


