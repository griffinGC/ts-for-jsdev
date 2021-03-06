---
description: '프로그래밍의 타입이 수학의 집합과 공유하는 성질, 그리고 타입을 집합으로 바라보는 관점에 대해 배운다.'
---

# 6.4 집합으로서의 타입

타입스크립트 문법으로부터 잠시 쉬어가는 시간을 갖자. 이 장에서 다루는 내용은 언뜻 보아선 타입스크립트와 동떨어진 듯 보일 수 있다. 하지만 일단 이해하고 나면 타입스크립트 뿐만 아니라 다른 여러 정적 타입 언어들을 이해하는 데에 큰 도움이 될 것이다.

### **타입은 집합이다**

프로그래밍 언어에서의 타입이란 뭘까? 비슷한 성격의 값들을 모아 놓은 무언가? 지켜야 할 규칙? 우리는 타입에 대해 자주 이야기하지만 정작 타입라는 개념이 갖는 함의에 대해서는 잘 이야기하지 않는다. 타입을 이해하기 위한 다양한 수단 중 상대적으로 쉽고 직관적인 개념이 있다. 바로 집합이다.

프로그래밍에서의 **타입**은 수학에서의 **집합**과 매우 많은 특징을 공유한다. 보다 과격하게 표현하자면, **타입은 집합이다**. 실제로 타입스크립트의 **값과 타입의 관계**는 집합의 **원소와 집합의 관계**와 굉장히 유사하다. 타입\(집합\)이란 결국 값\(원소\)들의 모임이 아닌가! 

어떤 값 `value`가 타입 `T`에 속한다는 사실을 원소 `value`가 집합 `T`에 속한다\(`value ∈ T`\)는 사실에 대응해 생각해보자. 예를 들어, `true` 라는 원소가 `Boolean` 이란 집합에 속한다는 식으로 말이다. 이런 관점으로 타입스크립트의 내장 타입 `number` 와 `type Binary = 0 | 1` 두 타입을 바라보면 아래와 같은 대응 관계들이 성립한다.

| 값과 타입 | 원소와 집합 |
| --- | --- | --- | --- | --- | --- |
| 값 `0`은 타입 `number`에 속한다. | 원소 `0`이 집합 `number`에 속한다. |
| 값은 여러 타입에 속할 수 있다. 예를 들어, `0`은 `number`와 `Binary` 두 타입에 모두 속한다. | 한 원소는 여러 집합에 속할 수 있다. |
| `Binary` 타입의 모든 값\(`0`, `1`\)은 `number` 타입의 값이기도 하다. 즉 `Binary`는 `number`의 서브타입이다. | `Binary`의 모든 원소는 `number`의 원소이기도 하다. 즉 `Binary`는 `number`의 부분집합이다. |
| 이미 존재하는 두 타입을 이용해 더 복잡한 타입을 만들 수 있다. `{ binary: Binary, num: number }` | 두 집합을 사용해 새로운 집합을 정의할 수 있다. `{ (binary, num) | binary ∈ Binary, num ∈ number }` |
| 어떤 값도 가질 수 없는 타입 `never`가 존재한다. `never`는 모든 타입의 서브타입이다. | 원소가 없는 공집합 `Ø`가 존재한다. `Ø`는 모든 집합의 부분집합이다. |

이 내용을 보다 일반적으로 확장해보자. 

* 어떤 프로그래밍 언어 L로 쓰인 프로그램이 가질 수 있는 모든 값의 집합을 V라 부르자. 
* 이때, **V의 원소 중 특정한 조건을 만족하는 값**을 모아 이 집합을 **L에서의 타입 T**라고 정의할 수 있다.
* 이렇게 타입을 집합으로서 바라볼 때, 아래와 같은 대응 관계가 성립한다.

| 원소와 집합 | 값과 타입 |
| --- | --- | --- | --- | --- | --- |
| 원소 `x`가 집합 `S`에 속한다. \(`x ∈ S`\) | 값 `x`는 `S` 타입에 속한다. \(혹은 `S` 타입을 값 `x`에 할당할 수 있다.\) |
| 한 원소는 여러 집합에 속할 수 있다. | 한 값은 여러 타입에 속할 수 있다. |
| 집합 `T`가 집합 `S`의 부분집합이다. \(`T ⊂ S`\) | 타입 `T`가 타입 `S`의 서브타입이다. |
| 조건 제시법을 이용해 기존에 존재하는 집합으로부터 새로운 집합을 만들어낼 수 있다. \(`S’ = { (x, y) | x ∈ S, y ∈ S }`\) | 기존 타입의 정의로부터 새로운 타입을 만들어낼 수 있다\(객체 타입, 유니온 타입, ...\) |
| 모든 집합의 부분집합인 공집합 `Ø`이 존재한다. | 모든 타입의 서브타입인 바닥 타입_bottom type_이 존재한다 \(혹은 존재할 수 있다\). |

이런 대응 관계에서 알 수 있듯이, 프로그래밍의 타입은 수학의 집합에, 값은 원소에 대응한다. **타입은 값의 모음, 즉 집합인 것이다**.

### **유니온 타입은 합집합이다**

위에서 타입이 집합과 매우 유사한 개념임을 알 수 있었다. 이제 이런 관점에서 유니온 타입을 한 번 바라보자.

```typescript
type Union = A | B;
```

위 코드가 갖는 의미는 다음과 같이 풀어쓸 수 있다.

* `A` 타입의 모든 값은 `Union` 타입의 값이다.
* `B` 타입의 모든 값은 `Union` 타입의 값이다.

그리고 이는 집합의 관점에서 다음과 같이 해석할 수 있다.

* 집합 `A`의 모든 원소는 집합 `Union`의 원소이다.
* 집합 `B`의 모든 원소는 집합 `Union`의 원소이다.

친숙하게 느껴지지 않는가? 이는 곧 **합집합**의 정의이다. 즉, 유니온 타입 `Union`은 타입 시스템에서 합집합 `A ∪ B` 를 표현하는 수단이다! 유니온 타입이 합집합이라는 의미의 유니온\(union\)이라는 이름을 가진 건 우연이 아니다.

### **인터섹션 타입은 교집합이다**

다음으로는 인터섹션 타입은 어떤 의미를 지닐지 생각해보자. 눈치 빠른 독자라면 벌써 눈치 챘을 것이다.

```typescript
type Intersection = A & B;
```

위 코드를 풀어쓰면 다음과 같다.

* `Intersection` 타입의 모든 값은 `A` 타입의 값이다.
* `Intersection` 타입의 모든 값은 `B` 타입의 값이다.

마찬가지로 이를 집합의 관점에서 해석해보자.

* 집합 `Intersection`의 모든 원소는 집합 `A`의 원소이다.
* 집합 `Intersection`의 모든 원소는 집합 `B`의 원소이다.

맞다. 이는 교집합 `A ∩ B`의 정의이다. 인터섹션 타입은 타입 시스템이 여러 타입 간의 교집합을 표현하는 수단이다. 짐작했겠지만, 인터섹션\(intersection\)은 수학에서 교집합이라는 의미를 갖는다.

### **차집합 = ???**

합집합과 교집합을 다루었으니 자연스레 ‘그럼 차집합은 어떻게 나타내지?’하는 질문이 들 수 있다.

집합 `A`에 대한 집합 `B`의 차집합 `B - A`는 다음 조건을 만족하는 집합으로 정의한다.

* 집합 `A`에는 속하고 집합 `B`에는 속하지 않는 모든 원소는 집합 `B - A`의 원소이다.
* 집합 `A`와 `B`에 동시에 속하는 모든 원소는 집합 `B - A`의 원소가 아니다.

타입스크립트 2.8에 추가된 `Exclude` 제너릭을 이용해 차집합을 나타낼 수 있다.

* `B` 타입에 할당 불가능한 `A` 타입의 값은 모두 `Exclude<A, B>`에 할당 가능하다.
* `A & B` 타입의 값은 모두 `Exclude<A, B>`에 할당 불가능하다.

`Exclude` 제너릭에 대해선 7장에서 보다 자세히 다룰 것이다.

