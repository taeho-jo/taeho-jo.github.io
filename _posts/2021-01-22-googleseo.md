---
title: Github Blog 구글 검색 노출 시키기
author: Jotang
date: 2021-01-21 01:12:00 +0900
categories: [SEO]
tags: [seo]
math: true
mermaid: true
image: https://searchengineland.com/figz/wp-content/seloads/2014/08/seo-idea-lightbulbs-ss-1920.jpg
---


블로그를 옮기기도 했고, Github blog를 구글에서 검색이 되게 하고싶었습니다. 이전까지는 제 블로그의 주소를 모르는 사람은 아예 올 수 없었지만, 왠지 구글에서도 저의 블로그가 검색이 잘 되기를 기대하면서, 시작해보겠습니다.

## 검색 엔진 최적화(SEO, Search Engine Optimization)

[SEO - MDN 용어사전](https://developer.mozilla.org/ko/docs/Glossary/SEO)

SEO를 검색을 하면, MDN에 SEO에 대해서 아주 간단 명료하게 설명이 잘 되어 있습니다.

쉽고 간단하게 설명을 하자면, `웹사이트의 검색 결과에 더 잘 보이도록 최적화 시켜주는 것` 이라고 할 수 있겠습니다.

저는 가장 많은 검색을 하고 있는 구글에 최적화를 진행하보려고합니다.

## Google Search Console 등록
[Google Search Console](https://search.google.com/search-console/about)

해당 링크를 통해 접속하여, Google Search Console에 저의 블로그를 등록해 줄 겁니다!
![](https://images.velog.io/images/jotang3726/post/f08d4b4a-59a2-4c06-98f1-ddc42188f603/image.png)

### 속성 등록
![](https://images.velog.io/images/jotang3726/post/b56cd94c-d8aa-455c-9c8f-0f4f3f355215/image.png)
속성 등록을 하게 되면, 
![](https://images.velog.io/images/jotang3726/post/d5726b94-fe82-475c-9397-c0003137105d/image.png)

아래와 같이 인증을 거치게 됩니다.

구글에서 주는 `html`파일을 다운로드 한 후에, 블로그 프로젝트의 루트폴더(최상단)에 넣으시면 됩니다.
```
taeho-jo.github.io
├── _post
├── _site
├── _config.yml
├── index.html
├── README.md
└── google0x000x000xxx0000.html [!] # 구글에서 받아온 html
```

이제 해당 프로젝트를 github으로 push해서 올려줍니다.
그리고나서, Google Search Console 페이지로 돌아와서 확인 버튼을 누르게 되면, 인증에 성공하게 됩니다. 

### sitemap.xml
sitemap.xml과 robots.txt를 작성해서 올리려고 하는데, 두 가지가 궁금하시다면 구글에 검색해 보시면 찾아보실 수 있고, 제가 참고했던 [사이트](https://www.ascentkorea.com/what-is-robots-txt-sitemap-xml/)입니다.

이제 sitemap.xml을 작성을 하려고 합니다. 역시 블로그 프로젝트의 루트폴더(최상단)에 sitemap.xml을 작성하시면 됩니다. 

```
taeho-jo.github.io
├── _post
├── _site
├── _config.yml
├── index.html
├── README.md
├── google0x000x000xxx0000.html [!] # 구글에서 받아온 html
└── sitemap.xml [!] # 작성할 sitemap.xml
```

저도 제가 직접 작성을 한 것은 아니지만, [링크](https://carbon.now.sh/?bg=rgba%285%2C5%2C5%2C1%29&t=material&wt=none&l=auto&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=56px&ph=56px&ln=false&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=---%250Alayout%253A%2520null%250A---%250A%253C%253Fxml%2520version%253D%25221.0%2522%2520encoding%253D%2522UTF-8%2522%253F%253E%250A%253Curlset%2520xmlns%253Axsi%253D%2522http%253A%252F%252Fwww.w3.org%252F2001%252FXMLSchema-instance%2522%2520xsi%253AschemaLocation%253D%2522http%253A%252F%252Fwww.sitemaps.org%252Fschemas%252Fsitemap%252F0.9%2520http%253A%252F%252Fwww.sitemaps.org%252Fschemas%252Fsitemap%252F0.9%252Fsitemap.xsd%2522%2520xmlns%253D%2522http%253A%252F%252Fwww.sitemaps.org%252Fschemas%252Fsitemap%252F0.9%2522%253E%250A%2520%2520%257B%2525%2520for%2520post%2520in%2520site.posts%2520%2525%257D%250A%2520%2520%2520%2520%253Curl%253E%250A%2520%2520%2520%2520%2520%2520%253Cloc%253E%257B%257B%2520site.url%2520%257D%257D%257B%257B%2520post.url%2520%257D%257D%253C%252Floc%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520if%2520post.last_modified_at%2520%253D%253D%2520null%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Clastmod%253E%257B%257B%2520post.date%2520%257C%2520date_to_xmlschema%2520%257D%257D%253C%252Flastmod%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520else%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Clastmod%253E%257B%257B%2520post.last_modified_at%2520%257C%2520date_to_xmlschema%2520%257D%257D%253C%252Flastmod%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520endif%2520%2525%257D%250A%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520if%2520post.sitemap.changefreq%2520%253D%253D%2520null%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Cchangefreq%253Eweekly%253C%252Fchangefreq%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520else%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Cchangefreq%253E%257B%257B%2520post.sitemap.changefreq%2520%257D%257D%253C%252Fchangefreq%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520endif%2520%2525%257D%250A%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520if%2520post.sitemap.priority%2520%253D%253D%2520null%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Cpriority%253E0.5%253C%252Fpriority%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520else%2520%2525%257D%250A%2520%2520%2520%2520%2520%2520%2520%2520%253Cpriority%253E%257B%257B%2520post.sitemap.priority%2520%257D%257D%253C%252Fpriority%253E%250A%2520%2520%2520%2520%2520%2520%257B%2525%2520endif%2520%2525%257D%250A%2520%2520%2520%2520%253C%252Furl%253E%250A%2520%2520%257B%2525%2520endfor%2520%2525%257D%250A%253C%252Furlset%253E)
에 올려두었으니 접속하여 그대로 작성하 사용하시면 됩니다.

> 주의사항 :  `sitemap.xml`을 작성하실 때, `들여쓰기(indent)`를 꼭 잘 지켜서 해주세요!! 


![](https://images.velog.io/images/jotang3726/post/be0bd478-0538-438b-a670-3365092fc259/image.png)



이제 다시 github으로 push를 진행하고, 잘 올라갔는지 확인을 할 차례입니다.
```
https://[깃허브블로그 주소]/sitemap.xml
```
위의 주소를 입력해서 접속해줍니다. 저 같은 경우에는 `https://taeho-jo.github.io/sitemap.xml`로 접속을 하면 되겠죠?

아래와 같은 화면이 나온다면 성공입니다!

![](https://images.velog.io/images/jotang3726/post/a4b9f501-cb34-473b-a5ef-25ad54701086/image.png)

### robots.txt
역시 프로젝트의 루트폴더(최상단)에 작성해줍니다.

```
taeho-jo.github.io
├── _post
├── _site
├── _config.yml
├── index.html
├── README.md
├── google0x000x000xxx0000.html [!] # 구글에서 받아온 html
├── sitemap.xml [!] # 작성 할 sitemap.xml
└── robots.txt [!] # 작성 할 robots.txt
```

아래의 코드를 붙여넣어 줍니다.
```
User-agent: *
Allow: /
Sitemap: https://[깃허브블로그 주소]/sitemap.xml
```

이제 마지막으로 깃허브로 push를 날려주고, 구글에 제출하러 가면 됩니다!

### sitemap.xml 제출하기
우측 메뉴에서 Sitemaps를 클릭하여 이동 후 페이지를 보게 되면
![](https://images.velog.io/images/jotang3726/post/c8654520-25a3-4a57-b84b-1722bad40896/image.png)

새 사이트맵 추가를 하면 됩니다.

처음에는 `가져올 수 없음`이 뜰텐데, `https://[깃허브블로그 주소]/sitemap.xml` 여기가 문제가 없었다면 걱정하지 않으셔도 됩니다. 저희가 잠든 동안 구글의 크롤링봇들이 긁어갈 것이고, 다음날 다시 들어와보면 성공! 이라고 되어있을 겁니다. 느긋하게 기다려줍니다.

URL 검사도 진행을 해보고, 전부 초록불이라면 더더욱 걱정하실 것 없이, 느긋하게 기다림이 필요합니다.

### 사이트 검색해보기
정상적으로 등록이 된 것을 확인하고, 크롤링봇들이 긁어간 것이 확인이 되고나서 제 사이트를 검색해봅니다.

![](https://images.velog.io/images/jotang3726/post/f109866e-9e50-49fe-8037-7dff734af6af/image.png)

![](https://images.velog.io/images/jotang3726/post/278ef57d-f38d-42c7-bc2d-3802dfd12f46/image.png)

검색되는 것을 확인할 수 있습니다!!

너무 부족한 글이라 민망하지만, 누군가에게는 도움이 되었으면 좋겠습니다!
