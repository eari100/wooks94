---
title: "[React]npx create-react-app [project] 명령이 실행되지 않을 때"
date: "2021-03-19"
template: "post"
draft: false
slug: "[React]npx create-react-app-[project]-명령이-실행되지-않을-때"
category: "React"
tags:
  - "#React"
description: "yarnpkg add --exact react react-dom react-scripts --cwd [directory]/[project] has failed. 오류 발생 시 npm install -g create-react-app를 사용합시다"
---

```bash
$ npx create-react-app [project]
```
create-react-app을 실행했더니

```bash
Aborting installation.
  yarnpkg add --exact react react-dom react-scripts --cwd [directory]/[project] has failed.
```
다음과 같이 오류가 뜨면

```bash
$ npm install -g create-react-app
```
을 실행해주면 해결이 됩니다.