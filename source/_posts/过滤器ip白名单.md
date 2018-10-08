---
title: 过滤器ip白名单
date: 2018-10-08 22:18:08
tags:
- spring
---

## Filter中注入Bean失败
在写这个过滤器的时候遇到个问题,Bean注入失败，为空，以前也没有注意到。
## 原理
其实Spring中，web应用启动的顺序是：listener->filter->servlet，先初始化listener，然后再来就filter的初始化，再接着才到我们的dispathServlet的初始化，因此，当我们需要在filter里注入一个注解的bean时，就会注入失败，因为filter初始化时，注解的bean还没初始化，没法注入。

## 解决方式
解决方式有好几种
我用的是手动getBean的方式<br>
```
ApplicationContext ac1 = WebApplicationContextUtils.getRequiredWebApplicationContext(ServletContext sc); 
ac1.getBean("beanId");
```

这里getBean内容注意大小写，经常因为没注意没获取到bean。

## 获取登录ip

```
/** 
 * 通过HttpServletRequest返回IP地址 
 * @param request HttpServletRequest 
 * @return ip String 
 * @throws Exception 
 */  
public String getIpAddr(HttpServletRequest request) throws Exception {  
    String ip = request.getHeader("x-forwarded-for");  
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
        ip = request.getHeader("Proxy-Client-IP");  
    }  
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
        ip = request.getHeader("WL-Proxy-Client-IP");  
    }  
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
        ip = request.getHeader("HTTP_CLIENT_IP");  
    }  
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
        ip = request.getHeader("HTTP_X_FORWARDED_FOR");  
    }  
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
        ip = request.getRemoteAddr();  
    }  
    return ip;  
}
```

