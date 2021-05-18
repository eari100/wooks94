---
title: "[Unity]Visual C# Compiler version error"
date: "2021-05-18"
template: "post"
draft: false
slug: "Unity-Visual-C-Sharp-Compiler-version-error"
category: "Unity"
tags:
  - "#Unity"
description: ""
---

Unity를 설치하고 실행해보았는데 console창에  
'Microsoft (R) Visual C# Compiler version 3.5.0-dev-20359-01 (8da8ba0c) Copyright (C) Microsoft Corporation. All rights reserved.'  
라는 error가 떠서 C# 스크립트를 실행할 수 없었습니다.

해결방안은  
`컴퓨터\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Command Processor\AutoRun`  
레지스트리 키를 삭제해주면 해결이 됩니다.

Anaconda 및 Python을 제거하면 Windows 레지스트리에 남아있는 아티팩트 때문에 이러한 문제가 발생한 것으로 예상이 됩니다.

####  reference
[Unity 2019.3.0a7: Microsoft Visual C# Compiler Errors](https://stackoverflow.com/questions/56913616/unity-2019-3-0a7-microsoft-visual-c-sharp-compiler-errors)