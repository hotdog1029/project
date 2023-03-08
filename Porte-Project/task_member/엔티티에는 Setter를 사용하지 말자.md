# 엔티티에 Setter를 사용하면 안되는 이유
1. 사용자의 의도를 파악하기 어렵다.
  * Entity를 set 메서드를 통해 값을 변경하는데 값을 생성하는 것인지, 변경하는 것인지 정확한 의도를 파악하기 어렵다.
2. 일관성을 유지하기 어렵다.
  * 엔티티를 변경하는 메소드인데, public으로 작성된 setter 메소드를 통해 어디서든 접근이 가능하기에 의도치 않게 post의 값을 변경하는 경우가 발생할 수 있다. 그렇다면 결국 post 객체의 일관성은 무너지게 되는 것이다.
  

# Setter 없이 데이터를 수정하는 방법
Setter의 경우 JPA의 Transaction 안에서 Entity의 변경사항을 감지하여 Update 쿼리를 생성한다. 즉 setter 메서드는 update 기능을 수행한다.

여러 곳에서 Entity를 생성하여 setter를 통해 update를 수행한다면 복잡한 시스템일 경우 해당 update 쿼리의 출처를 파악하는 비용은 어마어마할 것이다.

그렇다면 어떻게 setter를 배제할까? 아니 setter를 어떤 방식으로 대체하는지 알아보자.

# 사용한 의도나 의미를 알 수 있는 메서드를 작성하자.

<pre>
<code>
@Getter
@Entity
public class Post {
	
    private Long id;
    private String userId;
    private String title;
    private String cont;
    
    public void updatePost(Long id, String title, String cont) {
        this.id = id;
        this.title = title;
        this.cont = cont;
    }
}
</code>
</pre>

<pre>
<code>
post.updatePost(1L, "수정할 제목입니다.", "수정할 내용입니다.");
</code>
</pre>

위와 같이 Entity 내부에 createPost와 updatePost라는 메서드를 작성하였다.

setter 메서드를 작성하는 것보다 행위의 의도를 한눈에 알기 쉽다.

따라서 setter를 public으로 열어두는 것보다는 별도의 메서드를 통해 update 처리를 객체지향스럽게 쓰는게 좋다.

# 생성자를 통해 값을 넣어 일관성을 유지하도록 하자. (feat. @Builder)
Entity의 일관성을 유지하기 위해 생성시점에 값을 넣는 방식으로 setter를 배제할 수 있다.

<pre>
<code>
@Getter
@Entity
public class Post {

    private Long id;
    private String userId;
    private String title;
    private String cont;

    @Builder
    public Post(Long id, String userId, String title, String cont) {
        this.id = id;
        this.userId = userId;
        this.title = title;
        this.cont = cont;
    }
    
    // ... 이하 생략
}
</code>
</pre>

<pre>
<code>
Post post = Post.builder()
		.id(1L)
        .userId("member1")
        .title("제목입니다.")
        .cont("내용입니다.")
        .build();
</code>
</pre>

setter를 배제하기 위해 여러 생성자를 작성할 수도 있는데, 위와 같이 lombok의 @Builder 어노테이션을 통해 많은 생성자를 사용할 필요 없이 setter의 사용을 줄일 수 있다. 또한 빌더 패턴을 통해 post 객체의 값을 세팅할 수 있다.

<pre>
<code>
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class Post {

    private Long id;
    private String userId;
    private String title;
    private String cont;

    protected Post(Long id, String userId, String title, String cont) {
        this.id = id;
        this.userId = userId;
        this.title = title;
        this.cont = cont;
    }
}
</code>
</pre>

그리고 위와 같이 생성자의 접근제어자를 protected로 선언하면 new Post() 작성이 불가하기 때문에 객체 자체의 일관성 유지력을 높일 수 있다.

그리고 lombok에서 제공하는 @NoArgsConstructor 애노테이션을 사용하여 더 편하게 작성할 수 있다.

# private으로 선언하면 안되는 이유
JPA에서 proxy 객체를 만들어 해당 proxy 객체가 직접 만든 class 객체를 상속하기 때문에 public 이나 protected 까지 허용된다.

# 그렇다면 setter를 사용하면 안되는 걸까?
사실 Entity를 작성할 때 무조건적으로 setter를 배제하는 강박관념을 가질 필요는 없다.
그냥 경우에 따라 setter가 필요하면 사용하면 된다.

단순히 하나의 필드만 변경할 경우 setter도 사용할 수 있고, 별도의 의미를 가진 비즈니스 메소드를 작성할 수도 있는 것이다.

핵심 내용은 setter를 외부에 노출하는 것을 줄이는 것이다.
만약 setter를 사용할 경우 public setter를 통해 외부에서 아무 제약없이 사용할 수 있으니 이것을 사용하는 개발자들은 해당 setter 호출여부에 대해서 고민이 되기 때문이다.

# final..
Entity를 작성할 때 setter를 작성하는 의의에 대해 공부하면서 setter 사용 행위에 대한 옳고 그름을 재는 시야에서 벗어날 수 있었다.

setter 메소드를 작성해도 어떤 동작을 하는지 파악할 수 있으면 setter 작성으로도 충분할 것이지만, 애플리케이션 개발시 복잡한 객체 구조를 가지고 있을 경우 setter 메소드를 작성하는 것보다 별도의 의미를 가진 메서드를 작성하는 방식이 더 효율적일 것이다.

결국 코드의 가독성을 높여 다른 개발자와의 협업 간 불필요한 코드리딩 시간을 줄일 수 있다는 것이라고 생각이 들었다. 본인 또한 이러한 사항들을 고려하여 더 바람직한 방향으로 Entity를 설계할 수 있도록 많이 고민할 것이다.
