---
title: "JSON parse error: Unrecognized field xxx"
date: "2021-12-11"
template: "post"
draft: false
slug: "JSON-parse-error-Unrecognized-field-xxx"
category: "Spring Framework"
tags:
  - "#Spring Framework"
  - "#Java"
description: ""
---

```Java
@RestController
@RequiredArgsConstructor
public class ExampleController {
  private final ExampleService service;

  @PostMapping("/api/save")
  public Long save(@RequestBody HumanDto dto) {
    return service.save(dto);
  }
}
```

```Java
public class HumanDto {
  private Long id;
  private Set<ItemDto> items;
}
```

```Java
public class ItemDto {
  private Long itemId;
  private String itemName;
}
```
위 코드와 같이 중첩 JSON을 @RequestBody로 parsing하는 과정에서 ItemDto의 필드들을 인식없다는 오류가 나타납니다.

```bash
org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Unrecognized field "itemId" (class ItemDto), not marked as ignorable; nested exception is com.fasterxml.jackson.databind.exc.UnrecognizedPropertyException: Unrecognized field "itemId" (class ItemDto), not marked as ignorable (0 known properties: ])
```
이를 해결하기 위해서 `Setter`를 작성하면 됩니다.
```Java
@Setter
public class ItemDto {
  private Long itemId;
  private String itemName;
}
```

