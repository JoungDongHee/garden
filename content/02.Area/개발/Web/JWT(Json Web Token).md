---
date: 2024 년 12 월 23 일 16 시 12 분
tags:
  - Dev
  - JWT
  - Web
author: Joung Dong Hee
finish: true
share: true
---

# JWT(Json Web Token)

JWT는 비연결성(Connectionless)을 가진 HTTP에서 사용자를 인증하기 위한 방법 중 하나입니다. 인증 방식에는 [SESSION](SESSION.md) 기반과 토큰 기반 방식이 있으며, JWT는 토큰 방식에서 널리 사용됩니다.

JWT는 **Header (헤더)**, **Payload (내용)**, **Signature (서명)** 로 구성된 문자열이며, 각 부분은 `.`으로 구분됩니다.


```text
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJKb2UifQ.1KP0SsvENi7Uz1oQc07aXTL7kpQG5jBNIybqr60AlD4
```

## HEADER

헤더에는 알고리즘 과 토큰 타입을 정의할수 있다. 지원 하는 알고리즘의 경우 지원하는 알고리즘에 따라 다르며 
[[https://github.com/jwtk/jjwt |JJWT]] 는 다음과 같이 지원한다.

|Identifier|Signature Algorithm|
|---|---|
|`HS256`|HMAC using SHA-256|
|`HS384`|HMAC using SHA-384|
|`HS512`|HMAC using SHA-512|
|`ES256`|ECDSA using P-256 and SHA-256|
|`ES384`|ECDSA using P-384 and SHA-384|
|`ES512`|ECDSA using P-521 and SHA-512|
|`RS256`|RSASSA-PKCS-v1_5 using SHA-256|
|`RS384`|RSASSA-PKCS-v1_5 using SHA-384|
|`RS512`|RSASSA-PKCS-v1_5 using SHA-512|
|`PS256`|RSASSA-PSS using SHA-256 and MGF1 with SHA-256**1**|
|`PS384`|RSASSA-PSS using SHA-384 and MGF1 with SHA-384**1**|
|`PS512`|RSASSA-PSS using SHA-512 and MGF1 with SHA-512**1**|
|`EdDSA`|Edwards-curve Digital Signature Algorithm**2**|

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```


## PLAYLOAD 

JWT 에 포함 되는 데이터를 정의할수 있다.  PLAYLOAD 에 담는 정보를  클레임(**claim**) 라고 부르고 name / value 의 한쌍으로 이뤄져 있다. 

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

클레임 에도 종류가 존재하며 다음과 같다.
1. 등록된 (**registered**) 클레임
2. 공개 (**public**) 클레임,
3. 비공개 (**private**) 클레임

### 등록된 (**registered**) 클레임

이미 정해진 클레임 으로 프로그래밍 에서의 *예약어* 와 동일하다. 즉 이미 사용하고 있는 `name` 이기 때문에 해당 `name` 을 다른 용도로 사용할수 없다.

- `iss`: 토큰 발급자 (issuer)
- `sub`: 토큰 제목 (subject)
- `aud`: 토큰 대상자 (audience)
- `exp`: 토큰의 만료시간 (expiraton), 시간은 NumericDate 형식으로 되어있어야 하며 (예: 1480849147370) 언제나 현재 시간보다 이후로 설정되어있어야합니다.
- `nbf`: Not Before 를 의미하며, 토큰의 활성 날짜와 비슷한 개념입니다. 여기에도 NumericDate 형식으로 날짜를 지정하며, 이 날짜가 지나기 전까지는 토큰이 처리되지 않습니다.
- `iat`: 토큰이 발급된 시간 (issued at), 이 값을 사용하여 토큰의 `age` 가 얼마나 되었는지 판단 할 수 있습니다.
- `jti`: JWT의 고유 식별자로서, 주로 중복적인 처리를 방지하기 위하여 사용됩니다. 일회용 토큰에 사용하면 유용합니다.

### 공개 (public) 클레임

공개 클레임들은 충돌이 방지된 (collision-resistant) 이름을 가지고 있어야 합니다. 충돌을 방지하기 위해서는, 클레임 이름을 [ URL](URI%20,%20URL%20,%20URN.md#^c13188%20) 형식으로 짓습니다.

```json
{
	"https://example.com/role": "admin" 
}
```


### 비공개 (private) 클레임

클라이언트와 서버 간 협의된 데이터로 사용자 정보 등 민감한 정보를 담을 수 있습니다:

```json
{
    "username": "test"
}
```


## SIGNATURE

Signature는 헤더와 Payload를 결합한 후 비밀 키(secret)를 사용하여 해시 알고리즘으로 생성된 서명입니다. 이를 통해 토큰의 무결성과 신뢰성을 검증할 수 있습니다. 결과 값은 다시 Base64 URL-safe 방식으로 인코딩됩니다.

```scss
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```


# JWT 장점

1. JWT 는 [SESSION](SESSION.md) 과 다르게 서버에서 정보를 가지고 있을 필요가 없다. 오로지 해당 토큰 의 유효성 만 검증하면 되기 때문에 사용되는 리소스의 양 또한 적다.
2. 사용되는 리소스의 양이 적기 때문에 대량의 트래픽 에서도 큰 문제 없이 사용가능하다.
3. MSA(Microservices Architecture) 환경과 같이 여러 서버 간 인증 정보 공유가 필요한 경우 활용하기 적합합니다.

# JWT의 단점 및 주의사항

1. **보안 취약점**  
    Payload는 Base64로 인코딩된 것이지 암호화된 것이 아니므로, 누구나 디코딩하여 내용을 확인할 수 있습니다. 민감한 데이터(예: 비밀번호)는 포함하면 안 됩니다.
2. **탈취 문제**  
    JWT가 탈취되면 만료될 때까지 사용을 차단할 방법이 없습니다. 이를 보완하기 위해 다음 방법을 고려해야 합니다:
    - 토큰 블랙리스트를 유지
    - 짧은 만료 시간 설정
    - Refresh Token을 사용하여 주기적으로 갱신
3. **크기 문제**  
    JWT는 토큰 크기가 크기 때문에 네트워크 사용량 증가를 초래할 수 있습니다.


--- 

#  참고


https://velopert.com/2389

https://jwt.io/

