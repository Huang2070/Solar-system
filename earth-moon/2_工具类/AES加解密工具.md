## 对称加密算法AES
参数说明: `cipherType` 算法/模式/补码方式
举例: AES/ECB/PKCS5Padding


```Java
import java.security.NoSuchAlgorithmException;

import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;

import org.apache.commons.codec.binary.Base64;

public class AesUtil {

    private static final String KEY_ALGORITHM = "AES";
    private static final String DEFAULT_CIPHER_ALGORITHM = "AES/ECB/PKCS5Padding";

    private AesUtil() {
    }

    /**
     * 加密
     * @param content
     * @param key
     * @param cipherType
     * @return
     * @throws Exception
     */
    public static String encrypt(String content, String key, String cipherType) throws Exception {
        Cipher cipher = Cipher.getInstance(cipherType);
        byte[] byteContent = content.getBytes("utf-8");
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(Base64.decodeBase64(key), KEY_ALGORITHM));
        byte[] result = cipher.doFinal(byteContent);
        return Base64.encodeBase64String(result);
    }

    /**
     * 解密
     * @param content
     * @param key
     * @param cipherType
     * @return
     * @throws Exception
     */
    public static String decrypt(String content, String key, String cipherType) throws Exception {
        Cipher cipher = Cipher.getInstance(cipherType);
        cipher.init(Cipher.DECRYPT_MODE, new SecretKeySpec(Base64.decodeBase64(key), KEY_ALGORITHM));
        byte[] result = cipher.doFinal(Base64.decodeBase64(content));
        return new String(result, "utf-8");
    }

    /**
     * 生成密钥
     * @return
     */
    public static String randomSecretKey() {
        KeyGenerator kg;
        try {
            kg = KeyGenerator.getInstance(KEY_ALGORITHM);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
        kg.init(128);
        SecretKey secretKey = kg.generateKey();
        return Base64.encodeBase64String(secretKey.getEncoded());
    }

}
```