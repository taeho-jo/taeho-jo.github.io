---
title: TypeScript의 시작 & 개발환경 세팅하기
author: Jotang
date: 2021-01-17 18:06:00 +0900
categories: [TypseScript]
tags: [typescript]
math: true
mermaid: true
image: https://i2.wp.com/storage.googleapis.com/blog-images-backup/1*KTh3puTlJIF6vAuUUu_LAQ.jpeg?ssl=1
---


현재 진행하고 있는 프로젝트는 타입스크립트를 적용하지 않고 진행 중 입니다.

사실, 타입스크립트를 이전부터 공부하고 사용해보고 사용하고 싶었지만, 회사에서 진행하는 프로젝트들이 적용하지 않고 진행되는 부분이 많았고,
시간을 투자해서 공부를 하지 않았기에, 자신도 없었습니다. `any만 쓸거면 그냥 안쓰는게 낫지!` 하며..

지금 당장 타입스크립트를 적용시킬 수는 없겠지만,
지금부터라도 조금씩 준비하고, 공부해서 다음 프로젝트를 진행하게 될 때, "타입스크립트를 사용해서 해보겠습니다."라고
당당히 말할 수 있기를 바라며 공부를 해보려고 합니다.

## 동적 타입 언어 vs 정적 타입 언어
먼저 동적언어와 정적언어의 차이를 간단하게 알아보려고 합니다.


| 동적 타입 언어 | 정적 타입 언어 |
|---|---|
| 타입에 대한 고민을 하지 않기 때문에, 배우기가 쉽다. | 타입을 고려해야 하므로 진입 장벽이 높다. |
| 작은 규모의 프로젝트에 사용했을 때, 생산성이 높다. | 큰 규모의 프로젝트에 사용했을 때, 동적 타입 언어에 비해 생산성이 높다. |
| 런타임 시 오류가 발생 한다. | 컴파일 시 오류가 발생 한다. |


## 타입스크립트란?
그럼 타입스크립트란 무엇인가!?

![](https://media.vlpt.us/images/jo_love/post/d3f617f7-e108-4d7b-a54a-02dc8910ce97/ts.png)

위 그림은 타입스크립트를 알고 계신 분들이라면 보지 못한 분은 없을 것이라고 생각합니다.

타입스크립트는 자바스크립에 타입을 부여한 언어이고, 자바스크립트의 확장된 언어! 라고 생각할 수 있습니다.
마이크로소프트에서 개발하고 있고, 자바스크립트의 새로운 표준이 나오거나 거의 확실시되는 기능은 빠르고, 또 꾸준하게 업데이트 되고 있습니다.

저는 리액트를 사용하여 개발을 진행하고 있는데, 타입스크립트 측에서 리액트 개발자들의 의견을 잘 반영해 주어서, JSX 문법과 리액트 컴포넌트의 타입을 지정하는데 큰 어려움이 없다고 합니다.

## 타입스크립트 알아보기
먼저 타입스크립트로 개발할 수 있는 환경 설정을 해보려고 합니다.
[타입스크립트 공식홈페이지](https://www.typescriptlang.org/)로 들어가보면, 아래 이미지와 같이 install locally라고 되어 있는 것을 볼 수 있습니다.

![](https://images.velog.io/images/jotang3726/post/f25a499b-4ec8-4abd-a704-0c8954585097/image.png)

![](https://images.velog.io/images/jotang3726/post/c0f29c46-ff49-48ba-aae4-9b7577395ea6/image.png)

tsc 명령어를 사용해서 타입스크립트를 자바스크립트로 컴파일 할 수 있게 되었습니다.
> tsc : typescript compiler


타입스크립트 파일을 하나 만들어서 실행해 보려고 합니다.
아래와 같은 코드를 작성하고,

```ts
const logName = (name: string, age: number) => {
    console.log(name, age)
}

logName('Jotang', 20)
```

터미널에서 `tsc [파일명].ts` 명령어를 실행시켜주면, js파일로 컴파일 되며 js파일이 자동으로 생성되는 것을 확인할 수 있습니다.
```js
var logName = function (name, age) {
    console.log(name, age);
};
logName('Jotang', 20);
```
![](https://images.velog.io/images/jotang3726/post/b04aa712-5a5c-4d3e-8090-5406cb1743d2/image.png)

그리고 터미널에서 `node [파일명].js`를 실행하면, 터미널에서 콘솔이 찍히는 것을 확인 할 수 있습니다.
![](https://images.velog.io/images/jotang3726/post/0332c8a9-4c10-4aea-9d6e-c2b97a03206d/image.png)


브라우저에서 확인하기 위해서는 index.html에 컴파일된 js파일을 연결시켜주면 됩니다.

![](https://images.velog.io/images/jotang3726/post/60e83cb3-0de5-4b74-bd47-11b282ba4f34/image.png)

브라우저에서도 아주 잘 출력되게 되었습니다.

## 타입스크립트의 변화를 감시하기

이렇게 타입스크립트로 코드를 작성하고, tsc명령어를 통해서 js파일로 컴파일을 시켜야 브라우저에서 작동하는 것을 확인 할 수 있습니다.
하지만, 코드가 매번 바뀔 때 마다 tsc명령어를 실행시키는 것은 굉장히 귀찮은 일입니다 😭

`tsc -w` 명령어를 이용하여, 변경사항을 지켜보게 할 것 입니다. w는 Watch의 줄임말 입니다.

`tsc -w [파일명].ts`

위의 명령어를 실행시키게 되면,

![](https://images.velog.io/images/jotang3726/post/52810286-3009-4127-a383-1083b9f2be49/image.png)

아래와 같이 나타나게 됩니다.

> Starting compilation in watch mode... : 감시모드의 컴파일 프로세스가 시작되었다는 뜻!

이제 ts파일에서 코드를 변경하게 되면, js파일이 스스로 컴파일 되는 것을 확인 할 수 있습니다.


오늘은 간단하게 동적 타입의 언어와 정적 타입의 언어에 대해서 알아보았고, 타입스크립트를 사용할 수 있는 개발환경을 세팅하였습니다.
앞으로는 타입스크립트의 여러 가지 타입들과, 타입스크립트만의 기능들에 대해서 알아보고, 리액트에도 타입스크립트를 적용시켜 사용하는 것에 대해서 알아보도록 하겠습니다.


