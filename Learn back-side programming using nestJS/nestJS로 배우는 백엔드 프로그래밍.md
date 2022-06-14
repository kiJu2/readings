# nestJS로 배우는 백엔드 프로그래밍
## 들어가기 전에
```
백엔드를 개발할 때 express로 개발하고 있으나 흔히 백엔드에서 사용되는 DI, AOP, IoC, 테스트 등을 다루지 못하였다. 
백엔드 아키텍쳐에 관련된 개념을 찾아보다가 Spring의 구조가 잘 구조화 되어있다고 느꼈으나 구름의 어플리케이션 전반은 js로 짜여져있으므로 너무 동떨어진 느낌이었다고 느꼈는데 nestJS가 spring js 판이라고 할 수 있을 정도로 유사하며, 잘 짜여진 아키텍쳐 위에서 동작한다고 느껴서 nestJS를 통하여 백엔드 아키텍쳐를 학습해보고자 한다.
본 서적은 기본 개념부터 알려주므로 이미 알고 있는 개념의 챕터는 무시하며 독서할 것이다. 
```

## Hello NestJS
### Express vs NestJS
- Express
	- 가볍게 테스트 서버를 띄울 수 있음.
	- 확장되는 라이브러리를 찾는 데에 코스트가 많이 소비됨
	- 커뮤니티가 가장 많이 활성화 되어있음.
	- 자유도가 매우 높은 편.
- NestJS
	- alemfdnpdj, IoC, CQRS등 이미 많은 기능을 프레임워크 자체에 탑재.
	- 사용자는 문서로부터 대부분의 기능을 구현할 수 있음.
	- ME: 진입장벽이 높은 편인 듯

## NestJS를 배우기 전에
### 웹 프레임워크
- 웹 개발을 하는데에 필요한 골조 역할
- ME: 그냥 쉽게 웹 + 프레임워크
### NodeJS
- 단일 쓰레드에서 구동되는 non-blocking I/O 이벤트 기반 비동기 방식
	- 백그라운드에서는 쓰레드 풀을 구성해서 작업을 처리함.
	- 개발자가 직접 쓰레드 풀을 건드리지는 않고 NodeJS에 탑재된 libuv가 그 역할을 수행함
	> <b>*libuv</b>: NodeJS에서 사용하는 비동기 IO 라이브러리. C 기반이며 OS 커널을 사용하여 처리할 수 있는 비동기 작업이 있으면 바로 커널로 넘겨버림. 이 후 작업이 종료되어 커널로부터 시스템 콜 받으면 이벤트 루프에 콜백을 등록.
### 이벤트 루프
- ME: <b>해당 장은 깊숙히 다룰 필요가 있으므로 지금은 스킵함 2022.03.30</b>
### 패키지 의존성 관리
- npm은 Semantic Version Rules을 따른다.
	- `[Major].[Minor].[Patch]-[label]` 로 구분된다.
		- `label`을 제외한 `Major`, `Minor`, `Patch`는 숫자로 표기된다.
		- `Major` : 이전 버전과 호환이 불가능할 때 숫자를 하나 증가시킴.
			- ME: 예를 들면 osX 카탈리나 -> 몬터레이로 이해 됨.
		- `Minor`: 기능이 추가되는 경우 숫자가 증가됨. 하위 호환성을 깨트리지 않음.
		- `Patch`: 버그 수정 패치
		- `label`: pre, alpha, beta와 같이 부가 설명을 표기함.
- package-json 분석
	- .skip
- package.lock
	- .skip

### Typescript
- .skip

### Decorator

#### 데코레이터
- 실험적 단계이나, 안정적이므로 실제 서비스에서 많이 사용된다.
- 자바의 어노테이션과 유사(ME: 거의 동일한 듯)
- 클래스, 메서드, 접근자, 프로퍼티, 매개변수에 적용 가능하다.
##### 다음과 같이 적용 가능하다.
```typescript
class CreateUserDto {
	@IsEmail()
	readonly email: string;
}
```
##### 데코레이터 작성
```typescript
function deco(value: string) {
  console.log('데코레이터가 평가됨');
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log(value);
  }
}

class TestClass {
  @deco('HELLO')
  test() {
    console.log('함수 호출됨')
  }
}
// 데코레이터가 평가됨
// HELLO
// 함수 호출됨
```

##### 데코레이터 합성
```typescript
function first() {
  console.log("first(): factory evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("first(): called");
  };
}

function second() {
  console.log("second(): factory evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("second(): called");
  };
}

class ExampleClass {
  @first()
  @second()
  method() {
    console.log('method is called');
  }
}
// first(): factory evaluated
// second(): factory evaluated
// second(): called
// first(): called
// method is called
```
- first와 second는 평가, return되는 값은 호출이라고 칭한다.
- 평가는 위에서 아래순으로, 호출은 아래에서 위로 호출되는 것을 알 수 있다.

## 애플리케이션의 관문 - 인터페이스
### 컨트롤러
- MVC 패턴에서 말하는 컨트롤러를 말한다.
- .skip
#### 라우팅
```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```
- 위와 같은 구조를 가진다.
- 데코레이터를 적극 활용함으로써 개발자는 핵심 로직에만 집중할 수 있도록 도와준다.

#### 와일드 카드
```typescript
@Get('he*lo')
getHello(): string {
  return this.appService.getHello();
}
```
- 위와 같이 `*`를 사용하여 문자열을 대체할 수 있음.
- 이건 비단 라우팅 뿐 아니라 컴포넌트에서 이름 정할 때에도 쓰임.

#### 요청 객체
- Express처럼 라우터 파라메터에 `Req() req:Request`와 같이 req에 접근할 수 있다.
- ME: 이는 NestJS가 Express 위에서 동작하기 때문에 Express의 기능들을 열어두었기 때문이다.

#### 응답
```typescript
@Get()
findAll(@Res() res) {
  const users = this.usersService.findAll()

  return res.status(200).send(users);
}
// 위는 Express방식으로 응답해주는 형태이나 NestJS에서는 다음과 같이 응답하길 권장함.
// 우리 구름도 위와 같은 형태.
import { HttpCode } from '@nestjs/common';

@HttpCode(202)
@Patch(':id')
update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
  return this.usersService.update(+id, updateUserDto);
}
```
- 각 성공 상태 코드
	- 200
		- 서버가 요청을 제대로 처리하였음.
	- 201
		- 성공적으로 요청되었으며 새 리소스가 생성됨. 주로 Post로 보낼 때 반환 됨.
	- 202
		- 일단 요청은 성공됨. 그런데 아직 처리할 방법이 없거나 처리 중임.
		- ME: 요청는건 성공하였으나 결과는 서버도 모르니까 보장할 수 없다는 뜻.

#### 헤더
- .skip

#### 리디렉션
```typescript
@Get('redirect/docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}
```
- `http://localhost:3000/redirect/docs?version=5` 를 입력하면 `https://docs.nestjs.com/v5/` 페이지로 이동됨
- `302`는 상태코드(임시 이동)임.

#### 라우트 파라미터
```typescript
@Delete(':userId/memo/:memoId')
deleteUserMemo(@Param() params: { [key: string]: string }) {
  return `userId: ${params.userId}, memoId: ${params.memoId}`;
}
```
- 객체 형태로 파라메터를 받는 방법.
- 권장되지는 않음.
```typescript
@Delete(':userId/memo/:memoId')
deleteUserMemo(
  @Param('userId') userId: string,
  @Param('memoId') memoId: string,
) {
  return `userId: ${userId}, memoId: ${memoId}`;
}
```
- 위와 같이 작성하는 것을 권장함.
#### 하위 도메인 라우팅
- `etc/hosts`에서 도메인을 추가한다.
```typescript
...
127.0.0.1 api.localhost
127.0.0.1 v1.api.localhost
```
- 그리고 Controller에서 하위 도메인을 처리한다.
```typescript
@Controller({ host: 'api.localhost' }) // localhost로 변경
export class ApiController {
  @Get()
  index(): string {
    return 'Hello, API';
  }
}
```
- 또한 params를 활용하여 버저닝을 적용할 수도 있다.
```typescript
@Controller({ host: ':version.api.localhost' })
export class ApiController {
  @Get()
  index(@HostParam('version') version: string): string {
    return `Hello, API ${version}`;
  }
}
```
#### 페이로드 다루기
```typescript
// createUser.dto.ts
export class CreateUserDto {
  name: string;
  email: string;
}
// users.controller.ts
@Post()
create(@Body() createUserDto: CreateUserDto) {
  const { name, email } = createUserDto;

  return `유저를 생성했습니다. 이름: ${name}, 이메일: ${email}`;
}
```
- DTO는 raw data를 객체화시킬 때 사용 된다.
- db에서의 데이터나 클라이언트와 교신 시 데이터 등에서 가공시킬 때 사용된다.

### 유저 서비스의 인터페이스
- DTD의 쓰임새를 느낄 수 있는 코드는 다음과 같다.
```typescript
import { Body, Controller, Get, Param, Post, Query } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';
import { UserLoginDto } from './dto/user-login.dto';
import { VerifyEmailDto } from './dto/verify-email.dto';
import { UserInfo } from './UserInfo';

@Controller('users')
export class UsersController {
  @Post()
  async createUser(@Body() dto: CreateUserDto): Promise<void> {
    console.log(dto);
  }

  @Post('/email-verify')
  async verifyEmail(@Query() dto: VerifyEmailDto): Promise<string> {
    console.log(dto);
    return;
  }

  @Post('/login')
  async login(@Body() dto: UserLoginDto): Promise<string> {
    console.log(dto);
    return;
  }

  @Get('/:id')
  async getUserInfo(@Param('id') userId: string): Promise<UserInfo> {
    console.log(userId);
    return;
  }
}
```
### 관점 지향 프로그래밍(AOP)
- 관점 지향 프로그래밍은 횡단 관심사의 분리를 허용함으로써 모듈성을 증가시키는 것이 목적인 프로그래밍 패러다임이다.
- ME: 해당 패러다임은 Spring에서 잘 나타나있으므로 Spring 중요한 세 특징으로 찾아봄.
  - *<b>횡단 관심사(cross-cutting cencern) : </b> 유효성 검사, 로깅, 보안, 트랜잭션과 같이 앱 내 전반에 걸쳐서 제공되는 공통 요소.
- ME: 이해한 바로는 OOP는 유지하되 횡단 관심사를 모듈화시켜서 공통 모듈로 사용한다는 뜻 같음.

## 핵심 도메인 로직을 포함하는 프로바이더
### Provider
- ME: 컨트롤러는 Goorm의 Router와 같은 역할.
  - ME: 비즈니스 로직, 데이터 가공 등을 Router로 처리한다면 너무나 복잡해질 것임. 따라서 각각의 역할을 분리시키고 모듈화해야 함. 단일책임원칙에도 더 잘 어울림.
  - 예를 들어서
- 프로바이더는 서비스(Service), 레포지토리(Repository), 팩토리(Factory), 헬퍼(Helper) 등 여러가지 형태로 구현이 가능.
  - ME: 각각의 개념은 소프트웨어 아키텍쳐를 다루는 다른 자료를 참고할 것.
    - TODO: 위 아키텍쳐 자료 조사해보기
- Nest의 Provider의 핵심은 의존성 주입(DI)임.
```typescript
// 생성자를 이용한 의존성 주입
class BurgerChef {
    private BurgerRecipe burgerRecipe;

    public BurgerChef(BurgerRecipe burgerRecipe) {
        this.burgerRecipe = burgerRecipe;
    }
}

class BurgerRestaurantOwner {
    private BurgerChef burgerChef = new BurgerChef(new HamburgerRecipe());

    public void changeMenu() {
        burgerChef = new BurgerChef(new CheeseBurgerRecipe());
    }
}
```
```typescript
// 메서드를 이용한 의존성 주입
class BurgerChef {
    private BurgerRecipe burgerRecipe = new HamburgerRecipe();

    public void setBurgerRecipe(BurgerRecipe burgerRecipe) {
        this.burgerRecipe = burgerRecipe;
    }
}

class BurgerRestaurantOwner {
    private BurgerChef burgerChef = new BurgerChef();

    public void changeMenu() {
        burgerChef.setBurgerRecipe(new CheeseBurgerRecipe());
    }
}
```
- ***의존성 주입(중요함)***
  - ME: 클래스 모델 혹은 코드에는 런타임 시점(인스턴스화 시점)의 의존 관계가 드러나면 안됨.
    - 즉, 인터페이스로 연결되어야 하며, 의존관계는 제 3자가 클래스 모델이 인스턴스화 될 때 주입해주어야 함.
  - 의존관계 : 의존대상 B가 변하면, 그것이 A에 영향을 미친다.
  - 클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않는다. 그러기 위해서는 인터페이스만 의존하고 있어야 한다.
  - 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.
  - 의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로써 만들어진다.
### 프로바이더 등록과 사용
```typescript
// cats.service.ts
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```
```typescript
// cats.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```
- 위처럼 `@injectable`로 클래스를 감싸주면 의존성을 주입할 수 있다는 것을 nestJS에게 알려주게 된다.
```typescript
// 속성 기반 주입
@Injectable()
export class ServiceB extends BaseService {
  constructor(private readonly _serviceA: ServiceA) {
    super(_serviceA);
  }

  getHello(): string {
    return this.doSomeFuncFromA();
  }
}
```
```typescript
// 속성 기반 주입 using @Inject
export class BaseService {
  @Inject(ServiceA) private readonly serviceA: ServiceA;
    ...

    doSomeFuncFromA(): string {
    return this.serviceA.getHello();
  }
}
```
- ME: 위 코드들을 보면 신기한 점이 있음. services를 인스턴스화 시키지 않았음에도 사용될 수 있는건 nest가 `@injectable`를 module에서 `provider`로 지정했을 때 의존성 주입과 함께 인스턴스화 시키는 거로 보임.(추측)

### 스코프

- ***_멀티 테넌시_***
  - 하나의 인스턴스가 여러 사용자에게 각각 다르게 동작하도록 하는 SW 아키텍쳐.
  - 반대로 각 사용자마자 인스턴스가 새로 만들어지는 것은 ***_멀티 인스턴스_*** 방식
- DEFAULT: 싱글톤 인스턴스가 전체 어플리케이션에서 공유 됨. 인스턴스 수명 === 어플리케이션면수명, 부트 스트랩(시스템이 구동 되는 것) 과정을 마치면 모든 싱글톤 프로바이더의 인스턴스가 만들어짐.
- REQUEST: 들어오는 요청마다 별도의 인스턴스가 생성됨. 요청을 처리하고 나면 쓰레기 수집 됨.
- TRANSIENT: 임시 프로바이더를 주입하는 각 컴포넌트는 새로 생성된 전용 인스턴스를 주입받게 됨.
- Provider, Controller에 각각의 Scope를 지정할 수 있음.
- Controller -> Service -> Repository 와 같은 종속성 그래프에서 Service가 REQUEST이고 나머지는 DEFAULT일 때, Controller는 Service를 의존하므로 REQUEST가 됨. 그렇지만 Repository는 의존하지 않으므로 DEFAULT.

### 커스텀 프로바이더
#### Value Provider
```typescript
// 모의 객체 선언
const mockCatsService = {
/* 테스트에 적용할 값을 변경한다
...
*/
};

@Module({
imports: [CatsModule],
providers: [
  {
    provide: CatsService,
    useValue: mockCatsService,
  },
],
})
export class AppModule {}
```
  - `Provider`는 `CatsService`를 지정하지만, 실제 값은 `mockCatsService`를 사용하겠다는 것.
  - ME: `absctract class`를 `provide`로 지정하고 사용 될 `class`를 `useValue`로 사용하면 좀 더 결합성이 많이 느슨해질지도?
#### Class Provider
- 
```typescript
const configServiceProvider = {
  provide: ConfigService,
  useClass:
    process.env.NODE_ENV === 'development'
      ? DevelopmentConfigService
      : ProductionConfigService,
};

@Module({
  providers: [configServiceProvider],
})
export class AppModule {}
```
- 위처럼 `ConfigService`를 사용(참조?)하면서 실제 클래스는 `useClass`로 지정해줌.
- ME: 노드 환경을 dev, prov, def 등등을 지정해줄 때 용이할 듯.
- ME: 혹은 추상화 시킬 때도 좋을 것 같음.
#### Factory Provider
```typescript
const connectionFactory = {
  provide: 'CONNECTION',
  useFactory: (optionsProvider: OptionsProvider) => {
    const options = optionsProvider.get();
    return new DatabaseConnection(options);
  },
  inject: [OptionsProvider],
};

@Module({
  providers: [connectionFactory],
})
export class AppModule {}
```
- ___함수 실행 과정 중 다른 provider가 필요하다면___ 주입받아 사용할 수 있음.

#### Alias provider
```typescript
const loggerAliasProvider = {
  provide: 'AliasedLoggerService',
  useExisting: LoggerService,
};

@Module({
    ...
  providers: [LoggerService, loggerAliasProvider],
    ...
})
export class AppModule {}
//----------------------------------------------------
@Controller()
export class AppController {
  constructor(
    @Inject('AliasedLoggerService') private readonly serviceAlias: any,
  ) {}

  @Get('/alias')
  getHelloAlias(): string {
    return this.serviceAlias.getHello();
  }
}
```
- 위처럼 다른 이름으로 별칭을 목적으로 사용될 수 있음.
#### export
```typescript
const connectionFactory = {
  provide: 'CONNECTION',
  useFactory: (optionsProvider: OptionsProvider) => {
    const options = optionsProvider.get();
    return new DatabaseConnection(options);
  },
  inject: [OptionsProvider],
};

@Module({
  providers: [connectionFactory],
  exports: connectionFactory, /* or ['CONNECTION'] */,
})
export class AppModule {}
```
- 다른 모듈을 사용하려면 해당 모듈에서 `export`를 해주어야 함. 이는 토큰으로 내보낼 수도 있음.

## SW복잡도를 낮추기 위한 모듈 설계
### 모듈 - 응집성 있는 설계
#### 모듈 다시 내보내기
- imports, exports로만 만들어진 모듈로 디펜던시 트리를 잘 정립할 수 있다.
#### 전역 모듈
```typescript
@Global()
@Module({
  providers: [CommonService],
  exports: [CommonService],
})
export class CommonModule { }
```
- 헬퍼나 DB 연결 같은 전역적으로 사용 될 모듈은 Global로 지정할 수 있다.
## 동적 모듈을 활용한 환경변수 구성
### 동적 모듈
- 모듈을 인스턴스화 할 때 값을 다르게하여 동적으로 인스턴스화 해줌
- 대표적으로 Config가 있음.
### dotenv를 이용한 Config 설정
- ME: Keep.
### Nest에서 제공하는 Config 패키지
```typescript
...
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule.forRoot()],
  ...
})
export class AppModule { }
```
- `forRoot()` 함수는 동적 모듈을 리턴하는 정적 메서드.
### 유저서비스에 환경변수 구성하기
- `joi` 라이브러리 - 환경변수 유효성 검사에 사용 됨
  - ME: 주로 외부 모듈 이야기로 구성 되어있으므로 스킵
### IoC와 DI
#### IoC ?
- 제어의 역전
```typescript
export class UsersController {
  constructor(private readonly usersService: UsersService) { }
    ...
}
```
- 컨트롤러가 `UserService`를 사용하고 있지만 라이프 사이클에는 전혀 관여하지 않는다.
- 내부 구조를 모르며 그저 가져다 쓸 뿐이다. 이 역할이 `IoC`라고 한다.
#### DI ?
- 의존성 주입
- 의존성을 IoC 컨테이너가 주입해준다.
- 자세히 말하면 IoC 컨테이너가 직접 객체의 생명주기를 관리해주는 방식.

## 파이프와 유효성 검사 - 요청이 제대로 전달되었는가
### 파이프
- 파이프는 미들웨어와 유사함.
- 미들웨어와의 차이점은 순서.
- 다음과 같은 두가지 목적으로 사용됨.
  - 1. 변환: 입력데이터를 원하는 형식으로 변환
    - `/users/user/1` 내의 경로 파라메터 문자열 `1`을 숫자로환변환
  - 2. 유효성 검사: 입력데이터가 사용자가 정한 기준에 유효하지 않은 경우 예외 처리
- `@nest/common`에는 다음과 같은 내장 파이프가 마련되어있음.
  - ValidationPipe
  - ParseIntPipe
  - ParseBoolPipe
  - ParseArrayPipe
  - ParseUUIDPipe
  - DefaultValuePipe
- ME: `express`를 사용하면서 `middleware`의 순서에 대해 궁금한 점이 많았는데 이를 nest가 해소한 듯 하다. `pipe` 외에 `guard`, `gateway` 등 사실 모두 `express`의 `middleware`와 다를 바 없지만 순서라는 약속을 한 것 뿐으로 보인다.
### 파이프의 내부 구현 이해하기
- PipeTransform을 상속받아서 정의해주면 된다.
```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
        console.log(metadata);
    return value;
  }
}
```
## 영속화 - 데이터를 기록하고 다루기
## 요청과 응답을 입맛에 맞게 바꾸는 미들웨어
### 미들웨어
- 쿠키를 파싱할 때 사용될 수 있음.
- 인증/인가 가능. 다만 Guard 사용 권장.
- ME: Guard, Pipe, Interceptor도 다 미들웨어.
### Logger 미들웨어
```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
// ----------
import { MiddlewareConsumer, Module, NestModule } from '@nestjs/common';
import { LoggerMiddleware } from './logger/logger.middleware';
import { UsersModule } from './users/users.module';

@Module({
  imports: [UsersModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer): any {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('/users');
  }
}
```
- 위 코드로 /users에 들어오면 `Reqeust...`가 찍히게 된다.
### MiddlewrareConsumer
```typescript
...
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer): any {
    consumer
      .apply(LoggerMiddleware, Logger2Middleware)
      .exclude({ path: 'users', method: RequestMethod.GET },)
      .forRoutes(UsersController)
  }
}
```
- `exclude`는 제외로 지정된 paths는 제외됩니다.
### 전역으로 적용하기
## 권한 확인을 위한 가드 - JWT 인증/인가
- 가드는 다음 실행 컨텍스트를 알 수 있음.
- 또한 전역에서 사용하려면 부트스트랩 단에서 적용시켜주어야 함.
### 가드(Guard)
- 올바른 유저인지 검증하는 수단.
- 미들웨어는 단순히 자신의 일만 수행하고 next() 호출하므로 올바르지 못함.
### 가드를 이용한 인가
### 인증
- 대표적으로 세션과 JWT 인증 방식이 있다.
- 나머진 생략.

## 로깅
### 내장 로거
- 로그에는 레벨이 있음.
```typescript
const LOG_LEVEL_VALUES: Record<LogLevel, number> = {
  debug: 0,
  verbose: 1,
  log: 2,
  warn: 3,
  error: 4,
};
```
- 로 정의할 수 있음.
### 커스텀 로거
- 로거를 검색에 용이하게 만들기 위해서 커스텀 로거가 필요함.
- 그렇지만 데이터베이스 저장, 에러 메일 전송도 필요하므로 외부 로거를 사용하는 예제를 참고하겠음.
- `winston`
