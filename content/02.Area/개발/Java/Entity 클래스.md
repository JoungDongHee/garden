---
date: 2024 년 11 월 24 일 23 시 11 분
tags:
  - Dev
  - Entity
author: Joung Dong Hee
share: true
---

# Entity 

Entity 클래스는 **DB 와 1:1 로 매칭되는 핵심 클래스**이며 사용하고자 하는 테이블의 존재하는 컬럼들을 필드로 가지는 객체로 해당 테이블에 없는 필드는 가지고 있는 것을 지양해야 한다.

또한 요청(Request)이나 응답(Response) 에 사용되는 [DTO 나 VO](Java%20%20%EC%9D%98%20DTO%20%EC%99%80%20VO.md#^e94535) 와 다르게 외부에 노출되면 안되는 민감한 정보 또한 존재할수 있기때문에 요청(Request)이나 응답(Response)  에 사용을 해서는 안된다.


### DB의 테이블 구조

^390062

|Column Name|Data Type|Nullable|Description|
|---|---|---|---|
|id|BIGINT|NO|Primary Key|
|name|VARCHAR(100)|NO|이름|
|email|VARCHAR(255)|NO|이메일|
|created_at|TIMESTAMP|NO|생성일|
|updated_at|TIMESTAMP|YES|수정일|

### Java Entity Example

[테이블 구조](Entity%20%ED%81%B4%EB%9E%98%EC%8A%A4.md#^390062) 를 java 의 Entity 클래스를 만들게 되면 다음과 같이 만들수 있을 것이다.
**@Setter** 와 같이 해당 클래스에서 외부에서 손쉽게 접근하여 값을 변경 할수 없도록 해야 한다. 
Setter 를 사용하지 않음 으로서 **Entity의 불변성을 유지하고, 무분별한 상태 변경을 방지하기 위함**입니다.

```java
import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "user")
@Getter
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "name", nullable = false, length = 100)
    private String name;

    @Column(name = "email", nullable = false, unique = true, length = 255)
    private String email;

    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        this.createdAt = LocalDateTime.now();
    }

    @PreUpdate
    protected void onUpdate() {
        this.updatedAt = LocalDateTime.now();
    }
}

```