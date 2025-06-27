# spring-microservices-cheatsheet

Spring Cloud Gateaway - мощный инструмент для маршрутизации и управления HTTP-запросами в микросервисной архитектуре. Он выступает в роли API Gateway и предоставляет множество полезных функций.
Минимальная конфигурация:

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

