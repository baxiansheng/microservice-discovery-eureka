## 微服务架构实战03 服务注册中心eureka
用于提供微服务注册服务的服务发现组件eureka， 运行于8761端口和8762端口。
### 多个微服务注册中心相互注册
可以使用多个微服务注册中心相互注册， 保证微服务的鲁棒性， 和后续的负载均衡。
### 配置文件如下
```
---
spring:
  profiles: peer1

server:
  port: 8761

eureka:
  client:
    service-url:
      defaultZone: http://eureka2:8762/eureka/
---
spring:
  profiles: peer2

server:
  port: 8762

eureka:
  client:
    service-url:
      defaultZone: http://eureka1:8761/eureka/
```
解释如下：  
jar包运行的时候激活不同的配置项即可实现两个服务发现组件的自动相互注册。
```
java -jar xxx.jar --spring.active=peer1
```
### 小结
至此，可以使用微服务注册中心、微服务提供者，微服务消费者构成简单的微服务架构。  
启动两个eureka微服务发现中心（分别运行于8761和8762端口）  
启动两个微服务提供者，并向eureka集群注册（分别运行与8000和8001端口）  
启动服务消费者（8010端口）  
运行截图如下  
![](https://raw.githubusercontent.com/baxiansheng/ForSomething/master/imgs/task07-2.png)  
![](https://raw.githubusercontent.com/baxiansheng/ForSomething/master/imgs/task07-3.png)  
使用Postman测试结果如下所示  
![](https://raw.githubusercontent.com/baxiansheng/ForSomething/master/imgs/task07-4.png)
