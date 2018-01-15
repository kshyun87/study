#PostgreSQL Insert

## 데이터 저장하기

Django(python web framework)를 사용하다보면 여러 디비중 PostgreSQL를 사용하는 경우가 있다.(궁합이 잘 마자는다고 하는 말이 있음)
PostgreSQL소개는 다음에서 찾아보기 바란다.

장고에서는 쿼리문을 생각할 필요없이 Django ORM을 통해서 작성된 코드가 알아서 쿼리를 만들어서 수행했는데 일반적인 클라이언트는 psycopg2라는 패키지를 통해서 접근할 수 있다.

## 데이터 입력 방법
```python
import psycopg2 as pg2

conn = pg2.connect()
cur = conn.cursor()
conn.autocommit = True

sql = ""
cur.excute(sql,('parms...'))
```
먼저 디비와 커넥션(연결)을 맺는다.
그리고 나서 autocommit을 true로 설정했다.
쿼리문을 작동하는건 간단하다.
sql문을 작성하고 excute()메소드로 수행하면 된다.
이때 sql문 전체를 작성하는 것이 아닌 파라미더(인자)들은 제외하고 작성한다.

```python
sql_A = "INSERT INTO table_name VALUES (%s, %s)"
sql_B = "INSERT INTO table_name VALUES (a, b)"
cur.excute(sql_A, ("a","b"))
cur.excute(sql_B)
```

위와 같이 두가지 버전이 있는데 권장은 sql_A이다.
그 이유는 excute()에 있다.
ㄷㅌ쳣ㄷ()에서 사용하는 인자를 보면 sql문과 파라미터를 받는다.
즉 들어가는 파라미터를 메소드에 전달해주면 되는 방식이다.
이러면 sql문이 간결해져서 관리에 좋은것 같다.

참고로 저장하는데 필요한 필드 수가 일치해야하고 순서대로 입력되니 명심해야한다.

## Autoincrease는?
id로 Autoincrease를 사용하는 경우가 있다.
이런 경우 입력할 때 어떻게 해야할까?

일단 입력을 해보자.
```python
sql_A = "INSERT INTO table_name VALUES (%s, %s)"
cur.excute(sql_A, ("a","b"))
```
 만약 위의 코드와 같이 입력했을때 Autoincrease가 있는 id값이 "a"가 입력된다. 그럼 에러가 나면서 입력되지 않는다.
 숫자를 선언하면 상관은 없다. 단 매번 증가하는 값을 알고 있어야 한다.

 그래서 다음과 같이 입력하면된다.
 ```python
 sql_A = "INSERT INTO table_name VALUES (default, %s)"
 cur.excute(sql_A, ("b"))
 ```
 이렇게 입력하면 id값은 DB에서 설정한 default값으로 채우라는 의미이다. 그래서 자동증가를 사용했기에 자동으로 증가한 값을 입력해서 저장하게 된다.
