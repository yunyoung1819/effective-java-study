## :pushpin: 이펙티브 자바

### 아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라
- 장점
  - 이름을 가질 수 있다. (동일한 시그니처의 생성자를 두개 가질 수 없다)
  - 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다. (Boolean.valueOf)
  - 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다. (인터페이스 기반 프레임워크, 인터페이스에 정적 메소드)
  - 입력 매개변수가 따라 매번 다른 클래스의 객체를 반환할 수 있다. (EnumSet)
  - 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다. (서비스 제공자 프레임워크)
- 단점
  - 상속을 하려면 public이나 protected 생성하기 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
  - 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
  

```java
public class Order {
	private boolean prime;
	
	private boolean urgent;
	
	private Product product;
	
//	public Order() {}
//	
//	public Order(Product product, boolean prime) {
//		this.product = product;
//		this.prime = prime;
//    }
//
//	public Order(boolean prime, Product product) {
//		this.product = product;
//		this.prime = prime;
//	}
    
    public static Order primeOrder(Product product) {
		Order order = new Order();
		order.prime = true;
		order.product = product;
		return order;
    }
	
	public static Order urgentOrder(Product product) {
		Order order = new Order();
		order.urgent = true;
		order.product = product;
		return order;
    }
} 
```

### 아이템 1 완벽 공략
- p9, `열거 타입`은 인스턴스가 하나만 만들어짐을 보장한다.

#### 열거 타입 (Enumeration)
- 상수 목록을 담을 수 있는 데이터 타입.
- 특정한 변수가 가질 수 있는 값을 제한할 수 있다. 타입-세이프티 (Type-Safety)를 보장할 수 있다.
- 싱글톤 패턴을 구현할 때 사용하기도 한다. 