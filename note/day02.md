注意:由于开发阶段前端和后端是并行开发的，后端完成某个功能后，此时前端对应的功能可能还没有开发完成导致无法进行前后端联调测试。所以在开发阶段，后端测试主要以**接口文档测试**为主。



* 除login外，都校验jwtToken；使用拦截器拦截所有请求（除登录外）
* 在preHandle方法中，进行校验

接口设计：请求参数的格式：query， JSON



分页查询借助于PageHelper依赖，PageHelper会自动增加limit

PageHelper.startPage的源码，里面有ThreadLocal

![image-20240625210835299](F:/JAVA/JAVA_Program/cangQiongWaiMai/sky/note/day02.assets/image-20240625210835299.png)