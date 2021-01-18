---
title: useMemo & useCallback
author: Jotang
date: 2021-01-19 03:18:00 +0900
categories: [React]
tags: [react]
math: true
mermaid: true
image: https://alleyful.github.io/images/gallery/thumbnails/react.jpg
---

요즘 리액트로 코딩을 하면서, useCallback은 자주 사용하고, useMemo는 거의 사용하지 않고 있습니다. 사실 둘의 차이를 누가 설명하라고 하면, 정확히 할 자신이 없습니다. 지금 이 글을 작성하는 순간에도 과연 내가 이해하고 받아드린 것이 맞는지, 어떻게 사용하는 것이 좋을지에 대한 고민이 항상 함께 하고 있습니다.

그래도 제가 생각하고 이해한 useMemo와 useCallback에 대해서 글을 적어보려고 합니다. 읽게 되실 누군가에게 도움이 된다면, 너무 기쁠 것이고, 누군가 읽고 제가 이해를 잘못했거나, 다르게 알고 있는 부분을 알려주신다고 하여도 너무 기쁠 것 같습니다.

먼저 `메모이제이션 훅`이라는 아주 어려운 용어로써, 그 안에 `useMemo`와 `useCallback`이 있습니다.

이전 값을 기억해서 성능을 최적화하는 용도로 사용된다고 할 수 있습니다.

## useMemo
> 함수의 반환 값을 기억하여 재활용

코드를 작성하다 보면 함수를 만들게 되고, 그 함수는 복잡한 연산을 가진 경우가 많이 있습니다.
이 때 useMemo는 함수의 반환 값을 기억하여 재활용하는 용도로 사용 됩니다.

간단한 예시를 살펴 보려고 합니다.
```jsx
import React, { useState, useMemo } from 'react'

const Example = () => {

  const [fakeNum, setFakeNum] = useState(0)
  const [num, setNum] = useState({
    a: 10,
    b: 1
  })

  const fakeNumPlus = () => {
    setFakeNum(fakeNum + 1 )
  }

  const getSum = (a, b) => {
    console.log('덧셈함수')
    return a + b
  }

  const plus = () => {
    const { a, b } = num
    setNum({ ...num, a: a + 1 })
  }

  const minus = () => {
    const { a, b } = num
    setNum({ ...num, a: a - 1 })
  }

  const result = useMemo(() => getSum(num.a, num.b), [num])

  return (
    <div>
      {/*<div>{getSum(num.a, num.b)}</div>*/}
      <div>{result}</div>
      <div>{`fakeNum : ${fakeNum}`}</div>
      <button onClick={plus}>+</button>
      <button onClick={minus}>-</button>
      <button onClick={fakeNumPlus}>fakeNum + 버튼</button>
    </div>
  )
}

export default Example
```

getSum은 매개변수 a와 b를 받아서 덧셈한 후 return 하는 함수 입니다.

아래 주석되어 있는 코드

```jsx
{/*<div>{getSum(num.a, num.b)}</div>*/}
```

처럼 작성을 하고 동작을 하게 되어도, 정상적으로 작동을 할 것 입니다. 다만, getSum함수와 전혀 연관이 없는 fakeNum의 숫자를 올리는 `fakeNumPlus`함수가 실행될 때에도 `getSum함수는 계속해서 연산`을 하게 됩니다. console로 확인을 해본다면, 바로 확인 할 수가 있습니다.

하지만 useMemo를 사용하여 코드를 사용하게 된다면,

```jsx
<div>{result}</div>
```
useMemo의 두번째 매개변수로 작성한 의존성 배열안에 있는 값이 변경되지 않으면, 함수의 연산을 다시 하지않고 이전의 값을 기억하고 있는 것을 확인 할 수 있습니다.

useMemo의 첫 번째 매개변수로는 함수를 전달하고, useMemo는 `매개변수로 전달 받은 함수가 반환하는 값을 기억`하게 되는 것 입니다. 또, 두 번째 매개변수로는 의존성 배열을 전달하게 되는데, `의존성 배열에 해당하는 값이 변경되지 않는다면 이전의 반환된 값을 계속해서 재사용`하게 됩니다.
결국 useMemo는 의존성 배열의 값이 변경 되었을 때, 첫 번째 매개변수로 전달받은 함수를 재실행하게 되고 다시 그 반환값을 기억하게 됩니다.


## useCallback
> 렌더링 성능을 위해 제공되는 훅

리액트는 컴포넌트가 리랜더링 될 때마다, 함수를 생성, 새롭게 계산하게 됩니다.
이 때 불필요한 렌더링이 발생할 수 있습니다. 그래서 `특정한 함수를 매번 새로 생성하지 않고, 재사용하기 위해 사용`한다고 할 수 있습니다.

위의 코드에서

```jsx
const fakeNumPlus = () => {
    setFakeNum(fakeNum + 1 )
  }
```

fakeNum에 계속해서 +1씩 증가시키는 함수입니다.

state가 업데이트 됨에 따라서 컴포넌트는 재랜더링하게 되고, 그 때마다 fakeNumPlus의 함수를 새로 생성하게 됩니다. 해당 함수에 useCallback을 적용시키는 것이 적절한 예시가 아닐 수 있으나 어떻게 동작하는지 알아보기에는 쉬울 것 같습니다.

```jsx
const fakeNumPlus = useCallback(() => {
    setFakeNum(fakeNum + 1 )
  },[fakeNum])
```

useCallback은 useMemo와 마찬가지로 첫 번째 매개변수로 함수를 전달하고, 두 번째 매개변수로 의존성 배열을 전달하게 됩니다.

그리고 useCallback 역시 `의존성 배열에 해당하는 값이 바뀌지 않는다면, 첫 번째 매개변수로 받은 함수를 새로 생성하지 않고, 재사용`하게 되며

의존성 배열에 해당하는 값이 바뀐다면, 함수를 새로 생성하게 됩니다.

```jsx
const fakeNumPlus = useCallback(() => {
    setFakeNum(fakeNum + 1 )
  },[])
```

의존성 배열에 아무값도 전달하지 않는다면, 해당 함수는 새로 생성되지 않기 때문에 함수를 아무리 실행 작동시키더라도 보여지는 값에는 아무런 변화가 없을 것입니다.


아주 간단한 예제로 알아보았으나, useMemo와 useCallback을 적절히 활용한다면,
불필요한 랜더링을 줄일 수 있을 것 같습니다.

