注意:由于开发阶段前端和后端是并行开发的，后端完成某个功能后，此时前端对应的功能可能还没有开发完成导致无法进行前后端联调测试。所以在开发阶段，后端测试主要以**接口文档测试**为主。



* 除login外，都校验jwtToken；使用拦截器拦截所有请求（除登录外）
* @ConfigurationProperties(prefix = "sky.jwt")  读取yaml文件里面sky.jwt字段
* 在preHandle方法中，进行校验

接口设计：请求参数的格式：query， JSON



分页查询借助于PageHelper依赖，PageHelper会自动增加limit

PageHelper.startPage的源码，里面有ThreadLocal

![image-20240625210835299](F:/JAVA/JAVA_Program/cangQiongWaiMai/sky/note/day02.assets/image-20240625210835299.png)



**启用禁用员工账号**

* 获取路径参数，使用@PathVariable

* 等同

  ```java
  Employee employee = Employee.builder().status(status).id(id).build();
  
  ===========等同==============
  Employee employee = new Employee();
  employee.setStatus(status);
  employee.setId(id);
  ```

* 注意Swagger里面的token有没有更新



**根据id查询员工信息**  ==》 无问题

**修改员工信息功能**

* **注:因为是修改功能，请求方式可设置为PUT。**
* 使用BeanUtils可以将EmployeeDTO对象改为Employee对象
* 如果表单没有任何修改，还是可以执行保存操作，**数据库有新的记录产生，怎么处理？**



分类模块

* 什么类型的请求匹配社么样的方式，选择的依据是什么？区别是什么