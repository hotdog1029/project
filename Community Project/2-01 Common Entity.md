# CommonEntity

<pre>
<code>
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class commonEntity extends commonTimeEntity{

  @CreatedBy
  @Column(updatable = false)
  private String createdBy;

  @LastModifiedBy
  private String lastModifiedBy;
}
</code>
</pre>

# CommonTimeEntity

<pre>
<code>
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class commonTimeEntity {

  @CreatedDate
  @Column(updatable = false)
  private LocalDateTime createdDate;

  @LastModifiedDate
  private LocalDateTime lastModifiedDate;
}
</code>
</pre>

모든 엔티티들이 사용할 작성자,수정자,작성일,수정일 필드이다.

# @MappedSuperclass
* 객체의 입장에서 ㄷ공통 매핑 정보가 필요할 때 사용한다.
* 작성자,수정자,작성일,수정일은 객체 입장에서 볼 때 계속 나온다.
* 이렇게 공통 매핑 정보가 필요할 때, 부모 클래스에 선ㅇ언하고 속성만 상속 받아서 사용하고 싶을 때 @MappedSuperclass를 사용한다.
* DB 테이블과는 상관없다. DB는 매핑 정보를 다 따로 쓰고 있음. 객체의 입장이다.
* 상속관계 매핑이 아니먀 @MappedSuperClass가 선언되어 있는 클래스는 엔티티가 아님. 당연히 테이블과도 매핑이 안된다.
* 주로 등록일, 수정일, 등록자, 수정자 같은 전체 엔티티에서 공통으로 적용하는 정보를 모을 때 사용함
# @EntityListeners(AuditingEntityListener.class)
* EntityListeners는 Entity가 DB로 load / persist 되기 전후에 커스텀 로직을 선언하는 인터페이스이다.
* AuditingEntityListener는 해당 엔티티에 선언된 CreatedDate, LastModifiedDate, CreatedBy, LastModifiedBy 어노테이션을 탐색해 엔티티 변경 시 해당값을 자동으로 업데이트 해준다.

# @CreatedBy, @LastModifiedBy, @CreatedDate, @LastModifiedDate
데이터를 저장할 때 해당 어노테이션을 들을 읽어서 자동으로 업데이트 해준다.

# JPA Auditing
* 엔티티의 생성, 변경한 사람과 시간을 추적하는 기능
* 기본적으로 운영할 때는 등록일, 수정일, 등록자, 수정자를 모든 테이블에 적용하는 것이 좋다.
* 누가 등록했고, 누가 수정했는지 추적하기가 쉽다.
* 유지보수에 용이함
