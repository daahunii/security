# ⚜️SQL Injection⚜️
### SQL Injection이란 악의적인 사용자가 보안상의 취약점을 이용하여, 임의의 SQL 문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위이다.
<br>

- Blind SQL Injection (Boolean based SQL, Time based SQL)


  - Boolean based SQL : Blind SQL Injection은 데이터베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용합니다. 로그인 폼에 SQL Injection이 가능하다고 가정 했을 때, 서버가 응답하는 로그인 성공과 로그인 실패 메시지를 이용하여, DB의 테이블 정보 등을 추출해 낼 수 있습니다.
  
  ![R1280x0](https://user-images.githubusercontent.com/86994067/126589681-cc8d0bf5-7f30-4e1f-b371-404a10321432.png)

-> 위의 그림은 Blind Injection을 이용하여 데이터베이스의 테이블 명을 알아내는 방법입니다. (MySQL) 인젝션이 가능한 로그인 폼을 통하여 악의적인 사용자는 임의로 가입한 abc123 이라는 아이디와 함께 
`
abc123’ and ASCII(SUBSTR(SELECT name From information_schema.tables WHERE table_type=’base table’ limit 0,1)1,1)) > 100 --`이라는 구문을 주입합니다.

해당구문은 MySQL 에서 테이블 명을 조회하는 구문으로 limit 키워드를 통해 하나의 테이블만 조회하고, SUBSTR 함수로 첫 글자만, 그리고 마지막으로 ASCII 를 통해서 ascii 값으로 변환해줍니다. 만약에 조회되는 테이블 명이 Users 라면 ‘U’ 자가 ascii 값으로 조회가 될 것이고, 뒤의 100 이라는 숫자 값과 비교를 하게 됩니다.  거짓이면 로그인 실패가 될 것이고, 참이 될 때까지 뒤의 100이라는 숫자를 변경해 가면서 비교를 하면 됩니다.  공격자는 이 프로세스를 자동화 스크립트를 통하여 단기간 내에 테이블 명을 알아 낼 수 있습니다.

<br>

  - Time based SQL : Time Based SQL Injection 도 마찬가지로 서버로부터 특정한 응답 대신에 참 혹은 거짓의 응답을 통해서 데이터베이스의 정보를 유추하는 기법입니다. 사용되는 함수는 MySQL 기준으로 SLEEP 과 BENCHMARK 입니다.
  
  ![R1280x0](https://user-images.githubusercontent.com/86994067/126589984-2cf1bbb0-473a-44aa-8904-c1359bfce9e9.png)

-> 위의 그림은 Time based SQL Injection을 사용하여 현재 사용하고 있는 데이터베이스의 길이를 알아내는 방법입니다. 로그인 폼에 주입이 되었으며 임의로 abc123 이라는 계정을 생성해 두었습니다. 
악의적인 사용자가 `abc123’ OR (LENGTH(DATABASE())=1 AND SLEEP(2)) – `이라는 구문을 주입하였습니다. 여기서 LENGTH 함수는 문자열의 길이를 반환하고, DATABASE 함수는 데이터베이스의 이름을 반환합니다.

주입된 구문에서, LENGTH(DATABASE()) = 1 가 참이면 SLEEP(2) 가 동작하고, 거짓이면 동작하지 않습니다. 이를 통해서 숫자 1 부분을 조작하여 데이터베이스의 길이를 알아 낼 수 있습니다. 만약에 SLEEP 이라는 단어가 치환처리 되어있다면, 또 다른 방법으로 BENCHMARK 나 WAIT 함수를 사용 할 수 있습니다. BENCHMARK 는 BENCHMARK(1000000,AES_ENCRYPT('hello','goodbye')); 이런 식으로 사용이 가능합니다. 이 구문을 실행 하면 약 4.74초가 걸립니다.

<br>

---

<br>

# Low Level



<br>

# Medium Level



<br>

# High Level





