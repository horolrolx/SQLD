## SELECT 절의 구조와 순서
- FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY 순서

<img width="619" alt="스크린샷 2024-08-10 오후 3 14 07" src="https://github.com/user-attachments/assets/76154aaa-59f6-4e30-8883-5459da6e8327">


## WHERE절 특징
- WHERE절에서 함수를 사용하는 것은 가능하지만, 집계 함수(SUM, AVG, COUNT 등)를 직접 WHERE절에 사용하는 것은 허용되지 않습니다. 이는 WHERE절이 각 행을 개별적으로 평가하고 필터링하는데 사용되기 때문입니다. 집계 함수는 여러 행의 데이터를 요약하여 하나의 결과를 생성하는데, 이러한 연산은 WHERE절이 처리되기 전에 개별 행을 대상으로 조건을 적용할 때는 수행할 수 없습니다.

- 예를 들어, WHERE SUM(SALA) > 20000은 잘못된 사용 예입니다. 여기서 SUM(SALA)는 여러 행의 SALA값을 합산하는데, 이는 WHERE절에서 수행할 수 있는 개별 행에 대한 조건이 아닙니다.

- 이런 조건을 적용하고나 할 떄는 HAVING절을 사용해야 합니다. HAVING절은 GROUP BY절과 함께 사용되어, 그룹화된 결과에 대해 조건을 적용합니다. 예를 들어, 총합이 20000을 초과하는 그룹을 찾고자 한다면, 쿼리는 다음과 같이 작성됩니다

```sql
SELECT department, SUM(SALA) AS total_salary
FROM employee
GROUP BY department
HAVING SUM(SALA) > 20000;
```

- 이 쿼리는 employee 테이블에서 department 별로 SALA의 합을 계산하고, 그 합이 20000을 초과하는 부서만 선택합니다. HAVING 절은 GROUP BY로 생성된 그룹에 대해 조건을 적용하기 때문에, 여기서는 집계 함수의 결과에 조건을 적용하는 것이 적절합니다.