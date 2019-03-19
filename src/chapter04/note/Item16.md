## public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라


- 이따금 인스턴스 필드들을 모아놓는 일 외에는 아무 목적도 없는 퇴보한 클래스를 작성할 때가 있음

```
class Point {
    public double x;
    public double y;
}
```

- 이런 클래스는 데이터 필드에 직접 접근할 수 있으니 캡슐화의 이점을 제공하지 못한다(아이템 15)

- 철저한 객체 지향 프로그래머는 이런 클래스를 상당히 싫어해서 필드들을 모두 private으로 바꾸고 public 접근자(getter)를 추가한다.

```
class Point {
    private double x;
    private double y;
    
    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() { return x; }
    public double getY() { return y; }
    
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
} 
``` 

- public 클래스에서라면 이 방식이 확실하다.

- public 클래스의 필드가 불변이라면 직접 노출할 때의 단점이 조금은 줄어들지만, 여전히 결코 좋은 생각이 아니다.
 
```
[핵심정리]
1. public 클래스는 절대 가변 필드를 직접 노출해서는 안된다.
2. 불변 필드라면 노출해도 덜 위험하지만 완전히 안심할 수는 없다.
3. 하지만 package-private 클래스나 private 중첩 클래스에서는 종종 (불변이든 가변이든) 필드를 노출하는 편이 나을 때도 있다.
```