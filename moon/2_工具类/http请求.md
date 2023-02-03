# 依赖
``` xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>***</version>
</dependency>
```


# 代码
``` Java
import com.alibaba.fastjson.JSON;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.HttpClient;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.conn.ssl.NoopHostnameVerifier;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.entity.mime.content.StringBody;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.impl.conn.PoolingHttpClientConnectionManager;
import org.apache.http.message.BasicHeader;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

import java.net.URLEncoder;
import java.security.cert.CertificateException;
import java.security.cert.X509Certificate;
import java.util.*;
import java.util.concurrent.Callable;

import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManager;
import javax.net.ssl.X509TrustManager;


public class HttpUtil {

    private static final String CHARST = "UTF-8";
    private static HttpClient client = null;

    static {
        PoolingHttpClientConnectionManager cm = new PoolingHttpClientConnectionManager();
        cm.setMaxTotal(128);
        cm.setDefaultMaxPerRoute(128);
        client = HttpClients.custom().setConnectionManager(cm).build();
    }

    @SuppressWarnings({"rawtypes", "unchecked"})
    public static String postRequest(String url, Map<String, String> data) throws Exception {
        return postRequest(url, (Map<String, String>) null, data);
    }

    @SuppressWarnings({"rawtypes", "unchecked"})
    public static String postRequest(String url, Map<String, String> headers, Map<String, String> data) throws Exception {
        String result = null;
        HttpPost httpPost = new HttpPost(url);

        // 设置Header
        if (headers != null && !headers.isEmpty()) {
            for (Map.Entry<String, String> entry : headers.entrySet()) {
                httpPost.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }


        // 设置参数
        List<NameValuePair> list = new LinkedList<>();
        for (Map.Entry<String, String> entry : data.entrySet()) {
            list.add(new BasicNameValuePair(entry.getKey(), entry.getValue()));
        }
        if (list.size() > 0) {
            UrlEncodedFormEntity entity = new UrlEncodedFormEntity(list, CHARST);
            httpPost.setEntity(entity);
        }

        HttpResponse response = client.execute(httpPost);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }

        return result;
    }


    @SuppressWarnings({"rawtypes", "unchecked"})
    public static String postRequestTrustCA(String url, Map<String, String> headers, Map<String, String> data) throws Exception {
        String result = null;
        HttpPost httpPost = new HttpPost(url);

        // 设置Header
        if (headers != null && !headers.isEmpty()) {
            for (Map.Entry<String, String> entry : headers.entrySet()) {
                httpPost.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }


        // 设置参数
        List<NameValuePair> list = new LinkedList<>();
        for (Map.Entry<String, String> entry : data.entrySet()) {
            list.add(new BasicNameValuePair(entry.getKey(), entry.getValue()));
        }
        if (list.size() > 0) {
            UrlEncodedFormEntity entity = new UrlEncodedFormEntity(list, CHARST);
            httpPost.setEntity(entity);
        }

        HttpClient client = wrapClient();
        HttpResponse response = client.execute(httpPost);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }

        return result;
    }


    /**
     * description 忽略https证书验证
     */
    public static HttpClient wrapClient() {
        try {
            SSLContext ctx = SSLContext.getInstance("SSL");
            X509TrustManager tm = new X509TrustManager() {

                @Override
                public X509Certificate[] getAcceptedIssuers() {
                    return null;
                }

                @Override
                public void checkClientTrusted(X509Certificate[] arg0, String arg1) throws CertificateException {
                }

                @Override
                public void checkServerTrusted(X509Certificate[] arg0,
                    String arg1) throws CertificateException {
                }
            };
            ctx.init(null, new TrustManager[]{tm}, null);
            SSLConnectionSocketFactory ssf = new SSLConnectionSocketFactory(ctx, NoopHostnameVerifier.INSTANCE);
            CloseableHttpClient httpclient = HttpClients.custom().setSSLSocketFactory(ssf).build();
            return httpclient;
        } catch (Exception ex) {
            ex.printStackTrace();
            return HttpClients.createDefault();
        }
    }



    public static String postMultiRequest(String url, Map<String, String> headers, Map<String, Object> data) throws Exception {
        String result = null;
        HttpPost httpPost = new HttpPost(url);

        // 设置Header
        if (headers != null && !headers.isEmpty()) {
            for (Map.Entry<String, String> entry : headers.entrySet()) {
                httpPost.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }
        if (data != null && data.size() > 0) {
            // 设置参数
            MultipartEntityBuilder builder = MultipartEntityBuilder.create();
            for (Map.Entry<String, Object> entry : data.entrySet()) {
                Object value = entry.getValue();
                if (value instanceof List) {
                    for (Object v : (List) value) {
                        builder.addPart(entry.getKey(), new StringBody(v.toString(), ContentType.TEXT_PLAIN));
                    }
                } else {
                    builder.addPart(entry.getKey(), new StringBody(value.toString(), ContentType.TEXT_PLAIN));
                }
            }

            HttpEntity httpEntity = builder.build();
            httpPost.setEntity(httpEntity);
        }

        HttpResponse response = client.execute(httpPost);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }

        return result;
    }

    /**
     * Created by IDEA
     *
     * @param url    链接
     * @param body   报文体
     * @param header 报文头
     * @return
     */
    public static String postRequest(String url, String body, Map<String, String> header) throws Exception {
        HttpPost httpPost = new HttpPost(url);
        // 设置Header
        if (header != null && !header.isEmpty()) {
            for (Iterator<Map.Entry<String, String>> it = header.entrySet().iterator(); it.hasNext(); ) {
                Map.Entry<String, String> entry = it.next();
                httpPost.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }

        String result = null;
        // 设置参数
        httpPost.setEntity(new StringEntity(body, "UTF-8"));
        HttpResponse response = client.execute(httpPost);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }
        return result;
    }

    /**
     * Created by IDEA
     *
     * @param url    链接
     * @param body   报文体
     * @param header 报文头
     * @return
     */
    public static Map<String, String> postRequestForStatusCode(String url, String body
            , Map<String, String> header) throws Exception {

        Map<String, String> result = new HashMap<>();

        HttpPost httpPost = new HttpPost(url);
        // 设置Header
        if (header != null && !header.isEmpty()) {
            for (Iterator<Map.Entry<String, String>> it = header.entrySet().iterator(); it.hasNext(); ) {
                Map.Entry<String, String> entry = it.next();
                httpPost.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }

        // 设置参数
        httpPost.setEntity(new StringEntity(body, "UTF-8"));
        HttpResponse response = client.execute(httpPost);
        if (response != null) {
            int statusCode = response.getStatusLine().getStatusCode();
            result.put("statusCode", String.valueOf(statusCode));
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                String data = EntityUtils.toString(resEntity);
                result.put("data", data);
            }
        }
        return result;
    }




    public static String getRequestEncode(String url, Map<String, String> headers, Map<String, String> param) throws Exception {
        String result = null;
        List<String> tmp = new ArrayList<>();
        for (Map.Entry<String, String> entry : param.entrySet()) {
            tmp.add(entry.getKey() + "=" + URLEncoder.encode(entry.getValue(), "UTF-8"));
        }
        HttpGet httpGet = new HttpGet(url + "?" + String.join("&", tmp));

        // 设置Header
        if (headers != null && !headers.isEmpty()) {
            for (Map.Entry<String, String> entry : headers.entrySet()) {
                httpGet.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }

        HttpResponse response = client.execute(httpGet);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }

        return result;
    }


    public static String getRequest(String url, Map<String, String> headers, Map<String, String> param) throws Exception {
        String result = null;
        List<String> tmp = new ArrayList<>();
        for (Map.Entry<String, String> entry : param.entrySet()) {
            tmp.add(entry.getKey() + "=" + entry.getValue());
        }
        HttpGet httpGet = new HttpGet(url + "?" + String.join("&", tmp));

        // 设置Header
        if (headers != null && !headers.isEmpty()) {
            for (Map.Entry<String, String> entry : headers.entrySet()) {
                httpGet.setHeader(new BasicHeader(entry.getKey(), entry.getValue()));
            }
        }

        HttpResponse response = client.execute(httpGet);
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity);
            }
        }

        return result;
    }

    public static class RetryPostRequest implements Callable<String> {

        private String url;

        private Map<String, String> paramMap;

        private Map<String, String> headerMap;

        public RetryPostRequest(String url, Map<String, String> paramMap, Map<String, String> headerMap) {
            this.url = url;
            this.paramMap = paramMap;
            this.headerMap = headerMap;
        }

        @Override
        public String call()
                throws Exception {
            String resp = HttpUtil.postRequest(url, headerMap, paramMap);
            return resp;
        }
    }

    public static class RetryGetRequest implements Callable<String> {

        private String url;

        private Map<String, String> paramMap;

        private Map<String, String> headerMap;

        public RetryGetRequest(String url, Map<String, String> paramMap, Map<String, String> headerMap) {
            this.url = url;
            this.paramMap = paramMap;
            this.headerMap = headerMap;
        }

        @Override
        public String call()
                throws Exception {
            String resp = HttpUtil.getRequest(url, headerMap, paramMap);
            return resp;
        }
    }

    public static class RetryPostBodyRequest implements Callable<String> {

        private String url;

        private Map<String, String> paramMap;

        private Map<String, String> headerMap;

        public RetryPostBodyRequest(String url, Map<String, String> paramMap, Map<String, String> headerMap) {
            this.url = url;
            this.paramMap = paramMap;
            this.headerMap = headerMap;
        }

        @Override
        public String call()
                throws Exception {
            String resp = HttpUtil.postRequest(url, JSON.toJSONString(paramMap), headerMap);
            return resp;
        }
    }

}

```