---
date: 2024 년 11 월 27 일 00 시 11 분
tags:
  - Dev
  - DB
  - Troubleshooting
author: Joung Dong Hee
share: true
---

# Mysql To Many Connetion 에러

Mysql 서버에 [DB Connection](DB%20Connection.md) 이 설정된 최대의 커넥션을 초과 한 경우 발생하는 에러 


Mysql  의 경우 아무런 설정을 하지 않을경우 기본 설정으로 **최대 커넥션 갯수는 151** 

그리고 만약 이 **151** 커넥션 숫자를 초과하게 될 경우 `To Many Connetion` 메시지 와 함께 DB 에서 커넥션을 생성하지 못하면서 데이터를 가져오지 못함

최대 커넥션 수 는 다음 sql 로 확인이 가능 하다. 여기서 `max_connections` 이 현재 DB 의 최대 커넥션 수를 의미 한다.


```sql
show variables like '%max_connect%';
```

| Variable_name         | Value |     |
| --------------------- | ----- | --- |
| extra_max_connections | 1     |     |
| max_connect_errors    | 100   |     |
| max_connections       | 151   |     |

또한 다음 쿼리로 현재 DB 의 상태를 확인할수 있다.

```sql
show status like "%connect%";
```

| Variable_name                                 | Value               |     |
| --------------------------------------------- | ------------------- | --- |
| Aborted_connects                              | 200                 |     |
| Aborted_connects_preauth                      | 21                  |     |
| Connection_errors_accept                      | 0                   |     |
| Connection_errors_internal                    | 179                 |     |
| Connection_errors_max_connections             | 0                   |     |
| Connection_errors_peer_address                | 0                   |     |
| Connection_errors_select                      | 0                   |     |
| Connection_errors_tcpwrap                     | 0                   |     |
| Connections                                   | 51663               |     |
| Max_used_connections                          | 152                 |     |
| Max_used_connections_time                     | 2024-11-25 13:30:19 |     |
| Performance_schema_session_connect_attrs_lost | 0                   |     |
| Slave_connections                             | 0                   |     |
| Slaves_connected                              | 0                   |     |
| Ssl_client_connects                           | 0                   |     |
| Ssl_connect_renegotiates                      | 0                   |     |
| Ssl_finished_connects                         | 0                   |     |
| Threads_connected                             | 121                 |     |
