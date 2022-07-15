# PART 2 MongoDB 개발

## CHAPTER 2 인덱싱

### 5.1 인덱싱 소개
- 인덱스는 책의 인덱스와 유사하다.
- 몽고는 전체 내용을 살펴보는 대신 인덱스를 통해 특정 내용을 가리키는 정렬된 리스트를 확인한다.
- 엄청난 양의 명령을 더 빠르게 쿼리할 수 있다.
- 수정하거나 쓸 때에는 더 많은 시간이 걸린다.
    - 같은 컬렉션의 인덱스 정렬이 필요하기 때문이다.
- 인덱스를 사용하지 않는 쿼리를 `Collection scan`이라 한다.
    - 이는 서버가 쿼리 결과를 찾으려면 '전체 내용을 살펴봐야 함'을 의미한다.
- `.explain()`을 사용하면 쿼리 실행 과정을 볼 수 있다.
#### 5.1.1 인덱스 생성
```javascript
> db.user.createIndex({"username": 1})
```
- 컬렉션을 특히 크게 만들지 않는 한 인덱스는 몇 초면 충분하다.
- 
```javascript
> db.user.find().explain("executionStats");
{
        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "some.user",
                "indexFilterSet" : false,
                "parsedQuery" : {

                },
                "winningPlan" : {
                        "stage" : "COLLSCAN",
                        "direction" : "forward"
                },
                "rejectedPlans" : [ ]
        },
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 105,
                "executionTimeMillis" : 0,
                "totalKeysExamined" : 0,
                "totalDocsExamined" : 105,
                "executionStages" : {
                        "stage" : "COLLSCAN",
                        "nReturned" : 105,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 107,
                        "advanced" : 105,
                        "needTime" : 1,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "direction" : "forward",
                        "docsExamined" : 105
                }
        },
        "serverInfo" : {
                "host" : "goorm",
                "port" : 27017,
                "version" : "4.4.14",
                "gitVersion" : "0b0843af97c3ec9d2c0995152d96d2aad725aab7"
        },
        "ok" : 1
}
```

#### 5.1.2 복합 인덱스 소개
- 인덱스는 가능한 한 효율적으로 쿼리하려는 목적으로 사용한다.
- 인덱스는 모든 값을 정렬된 순서로 보관하므로 정렬 작업이 훨씬 빨라지게 된다.
- 하지만 인덱스가 앞 부분에 놓일 때만 정렬에 도움된다.
```javascript
> db.users.find().sort({"age": 1, "username": 1});
// 이것은 age로 정렬 후 username으로 정렬하는데 완전 정렬은 별로 도움이 되지 않는다.
```
- 위 커맨드를 최적화 시키려면 `age`와 `username`에 인덱스를 만들어야 한다.
```javascript
> db.users.createIndex({"age": 1, "username": 1})
// 이것은 두 개의 키를 인덱스로 만든다.
```
- 위를 ***복합 인덱스***라고 표현한다.

#### 5.1.3 몽고DB가 인덱스를 선택하는 방법
- 5개의 인덱스가 있다고 가정하자.
    - 쿼리가 들어오면 몽고는 ***쿼리 모양(query shape)***을 확인한다.
    - 모양은 검색할 필드와 정렬 여부 등 추가 정보와 관련 있다.
    - 정보를 기반으로 쿼리를 충족하는 데 사용할 인덱스 후보 집합을 식별한다.
- 쿼리가 들어오고 인덱스 5개 중 3개가 쿼리 후보로 식별됐다고 가정하자.
- 몽고는 각 인덱스 후보에 하나씩 총 3개의 ***쿼리 플랜(query plan)***을 만든다.
- 그리고 병렬 쓰레드에서 쿼리를 실행한다.
    - 이 방법은 레이스와 유사하다.
    - 가장 먼저 목표 상태에 도달하는 쿼리 플랜이 승자가 된다.
    - 플랜은 ***일정 기간(trial period)*** 동안 서로 경쟁하며, 각 레이스의 결과로 전체 승리 플랜을 산출한다.
- 승리한 쿼리 플랜은 향 후 같은 쿼리 모양에 사용되기 위하여 캐시에 저장 된다.
    - 시간이 지나서 컬렉션과 인덱스가 변경되면 쿼리 플랜이 캐시에서 제거된다.
    - 몽고는 다시 가능한 쿼리 플랜을 실험해서 해당 컬렉션 및 인덱스 집합에 가장 적합한 플랜을 찾는다.
    - 인덱스를 추가하거나, 삭제하면 플랜이 캐시에서 제거된다.
    - 쿼리 플랜 캐시는 명시적으로도 지울 수 있다.
    - **mongod** 프로세스를 다시 시작할 때에도 삭제된다.

#### 5.1.4 복합 인덱스 사용
- 두 개 이상의 인덱스를 사용하는 복합 인덱스는 매우 강력하다.
- 인덱스를 올바르게 설계하려면 실제 워크로드에서 인덱스를 테스트하고 조정해야 한다.
  - ME: 이 말은 즉, 이론적으로 best-practice를 찾는건 힘들다는 건가?
- 먼저 인덱스의 선택성을 고려한다.
  - ㅁㄴㅇㄴㅁㅇㅁㄴㅇ
- TODO: 아 이해하기 어렵다 실습을 통해서 `.explain()`을 통해 이해해봐야 할 것 같다.
- ME: `totalKeysExamined` 가 뭘 의미 하는거지? `nReturned`? 그래서 복합 인덱스는 어떻게 처리되는거지?
- 복합 인덱스를 설계할 때는 "인덱스를 사용할 공통 쿼리 패턴의 동등 필터, 다중값 필터, 정렬 구성 요소를 처리하는 방법"을 알아야 한다.
- 