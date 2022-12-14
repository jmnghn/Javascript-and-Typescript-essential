## 폴리필

폴리필은 현대 웹 개발의 근간이 되는 아주 중요한 컨셉 중에 하나다.<br />

우리는 트랜스파일링 환경에서 개발을 해나가고 있는데, (Babel, TypeScript)<br />
기본적으로 브라우저가 지원하지 않는 자바스크립트 기능을 지원할 수 있는 코드로 변환하는 일을 하는 게 바로 Babel이라고 하는 도구인데,<br />
"이게 어떻게 가능한 것일까?"<br />
브라우저가 지원하지 않는 언어 기능을 어떻게 지원하게 만드는 걸까?<br />

기존의 자바스크립트 코드로 상위의 자바스크립트 코드의 기능을 대신 구현하는 것.<br />
(마치 똑같이 구현하는 것처럼...)<br />
이와 같은 개념을 바로 폴리필이라고 한다.

---

```
1. 정의
2. map 함수 구현
3. corejs
```

<br />

### map 함수 구현

```js
Array.prototype.MyMap = function (cb) {
  let arr = [];

  for (let i = 0; i < this.length; i++) {
    arr.push(cb(this[i], i, this));
  }

  return arr;
};
```

결과 확인

```
const arr = ['a', 'b', 'c'];

arr.MyMap((o, i) => o + '1')

> (3) ['a1', 'b1', 'c1']
```

<br />

수많은 ES6 이후의 문법들이 이렇게 폴리필 처리가 돼서 브라우저에 주입되어<br />
하위 브라우저에서도 잘 동작되게 만드는 컨셉이 바로 '폴리필'이다.<br />
거의 표준처럼 작동하는 폴리필 라이브러리 `core-js`.<br />
이 `core-js`가 babel 내부에 탑재돼서 최신 버전의 자바스크립트 기능이 하위 버전에서도 동작될 수 있는 코드들을 주입할 수 있는것이다.

※ [코드감상 & 어떤 식으로 폴리필을 하는지 구경해볼 수 있는 저장소...](https://github.com/zloirock/core-js)
