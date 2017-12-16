##eureka 注册中心
eureka的基本架构，由3个角色组成：
1. Eureka Server
    * 提供服务注册和发现
2. Service Provider
    * 服务提供方
    * 将吱声服务注册到Eureka，从而是服务消费方能够找到
3. Service Consumer
    * 服务消费方
    * 从Eureka 获取注册服务列表，从而能够消费服务。

Eureka Server 实现注册中心只需要简单的几个步骤

1. Pom中添加依赖

   ```
   <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
   </dependencies>
   ```
2. Spring boot 中的 class Application 加 @EnableEurekaServer
    ```
    @SpringBootApplication
    @EnableEurekaServer
    public class SpringCloudEurekaApplicaiotn{
        public static void main(String[] args){
            SpringApplication.run(SpringCloudEurekaApplication.class);
        }
    }
    ```
    
3. 配置文件
    在默认设置下，该服务注册中心也会将自己作为客户端注册它自己，所以我们需要禁用它的客户端注册行为，   在application.properties添加一下配置：
    ```
        spring.application.name=spring-cloud-eureka
        
        server.port=8000
        eureka.client.register-with-eureka=false
        eureka.client.fetch-registry=false
        
        eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/

    ```
    
    * `eureka.client.register-with-eureka` ：表示是否将自己注册到Eureka Server，默认为true。
    * `eureka.client.fetch-registry` ：表示是否从Eureka Server获取注册信息，默认为true。
    * `eureka.client.serviceUrl.defaultZone` ：设置与Eureka Server 交互的地址，查询服务和注册服务都需要依赖这个地址。  默认是`http://localhost:8761/eureka` ；多个地址可使用 , 分隔。

启动工程后，访问：http://localhost:8000/,即可看到eureka注册中心 