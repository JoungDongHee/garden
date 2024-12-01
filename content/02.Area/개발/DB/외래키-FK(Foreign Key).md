---
date: 2024 년 11 월 29 일 16 시 11 분
tags:
  - Dev
  - DB
  - FK
  - ForeignKey
  - 외래키
author: Joung Dong Hee
share: true
---

# 외래키-FK(Foreign Key)


[관계형 데이터 베이스(RDBMS)](%EA%B4%80%EA%B3%84%ED%98%95%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%B2%A0%EC%9D%B4%EC%8A%A4(RDBMS).md) 에서 관계를 지정하기 위한 Key 로  [데이터의 무결성](%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9D%98%20%EB%AC%B4%EA%B2%B0%EC%84%B1.md)을 위한 제약 조건 이다.

외래키는 한 테이블의 특정 컬럼이 다른 테이블의 **기본키(Primary Key)** 를 참조하도록 설정하며, 데이터 일관성을 유지하고, 삭제나 업데이트 시 제약 조건을 통해 예기치 않은 데이터 손실이나 변경을 방지한다.

외래키 관계에는 다음과 같은 주요 유형이 있다.

## 관계의 종류

### 1. One-to-One (1대1 관계)

한 테이블의 각 레코드가 다른 테이블의 정확히 하나의 레코드와 매칭되는 관계.

예: `Students` 테이블의 `StudentID`와 `ContactInfo` 테이블의 `StudentID`

- `Students`의 `StudentID`는 **기본키(PK)**,
- `ContactInfo`의 `StudentID`는 **외래키(FK)** 가 된다.
- 외래키의 값은 참조하는 기본키와 **동일해야 하며, 중복될 수 없다**.


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241201151383.png)


### 2. One-to-Many / Many-to-One (1대다 또는 다대1 관계)

한 테이블의 레코드가 다른 테이블의 여러 레코드와 연결되는 관계.  
예: `Customers`와 `Orders`

- `Customers`의 `CustomerID`는 **기본키(PK)**,
- `Orders`의 `CustomerID`는 **외래키(FK)** 가 된다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241201151251.png)


## Many-to-Many

다 대 다 로 맺어지는 관계를 의미 한다.

한 테이블의 값이 다른 여러개의 테이블 과 연관될때 발생한다. 보통 1 대 다 관계 에서 JOIN 을 활용하기 위한 방법으로 많이 구현이 된다.

다음과 같이 학생(Students)은 학급(Enrollments) 에 등록될수 있으며 이러한 학급의 번호는 Classes 테이블에 포함된다.


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241201151752.png)


---

### Example Tables

#### Students Table
| **StudentID (PK)** | **Name**     | **Age** |
|---------------------|--------------|---------|
| 1                   | Alice        | 15      |
| 2                   | Bob          | 16      |
| 3                   | Charlie      | 17      |

#### Classes Table
| **ClassID (PK)** | **ClassName**     | **Teacher**  |
|------------------|-------------------|--------------|
| 101              | Mathematics       | Mr. Smith    |
| 102              | Science           | Mrs. Taylor  |
| 103              | History           | Mr. Johnson  |

#### Enrollments Table (Join Table)
| **EnrollmentID (PK)** | **StudentID (FK)** | **ClassID (FK)** |
|------------------------|--------------------|------------------|
| 1                      | 1                  | 101              |
| 2                      | 1                  | 102              |
| 3                      | 2                  | 101              |
| 4                      | 3                  | 103              |
| 5                      | 2                  | 103              |

---


## 외래키(FK) 사용 시 장단점

### 장점

1. **데이터 무결성 보장**
    - 외래키는 참조 무결성을 유지하도록 강제하여, 잘못된 데이터 삽입 및 삭제를 방지한다.
2. **데이터 관계 명시**
    - 테이블 간의 명확한 관계를 정의하여, 데이터 구조를 더 쉽게 이해하고 관리할 수 있다.
3. **자동화된 데이터 관리**
    - **ON DELETE** 및 **ON UPDATE** 옵션을 통해 참조 데이터 삭제/수정 시 자동으로 동기화된다.  
        예: `ON DELETE CASCADE` 설정 시, 부모 데이터를 삭제하면 참조 데이터도 자동 삭제.
4. **쿼리 최적화**
    - 외래키는 관계형 데이터베이스의 쿼리 최적화에 활용된다. 특히 조인 쿼리에서 성능을 높일 수 있다.

### 단점

1. **복잡성 증가**
    - 복잡한 외래키 관계는 데이터베이스 설계와 관리 난이도를 높일 수 있다.
2. **데이터 삽입/삭제 제약**
    - 외래키 관계로 인해 테이블 데이터 삽입 순서가 제한된다.  
        예: 부모 테이블의 데이터가 먼저 삽입되어야 자식 테이블 데이터를 추가할 수 있다.
3. **성능 저하 가능성**
    - 많은 외래키 제약 조건은 데이터베이스 성능에 영향을 미칠 수 있다. 특히 대량의 데이터 삽입/삭제 시 트랜잭션 처리 속도가 느려질 수 있다.
4. **스키마 변경 어려움**
    - 외래키로 연결된 테이블은 구조 변경이 제한될 수 있다. 외래키를 제거하거나 관계를 재정의해야 스키마를 변경할 수 있다.

---

## 외래키 사용 시 주의할 점

1. **설계 단계에서 신중한 고려**
    - 테이블 간의 관계를 명확히 정의하고, 필요 이상으로 복잡한 외래키를 설정하지 않도록 주의한다.
2. **필요한 경우에만 사용**
    - 모든 관계를 외래키로 정의할 필요는 없다. 비즈니스 요구사항과 시스템 복잡도를 고려해 결정한다.
    - 불 필요한 외래키 설정은 성능 저하에 큰 원인이 된다.
1. **ON DELETE/ON UPDATE 제약 조건 활용**
    - 데이터 무결성을 유지하면서, 애플리케이션 로직에 적합한 제약 조건을 설정한다.
2. **성능 테스트**
    - 외래키로 인해 발생할 수 있는 성능 문제를 미리 검토하고, 필요 시 인덱스 추가 등을 통해 최적화한다.

---

# 참고


https://puleugo.tistory.com/149

https://help.claris.com/archive/help/18/fmp/en/index.html#page/FMP_Help%2Fone-to-one-relationships.html%23