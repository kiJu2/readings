# PART 1 MongoDB 시작

## CHAPTER 1 몽고DB 소개

### 1.1 손쉬운 사용

- 몽고는 도큐먼트의 키와 값을 미리 정의하지 않는다.
- 고정된 스키마가 없으므로 쉽게 필드를 추가하거나 삭제할 수 있다.

### 1.2 확장 가능한 설계

- '데이터베이스를 어떻게 확장할 것인가'와 같은 어려운 의사 결정에 직면.
- 몽고는 분산 확장을 염두에 두고 설계되었다.
- 데이터를 여러 서버에 더 쉽게 분산하게 해준다.
- 도큐먼트를 자동으로 재분배하고 사용자 요청을 올바른 장비에 라우팅함으로써 클러스터 내 데이터 양과 부하를 조절할 수 있다.

### 1.3 다양한 기능

- 인덱싱
     - unique, compound, full-text 인덱싱 기능 제공
     - 중첩된 도큐먼트(nested document) 및 배열과 같은 계층 구조의 보조 인덱스도 지원함.

- 집계
     - 몽고는 데이터 처리 파이프라인 개념을 기반으로 한 집계 프레임워크를 제공함.
     - 집계 파이프라인(aggregation pipeline)은 데이터베이스 최적화를 최대한 활용하여 서버 측에서 비교적 간단한 일련의 단계로 데이터를 처리하는 것.

- 특수한 컬렉션 유형
     - 몽고는 로그와 같은 최신데이터를 유지하고자 세션이나 고정 크기 컬렉션(capped collection)과 같이 특정 시간에 만료해야 하는 데이터에 대해 유효 시간(time to live - TTL)컬렉션을 지원한다(!!)
     - 기준 필터(criteria filter)와 일치하는 도큐먼트에 한정된 부분 인덱스(partial index)를 지원하여 효율성을 높이고 저장 공간을 줄인다.
- 파일 스토리지
     - 몽고는 큰 파일의 파일 메타데이터를 저장하는 프로토콜을 지원한다.

### 1.4 고성능

- skip

### 1.5 몽고DB의 철학

- 결국 몽고는 확장성이 높으며 유연하고 빠른, 즉 완전한 기능을 갖춘 데이터 스토리지를 만드는 일이다.

## CHAPTER 2 몽고DB 기본

### 2.0 들어가며

- 몽고 데이터의 기본 단위는 *도큐먼트*이다.
- *컬렉션*은 테이블과 유사하다.
- 몽고의 단일 인스턴스는 자체적인 컬렉션을 갖는 여러 개의 독립적인 *데이터베이스*를 호스팅한다.
- 모든 도큐먼트는 컬렉션 내에서 고유한 특수키인 \_*id*를 가진다.
- 몽고 셸이라는 간단하지만 강력한 도구와 함꼐 배포된다. 자바스크립트 해석기다.

### 2.1 도큐먼트

- 몽고의 핵심은 정렬된 키와 연결된 값의 집합으로 이루어진 *도큐먼트*다.
- 해시, 딕셔너리, 맵 등과 같이 표현되는 자료구조를 말한다.
- 데이터형과 대소문자를 구별한다.
- 키가 중복될 수 없다.

### 2.2 컬렉션

### 2.2.1 동적 스키마

- 몽고는 자유롭다. "같은 컬렉션에서도 키, 값, 데이터형 모두 다르게 설정할 수 있는데 왜 굳이 컬렉션이 필요하지?"
     1. 컬렉션 별로 나뉘어져 있으면 개발자와 관리자 모두에게 편하다.
     2. 컬렉션이 분류되어있으면 쿼리로 목록을 뽑을 때 속도가 훨씬 빠르다.(ME:정리 되어있으니까.)
     3. 데이터 지역성에 좋다.
     4. 인덱스를 정의할 수 있는데 인덱스는 컬렉션 별로 정의한다.

#### 2.2.2 네이밍

- 컬렉션은 이름으로 식별된다.
     - 빈 문자열은 안된다.
     - `\0`은 사용할 수 없다.
     - 예약어는 사용될 수 없다.
     - `$`를 포함할 수 없다.

### 2.3 데이터베이스

- 몽고는 컬렉션에 도큐먼트를 그룹화할 뿐 아니라 데이터베이스에 컬렉션을 그룹 지어 놓는다.
- 몽고는 단일 인스턴스에 여러 데이터베이스를 호스팅할 수 있다.
- 한 앱의 데이터를 동일한 데이터베이스에 저장하는 것은 좋은 방식이다.

### 2.4 몽고DB 시작

- skip

### 2.5 몽고DB 셸 소개

#### 2.5.1 셸 실행

- skip

#### 2.5.2 몽고DB 클라이언트

- skip

#### 2.5.3 셸 기본 작업

- skip

### 2.6 데이터형

#### 2.6.1 기본 데이터형

- 몽고는 'JSON'과 닮았다고 생각할 수 있다.
     - JSON의 한계
          - 기본 데이터 폼이 한정적임. (배열, 객체, 문자열, 숫사, 불리언)
          - 부동소수점과 정수형을 표현하는 방법이 없고 32비트, 64비트도 구별되지 않음.
- 몽고는 JSON의 키/값 쌍 성질을 유지하면서 추가적인 데이터형을 지원한다.
     - null을 지원한다.
     - 불리언을 지원한다.
     - 숫자를 지원한다.
     - 문자열
     - 날짜
     - 정규 표현식
     - 배열
     - 내장 도큐먼트
     - 객체 ID(ObejctId())
     - 이진 데이터
     - 코드(ME: 함수를 집어넣을 수 있다..!)

#### 2.6.2 날짜

- skip

#### 2.6.3 배열

- skip

#### 2.6.4 내장 도큐먼트

- 도큐먼트 안에 도큐먼트를 의미한다.
- 몽고는 내장 도큐먼트를 이해하고, 인덱스를 구성하고, 쿼리하며, 갱신하기 위헤 내장 도큐먼트 내부에 접근한다.
- 내장 도큐먼트를 분리하지 않으면 내장 도큐먼트를 가지고는있는 도큐먼트들을 모두 수정해야 한다.
     - ME: 그렇다고 해서 권장하지 않는다 라고는 말할 수 없을 듯.
          - 왜냐하면, 도큐먼트를 모두 1-depth를 유지하게 되면 조인이 불가피하기 때문

#### 2.6.5 \_id와 ObjectId

- `_id`는 컬렉션 내에서 고유하다.
- 따라서 각기 다른 두 컬렉션에서 같은 `_id`가 존재할 수 있다.

##### ObjectId

- ObjectId는 12바이트 스토리지를 사용한다.
- 24자리 16진수 문자열로 표현이 가능하다.
- 흔히 거대한 16진수 문자열로 표현되기는 하지만, 실제로 문자열은 저장된 데이터의 두 배만큼 길다.

```plaintext
  0 1 2 3 4 5 6 7 8 9 10 11
  타임스탬프 랜덤       카운터(랜덤 시작 값)
  ObjectId는 위와 같은 형태로 이루어진다.
```

- ObjectId의 첫 4바이트는 타임스탬프이다.
- 다음 5바이트는 랜덤한 값이다. 최종 3바이트는 서로 다른 시스템의 충돌을 방지한다.
- 9바이트 이 후부터는 1초 동안 여러 장비와 프로세스를 거쳐 유일성을 보장한다.
- 고유한 ObjectId는 프로세스당 1초에 `1677만 7216개`까지 생성된다.

### 2.7 몽고DB 셸 사용

#### 2.7.1 셸 활용 팁

- `help`를 입력하면 셸의 내장된 도움말을 볼 수 있다.
- 데이터베이스 수준의 도움말은 `db.help()`로 알 수 있다.
- 컬렉션 수준의 도움말은 `db.foo.help()`로 알 수 있다.
- 함수의 기능을 알고 싶다면 `js`처럼 괄호를 없애면 된다.
     - `db.movies.updateOne` 입력.
     - 매개변수 순서나 어떻게 동작하는지 알 수 있다.

#### 2.7.2 셸에서 스크립트 실행하기

- 단순히 명령행에 스크립트만 넘겨서 스크립트를 사용할 수도 있다.
     - `mongo script1.js script2.js` 실행.
- 배너 출력을 막고싶으면 `--quiet` 옵션을 주면 된다.
- 스크립트는 db 변수에 대한 접근 권한을 가진다.
     - 하지만 usedb나 show collection과 같은 셸 보조자는 파일에서 동작하지 않는다.
     - 셸 보조자의 `use video`는 JS 스크립트의 `db.getSisterDB("video")`와 같은 의미이다.
     - 셸 보조자의 `show dbs`는 JS 스크립트의 `db.getMongo().getDBS()`와 같은 의미이다.
     - 셸 보조자의 `show collections`는 JS 스크립트의 `db.getCollectionNames()`와 같은 의미이다.
- 셸에 스크립트를 로드할 수도 있다. `load('script1.js')`
- 기본적으로 셸은 셸을 시작한 디렉터리에서 스크립트를 찾는다.

#### 2.7.3 .mongorc.js 만들기

- 몽고는 다음과 같이 `mongorc.js`를 만들 수 있다.

```javascript
// .mongorc.js

var compliment = ["attractive", "intelligent", "like Batman"];
var index = Math.floor(Math.random() * 3);

print("ㅎㅇㅎㅇ!" + compliment + "!");
// MongoDB Shell version: 4.2.1
// connecting to: test
// ㅎㅇㅎㅇlike Batman!
```

- 위 스크립트처럼 `js`를 활용하여 설정을 할 수도 있다.

```javascript
var no = function () {
          print("Not on my watch.");
};
// DB 삭제 방지
db.dropDatabase = DB.prototypedropDatabase = no;
// 컬렉션 삭제 방지
DBCollection.prototype.drop = no;
// 인덱스 삭제 방지
DBCollection.prototype.dropIndex = no;
// 인덱스 삭제 방지
DBCollection.prototype.dropIndexes = no;

// ME: Database가 실행 된 후 스크립트가 실행되는 것으로 보인다.
```

- `--norc`를 통해 `.mongorc.js` 로딩을 비활성화 할 수 있다.

#### 2.7.4 프롬프트 커스터마이징하기

```javascript
prompt = function () {
          return new Date() + "> ";
};
```

- 마지막 작업 완료 시각을 얻으려면 위와 같이 작성하면 된다.

#### 2.7.5 복잡한 변수 수정하기

```javascript
EDITOR = "/usr/bin/emacs";
// 위처럼 emacs를 셸 편집기로 설정할 수 있다.
```

#### 2.7.6 불편한 컬렉션명

- skip

## CHAPTER 3 도큐먼트 생성, 갱신, 삭제

### 3.1 도큐먼트 삽입

#### 3.1.1 insertMany

- 에러 지점까지는 삽입이 되며, 에러 지점 이후는 삽입이 되지 않는다.

#### 3.1.2 삽입 유효성 검사

- 모든 도큐먼트는 `16mb`보다 작아야 한다.
     - 나쁜 스키마 설계를 예방하고, 일관된 성능을 보장하기 위함.
- 최소한의 검사를 하는 이유는 유효하지 않은 데이터 입력 방지를 위함.
- 그러므로 App 서버와 같은 신뢰성 있는 소스만 DB에 연결되어야 한다.
- 대부분 드라이버는 DB에 보내기 전에 유효성 검사를 한다.
     1. 도큐먼트가 너무 크지 않은지
     2. `UTF-8`이 아닌 문자열을 사용하는지
     3. 인식할 수 없는 데이터형을 포함하는지 등 확인한다.

#### 3.1.3 삽입

- 몽고 3.0 이후 부터는 `insert` 메써드가 아닌 `insertMany`와 `insertOne` 메써드를 사용하자.

#### 3.2 도큐먼트 삭제

- 몽고 3.0 이 후 부터는 `deleteOne`과 `deleteMany`를 사용하자.

#### 3.2.1 drop

- 컬렉션을 삭제하려면 `drop`을 사용하자.
- `db.movies.drop()`
- 이 전에 백업된 데이터를 복원하는 방법 외에 delete 또는 drop 작업을 취소하거나 삭제된 도큐먼트를 복구하는 방법은 없다.

### 3.3 도큐먼트 갱신

- `updateOne`, `updateMany`, `replaceOne`과 같은 갱신 메서드를 사용해 변경한다.

#### 3.3.1 도큐먼트 치환

- `replaceOne`은 도큐먼트를 새로운 것으로 완전히 치환한다.

#### 3.3.2 갱신 연산자

##### 제한자

- 갱신 연산자는 키를 변경, 추가, 제거한다.
- 배열과 내장 도큐먼트를 조작하는 복잡한 갱신 연산을 지정하는 데 사용하는 *특수키*이다.
- *제한자(modifier)*라고 부른다.

```javascript
db.users.updateOne({ name: "lee" }, { $inc: { age: "f" } });
// 위는 에러가 출력된다.
// $inc로 증가만 할 수 있도록 제한하였기 때문. 그 외 제한자들은 찾아볼 것.
```

- `$set`는 필드가 존재하지 않으면 새 필드를 생성하는 *제한자*이다.
     - 키의 데이터 형도 변경할 수 있다.
     - EXAMPLE: string to array
- `$unset`은 키와 값을 모두 제거한다.

##### 증가와 감소

- `$inc`는 `int`, `long`, `duble`, `decimal` 타입 값에만 사용할 수 있다.
     - 그 외 데이터형에서는 에러가 출력된다. 위 코드 참조.
     - `$inc`의 키 값은 무조건 숫자여야 한다.

##### 요소 추가하기

- `$push`는 이미 배열이 존재하면 요소 끝에 추가한다.
     - 배열이 존재하지 않으면 새로운 배열을 생성한다.
- `$each`는 `$push` 한 번에 여러 배열 요소를 추가할 수 있다.

```javascript
db.stock.ticker.updateOne({
    "_id": "G00G"
  }, {
    $push:{
      "hourly" : 기
        "$each": [562.44, 222.44, 123.55]
      }
    }
  });
```

- `
- `$slice`는 `$push`와 결합하여 배열이 특정 크기 이상으로 늘지 않도록 한다.

```javascript
db.stock.ticker.updateOne(
          {
                    _id: "G00G",
          },
          {
                    $push: {
                              hourly: {
                                        $each: [562.44, 222.44, 123.55],
                              },
                    },
                    $slice: -10, // 배열에 추가할 수 있는 요소의 개수를 10개로 제한.
                    // 10개보다 작으면 유지, 보다 크면 마지막 10개 요소로 유지.
                    // 큐를 만드는 데 용이함.
          }
);
```

##### 배열을 집합으로 사용하기

- `$ne`를 사용하여 배열을 집합처럼 사용할 수 있다.

```javascript
db.papers.updateOne(
          {
                    "autohrs cites": {
                              $ne: "Richie",
                    },
          },
          {
                    $push: {
                              "authors cited": "Richie",
                    },
          }
);
// 인용 목록에 저자가 존재하지 않을 때만 해당 저자를 추가.
```

- `$addToSet`을 사용하여 배열에 존재하지 않는 것들만 추가할 수 있다.

##### 요소 제거하기

- `$pop`을 사용하여 요소를 제거할 수 있다.

```javascript
db.papers.updateOne({
          "autohrs cites": {
                    $ne: "Richie",
          },
          $pop: { key: 11 },
          // 배열의 마지막 요소를 제거
          // "$pop": {"key": -1}은 첫번째 부분 제거
});
```

- `$pull`은 주어진 조건에 맞는 배열 요소를 모두 제거

##### 배열의 위치 기반 변경

- 위치 연산자(`$`)를 사용한다.

```json
"comments": [
  {
    "comment": "good post",
    "author": "John",
    "votes": 0,
  },
  {
    "comment": "hello",
    "author": "Pooop",
    "votes": -2,
  },
  {
    "comment": "good post",
    "author": "max",
    "votes": 2,
  },
  {
    "comment": "good post hh",
    "author": "alex",
    "votes": 4,
  },
]
```

- 첫 번째 comments를 수정하고자 할 때는 `db.blog.updateOne({"post" : post_id}, {"$inc": {"comments.0.votes": 1}})`를 사용하면 된다.
- 다음과 같이 사용할 수도 있다. `db.blog.updateOne({"comments.author" : "john"}, {"$set": {"comments.$.author": "jim"}})`

##### 배열 필터를 이용한 갱신

- `arrayFilters`를 이용하여 특정 조건에 맞는 배열 요소를 갱신할 수 있다.

```javascript
db.blog.updateOne(
          {
                    post: post_id,
          },
          {
                    $set: { "comments.$[elem].hidden": true },
          },
          {
                    arrayFilters: [{ "elem.votes": { $lte: -5 } }],
          }
);
```

#### 3.3.3 갱신 입력

- 도큐먼트가 존재하면 갱신을, 도큐먼트가 존재하지 않으면 입력을 수행한다.
- `upsert`라고 표현한다.(ME: `update` + `insert`)
- 세번째 옵션으로 추가할 수 있다.

```javascript
db.anaytics.updateone(
  {
    "url": "/blog"
  },
  {
    "$inc": {
      {
        "$pageviews": 1
      }
    }
  },
  {
    "upsert": true
  }
)
```

##### 저장 셸 보조자

- `save`도 갱신 입력을 돕는다.
- 하나의 파라메터를 받는 함수이다.
- `_id`가 없다면 입력을 수행하며 `_id`가 있다면 갱신을 수행한다.

#### 3.3.4 다중 도큐먼트 갱신

- `updateMany`는 다중 도큐먼트 갱신 함수이다.
- skip

#### 3.3.5 갱신한 도큐먼트 반환

- 수정된 도큐먼트를 반환하는게 중요한데 다음과 같은 메써드들을 사용하자.
     - `findOneAndDelete`
     - `findOneAndReplace`
     - `findOneUpdate`
- 해당 메써드는`mutual exclusion` 목적도 가지고 있다.

## CHAPTER 4 쿼리

### 4.1 find 소개

#### 4.1.1 반환받을 키 지정

- `find`의 두번째 매개인자는 반환 받을 키를 지정하는 용도임.
- skip

#### 4.1.2 제약 사항

- 데이터베이스에서 쿼리 도큐먼트 값은 상수여야 한다.
- skip

### 4.2 쿼리 조건

#### 4.2.1 쿼리 조건절

- `<`, `<=`, `>`, `>=`에 해당하는 비교 연산자는 각각 `$lt`, `$lte`, `$gt`, `$gte`이다.
- skip

#### 4.2.2 OR 쿼리

- OR 쿼리는 두 가지 방법이 있다.
- `$in`과 `$or`이다.
- skip

#### 4.2.3 $not

- `$not`은 메타 조건절이며 어떤 조건에도 적용할 수 있다.
- skip

### 형 특정 쿼리

#### 4.3.1 null

- ME: `js`에서 객체에 `null`을 찾는것과 같음.
     - 예를 들어 `null`의 `falsy` 특징
     - `find`로 없는 값을 쿼리하면 null을 가진 키도 검색이 되는 등.
- skip

#### 4.3.2 정규 표현식

- skip

#### 4.3.3 배열에 쿼리하기

```javascript
db.food.insertOne({ fruit: ["apple", "banana", "peach"] });
// 위 도큐먼트를 다음과 같이 찾을 수 있다.
db.food.find({ fruit: "banana" });
```

- 유효한 도큐먼트는 아니지만 위 `db.food.insertOne({ fruit: ["apple", "banana", "peach"] });` 도큐먼트를 다음과 같다고 가정하자.

```javascript
{"fruit": "apple", "fruit":"banana", "fruit": "peach"}
```

##### $all 연산자
- 배열로 쿼리를 할 수 있도록 도와준다.
- 예를 들어 `['apple', 'banana']`로 쿼리를 할 수 있다.

##### $size 연산자
- 크기의 범위로 쿼리할 수 있다.
- ref: [Doc](https://www.mongodb.com/docs/manual/reference/operator/query/size/?_ga=2.7690866.1116110856.1655795628-1824102457.1630638152&_gac=1.120583546.1655795637.Cj0KCQjw2MWVBhCQARIsAIjbwoP5yQGje28mw85qhj5fzBNUYcDZBR_t2-RctN86O8nwMiUcaKPGbF0aAnqSEALw_wcB)

##### $slice 연산자
- 배열 요소를 자르는 연산자.
- ref: [Doc](https://www.mongodb.com/docs/manual/reference/operator/update/slice/?_ga=2.206517587.1116110856.1655795628-1824102457.1630638152&_gac=1.191821400.1655795637.Cj0KCQjw2MWVBhCQARIsAIjbwoP5yQGje28mw85qhj5fzBNUYcDZBR_t2-RctN86O8nwMiUcaKPGbF0aAnqSEALw_wcB)

#### 4.4 $where 쿼리
- 키/값 쌍만으로 꽤 다양한 쿼리를 할 수 있지만 표현할 수 없는 쿼리도 있다.
- 이 때 `$where`절을 사용하여 임의의 자바스크립트를 쿼리의 일부분으로 실행하면 모든 쿼리를 표헌할 수 있다.
- 다만, 보안상의 이유로 `$where` 절 사용을 제한해야 한다.
```javascript
db.foo.insertOne({"apple": 1, "banana": 6, "peach": 3})
db.foo.insertOne({"apple": 8, "spinach": 4, "watermelon": 4})

// 위 상황에서 spinach와 watermelon이 둘 다 4이기에 $ 조건문을 사용할 수 없다
// $where절로 쿼리를 만들어낼 수 있다.

db.foo.find({"$where": function () {
  for (var current in this) {
    for (var other in this) {
      if (current != other && this[current] == this[other]) {
        return true;
      }
    }
  }
}})
```

#### 4.5 커서
##### 4.5.0
- 데이터베이스는 커서를 사용해 `find`의 결과를 반환한다.
```javascript
> var here = db.user.find();
> here;

/*
{ "_id" : ObjectId("62a7eb6b852fcfb331a3b914"), "name" : "hello" }
{ "_id" : ObjectId("62a7ec08852fcfb331a3b915"), "name" : "lee", "age" : 27 }
*/
> here;
/* 아무것도 출력되지 않는다. 커서가 끝까지(2) 다 순회했기 때문으로 추측(ME) */
> here.hasNext();
// false

> var here = db.user.find();
> here.hasNext();
// true
> here.next();
// { "_id" : ObjectId("62a7eb6b852fcfb331a3b914"), "name" : "hello" }

> here.forEach((v) => print(v));
/*
[object BSON]
[object BSON]
*/
```
- ME: 다만, 웹서버에서 혹은 mongodb 내에서 위처럼 `find`로 불러와서 하나씩 탐색하면 성능 이슈는 없을까?
  - ME: 객체의 양이 매우 크다면?
    - ME: 아마도 `default max-size`가 있지 않을까
  - find 호출할 때 셸이 몽고를 즉시 쿼리하지 않는다.(ME: 해결)
  - 결과를 요청하는 쿼리를 보낼 때까지 기다린다.
  - 그 전에 옵션을 추가할 수 있다.
    - ex: `sort`, `limit`, `skip` 등
  - 셸은 `next` 혹은 `hasNext` 메서드 호출 시 서버 왕복 횟수를 줄이기 위해 적은 값을 반환한다.
    - `100개 도큐먼트` 혹은 `4MB` 더 적은 것을 가져온다.

##### 4.5.1 제한, 건너뛰기, 정렬
- 쿼리가 데이터베이스에 전송 되기 전에 추가해야 한다.
```javascript
> db.c.find().limit(3)
// 도큐먼트 반환을 3개로 제한을 둔다.
> db.c.find().skip(3)
// 3개를 건너뛴다.
> db.c.find().sort({username: 1, age: -1})
// 1은 오름차순, -1은 내림차순.
```
- 몽고는 데이터형을 비교하는 위계 구조가 있다.
  1. 최솟값
  2. null
  3. 숫자(int, long, double, decimal)
  4. 문자열
  5. 객체/도큐먼트
  6. 배열
  7. 이진 데이터
  8. 객체ID
  9. 불리언
  10. 날짜
  11. 타임스탬프
  12. 정규 표현식
  13. 최댓값
##### 4.5.2 많은 수의 건너뛰기 피하기
- `skip`은 생략된 결과물들을 모두 찾고 폐기한다.
  - 이는 성능 저하에 심각한 원인이 될 수 있다.
```javascript
var latest = null;

var page1 = db.user.find().limit(10);
// 첫 페이지
while (page1.hasNext()) {
  latest = page1.next();
  print(latest);
}

// 다음 페이지
var page2 = db.user.find();
page2.limit(10);
```
- ME: 위 처럼하고 `next` 쓰면 된다는 것 같은데, 그럼 skip과 다를게 뭐지? 어차피 둘 다 폐기하는건 똑같은 것 같은데
##### 4.5.3 종료되지 않는 커서
- 커서는 두가지 측면이 있다.
  1. 클라이언트가 보는 커서
  2. 클라이언트가 나타내는 데이터베이스 커서
- 서버측에서의 커서는 메모리와 리소스를 점유한다.
- 커서가 더는 가져올 결과가 없거나, 클라이언트로부터 종료 요청을 받으면 몽고는 리소스를 해제한다.
- 그럼 합당한 이유로 리소스를 해제해야 할 것이다.
- 서버가 커서를 종료하는 몇 가지 조건이 있다.
  1. 커서는 조건에 일치하는 결과를 모두 살펴본 후 스스로 정리한다.
  2. 클라이언트 측에서 유효 영역을 벗어나면 드라이버는 몽고에 메시지를 보내 커서를 종료해도 된다고 알린다.
  3. 커서가 여전히 유효 영역 내에 있더라도 10분 동안 활동이 없으면 커서는 죽는다.
  4. 따라서 어떠한 충돌이나 버그가 있더라도 몽고에서 활동중인 커서가 수천개가 될 일은 없다.
- 대부분의 앱은 위 원칙을 따르지만 예외로 타임아웃을 시키지 못하게 할 수도 있을 것이다.
  - 이는 `immortal` 함수로 해결할 수 있다.

