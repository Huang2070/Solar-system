# Spring Cache结合caffeine实现本地缓存

## 引入Java包
```xml
<dependency>
    <groupId>com.github.ben-manes.caffeine</groupId>
    <artifactId>caffeine</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

## 配置缓存管理器

### 方式一
```java
@Configuration
public class CacheConfig {
    /**
     * 本地缓存
     * 配置缓存管理器
     * 注意: caffeineCacheManager是cacheName
     */
    @Bean("caffeineCacheManager")
    public CacheManager cacheManager() {
        CaffeineCacheManager cacheManager = new CaffeineCacheManager();
        cacheManager.setCaffeine(Caffeine.newBuilder()
            // 设置最后一次写入或访问后经过固定时间过期
            .expireAfterAccess(60, TimeUnit.SECONDS)
            // 初始的缓存空间大小
            .initialCapacity(100)
            // 缓存的最大条数
            .maximumSize(1000));
        return cacheManager;
    }
}
```

### 方式二
```java
@Configuration
public class CacheConfig {

    /**
     * 本地缓存
     * 配置缓存管理器
     * 注意: commonProxyCache是cacheName
     */
    @Bean("caffeineCacheManager")
    public CacheManager cacheManager() {
        SimpleCacheManager cacheManager = new SimpleCacheManager();
        ArrayList<CaffeineCache> caches = Lists.newArrayList();
        
        caches.add(new CaffeineCache("commonProxyCache",
           Caffeine.newBuilder()
               .expireAfterWrite(10, TimeUnit.SECONDS)
               .maximumSize(1000)
               .build()));
        
        cacheManager.setCaches(caches);
        return cacheManager;
    }
}

```


## 定义本地缓存
```Java
@Component
public class DemoCache {
    //value是配置的cacheName
    @Cacheable(value = "caffeineCacheManager", key = "#root.methodName + '.' + #id")
    public DemoDo getDemoDo(Long id) {
        return demoMapper.getById(id);
    }
}

```


# 注意点
* @Cacheable是AOP代理实现的, 所以同一个类里, 内部调用@Cacheable注解的方法, 不会走到代理, 也就不会走缓存, 出现这种情况, 要分两个类.
* 如果类实现了接口, 那被@Cacheable注解的方法, 要创建接口.
* static方法@Cacheable不生效.