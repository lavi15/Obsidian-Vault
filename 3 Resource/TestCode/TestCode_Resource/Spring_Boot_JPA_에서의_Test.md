
### Layered Architecture
- 관심사의 분리
- 책임을 나누고 유지보수를 용이하게 함
![[Pasted image 20231102141503.png]]

#### application.yml
- 기본 properties와 test properties를 다르게 설정
```yml
spring:
	profiles:
		default: local

	datasource:
		url: jdbc:h2:mem:~/cafeKioskApplication
		driver-class-name: org.h2.Driver
		username: sa
		password:

	jpa:
		hibernate:
			ddl-auto: none

  

---
spring:
	config:
		activate:
			on-profile: local

	jpa:
		hibernate:
			ddl-auto: create
		show-sql: true
		properties:
			hibernate:
				format_sql: true
		defer-datasource-initialization: true # (2.5~) Hibernate 초기화 이후 data.sql 실행

	h2:
		console:
			enabled: true

---

spring:
	config:
		activate:
			on-profile: test

	jpa:
		hibernate:
			ddl-auto: create
		show-sql: true
		properties:
			hibernate:
				format_sql: true
	sql:
		init:
			mode: never
```

## Persistence Layer
- Database Access의 역활
- 비즈니스 가공 로직이 포함되어서는 안 됨
- Repository Test를 통해 Test 가능
- **@SpringBootTest**, @DataJpaTest를 통해 가능 
```java
@ActiveProfiles("test")
@SpringBootTest
class ProductRepositoryTest {

	@Autowired
	private ProductRepository productRepository;

	@DisplayName("원하는 판매상태를 가진 상품들을 조회한다")
	@Test
	void findAllBySellingStatusIn() {
		Product product1 = Product.builder()
								.productNumber("001")
								.type(HANDMADE)
								.sellingStatus(SELLING)
								.name("아메리카노")
								.price(4000)
								.builder();


		Product product2 = Product.builder()
								.productNumber("002")
								.type(HANDMADE)
								.sellingStatus(HOLD)
								.name("카페라떼")
								.price(4500)
								.builder();


		Product product3 = Product.builder()
								.productNumber("003")
								.type(HANDMADE)
								.sellingStatus(STOP_SELLING)
								.name("팥빙수")
								.price(8000)
								.builder();
	
		productRepository.saveAll(List.of(product1, product2, product3))
		//when
		productRepository.findAllBySellingStatusIn(List.of(SELLING, HOLD))

		//then
		assertThat(products).hasSize(2)
					.extracting("productNumber", "name", "sellingStatus")
					.containsExactlyInAnyOrder(
						tuple("001", "아메리카노", SELLING),
						tuple("002", "카페라떼", HOLD)
					);
					//extracting() : 원하는 값만 추출가능
					//containsExactly : 값과 순서가 맞는지 확인
						//containsExactlyInAnyOrder : 순서와 상관없이 값이 맞는지 	
	}
}
```


## Business Layer
- 비즈니스 로직을 구현
- Persistence Layer와의 상호 작용을 통해 로직 전개
- **트랜젝션 보장**(Rollback 과 같은 부분 보장)
- Service Test (Business Layer, Persistence Layer 두개를 합친 테스트)

✲ isEqualByComparingTo() : Enum값 비교

```java
@ActiveProfiles("test")  
@SpringBootTest  
class OrderServiceTest {  
  
    @Autowired  
    private ProductRepository productRepository;  
  
    @Autowired  
    private OrderRepository orderRepository;  
  
    @Autowired  
    private OrderProductRepository orderProductRepository;  
  
    @Autowired  
    private StockRepository stockRepository;  
  
    @Autowired  
    private OrderService orderService;  
  
    @AfterEach  
    void tearDown() {  
        orderProductRepository.deleteAllInBatch();  
        productRepository.deleteAllInBatch();  
        orderRepository.deleteAllInBatch();  
        stockRepository.deleteAllInBatch();  
    }  
    @DisplayName("주문번호 리스트를 받아 주문을 생성한다.")  
    @Test  
    void createOrder() {  
        // given  
        LocalDateTime registeredDateTime = LocalDateTime.now();  
  
        Product product1 = createProduct(HANDMADE, "001", 1000);  
        Product product2 = createProduct(HANDMADE, "002", 3000);  
        Product product3 = createProduct(HANDMADE, "003", 5000);  
        productRepository.saveAll(List.of(product1, product2, product3));  
  
        OrderCreateRequest request = OrderCreateRequest.builder()  
                .productNumbers(List.of("001", "002"))  
                .build();  
  
        // when  
        OrderResponse orderResponse = orderService.createOrder(request, registeredDateTime);  
  
        // then  
        assertThat(orderResponse.getId()).isNotNull();  
        assertThat(orderResponse)  
                .extracting("registeredDateTime", "totalPrice")  
                .contains(registeredDateTime, 4000);  
        assertThat(orderResponse.getProducts()).hasSize(2)  
                .extracting("productNumber", "price")  
                .containsExactlyInAnyOrder(  
                        tuple("001", 1000),  
                        tuple("002", 3000)  
                );    }  
    @DisplayName("중복되는 상품번호 리스트로 주문을 생성할 수 있다.")  
    @Test  
    void createOrderWithDuplicateProductNumbers() {  
        // given  
        LocalDateTime registeredDateTime = LocalDateTime.now();  
  
        Product product1 = createProduct(HANDMADE, "001", 1000);  
        Product product2 = createProduct(HANDMADE, "002", 3000);  
        Product product3 = createProduct(HANDMADE, "003", 5000);  
        productRepository.saveAll(List.of(product1, product2, product3));  
  
        OrderCreateRequest request = OrderCreateRequest.builder()  
                .productNumbers(List.of("001", "001"))  
                .build();  
  
        // when  
        OrderResponse orderResponse = orderService.createOrder(request, registeredDateTime);  
  
        // then  
        assertThat(orderResponse.getId()).isNotNull();  
        assertThat(orderResponse)  
                .extracting("registeredDateTime", "totalPrice")  
                .contains(registeredDateTime, 2000);  
        assertThat(orderResponse.getProducts()).hasSize(2)  
                .extracting("productNumber", "price")  
                .containsExactlyInAnyOrder(  
                        tuple("001", 1000),  
                        tuple("001", 1000)  
                );    }  
    @DisplayName("재고와 관련된 상품이 포함되어 있는 주문번호 리스트를 받아 주문을 생성한다.")  
    @Test  
    void createOrderWithStock() {  
        // given  
        LocalDateTime registeredDateTime = LocalDateTime.now();  
  
        Product product1 = createProduct(BOTTLE, "001", 1000);  
        Product product2 = createProduct(BAKERY, "002", 3000);  
        Product product3 = createProduct(HANDMADE, "003", 5000);  
        productRepository.saveAll(List.of(product1, product2, product3));  
  
        Stock stock1 = Stock.create("001", 2);  
        Stock stock2 = Stock.create("002", 2);  
        stockRepository.saveAll(List.of(stock1, stock2));  
  
        OrderCreateRequest request = OrderCreateRequest.builder()  
                .productNumbers(List.of("001", "001", "002", "003"))  
                .build();  
  
        // when  
        OrderResponse orderResponse = orderService.createOrder(request, registeredDateTime);  
  
        // then  
        assertThat(orderResponse.getId()).isNotNull();  
        assertThat(orderResponse)  
                .extracting("registeredDateTime", "totalPrice")  
                .contains(registeredDateTime, 10000);  
        assertThat(orderResponse.getProducts()).hasSize(4)  
                .extracting("productNumber", "price")  
                .containsExactlyInAnyOrder(  
                        tuple("001", 1000),  
                        tuple("001", 1000),  
                        tuple("002", 3000),  
                        tuple("003", 5000)  
                );  
        List<Stock> stocks = stockRepository.findAll();  
        assertThat(stocks).hasSize(2)  
                .extracting("productNumber", "quantity")  
                .containsExactlyInAnyOrder(  
                        tuple("001", 0),  
                        tuple("002", 1)  
                );    }  
    @DisplayName("재고가 부족한 상품으로 주문을 생성하려는 경우 예외가 발생한다.")  
    @Test  
    void createOrderWithNoStock() {  
        // given  
        LocalDateTime registeredDateTime = LocalDateTime.now();  
  
        Product product1 = createProduct(BOTTLE, "001", 1000);  
        Product product2 = createProduct(BAKERY, "002", 3000);  
        Product product3 = createProduct(HANDMADE, "003", 5000);  
        productRepository.saveAll(List.of(product1, product2, product3));  
  
        Stock stock1 = Stock.create("001", 2);  
        Stock stock2 = Stock.create("002", 2);  
        stock1.deductQuantity(1); // todo  
        stockRepository.saveAll(List.of(stock1, stock2));  
  
        OrderCreateRequest request = OrderCreateRequest.builder()  
                .productNumbers(List.of("001", "001", "002", "003"))  
                .build();  
  
        // when // then  
        assertThatThrownBy(() -> orderService.createOrder(request, registeredDateTime))  
                .isInstanceOf(IllegalArgumentException.class)  
                .hasMessage("재고가 부족한 상품이 있습니다.");  
    }  
  
    private Product createProduct(ProductType type, String productNumber, int price) {  
        return Product.builder()  
                .type(type)  
                .productNumber(productNumber)  
                .price(price)  
                .sellingStatus(SELLING)  
                .name("메뉴 이름")  
                .build();  
    }  
}
```


## Presentation Layer

```java
  
@WebMvcTest(controllers = OrderController.class)  
class OrderControllerTest {  
  
    @Autowired  
    private MockMvc mockMvc;  
  
    @Autowired  
    private ObjectMapper objectMapper;  
  
    @MockBean  
    private OrderService orderService;  
  
    @DisplayName("신규 주문을 등록한다.")  
    @Test  
    void createOrder() throws Exception {  
        // given  
        OrderCreateRequest request = OrderCreateRequest.builder()  
            .productNumbers(List.of("001"))  
            .build();  
  
        // when // then  
        mockMvc.perform(  
                post("/api/v1/orders/new")  
                    .content(objectMapper.writeValueAsString(request))  
                    .contentType(MediaType.APPLICATION_JSON)  
            )            .andDo(print())  
            .andExpect(status().isOk())  
            .andExpect(jsonPath("$.code").value("200"))  
            .andExpect(jsonPath("$.status").value("OK"))  
            .andExpect(jsonPath("$.message").value("OK"));  
        ;    }  
    @DisplayName("신규 주문을 등록할 때 상품번호는 1개 이상이어야 한다.")  
    @Test  
    void createOrderWithEmptyProductNumbers() throws Exception {  
        // given  
        OrderCreateRequest request = OrderCreateRequest.builder()  
            .productNumbers(List.of())  
            .build();  
  
        // when // then  
        mockMvc.perform(  
                post("/api/v1/orders/new")  
                    .content(objectMapper.writeValueAsString(request))  
                    .contentType(MediaType.APPLICATION_JSON)  
            )            .andDo(print())  
            .andExpect(status().isBadRequest())  
            .andExpect(jsonPath("$.code").value("400"))  
            .andExpect(jsonPath("$.status").value("BAD_REQUEST"))  
            .andExpect(jsonPath("$.message").value("상품 번호 리스트는 필수입니다."))  
            .andExpect(jsonPath("$.data").isEmpty())  
        ;    }  
}
```