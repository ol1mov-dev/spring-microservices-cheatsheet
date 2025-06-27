# spring-microservices

<p>Spring Cloud Gateaway - мощный инструмент для маршрутизации и управления HTTP-запросами в микросервисной архитектуре. Он выступает в роли API Gateway и предоставляет множество полезных функций.</p>
<p>Минимальная конфигурация:</p>

<code>
  <pre>
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
        include: "*"</pre>
</code>

<h2>Eureka Server</h2>
<p>Eureka Server — это сервер для регистрации и обнаружения микросервисов (Service Discovery), разработанный Netflix и интегрированный в Spring Cloud. Он помогает микросервисам находить друг друга без жёсткой привязки к URL или IP-адресам.</p>
<p>В микросервисной архитектуре сервисы динамически масштабируются, их количество и расположение меняются. Eureka решает две основные задачи:</p>

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

<p>🔹 Как работает Eureka? </p>
Eureka Server — центральный сервер, хранящий информацию о всех зарегистрированных сервисах.

Eureka Client — библиотека, встроенная в микросервисы, которая регистрирует сервис и запрашивает данные о других.

<code>
<pre>@EnableEurekaClient
  @SpringBootApplication
  public class OrderServiceApplication {
    public static void main(String[] args) {
      SpringApplication.run(OrderServiceApplication.class, args);}
  }</pre>
</code>