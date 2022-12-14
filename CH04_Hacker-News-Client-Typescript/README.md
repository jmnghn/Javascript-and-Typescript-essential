## Chapter 04. Hacker News Client - Typescript

### 01. 타입스크립트를 위한 환경셋팅

```
1. Typescript 환경 설정
2. tsconfig.json
3. 첫 번째 실행
4. Source Map
```

#### 포팅

A라는 언어로 개발된 앱을 B라는 언어로 완전히 바꾼다거나 하는 의미. (JavaScript → TypeScript)<br />
(요즘 잘 안쓰는 용어 😅)

- `.js` 확장자명을 `.ts`로 바꾸고
- `index.html`의 `<script>`의 src속성을 변경하면,
- 끝. 😳😄

#### tsconfig.json

- [What is tsconfig.json](https://www.typescriptlang.org/ko/docs/handbook/tsconfig-json.html)
- [컴파일링 옵션](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES5",
    "module": "CommonJS",
    "alwaysStrict": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "sourceMap": true,
    "downlevelIteration": true
  }
}
```

- strict (타입스크립트의 기능을 얼마나 엄격하게 적용해서 변환시킬 것인지에 대한 속성)

  \: 타입스크립트가 제공하는 기능 중에 가장 핵심적인 기능은 자바스크립트의 타입 확장 구문을 제공하는 것이다.<br />
  자바스크립트는 타입은 있지만, 문법으로 기술하고 있진 않다. 상황적으로 어떤 데이터 타입이구나 하고 자동으로 변환이 이루어지는 아주 느슨한 타입 체계를 갖고 있는데, 그러다 보니 자바스크립트 파일을 타입스크립트로 변환하겠다고 했을 때는 타입이 아무것도 정의되어 있지 않다. 이럴 때 그 변환해야 되는 자바스크립트 파일의 코드 규모가 굉장히 크다면 굉장히 수정해야 될 것들이 많을 것이다.<br />
  
  타입스크립트 팀 입장에서는 이미 자바스크립트로 웹 애플리케이션을 만들고 있는 세상에서 타입스크립트라고 하는 새로운 언어를 전파시키려면 전략적인 접근이 좀 필요했을 것이다.<br />
  
  처음부터 이렇게 변환에 엄격함만 추구한다고 하면 자바스크립트 개발자들이 아마 타입스크립트를 가지고 기존의 애플리케이션을 변환시키는 일을 잘 안했을 것이다. 하지만, 옵션을 제공함으로써 "처음엔 아무것도 안 해도 돼. 너희들이 바꿀 게 100개면 그중에 처음엔 한 2~3개 정도만 바꾸고 나머진 그대로 둔 상태에서도 작동시키게 해줄께. 그리고 시간이 나면 10개 바꾸고, 30개 바꾸고, 나중에 결국 100개 다 바꿔서 완전히 타입스크립트 파일로 전환시키는 작업이 가능할 수 있도록 그렇게 우리가 소프트 랜딩하게 해줄게." 라고 하는 전략을 취한 것으로 볼 수 있다.

  그것이 바로 이 `strict` 모드다.

  디폴트로 False로 해놓으면, 마치 이게 타입스크립트 파일인지 자바스크립트 파일인지 구분하기 어려울 정도로 그냥 보통의 자바스크립트 파일에서도 아무런 오류 없이 잘 작동하는 모습을 볼 수 있다.

  True로 켜면, 타입스크립트가 뿜는 에러를 찾아가며 수정할 수 있는 것이다.

- noImplicitAny, noImplicitThis, ...

  \: strict의 세부 옵션들이다.<br />
  엄격한 옵션들 중에서도 특별한 옵션들 몇 가지를 세부적으로 세세하게 끄고 켤 수 있는 그런 옵션들이다.<br />
  여러가지가 있지만, 이 프로젝트에서는 2가지 정도만 사용한다.

- sourceMap

  \: dist라고 하는 디렉토리는 타입스크립트 컴파일러가 타입스크립트 파일을 변환한 결과를 출력해 놓은 타겟 디렉토리라고 할 수 있다. 여기에 보면 `app.c61986b1.js`도 있고, `app.c61986b1.js.map` 파일도 있는걸 확인할 수 있다.<br />
  그리고 또 `index.html`을 보면, 우리가 script태그에 `src="app.ts"`라고 해놓은 부분이 `src="/app.c61986b1.js"`로 변경되있는걸 볼수 있다.
  이때 `app.c61986b1.js.map`가 sourceMap 파일인 것이다.

  코드 작성은 타입스크립트. 브라우저 실행은 자바스크립트.<br />
  하지만 자바스크립트 파일을 실행하는 가운데 만약 자바스크립트 코드에 문제가 있는 경우 그 변환된(직접 작성하지 않은) 코드로 문제를 발견하는 일은 쉽지 않을 것이다.<br /> 
  그럴 때 실제 브라우저에서 우리가 작성했던 원본 코드, 타입스크립트 코드를 보여 주면서 그 상태에서 문제점을 찾을 수 있게 한다.<br />
  실제 내부적으로 실행은 자바스크립트로 되지만, 그런 기능을 제공해 주기 위한 일종의 연결 정보를 갖고 있는 파일이라고 볼 수 있는 것. (소스의 지도)<br />
  > _개발자도구 Sources 탭에서 원본 .ts를 확인할 수 있게 해주는 것._

- target

  \: ts파일을 컴파일하면 나오는 자바스크립트의 문법 체계다.

- module

#### 첫번째 실행

parcel을 실행하면 `node_modules` / `dist` / `.cache` / `package.json`이 생성된다.<br />
parcel이 알아서 detect해서 패키지도 다운로드 하고 등등 셋팅한다!

<br />

### 02. 변수에 타입 작성하기

```
1. 첫 번째 타입 지정
2. 타입 알리아스
3. 타입 추론
4. 타입 가드
```

<br />

### 03. 함수의 규격 작성하기

```
1. 첫번째 함수
2. REST Client
3. 제네릭
4. 마무리 타이핑
```

- VS Code Extension - REST Client 👍🏻
- 제네릭
  - 굉장히 난이도가 높은 기술 중에 하나다.
  - 타입스크립트만 제공하는 건 아니고 제네릭을 지원하는 많은 언어들이 있다.<br />
    그런 언어들에서조차도 제네릭은 굉장히 유연하고 유용한 기능이긴 하지만,
    난이도가 상황에 따라 한도 끝도 없이 올라갈 수 있는 문법 요소 중 하나다.

<br />

### 04.타입과 인터페이스

```
1. 인터페이스와 타입 별칭
2. 인터페이스로 전환
3. 인터페이스 상속
4. 풍성한 타입 설명
```

지금까지는 타입 알리아스를 이용해서 타이핑을 해왔다.
> 타입스크립트가 제공하는 기능을 이용해서 타입을 기술하는 방법 → 보통 '타이핑'한다고 얘기한다.
- 뭔가 다른 방법도 있다는 뉘앙스...? → 👌🏻, 인터페이스.
- 이때 중요한 키워드가 '일관성'이다. → 타입 알리아스 파 VS 인터페이스 파 😅

```typescript
type NewsFeed = News & {
  comments_count: number;
  points: number;
  read?: boolean;
};
```

```typescript
interface NewsFeed extends News {
  comments_count: number;
  points: number;
  read?: boolean;
}
```

- 유니온 타입은 지원하지 않는다. <br />
  이런 경우에는 쓰려면 타입 알리아스를 사용해야한다. <br />
  그 외에는 인터페이스라고 하는 것을 주로 많이 쓰는 경향성이 좀 있다.

<br />

#### 인터페이스로 전환

(1) 타입 알리아스 (기존)

```typescript
type Store = {
  currentPage: number;
  offset: number;
  limit: number;
  feeds: NewsFeed[];
};

type News = {
  id: number;
  time_ago: string;
  title: string;
  url: string;
  user: string;
  content: string;
};

type NewsFeed = News & {
  comments_count: number;
  points: number;
  read?: boolean;
};

type NewsDetail = News & {
  comments: NewsComment[];
};

type NewsComment = News & {
  comments: [];
  level: number;
};
```

(2) 인터페이스

```typescript
interface Store {
  currentPage: number;
  offset: number;
  limit: number;
  feeds: NewsFeed[];
}

interface News {
  readonly id: number;
  readonly time_ago: string;
  readonly title: string;
  readonly url: string;
  readonly user: string;
  readonly content: string;
}

interface NewsFeed extends News {
  readonly comments_count: number;
  readonly points: number;
  read?: boolean;
}

interface NewsDetail extends News {
  readonly comments: NewsComment[];
}

interface NewsComment extends News {
  readonly comments: [];
  readonly level: number;
}
```
