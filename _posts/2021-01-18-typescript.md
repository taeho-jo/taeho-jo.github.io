---
title: TypeScript의 타입
author: Jotang
date: 2021-01-18 11:05:00 +0900
categories: [TypseScript]
tags: [typescript]
math: true
mermaid: true
image: https://i2.wp.com/storage.googleapis.com/blog-images-backup/1*KTh3puTlJIF6vAuUUu_LAQ.jpeg?ssl=1
---

# 타입스크립트의 기본 타입
--------------------
타입스크립트의 기본 타입은 12가지가 있습니다.

> 1. String
2. Number
3. Boolean
4. Null
5. Undefined
6. Object
7. Array
8. Tuple
9. Void
10. Never
11. Enum
12. Any

# 타입 명시 (Type Annotation)
--------------------
변수를 선언할 때, 변수 값의 타입을 명시함으로써 변수 값의 데이터 타입을 지정하는 것입니다.

```ts
// syntax
let x: string = '나는 문자열 타입'
````

위 코드에서 처럼 `:`를 이용해서 자바스크립트 코드에 타입을 정의하는 방식을 타입명시, 타입표기 라고 합니다


## String
문자 뜻 그대로 string, 문자열을 뜻한다.

```ts
let name: string = 'Jotang'

// 문자열 리터럴 타입
let city: '부산' | '서울'
city = '김포'   // 타입에러, '부산' 또는 '서울'만 가능
```
문자열 리터럴로 타입을 정의하게 되면, 해당 변수에는 지정된 타입의 문자만 할당 될 수 있습니다.



## Number
이것 또한 문자 뜻 그대로 number, 숫자를 뜻한다

```ts
let age: number = 20

// 숫자 리터럴 타입
let time: 10 | 11 | 12
time = 9   // 타입에러, 10, 11 또는 12만 가능
```
문자열 리터럴 타입과 같이, 해당 변수에는 지정된 타입의 숫자만 할당 될 수 있습니다.

## Boolean
참, 거짓의 진위값에 대한 타입

```ts
let isMarried: boolean = false
```

## Null & Undefined
타입스크립트에서는 자바스크립트에서 값으로 존재하는 `null`과 `undefined`는 각각의 타입으로 존재하게 됩니다.

```ts
let milk: undefined = undefined
let candy: null = null

milk = true      // 타입에러
candy = 'candy'  // 타입에러

let food: string | null = null
food = 'rice'
```

`null`과 `undefined`는 각각의 타입으로 존재하기 때문에, 다른 타입을 할당하게되면 타입에러를 내뿜게 됩니다.

따라서 아래 `let food: string | null = null`와 같이 다른 타입과 함께 유니온 타입으로 정의할 때 많이 사용되게 됩니다.


## Object
자바스크립트에서 일반적으로 사용되는 객체의 타입입니다.

```ts
let human: object

human = {
  name: 'Jotang',
  age: 20,
  isMarried: false
}

console.log(human.name)   // 타입에러
console.log(human.age)   // 타입에러
console.log(human.isMarried)   // 타입에러
```

객체의 속성에 대한 타입 정보가 없기 때문에, 특정 속성값에 접근하면 타입에러가 발생하게 됩니다. 객체내의 속성 정보를 포함하여 타입을 정의하기 위해서는 `인터페이스(interface)`를 사용해야 하는데, 인터페이스에 대해서는 추후에 더 자세히 알아보도록 하겠습니다.


## Array
배열 타입은 두 가지의 방법으로 정의할 수 있습니다.

```ts
let lotto: number[] = [1, 2, 3, 4]

또는

let lotto: Array<number> = [1, 2, 3, 4]
```

숫자 배열에 문자열을 입력하면 타입에러가 발생하게 됩니다. 예를 들어

```ts
lotto.push('로또')
```
문자열을 lotto배열에 push하게 되면, 숫자 배열에 문자열을 추가하려는 것이기 때문에, 타입에러가 발생하게 됩니다.


## Tuple
튜플은 조금 생소한 타입일 수 있습니다.
튜플은 배열과 유사하지만, 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열의 형식을 의미합니다.

아래의 코드로 예시를 들어보도록 하겠습니다.

```ts
let lotto: [string, number, number, string] = ['로또', 12, 39, '실패']
```

튜플 타입은 정확히 명시된 개수 만큼의 원소만을 가질 수 있습니다. 만약 타입 정의보다 더 많은, 혹은 더 적은 원소를 갖는 배열을 할당한다면 에러를 낸다.

```ts
let lotto: [string, number, number, string] = ['로또', 12, 39]
// Type '[string, number, number]' is not assignable to type '[string, number, number, string]'.
// Source has 3 element(s) but target requires 4.

let lotto: [string, number, number, string] = ['로또', 12, 39, '실패', '성공']
// Type '[string, number, number, string, string]' is not assignable to type '[string, number, number, string]'.
// Source has 5 element(s) but target allows only 4.
```

그리고 튜플의 원소에 인덱스로 접근할 경우, 해당 원소의 타입이 아니라면 타입에러가 발생하게 됩니다.

```ts
let lotto: [string, number, number, string] = ['로또', 12, 39, '실패']

lotto[1] = '문자!'
// Type 'string' is not assignable to type 'number'.

lotto[1].substr()
// Property 'substr' does not exist on type 'number'.
```

문자열과 숫자로 구성된 튜플타입에 boolean을 넣으려고 한다면, 에러가 발생하게 됩니다.
```ts
let lotto: [string, number] = ['로또', 12]

lotto.push(true)
//Argument of type 'boolean' is not assignable to parameter of type 'string | number'.
```


## Void
함수가 아무것도 반환하지 않는 함수에 사용됩니다.

타입스크립트에서 함수의 타입을 지정할 때는, 매개변수 타입과 반환 타입이 필요합니다.
```ts
// systax
function test(a: number, b: number): number {
  return a + b
}

const test1 = (a: number, b: number): number => {
  return a + b
}
```

반환 타입의 경우에는 꼭 타입을 꼭 명시하지 않아도 무방합니다. 타입스크립트는 타입추론이라는 것을 통해서 함수가 반환하는 타입을 자동으로 확인하여 설정하여 줍니다. 특별한 경우가 아니라면 함수가 반환하는 값의 타입을 지정하지 않아도 괜찮습니다. 혹여나, 전달 받은 매개변수의 타입은 모두 number인데, string을 반환하려고 한다면, 타입에러가 발생하게 될 것 입니다.

이처럼, 함수가 어떠한 값을 반환을 하게 될 때에는 타입을 지정해야 할 필요가 있으나, 아무것도 반환하지 않는 함수는 `void`타입을 지정하게 됩니다.

```ts
const printName = (name: string): void => {
  console.log(name)
}

printName('Jotang')
```
void가 지정된 함수는 undefined를 반환하게 됩니다. 앞서 함수에 타입을 지정할 때 처럼 타입스크립트는 아무것도 반환하지 않는 함수에 void타입을 알아서 지정해주게 됩니다. 따라서 void 타입을 별도로 지정해주지 않으셔도 괜찮습니다.

## Never
never 타입은 절대로 발생하지 않는 값의 타입을 나타냅니다.
never를 반환하는 함수의 몇 가지 예는 다음과 같습니다.
```ts
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}
// Inferred return type is never
function fail() {
    return error("Something failed");
}
// Function returning never must have unreachable end point
function infiniteLoop(): never {
    while (true) {
    }
}
```


## Enum
열거형타입이라고 할 수 있습니다. enum은 자바스크립트에는 없는 개념이라 다소 생소 할 수 있습니다. enum은 특정 값들의 집합을 의미하는 자료형입니다. 쉽게 이야기하면 연관된 아이템들을 함께 묶어서 표현 할 수 있는 수단이라고 할 수 있습니다.

enum은 `숫자 열거형(Numberic Enum)`, `문자열 열거형(String Enum)` 그리고 `이종 열거형(Heterogeneous Enum)`으로 나누어 진다고 볼 수 있다. 이종 열거형의 경우는 숫자와 문자를 섞어 열거형을 만들 수는 있지만 추천하는 방법은 아니라고 합니다.

그리고 열거형 타입의 각 원소는 값으로 사용될 수 있고, 타입으로 사용될 수도 있습니다.

#### 숫자 열거형
---
```ts
enum Hero {
  IronMan,
  BlackPanther,
  CaptainAmerica,
  Spiderman
}

const firstHero: Hero = Hero.IronMan
const secondHero: Hero.BlackPanther | Hero.CaptainAmerica = Hero.CaptainAmerica

console.log(firstHero, secondHero, Hero[3]) // 0, 2, Spiderman
```
실제로 작성한 코드를 보게 되면

![](https://images.velog.io/images/jotang3726/post/ef804845-e5bf-4b22-9d34-5a17f28544cc/image.png)

enum의 각 원소 옆에 숫자가 보이는데, 값을 할당하지 않으면 `자동으로 0이 할당`되고, `1씩 증가`한 값이 자동으로 할당 됩니다.
명시적으로 값을 할당 할 수 있는데, 값을 할당하게 되면,

![](https://images.velog.io/images/jotang3726/post/43da444e-f698-416d-a3e7-400cb1b08dfb/image.png)

위 코드와 같이, 명시된 값이 할당되고, 그 이후는 또 1씩 증가한 값이 자동으로 할당되게 됩니다.

다른 타입들과 달리 열거형 타입은 컴파일 후에도 관련된 코드가 남아있습니다.
컴파일 된 js 파일을 보게 되면 확인할 수 있습니다.
```js
var Hero;
(function (Hero) {
    Hero[Hero["IronMan"] = 0] = "IronMan";
    Hero[Hero["BlackPanther"] = 4] = "BlackPanther";
    Hero[Hero["CaptainAmerica"] = 5] = "CaptainAmerica";
    Hero[Hero["Spiderman"] = 6] = "Spiderman";
})(Hero || (Hero = {}));
var firstHero = Hero.IronMan;
var secondHero = Hero.CaptainAmerica;
console.log(firstHero, secondHero, Hero[3]);
```

열거형 타입은 결국 객체로 존재하기 때문에, 해당 객체를 런타임에서도 사용할 수 있고,
숫자 열거형의 경우, `이름과 값이 양방향으로 매핑`되게 됩니다. 따라서 값을 이용해서 이름을 가져올 수 있습니다.
```ts
console.log(Hero[0], Hero.BlackPanther, Hero[5]) // IronMan, 4, CaptainAmerica
```

#### 문자열 열거형
---
문자열 열거형은, 말 그대로 문자열을 할당하는 코드입니다.

```ts
enum Hero {
  IronMan = 'ironMan',
  BlackPanther = 'blackPanther',
  CaptainAmerica = 'captainAmerica',
  Spiderman = 'spiderman'
}
```
컴파일 된 js 파일을 확인하면 아래 코드와 같습니다.
```js
var Hero;
(function (Hero) {
    Hero["IronMan"] = "ironMan";
    Hero["BlackPanther"] = "blackPanther";
    Hero["CaptainAmerica"] = "captainAmerica";
    Hero["Spiderman"] = "spiderman";
})(Hero || (Hero = {}));
```

문자열 열거형의 경우에는 숫자 열거형과 다르게 양방향이 아니라, `단방향으로 매핑`됩니다.

기존과 같이 console을 찍어보게 되면
```ts
const firstHero: Hero = Hero.IronMan
const secondHero: Hero.BlackPanther | Hero.CaptainAmerica = Hero.CaptainAmerica

console.log(firstHero, secondHero, Hero[3]) // ironMan, captainAmerica, undefined
```
숫자의 값으로는 접근할 수 없다는 것을 확인할 수 있습니다.

#### 상수 열거형 타입
---
열거형 타입은 컴파일 후에도 남아있다고 했습니다. 이로 인해서 번들 파일의 크기가 불필요하게 커질 수 있습니다. 열거형 타입의 객체에 접근을 하지 않을 것이라면, 컴파일 후에 굳이 객체를 남겨 놓을 필요가 없어지게 되는데, 이 때 상수 열거형 타입을 사용하면 컴파일 결과에 열거형 타입의 객체를 남겨놓지 않을 수 있습니다.
```ts
const enum Phone {
  Iphone = 'iphone',
  Samsung = 'samsung'
}

const iPhone = Phone.Iphone
const galaxy = Phone.Samsung
```
컴파일 된 js파일을 확인해보면, 아까와는 달리 객체를 생성하는 코드가 보이지 않게 됩니다.
```js
var iPhone = "iphone" /* Iphone */;
var galaxy = "samsung" /* Samsung */;
```

상수 열거형 타입을 모든 경우에 쓸 수 있는 것은 아니며, 열거형 타입을 상수로 정의하면 열거형 타입의 객체를 사용할 수 없게 된다.


## Any
모든 종류의 값을 허용하는 타입입니다.
any 타입에서는 숫자와 문자열뿐만 아니라 함수도 입력될 수 있습니다. 실제로 타입을 알 수 없는 경우나 타입이 정의가 안 된 외부 패키지를 사용하는 경우에 사용하기 좋습니다.
다만, any 타입을 남발하면 타입스크립트를 사용하는 의미가 퇴색되기 때문에 되도록 피하는게 좋다고 생각합니다.

```ts
let all: any

all = 'Jotang'
all = 20
all = false
```

