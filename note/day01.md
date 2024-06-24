# day01

### nginx

* 反向代理的好处：
  1、提高访问速度，可以缓存

​	   2、进行负载均衡，把大量的请求按照我们所指定的方式均衡的分配给集群中的每台服务器

​       3、保证后端服务安全：因为一般后台服务地址不会暴露，所以使用浏览器不能直接访问，可以把nginx作为请求访问的入口，请求到达nginx后转发到具体的服务中

* 怎么配置

   ```nginx
   server {
       listen 80;
       server_name localhost;
       
       location /api/ {
           proxy_pass http://localhost:8080/admin/; # 反向代理
       }
   }
   ```

* ```nginx
  upstream webservers{
      server 192.168.100.128:8080;
      server 192.168.100.129:8080;
  }
  server{
      listen 80;
      server_name localhost;
      
      location /api/{
          proxy_pass http://webservers/admin;#负载均衡
      }
  }
  ```

### nginx 负载均衡策略：

| **名称**   | **说明**                                               |
| ---------- | ------------------------------------------------------ |
| 轮询       | 默认方式                                               |
| weight     | 权重方式，默认为1，权重越高，被分配的客户端请求就越多  |
| ip_hash    | 依据ip分配方式，这样每个访客可以固定访问一个后端服务   |
| least_conn | 依据最少连接方式，把请求优先分配给连接数少的后端服务   |
| url_hash   | 依据url分配方式，这样相同的url会被分配到同一个后端服务 |
| fair       | 依据响应时间方式，响应时间短的服务将会被优先分配       |

### **登录MD5 **

* MD5的加密处理：123456 ==》 32位16进制的字符串 ==》 存入数据库

### **VO、DTO**

* 前端表单发送给后端，后端接受一个DTO（data transform obj）对象
* 后端处理好了，返回给前端一个VO对象  view obj
* 两者都实现序列化接口，用于传输对象

### Swagger

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务(<https://swagger.io/>)。

Spring已经将Swagger纳入自身的标准，建立了Spring-swagger项目，现在叫Springfox。通过在项目中引入Springfox ，即可非常简单快捷的使用Swagger。

**已添加依赖** 

* 常用注解



| **注解**          | **说明**                                               |
| ----------------- | ------------------------------------------------------ |
| @Api              | 用在类上，例如Controller，表示对类的说明               |
| @ApiModel         | 用在类上，例如entity、DTO、VO                          |
| @ApiModelProperty | 用在属性上，描述属性信息                               |
| @ApiOperation     | 用在方法上，例如Controller的方法，说明方法的用途、作用 |

