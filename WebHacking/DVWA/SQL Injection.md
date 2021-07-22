# ⚜️SQL Injection⚜️
### SQL Injection이란 악의적인 사용자가 보안상의 취약점을 이용하여, 임의의 SQL 문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위이다.
<br>

### 종류

- Error based SQL Injection : 논리적 에러를 이용한 SQL Injection은 가장 많이 쓰이고, 대중적인 공격 기법입니다.

 ‘ OR 1=1 -- 로  WHERE 절에 있는 싱글쿼터를 닫아주기 위한 싱글쿼터와 OR 1=1 라는 구문을 이용해 WHERE 절을 모두 참으로 만들고, -- 를 넣어줌으로 뒤의 구문을 모두 주석 처리 해주었습니다.

![R1280x0](https://user-images.githubusercontent.com/86994067/126586069-a31cd147-ab9a-4476-ac32-cd9165de75b1.png)

<br>

- Union based SQL Injection :  SQL 에서 Union 키워드는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테이블로 보여주게 하는 키워드 입니다. 정상적인 쿼리문에 Union 키워드를 사용하여 인젝션에 성공하면, 원하는 쿼리문을 실행할 수 있게 됩니다. Union Injection을 성공하기 위해서는 두 가지의 조건이 있습니다. 하나는 Union 하는 두 테이블의 컬럼 수가 같아야 하고, 데이터 형이 같아야 합니다. 

![R1280x0](https://user-images.githubusercontent.com/86994067/126589468-956034bb-0086-4c92-a199-c050f96f7716.png)

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

- Stored Procedure SQL Injection

-> 저장 프로시저(Stored Procedure) 은 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것입니다. 공격에 사용되는 대표적인 저장 프로시저는 MS-SQL 에 있는 xp_cmdshell로 윈도우 명령어를 사용할 수 있게 됩니다. 단, 공격자가 시스템 권한을 획득 해야 하므로 공격난이도가 높으나 공격에 성공한다면, 서버에 직접적인 피해를 입힐 수 있는 공격 입니다.

<br>

- Mass SQL Injection

->  2008년에 처음 발견된 공격기법으로 기존 SQL Injection 과 달리 한번의 공격으로 다량의 데이터베이스가 조작되어 큰 피해를 입히는 것을 의미합니다. 보통 MS-SQL을 사용하는 ASP 기반 웹 애플리케이션에서 많이 사용되며, 쿼리문은 HEX 인코딩 방식으로 인코딩 하여 공격합니다. 보통 데이터베이스 값을 변조하여 데이터베이스에 악성스크립트를 삽입하고, 사용자들이 변조된 사이트에 접속 시 좀비PC로 감염되게 합니다. 이렇게 감염된 좀비 PC들은 DDoS 공격에 사용됩니다.

<br>

---

<br>

# Low Level

1~5까지 넣어보면 각각 아이디에 저장된 데이터베이스 정보를 확인할 수 있다.

<img width="716" alt="스크린샷 2021-07-22 오후 3 10 12" src="https://user-images.githubusercontent.com/86994067/126596454-d09abf98-e92c-4896-9d81-569e8384c8d8.png">

이때 다음과 같은 명령어 `‘ OR 1=1 --`로 논리적 에러를 이용한 SQL Injection 공격을 시도하면 (- 대신 #을 이용해서 뒷내용을 주석처리 해준다.)
<img width="707" alt="스크린샷 2021-07-22 오후 3 11 15" src="https://user-images.githubusercontent.com/86994067/126596676-5c8e318a-6a7e-433a-92b3-d62bc8859f1e.png">

데이터베이스 내에 정보들이 출력되면서 정보를 확인할 수 있다.


<br>

# Medium Level




<br>

# High Level

