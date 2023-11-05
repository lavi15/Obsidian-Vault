
- 하나의 테스트에서 값을 바꿔가며 여러번 진행할 때 사용

```java
@DisplayName("상품 타입이 재고 관련 타입인지를 체크한다.")  
@CsvSource({"HANDMADE,false","BOTTLE,true","BAKERY,true"})  
@ParameterizedTest  
void containsStockType4(ProductType productType, boolean expected) {  
    // when  
    boolean result = ProductType.containsStockType(productType);  
  
    // then  
    assertThat(result).isEqualTo(expected);  
}  
```

```java  
private static Stream<Arguments> provideProductTypesForCheckingStockType() {  
    return Stream.of(  
        Arguments.of(ProductType.HANDMADE, false),  
        Arguments.of(ProductType.BOTTLE, true),  
        Arguments.of(ProductType.BAKERY, true)  
    );}  
  
@DisplayName("상품 타입이 재고 관련 타입인지를 체크한다.")  
@MethodSource("provideProductTypesForCheckingStockType")  
@ParameterizedTest  
void containsStockType5(ProductType productType, boolean expected) {  
    // when  
    boolean result = ProductType.containsStockType(productType);  
  
    // then  
    assertThat(result).isEqualTo(expected);  
}
```