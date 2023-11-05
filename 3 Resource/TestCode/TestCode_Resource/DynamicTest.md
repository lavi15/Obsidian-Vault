
- 단계별로 시나리오에 따른 테스트를 진행 시 사용
```java
@DisplayName("재고보다 많은 수의 수량으로 차감 시도하는 경우 예외가 발생한다.")  
@Test  
void deductQuantity2() {  
    // given  
    Stock stock = Stock.create("001", 1);  
    int quantity = 2;  
  
    // when // then  
    assertThatThrownBy(() -> stock.deductQuantity(quantity))  
        .isInstanceOf(IllegalArgumentException.class)  
        .hasMessage("차감할 재고 수량이 없습니다.");  
}  
  
@DisplayName("재고 차감 시나리오")  
@TestFactory  
Collection<DynamicTest> stockDeductionDynamicTest() {  
    // given  
    Stock stock = Stock.create("001", 1);  
  
    return List.of(  
        DynamicTest.dynamicTest("재고를 주어진 개수만큼 차감할 수 있다.", () -> {  
            // given  
            int quantity = 1;  
  
            // when  
            stock.deductQuantity(quantity);  
  
            // then  
            assertThat(stock.getQuantity()).isZero();  
        }),        DynamicTest.dynamicTest("재고보다 많은 수의 수량으로 차감 시도하는 경우 예외가 발생한다.", () -> {  
            // given  
            int quantity = 1;  
  
            // when // then  
            assertThatThrownBy(() -> stock.deductQuantity(quantity))  
                .isInstanceOf(IllegalArgumentException.class)  
                .hasMessage("차감할 재고 수량이 없습니다.");  
        })    \
	);
}
```