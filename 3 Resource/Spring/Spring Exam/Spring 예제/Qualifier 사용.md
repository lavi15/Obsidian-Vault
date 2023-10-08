
**※ Qualifier(”arrayList”) // 매핑할 ID 입력**

Bean 은 ArrayList<>로 만들고 실제 변수는 List<>로 만들었을 때 어떤 자식으로 만들지 지명해줄 때 사용

```jsx
@Autowired
@**Qualifier("arrayList")**
private List<SungJukDTO2> list;

@Bean(name="arrayList")
public ArrayList<SungJukDTO2> arrayList() {
    return new ArrayList<>();
}
```
