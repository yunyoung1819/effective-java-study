## Item 15. 클래스와 멤버의 접근 권한을 최소화하라

- 어설프게 설계된 컴포넌트와 잘 설계된 컴포넌트의 가장 큰 차이는 바로 클래스 내부 데이터와 내부 구현 정보를 외부 컴포넌트로부터 얼마나 잘 숨겼느냐이다.

- 잘 설계된 컴포넌트는 모든 내부 구현을 완벽히 숨겨 구현과 API를 깔끔히 분리함

- 정보 은닉 혹은 캡슐화라고 불리는 이 개념은 소프트웨어 설계의 근간이 되는 원리이다.

- 정보 은닉의 장점은 정말 많다.

    - 시스템 개발 속도를 높임. 여러 컴포넌트를 병렬로 개발할 수 있기 때문
  
    - 시스템 관리 비용을 낮춤. 각 컴포넌트를 더 빨리 파악하여 디버깅할 수 있고, 다른 컴포넌트로 교체하는 부담도 적기 때문
  
    - 정보 은닉 자체가 성능을 높여주지는 않지만 성능 최적화에 도움. 완성된 시스템을 프로파일링해 최적화할 컴포넌트를 정한 다음(아이템 67), 다른 컴포넌트에 영향을 주지 않고 해당 컴포넌트만 최적화할 수 있기 때문
  
    - 소프트웨어 재사용성을 높임. 외부에 거의 의존하지 않고 독자적으로 동작할 수 있는 컴포넌트라면 그 컴포넌트와 함께 개발되지 않은 낯선 환경에서도 유용하게 쓰일 가능성이 크기 때문     
    
    - 큰 시스템을 제작하는 난이도를 낮춰줌. 시스템 전체가 아직 완성되지 않은 상태에서도 개별 컴포넌트의 동작을 검증할 수 있기 때문 
    
 - 자바는 정보 은닉을 위한 다양한 장치를 제공함
 
 - 접근 제어 메커니즘은 클래스, 인터페이스, 멤버의 접근성(접근 허용 범위)을 명시. 
 
 - 각 요소의 접근성은 그 요소가 선언된 위치와 접근 제한자(private, protected, public)로 정해짐.  
 
 - 이 접근 제한자를 제대로 활용하는 것이 정보 은닉의 핵심
 
 - 기본 원칙 : 모든 클래스와 멤버의 접근성을 가능한 한 좁혀야함. 달리 말하면, 소프트웨어가 올바로 동작하는 한 항상 가장 낮은 접근 수준을 부여해야 한다는 뜻
 
 - 멤버(필드, 메서드, 중첩 클래스, 중첩 인터페이스)에 부여할 수 있는 접근 수준은 네가지. 접근 범위가 좁은 것부터 순서대로 살펴보면
 
 ```
 - private : 멤버를 선언한 톱 레벨 클래스에서만 접근할 수 있음
 - package-private : 멤버가 소속된 패키지 안의 모든 클래스에서 접근할 수 있음. 접근 제한자를 명시하지 않았을 때 적용되는 패키지 접근 수준(단, 인터페이스의 멤버는 기본적으로 public이 적용됨)
 - protected : package-private의 접근 범위를 포함하며, 이 멤버를 선언한 클래스의 하위 클래스에서도 접근할 수 있다(제약이 조금 따른다)
 - public : 모든 곳에서 접근할 수 있다 
 ```
 
 - 클래스의 공개 API를 세심히 설계한 후, 그 외의 모든 멤버는 private으로 만들자. 그런 다음 오직 같은 패키지의 다른 클래스가 접근해야 하는 멤버에 한하여(private 제한자를 제거해) package-private으로 풀어주자. 
 
 - 권한을 풀어주는 일을 자주 하게 된다면 시스템에서 컴포넌트를 더 분해해야 하는 것은 아닌지 고민해보자.
 
 - 멤버 접근성을 좁히지 못하게 방해하는 제약 : 상위 클래스의 메서드를 재정의할 때는 그 접근 수준을 상위 클래스에서 보다 좁게 설정할 수 없다는 것
 
 - public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다. 정적 필드에서도 마찬가지이다.
    - 예외: 해당 클래스가 표현하는 추상 개념을 완성하는데 꼭 필요한 구성요소로써의 상수라면 public static final 필드로 공개해도 좋다.
    
 - 길이가 0이 아닌 배열은 모두 변경 가능하니 주의하자. 클래스에서 public static final 배열 필드를 두거나 이 필드를 반환하는 접근자 메서드를 제공해서는 안된다. 이런 필드나 접근자를 제공한다면 클라이언트에서 그 배열의 내용을 수정할 수 있게 됨
 
 ```
 // 보안 허점이 숨어 있다
 public static final Thing[] VALUES = {...};
 ```
 
 - 자바 9에서는 모듈 시스템이라는 개념이 도입되면서 두 가지 암묵적 접근 수준이 추가됨
 
 ```
 [핵심 정리]
 1. 프로그램 요소의 접근성은 가능한 한 최소한으로 하라.
 2. 꼭 필요한 것만 골라 최소한의 public API를 설계하자.
 3. 그 외에는 클래스, 인터페이스, 멤버가 의도치 않게 API로 공개되는 일이 없도록 해야 한다.
 4. public 클래스는 상수용 public static final 필드 외에는 어떠한 public 필드도 가져서는 안된다ㅏ.
 5. public static final 필드가 참조하는 객체가 불변인지 확인하라.  
 
 ```