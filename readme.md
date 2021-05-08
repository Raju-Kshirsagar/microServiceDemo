# Spring boot microservice example [![Build Status](https://travis-ci.com/subhashlamba/spring-microservices.svg?branch=master)](https://travis-ci.com/subhashlamba/spring-microservices)


This is example of spring boot microservice example with Eureka Server + Eureka Client + Zuul routing

## Checkout repository


```sh
> git clone https://github.com/subhashlamba/spring-microservices.git
> cd spring-boot-microservices-example
```



## Step 1: Start all services

### 1.1 For windows:

```sh

mvn clean install -f .\spring-boot-cloud-eureka-server\pom.xml
mvn clean install -f .\spring-boot-cloud-zuul-routing\pom.xml
mvn clean install -f .\spring-boot-cloud-eureka-user-service\pom.xml
mvn clean install -f .\spring-boot-cloud-eureka-order-service\pom.xml
mvn clean install -f .\spring-boot-cloud-authentication-service\pom.xml

START "Server" java -jar spring-boot-cloud-eureka-server/target/eureka-server.jar 
START "API Gateway" java -jar spring-boot-cloud-zuul-routing/target/zuul-api-gateway.jar --server.port=8080 
START "User Service" java -jar spring-boot-cloud-eureka-user-service/target/user-service.jar --server.port=8181
START "Order Service" java -jar spring-boot-cloud-eureka-order-service/target/order-service.jar --server.port=8282
START "Authentication Service" java -jar spring-boot-cloud-authentication-service/target/authentication-service.jar --server.port=8383


## Step 2: Check Eureka Server

Eureka server is running 8761 port, Now let's open it. Where we can check that:

* 3 instance of account-server is running.
* 1 instance of zuul-service is running.


### Eureka server : [http://localhost:8761/](http://localhost:8761/)

![eureka server](eureka-server.PNG)


```
curl -X POST \
  http://localhost:8080/oauth/token \
  -H 'authorization: Basic amF2YWRldmVsb3BlcnpvbmU6c2VjcmV0' \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: d2629b30-a7cf-fa72-df64-d71118afb549' \
  -F grant_type=password \
  -F username=zone1 \
  -F password=mypassword
```

It will return authenication token

```
{
    "access_token": "imdUX2_t_WQLSTUlaLBTjVyHUTg",
    "token_type": "bearer",
    "refresh_token": "zLufOQtLQO1u-8JP7KN64Dsc3wc",
    "expires_in": 522,
    "scope": "read write"
}
```

Create User

```
curl -X POST \
  http://localhost:8080/user/ \
  -H 'authorization: Bearer imdUX2_t_WQLSTUlaLBTjVyHUTg' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: d73bb63f-969c-4081-1dee-77fa2b0f1f5a' \
  -d '{"firstName":"subhash", "lastName": "Lamba"}'
 
 ```

Place order
 
```
curl -X POST \
  http://localhost:8080/order/ \
  -H 'authorization: Bearer DF977lxKiBzdWKTkZsE7XevqK40' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: 2256b83e-30dc-3b8e-26f0-8d13c0dcfd44' \
  -d '{"userId":1, "orderDate": "2021-01-01"}'
```

Get User

```
curl -X GET \
  http://localhost:8080/user/1 \
  -H 'authorization: Bearer DF977lxKiBzdWKTkZsE7XevqK40' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: 5163fb23-f54f-c793-9faa-e9df54eab9d5' \
  -d '{"userId":1, "orderDate": "2021-01-01"}'
```

```
{
    "user": {
        "id": 1,
        "firstName": "subhash",
        "lastName": "Lamba"
    },
    "orders": [
        {
            "id": 5,
            "orderDate": "2021-01-01T00:00:00.000+00:00",
            "userId": 1
        }
    ]
}
```





