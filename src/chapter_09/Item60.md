## Item 60. 정확한 답이 필요하다면 float 와 double은 피하라

- float와 double 타입은 과학과 공학 계산용으로 설계되었다.

- 이진 부동소수점 연산에 쓰이며, 넓은 범위의 수를 빠르게 정밀한 '근사치'로 계산하도록 세심하게 설계되었다.

- 따라서 정확한 결과가 필요할 때는 사용하면 안된다.

- float와 double 타입은 특히 금융 관련 계산과는 맞지 않는다.

- 예를 들어 1달러가 있고 10센트, 20센트, 30센트, ..., 1달러짜리 사타을 하나씩 산다고 가정해보자. 사탕은 몇 개나 살 수 있고 잔돈은 얼마가 남을까?


```
public static void main(String[] arg) {
    double funds = 1.00;
    int itemsBought = 0;
    for (double price = 0.10; funds >= price; price += 0.10) {
        funds -= price;
        itemsBought++;
    }
    System.out.println(itemsBought + "개 구입");
    System.out.println("잔돈(달러):" + funds);
}
```

- 프로그램을 실행해보면 사탕 3개를 구입한 후 잔돈은 0.39999999999 달러가 남는다. (잘못된 결과!)

- 금융 계산에는 BigDecimal, int 혹은 long을 사용해야 한다.

```
public static void main(String[] args) {
    final BigDecimal TEN_CENTS = new BigDecimal(".10");
    
    int itemsBought = 0;
    BifDecimal funds = new BigDecimal("1.00");
    for (BigDecimal price = TEN_CENTS;
            funds.compareTo(price) >= 0;
            price = price.add(TEN_CENTS)) {
        funds = funds.subtract(price);
        itemsBought++;     
    }
    System.out.println(itemBought + "개 구입");
    System.out.println("잔돈(달러): " + funds);
}
```

- 하지만 BigDecimal에는 단점이 두 가지 있다.
    1) 기본 타입보다 쓰기가 훨씬 불편하다.
    2) 기본 타입보다 훨씬 느리다.
    
- BigDecimal의 대안으로 int 혹은 long 타입을 쓸 수도 있다.
    1) 다룰 수 있는 값의 크기가 제한된다.
    2) 소수점을 직접 관리해야 한다.
    3) 숫자를 아홉 자리 십진수로 표현할 수 있다면 int를 사용
    4) 숫자를 열여덟 자리 십진수로 표현할 수 있다면 long을 사용
    5) 열여덟 자리를 넘어가면 BigDecimal을 사용
    