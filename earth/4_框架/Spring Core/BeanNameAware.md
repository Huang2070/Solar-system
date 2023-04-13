BeanNameAware接口作用：让Bean对Name或id有知觉, 可以获取当前bean的名称.

```Java
import org.springframework.beans.factory.BeanNameAware;
import org.springframework.stereotype.Service;

/**
 * BeanNameAware作用：让Bean对Name有知觉
 */
@Service
public class BizService implements BeanNameAware {
    private String beanName;

    @Override
    public void setBeanName(String s) {
        this.beanName = s;
    }

    public String getBeanName() {
        return beanName;
    }
}
```
