<pre>
<code>
@Entity
@Getter
@Setter
@Builder
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class TaskMember extends CommonEntity {
    @Id
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "task_member_seq")
    private Integer id;

    @ManyToOne
    @NotNull
    private Task task;

    @ManyToOne
    @NotNull
    private Member member;
}
</code>
</pre>

# 엔티티에는 Setter 사용 금지
* 엔티티에 Setter를 무분별하게 남용하다 보면 여기저기서 엔티티의 값을 변경할 수 있으므로 객체의 일관성을 보장할 수 없다.
* 그리고 Setter는 그 의도를 알기 힘들다 , 예를 들어 멤버 객체를 set매소드를 통해 변경하는데 무엇을 하는건지 한번에 알 수 없습니다.
* 그래서 엔티티 내부에 메서드를 생성해서 사용하자
* 추후 모든 엔티티에 Setter를 지우고 메서드를 생성하거나 빌더 패턴을 사용하자


