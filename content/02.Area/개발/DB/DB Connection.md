---
date: 2024 년 10 월 09 일 18 시 10 분
tags:
  - Dev
  - DB
  - Connection
share: true
---
# DB 커넥션

**DB에서의 커넥션**은 애플리케이션 서버와 DB 간의 **논리적 연결**을 의미합니다. 커넥션을 통해 애플리케이션 서버는 DB에 쿼리를 전달하고, 그 결과를 받아오며, **INSERT**, **UPDATE**, **DELETE** 등의 작업을 수행할 수 있습니다.

Java에서는 **JDBC**(Java Database Connectivity)라는 라이브러리를 사용하여 DB와 통신하고, 커넥션을 맺고 끊습니다.

## Example Code

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.sql.SQLException;

public class DatabaseConnectionExample {

    // JDBC URL, 사용자 이름, 비밀번호는 해당 DB 설정에 맞게 변경
    private static final String JDBC_URL = "jdbc:mysql://localhost:3306/mydatabase";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "password";

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        try {
            // 1. JDBC 드라이버 로드 (Java 6 이상에서는 생략 가능)
            Class.forName("com.mysql.cj.jdbc.Driver");

            // 2. 커넥션을 맺음 (DB에 연결)
            conn = DriverManager.getConnection(JDBC_URL, DB_USER, DB_PASSWORD);

            // 3. Statement 객체 생성
            stmt = conn.createStatement();

            // 4. SQL 쿼리 실행 (SELECT 예시)
            String sql = "SELECT id, name FROM users";
            rs = stmt.executeQuery(sql);

            // 5. 쿼리 결과 처리
            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                System.out.println("ID: " + id + ", Name: " + name);
            }

        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            // 6. 자원 정리 (역순으로 close)
            try {
                if (rs != null) rs.close();
                if (stmt != null) stmt.close();
                if (conn != null) conn.close(); // 커넥션 끊기
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```

^b425b7


### 설명

위 코드 중 `conn = DriverManager.getConnection(JDBC_URL, DB_USER, DB_PASSWORD)` 부분이 **DB와 통신을 위해 커넥션을 생성하는** 부분입니다. ^52338f

이후 `finally` 를 통해 에러가 발생해도 반드시 커넥션이 끊어지도록 `conn.close()`를 한다.
### 커넥션 자원의 관리

커넥션은 **DB에서 관리하는 유한한 자원**이므로, **사용이 끝난 후 반드시 자원을 반환**(즉, 커넥션을 닫는 작업)을 해주어야 합니다. 이를 소홀히 할 경우, 커넥션이 계속해서 열려 있는 상태로 남아 자원이 고갈될 수 있습니다.

만약 커넥션을 닫지 않고 유지할 경우, 데이터베이스는 더 이상 새로운 커넥션을 처리할 수 없게 되어 **"Too many connections"** 같은 에러가 발생할 수 있습니다.


> [!danger] Error
> java.sql.SQLException: Too many connections



### 커넥션 부족에 따른 문제점

커넥션이 부족한 경우, 새로운 요청이 들어와도 커넥션 풀에서 사용할 커넥션이 반환될 때까지 대기 상태가 됩니다. 이로 인해 애플리케이션의 응답 시간이 길어지거나 **시스템 장애**가 발생할 수 있습니다. 특히, 커넥션이 고갈되면 시스템이 정상적으로 동작하지 않으며, 심각한 성능 저하를 초래할 수 있습니다.