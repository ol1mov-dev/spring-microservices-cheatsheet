# spring-microservices

Spring Cloud Gateaway - мощный инструмент для маршрутизации и управления HTTP-запросами в микросервисной архитектуре. Он выступает в роли API Gateway и предоставляет множество полезных функций.</p>
Минимальная конфигурация:

```yaml
  spring:
  application:
    name: api-gateaway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
          lower-case-service-id: true
  server:
  port: 8082
  error:
    whitelabel:
      enabled: false
  eureka:
  client:
    service-url:
      default-zone: http://localhost:8761/eureka # Адрес обращаясь на который наши микросервисы будут регистрироваться
  management:
  endpoints:
    gateway:
      enabled: true
    web:
      exposure:
        include: "*"
```

# Eureka Server
<hr>
Eureka Server — это сервер для регистрации и обнаружения микросервисов (Service Discovery), разработанный Netflix и интегрированный в Spring Cloud. Он помогает микросервисам находить друг друга без жёсткой привязки к URL или IP-адресам.</p>

В микросервисной архитектуре сервисы динамически масштабируются, их количество и расположение меняются. Eureka решает две основные задачи:</p>

<ol>
    <li> Регистрация сервисов. Каждый микросервис при старте отправляет свои данные (имя, адрес, порт) в Eureka.</li>
    <li> Обнаружение сервисов. Когда микросервисы требуют других микросервисов, они спрашивают Eureka.</li>
</ol>

<p> 👉 Пример: </p>
<blockquote>
Сервис order-service хочет обратиться к user-service.

Вместо указания конкретного URL он спрашивает у Eureka: "Где находится user-service?"

Eureka возвращает доступные экземпляры user-service, и order-service выбирает один из них (с балансировкой нагрузки).
</blockquote>

### 🔹 Как работает Eureka?
Eureka Server — центральный сервер, хранящий информацию о всех зарегистрированных сервисах.

Eureka Client — библиотека, встроенная в микросервисы, которая регистрирует сервис и запрашивает данные о других.


### Eureka Server Configuration

```java
@EnableEurekaServer
@SpringBootApplication
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

```yaml
spring:
  application:
    name: eureka-server
  server:
    port: 8761
  error:
    whitelabel:
      enabled: false
  eureka:
    client:
      fetch-registry: false
      register-with-eureka: false
      service-url:
        default-zone: http://localhost:8761/eureka
```


### Eureka Client Configuration
```java
    @EnableEurekaClient
    @SpringBootApplication
    public class OrderServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(OrderServiceApplication.class, args);
        }
    }
```

```yaml
spring:
  application:
    name: products-service
server:
  port: 0
  error:
    whitelabel:
      enabled: false
eureka:
  instance:
    instance-id: ${spring.application.name}:${random.uuid} # Нужно чтобы каждый экземпляр был уникальным.
  client:
    service-url:
      default-zone: http://localhost:8761/eureka # Адрес обращаясь на который наши микросервисы будут регистрироваться
```