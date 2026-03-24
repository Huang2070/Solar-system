```xml
-- 工具包
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>20131018</version>
</dependency>
```


```Java
import org.json.XML;

private String jsonTransToXml(String json) {
    org.json.JSONObject jsonObject = new org.json.JSONObject(json);
    String xml = XML.toString(jsonObject);
    xml = "<?xml version=\"1.0\" encoding=\"UTF-8\"?>" + xml;
    return xml;
}

private String xmlTransToJson(String xml) {
    int index = StringUtils.indexOf(xml, "<rtn>");
    xml = xml.substring(index);
    return JSON.toJSONString(XML.toJSONObject(xml));
}
```