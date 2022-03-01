# Programmers-SQL-Kit
SQL kit in programmers(MySQL)

## SELECT
> 모든 레코드 조회하기
```SQL
SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID
```
> 역순 정렬하기
```SQL
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC
```
> 아픈 동물 찾기
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION = 'Sick' ORDER BY ANIMAL_ID
```
> 어린 동물 찾기
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE NOT INTAKE_CONDITION = 'Aged' ORDER BY ANIMAL_ID
```

> 동물의 아이디와 이름
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS ORDER BY ANIMAL_ID
```

> 여러 기준으로 정렬하기
```SQL
SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME, DATETIME DESC
```
## SUM, MAX, MIN
> 최댓갑 구하기
```SQL
SELECT MAX(DATETIME) FROM ANIMAL_INS
```
> 최솟값 구하기
```SQL
SELECT MIN(DAtETIME) FROM ANIMAL_INS
```
> 동물 수 구하기
```SQL
SELECT COUNT(*) FROM ANIMAL_INS
```
> 중복 제거하기
```SQL
SELECT COUNT(DISTINCT NAME) FROM ANIMAL_INS
```

## GROUP BY
> 고양이와 개는 몇 마리 있을까?
```SQL
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE
```

> 동명 동물 수 찾기
```SQL
SELECT NAME, COUNT(NAME) FROM ANIMAL_INS GROUP BY NAME having count(name) >= 2 ORDER BY NAME
```


> 입양 시각 구하기(1)
```SQL
SELECT HOUR(DATETIME) AS HOUR, count(*) AS COUNT 
FROM ANIMAL_OUTS WHERE HOUR(DATETIME) BETWEEN 9 and 19
GROUP BY HOUR(DATETIME)
ORDER BY HOUR(DATETIME)
```


> 입양 시각 구하기(2)
```SQL
SET @HOUR = -1;

SELECT 
(@HOUR := @HOUR + 1) AS HOUR,
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE @HOUR = HOUR(DATETIME)) AS COUNT
FROM ANIMAL_OUTS
WHERE @HOUR < 23
```

## IS NULL
> 이름이 없는 동물의 아이디
```SQL
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NULL
```
> 이름이 있는 동물의 아이디
```SQL
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NOT NULL ORDER BY ANIMAL_ID
```

> NULL 처리하기
```SQL
SELECT
ANIMAL_TYPE,
IFNULL(name, "No name") AS NAME,
SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

## JOIN
> 없어진 기록 찾기
```SQL
SELECT ANIMAL_ID, NAME
FROM ANIMAL_OUTS 
WHERE ANIMAL_ID 
NOT IN (SELECT animal_id from animal_ins)
```
> 있었는데요 없었습니다
```SQL
SELECT A.ANIMAL_ID, A.NAME 
FROM ANIMAL_INS A
JOIN ANIMAL_OUTS B
ON (A.ANIMAL_ID = B.ANIMAL_ID)
WHERE A.DATETIME > B.DATETIME 
ORDER BY A.DATETIME
```
> 오랜 기간 보호한 동물(1)
```SQL
SELECT NAME, DATETIME 
FROM ANIMAL_INS 
WHERE ANIMAL_ID NOT IN (SELECT ANIMAL_ID FROM ANIMAL_OUTS)
ORDER BY DATETIME LIMIT 3
```
> 보호소에서 중성화한 동물
```SQL
SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME FROM ANIMAL_OUTS A
JOIN ANIMAL_INS B ON (A.ANIMAL_ID = B.ANIMAL_ID) 
WHERE NOT A.SEX_UPON_OUTCOME = B.SEX_UPON_INTAKE
ORDER BY A.ANIMAL_ID
```

## String, Date
> 루시와 엘라 찾기
```SQL
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Sabrina', 'Mitty')
```
> 이름에 el들어가는 동물 찾기
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS 
WHERE NAME LIKE '%el%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```
> 중성화 여부 파악하기
```SQL
SELECT ANIMAL_ID, NAME, 
CASE 
    WHEN SEX_UPON_INTAKE LIKE '%Spayed%' OR SEX_UPON_INTAKE LIKE '%Neutered%' THEN 'O'
    ELSE 'X' 
END AS '중성화'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

> 오랜 기간 보호한 동물(2)
```SQL
SELECT A.ANIMAL_ID, A.NAME FROM ANIMAL_INS A
JOIN ANIMAL_OUTS B ON(A.ANIMAL_ID = B.ANIMAL_ID)
ORDER BY B.DATETIME - A.DATETIME DESC 
LIMIT 2
```

> DATETIME에서 DATE로 형 변환
```SQL
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') 
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID
```
