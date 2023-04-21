# 注解 + @Around 模式
``` Java

import java.lang.reflect.Method;
import java.util.Map;
import java.util.Objects;

import com.google.common.collect.Maps;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.ObjectUtils;
import org.apache.commons.lang3.StringUtils;
import org.apache.commons.lang3.math.NumberUtils;
import org.apache.commons.lang3.tuple.ImmutablePair;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.core.annotation.AnnotationUtils;

/**
 * 手机号运营商信息校验
 * @author huangjin
 */
@Aspect
@EnableAspectJAutoProxy(exposeProxy = true)
@Configuration
@Slf4j
public class ExampleAspect {

    //这里写注解类的路径,例如类名叫com.huangjin.ExampleAnnotation
    @Around("@annotation(com.huangjin.ExampleAnnotation)")
    public Object around(final ProceedingJoinPoint point) throws Throwable {

        //获取方法参数
        MethodSignature signature = (MethodSignature)point.getSignature();
        Map<String, Object> param = this.getParameters(point, signature);

        //获取注解的信息
        Method method = signature.getMethod();
        ExampleAnnotation exampleAnnotation = AnnotationUtils.getAnnotation(method, ExampleAnnotation.class);

        //todo,执行切面逻辑,@Around模式有问题,可以直接返回

        //执行方法体逻辑,返回
        return point.proceed();
    }

    /**
     * 获取方法请求参数
     * @param joinPoint
     * @param signature
     * @return
     */
    private Map<String, Object> getParameters(ProceedingJoinPoint joinPoint, MethodSignature signature) {
        //参数值
        Object[] paramValues = joinPoint.getArgs();
        //参数名
        String[] paramNames = signature.getParameterNames();

        if (paramValues == null || paramNames == null || paramValues.length == 0 || paramNames.length == 0) {
            return Maps.newHashMap();
        }

        Map<String, Object> paramMap = Maps.newHashMap();
        for (int i = 0; i < paramValues.length && i < paramNames.length; i++) {
            paramMap.put(paramNames[i], paramValues[i]);
        }
        return paramMap;
    }
}


```