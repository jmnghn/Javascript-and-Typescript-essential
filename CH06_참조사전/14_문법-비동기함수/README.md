## 비동기 함수

실제로 동작하는 건 비동기로 작동을 하고, <br />
코드적으로만 동기적으로 작동하는 것 처럼 보여주게 만드는 <br />
비동기 함수의 마법(!) ^^;

Promise를 반환하는 함수 앞에 await만 붙여 주고, <br />
reject는 catch에서 처리하면 되는 초 씸플한 구문! <br />

하지만, 이 비동기 함수도 Promise 베이스의 메커니즘 위에서 작동되는 코드이기 때문에 <br />
Promise의 매커니즘을 충분히 이해하고 있는 것이 무엇보다 중요하다.

Prmoise 이해가 굉장히 부족한 상태에서 <br />
복잡한 비동기 처리 코드인 경우, 비동기 async 함수를 마구마구 남발하게 되면 <br />
`코드가 어떻게 돌아가는지 이해하기가 굉장히 어려운 상황`이 올 수 있다. <br />

---

```
1. 지연(delay) 함수
2. 비동기 함수
3. reject
```
