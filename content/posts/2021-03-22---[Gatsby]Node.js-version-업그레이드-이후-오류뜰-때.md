---
title: "[Gatsby]Node.js version 업그레이드 이후 오류뜰 때"
date: "2021-03-21"
template: "post"
draft: false
slug: "[Gatsby]Node.js-version-업그레이드-이후-오류뜰-때"
category: "Gatsby"
tags:
  - "#Gatsby"
description: "yarnpkg add --exact react react-dom react-scripts --cwd [directory]/[project] has failed. 오류 발생 시 npm install -g create-react-app를 사용합시다"
---

+ 실행 환경 : windows10

Node.js version 업그레이드한 후 `gatsby-develop`을 실행했는 데 다음과 같은 에러가 나타났습니다.

```bash
 ERROR #98123  WEBPACK

undefined failed

Missing binding
[directory]\node_modules\node-sass\vendor\win32-x64-83\binding.node
Node Sass could not find a binding for your current environment: Windows 64-bit
with Node.js 14.x

Found bindings for the following environments:
  - Windows 64-bit with Node.js 12.x

This usually happens because your environment has changed since running `npm
install`.
Run `npm rebuild node-sass` to download the binding for your current
environment.

File: src\assets\scss\init.scss
```

로그에서 알려주는 대로 해당 프로젝트 폴드에서 `npm rebuild node-sass`를 실행하면 해결됩니다.

### 참고
[https://stackoverflow.com/questions/37986800/node-sass-couldnt-find-a-binding-for-your-current-environment](https://stackoverflow.com/questions/37986800/node-sass-couldnt-find-a-binding-for-your-current-environment)