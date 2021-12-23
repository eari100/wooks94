---
title: "[JUint5] MockBean 구현 없이 테스트하는 방법"
date: "2021-12-11"
template: "post"
draft: false
slug: "[JUint5]-MockBean-구현-없이-테스트하는-방법"
category: "JUnit"
tags:
  - "#JUnit"
  - "#Spring Framework"
  - "#Java"
description: "@AutoConfigureMockMvc와 MockMvcBuilders"
---

```Java
@WebMvcTest(GreetingController.class)
public class WebMockTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private GreetingService service;

    @Test
    public void greetingShouldReturnMessageFromService() throws Exception {
        when(service.greet()).thenReturn("Hello, Mock");

        mockMvc.perform(get("/greeting"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string(containsString("Hello, Mock")));
    }
}
```

`@WebMvcTest`는 `@Repository`, `@Service`, `@Component`는 스캔 대상이 아니기 때문에  
위의 테스트 코드에서 `@MockBean`을 명시한 후

```Java
when(service.greet()).thenReturn("Hello, Mock");
```
위와 같이 `GreetingService`을 구현을 하게 됩니다. 하지만 매번 직접 구현하기는 번거롭기 때문에 `@Service`를 스캔하는 방법을 알아봅시다.

1. `@SpringBootTest` + `@AutoConfigureMockMvc`

```Java
@SpringBootTest
@AutoConfigureMockMvc
public class WebMockTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void greetingShouldReturnMessageFromService() throws Exception {
        mockMvc.perform(get("/greeting"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string(containsString("Hello, Real")));
    }
}
```
`@WebMvcTest`와 달리 `@AutoConfigureMockMvc`를 사용하면 `@Service`, `@Repository`들도 스캔의 대상으로 삼을 수 있습니다.

2. `@SpringBootTest` + `MockMvcBuilders`

```Java
@SpringBootTest
public class WebMockTest {

    private MockMvc mockMvc;

    @Autowired
    private WebApplicationContext context;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders
                .webAppContextSetup(context)
                .build();
    }

    @Test
    public void greetingShouldReturnMessageFromService() throws Exception {
        mockMvc.perform(get("/greeting"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string(containsString("Hello, Real")));
    }
}
```
`@SpringBootTest`에서 `MockMvc`는 `@Autowired`를 적용할 수 없으므로 `@BeforeEach`에서 `MockMvcBuilders`를 이용하여 주입합니다.

##### reference

[Testing the Web Layer](https://spring.io/guides/gs/testing-web/)  
[스프링 부트와 AWS로 혼자 구현하는 웹 서비스, 이동욱 저](http://www.yes24.com/Product/Goods/83849117)  
[Junit으로 컨트롤러 테스트](https://dodadada.tistory.com/113)