<pre>
<code>
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class TaskMember extends CommonEntity {

  @Id
  @GeneratedValue(
    strategy = GenerationType.SEQUENCE,
    generator = "member_task_seq")
  private Integer id;

  @ManyToOne
  @NotNull
  private Task task;

  @ManyToOne
  @NotNull
  private Member member;

  @Builder
  public TaskMember(Task task, Member member) {
    this.task = task;
    this.member = member;
  }
}
</code>
<pre>

기존에 클래스에 있던 @Setter, @Builder, @AllArgsConstructor 어노테이션을 삭제하고 다음과 같이 @Builder 패턴을 사용할 수 있는 생성자를 만들어줌
