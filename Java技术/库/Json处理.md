# 校验JSON
```Java
import com.alibaba.fastjson.JSON;
import org.apache.commons.lang3.StringUtils;

public class JsonUtils {

    /**
     * 是否合法的jsonNode
     *
     * @param json
     * @return
     */
    public static boolean isJsonNode(String json) {
        if (StringUtils.isBlank(json)) {
            return false;
        }
        try {
            JSON.parseObject(json);
            return true;
        } catch (Throwable e) {
            return false;
        }
    }

    /**
     * 是否合法的json
     *
     * @param json
     * @return
     */
    public static boolean isValidJson(String json) {
        if (StringUtils.isBlank(json)) {
            return false;
        }
        return isJsonNode(json) || isJsonArray(json);
    }

    /**
     * 是否合法的jsonArray
     *
     * @param json
     * @return
     */
    public static boolean isJsonArray(String json) {
        if (StringUtils.isBlank(json)) {
            return false;
        }
        try {
            JSON.parseArray(json);
            return true;
        } catch (Throwable e) {
            return false;
        }
    }
}
```


# JSONArray排序
``` Java
//根据array中对象里time成员排序.
logArray.sort(Comparator.comparing(obj -> ((JSONObject) obj).getDate("time")));
```