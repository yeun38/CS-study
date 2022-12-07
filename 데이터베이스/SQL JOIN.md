## SQL JOIN
- JOIN이란 **'연결하다'** 라는 뜻을 지닌 단어이다. 데이터베이스에서는 둘 이상의 테이블을 연결해서 테이블을 검색하는 방법을 이야기 한다.

## JOIN의 종류
![](https://velog.velcdn.com/images/cil05265/post/5490e38a-93d3-412f-a5c0-e41b8029c416/image.png)

### INNER JOIN
![](https://velog.velcdn.com/images/cil05265/post/51d6f911-f457-4328-b035-bee43d1fd293/image.png)

- INNER JOIN은 위의 그림처럼 두 개의 테이블에서 공통된 요소들을 통해 결합하는 조인방식이다. 
- 가장 일반적인 조인으로, 조인 사용시 명령어로 INNER JOIN 대신 JOIN 만을 입력해도 INNER JOIN이 사용된다.

```sql
SELECT Star.Name, Dep.DepName
From Star JOIN Dep ON Star.DepNo = Dep.DepNo
```

### OUTER JOIN
- 위의 **INNER JOIN**은 공통된 부분이 있는 행만 출력을 해주었는데, 공통된 부분이 없는 데이터도 함께 보고싶은 경우가 있을 것이다.
- 이럴 때 사용하는 것이 바로 **OUTER JOIN**이다.
- OUTER JOIN은 크게 **LEFT(OUTER) JOIN과 RIGHT(OUTER) JOIN, FULL OUTER JOIN**으로 나눌 수 있다.

#### LEFT(OUTER) JOIN
![](https://velog.velcdn.com/images/cil05265/post/969e6025-ac1e-4a3f-a51c-d9f30de3472e/image.png)

```sql
SELECT Star.Name, Dep.Name
FROM Star LEFT JOIN Dep
ON Star.DepNo = Dep.DepNo
```

- 공통된 값 + 왼쪽 테이블에만 있는 값이 출력된다.

- 반면, 여기서 만약 공통된 부분마저 제외하고 왼쪽에만 있는 것을 출력하고 싶다면 다음과 같이 NULL을 이용하면 된다.

![](https://velog.velcdn.com/images/cil05265/post/8cea5465-fb35-4de0-8eab-b71fd2e1acf7/image.png)

```sql
SELECT Star.Name, Dep.Name
From Star LEFT JOIN Dep
ON Star.DepNo = Dep.DepNo
WHERE Star.DepNo IS NULL
```

#### RIGHT(OUTER) JOIN
![](https://velog.velcdn.com/images/cil05265/post/aa6cc4ec-4f1e-4e01-81a4-4d4f5119191c/image.png)


```sql
SELECT Star.Name, Dep.Name
FROM Star RIGHT JOIN Dep
ON Star.DepNo = Dep.DepNo
```

- 여기서 만약 공통된 부분마저 제외하고 오른쪽에만 있는 것을 출력하고 싶다면 다음과 같이 NULL을 이용하면 된다.

```sql
SELECT Star.Name, Dep.Name
From Star LEFT JOIN Dep
ON Star.DepNo = Dep.DepNo
WHERE Dep.DepNo IS NULL
```

#### FULL OUTER JOIN
![](https://velog.velcdn.com/images/cil05265/post/f4fc1181-8e54-4f62-b570-58828413fd55/image.png)

SELECT Star.Name, Dep.Name
FROM Star FULL OUTER JOIN Dep
ON Star.DepNo = Dep.DepNo

```sql
SELECT Star.Name, Dep.Name
FROM Star FULL OUTER JOIN Dep
ON Star.DepNo = Dep.DepNo
```
- LEFT OUTER JOIN과 RIGHT OUTER JOIN의 결과값을 합친 것이다.


### CROSS JOIN
### SELF JOIN
