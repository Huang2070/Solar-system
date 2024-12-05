```Java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

public static String HMACSHA256(String key, String data) throws Exception {
    Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
    SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(StandardCharsets.UTF_8), "HmacSHA256");
    sha256_HMAC.init(secret_key);
    return bytesToHexString(sha256_HMAC.doFinal(data.getBytes(StandardCharsets.UTF_8)));
}


/**
 * 从字节数组到十六进制字符串转换
 *
 * @param src
 * @return 十六进制字符串
 */
public static String bytesToHexString(byte[] src) {
    StringBuilder stringBuilder = new StringBuilder("");
    if (src == null || src.length <= 0) {
        return null;
    }
    for (int i = 0; i < src.length; i++) {
        int v = src[i] & 0xFF;
        String hv = Integer.toHexString(v);
        if (hv.length() < 2) {
            //byte数组转换成16进制字符串会补足两位自动在前面加0
            stringBuilder.append(0);
        }
        stringBuilder.append(hv);
    }
    return stringBuilder.toString();
}

```