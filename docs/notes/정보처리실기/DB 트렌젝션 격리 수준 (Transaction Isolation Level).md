### 정의

- 동시에 여러 트렌젝션 처리시 각 트렌젝션의 고립도 규칙을 정함.

![](https://velog.velcdn.com/images/csk917work/post/b3ebce02-4621-4445-b46a-99f2d2a7629b/image.png)

### 트렌젝션 일관성과 동시성의 역설

![](https://velog.velcdn.com/images/csk917work/post/fd906a30-8a5d-4390-af55-830d26f64417/image.png)

### LV.0 READ UNCOMMITED

#### 정의

- 커밋되지 않은 데이터에 접근 가능.

#### 발생가능 문제

- **Dirty read** : UPDATE 또는 INSERT가 되기 전에 읽어서 최신 데이터로 읽을 수 없거나 오류가 발생함.

### LV.1 READ COMMITED

#### 정의

- 이미 커밋된 데이터만 조회 가능.

#### 발생가능 문제

- **Non-repeatable Read** : 하나의 트렌젝션이 같은 값 조회시 다른 값이 검색되는 현상. 첫 번째 조회시 트렌젝션하고 두 번째 조회시 트렌젝션 정보가 달라짐.

### LV.2 REAPEATABLE READ

#### 정의

- 트렉젝션 완료 전까지 한 번 조회한 값은 계속 같은 걸로 조회됨.

#### 발생가능 문제

- **Phantom Read** : 유령처럼 데이터가 사라지가너 없어지는 현상. 트렌젝션 시적점에서 없던 데이터가 끝나는 시점에서는 조회가 됨.

### LV.3 SERIALIZABLE

#### 정의

- 트렌젝션이 특정 테이블을 읽을시 다른 트렌젝션은 그 테이블 데이터 접근 불가.

#### 발생가능 문제

- Dirty Read, Non-Repeatable Read, Phantom Read 등의 정합성 문제는 없지만 동시 처리 성능이 가장 떨어짐.

### 출처

[https://powerdev.tistory.com/44](https://powerdev.tistory.com/44)

[https://raisonde.tistory.com/entry/데이터베이스-회복-기법-정리](https://raisonde.tistory.com/entry/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%9A%8C%EB%B3%B5-%EA%B8%B0%EB%B2%95-%EC%A0%95%EB%A6%AC) [지식잡식]

[https://velog.io/@jude0124/Transaction-Isolation-Level](https://velog.io/@jude0124/Transaction-Isolation-Level)

[https://jordy-torvalds.tistory.com/121](https://jordy-torvalds.tistory.com/121)