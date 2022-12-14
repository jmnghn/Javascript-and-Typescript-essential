## 인스턴스

클래스라고 하는 설계도가 구체적으로 현실화된 객체. <br />
(붕어빵 붕어빵틀은 그만 😣)

자바스크립트에서는 2가지가 제공된다.<br />
고전적인 방법과 현대적인 방법.

---

```
1. 인스턴스 객체 만들기
2. 함수와 new 연산자
3. 함수명 표기법
```

<br />

**`new` 연산자의 암묵적 매커니즘 ⭐️**

(1) 일단 자바스크립트가 암묵적으로 빈 객체 하나를 만든다.

(2) 이를 new 연산자 다음에 있는 함수가 호출되면서 전달해준다.

(3) 하지만, 암묵적으로 만든 객체기 때문에 특별한 지시어를 사용해야 하는데 그 키워드가 바로 `this`라고 하는 키워드다.<br />
※ `this`는 실행 컨텍스트를 사용할 때 사용하는데, 이때는 실행 컨텍스트가 아니라 인스턴스 객체, 새로 만들어진 빈 객체. <br />
즉, 인스턴스 객체라고 부르는 그 객체를 '지시'하게 된다. 이렇게 하나를 가지고 여러 용도로 사용하게 되면 이 맥락을 알고있어야 되서 어려운 측면이 있다.(실행컨텍스트의 this)<br /> 
그래서 이때 this 객체는 새로 만들어진 인스턴스 객체이기 때문에 `this.counter = 0` 식의 동적 바인딩으로 속성들을 추가하고 있는 것이다.

(4) 이후 아무런 리턴이 없는데, <br /> 
여기에 new 연산자로 인한 '두 번째 암묵적 동작'이 실행 된다. <br />
함수를 new 연산자로 실행했을 때 인스턴스 객체를 만들고 나서 함수가 종료되면 자동으로 이 this 객체를 리턴하게 되어 있다. <br />
명시적으로 리턴을 사용하지 않아도 말이다. 그래서 `new Fn();`을 `받는 변수에 인스턴스 객체가 반환`이 된다.

(5) 이렇게 암묵적인 매커니즘이 작동하고, <br />
내부적으로는 (1)과 (2) 단계에서 더 복잡한 매커니즘이 하나 더 추가된다. (숨겨진 매커니즘 v2 😣😭) <br />
바로 만들어진 빈 this객체에 해당 함수의 `프로토타입 속성(Fn.prototype)`을 `__prototype__`에 할당한다.

그렇게 하여 인스턴스 객체에서 속성을 접근할 때 <br />
인스턴스 객체 자체가 갖고 있는 속성을 찾아보고, <br />
없으면 `__prototype__`을 찾게 될 테니, <br />
`Fn.prototype.FooMethod = function () {}` 과 같은 메소드 또는 어떤 속성에 접근이 가능하게 되는 매커니즘을 갖고 있다. (더 자세히는 `23_문법-프로토타입`에 정리.)

<br />

이처럼 `new`연산자에는 암묵적인 매커니즘이 3가지가 존재하게 된다.

- 빈 객체가 생성되고 이를 함수의 this가 가리킨다. (※ 그리고 이를 받는 변수에 반환해서 인스턴스로 사용하는 것이다)
- 함수와 함께 사용한 경우 return이 없어도 할당된다.
- 빈 객체를 this가 가리키는 단계에서 `prototype 속성`을 `__prototype__`에 할당한다.

코드의 가독성, 표현력의 관점에서는 그렇게 좋지 않은 문법 메커니즘이라고 할 수 있다. <br />
(함수, 함수.proptotype 으로 작성하는 모습..., 사용, 작성 모두가 불편한 불안정한 문법 ^^;)

<br />

**class의 메소드**

작성은 함수의 메소드처럼 문법적으로 선명하게 드러나는 형식으로 작성하지만, <br />
내부적인 프로토타입 체인에 다 걸린(!)다.

new 연산자의 암묵적 매커니즘은 class 문법의 청사진...

<br />

**static**

인스턴스 객체에는 드러나지 않고,<br />
클래스 자체에 붙어 있는 속성 혹은 메소드.

<br />

**함수명 표기법**

함수인데 대문자로 시작하는 표기법. <br />
함수를 클래스처럼 사용하는데, 이를 문법적으로 (new연산자를 강제하는 방법으로) 막을 방법이 없어 <br />
개발자들끼리 암묵적으로 만든 컨벤션. <br />
(안쓰러운(!) 컨벤션 😅)

이와 다르게 클래스는 new 연산자를 사용안하면, 에러가 난다. <br />
클래스명도 소문자여도 되지만, 변수명에 할당하는 경우를 고려해 대문자로 시작하는 표기법 컨벤션을 범용적으로 사용한다.
