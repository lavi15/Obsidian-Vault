
- 가짜 객체를 만들고 해당 객체가 어떻게 동작할지 설정 가능 
- stubbing (Mock객체에 원하는 값을 넣는 것)
- Mock을 쓰면 좋은 경우 (외부계 시스템, Presentation Layer Test)
```java
// stubbing  
//String 객체의 아무객체나 넣은후 true로 반환
Mockito.when(mailSendClient.sendEmail(any(String.class), any(String.class), any(String.class), any(String.class)))  
    .thenReturn(true);
```

### Test Double
- Dummy : 아무 것도 하지 않는 깡통 객체
- Fake : 단순한 형태로 동일한 기능은 수행하나, 프로덕션에서 쓰기에는 부족한 객체
- Stub : 테스트에서 요청한 것에 대해 미리 준비한 결과를 제공하는 객체.(그 외에는 응답 안함)
- Spy
	- Stub이면서 호출된 내용을 기록하여 보여줄 수 있는 객체
	- 일부는 실제 객체처럼 동작시키고 일부만 Stubbing 가능
- Mock : 행위에 대한 기대를 명세하고 그에 따라 동작하도록 만들어진 객체 


### @Mock
- **@ExtendWith(MockitoExtension.class)** 를 클래스에 걸고 객체위에 @Mock 어노테이션시 Mock객체로 생성
```java
//해당 객체를 1번 불렀는지 확인 가능, 타임아웃 관련도 확인 가능
Mockito.verify(객체, times(1)).save(any(매개변수객체))
```

### @InjectMocks
- Mock 객체의 매개변수를 객체위에 선언시 알아서 채워서 Mock 객체를 만들어 줌

### @Spy
- 여러 객체를 하나의 메서드에서 호출할 때 일부의 객체만 Mock 객체를 쓰고 일부는 실제 객체를 쓸 때 사용
```java
//when이 아닌 doReturn을 써야함
//해당 doReturn 만 stubbing을 하고 나머지는 실제 객체를 씀
doReturn(true)
	.when(mailSendClient)
	.sendEmail(anyString(), anyString(), anyString(), anyString());
```