<pre>
<code>
public interface TaskMemberRepository extends JpaRepository<TaskMember,Integer> {
    public List<TaskMember> findByTaskIdOrderByIdDesc(Integer taskId);

    public List<TaskMember> findByMemberIdOrderByIdDesc(Integer memberId);

    public void deleteByTaskIdAndMemberId(Integer taskId, Integer memberId);

}
</code>
</pre>

# JpaRepository
* JpaRepository는 인터페이스로 기본적인 CRUD 기능의 메서드를 정의 해놓은 것이다. 그러하여 메소드를 호출하는 것만으로 편리하게 데이터를 수정하고 삽입할 수 있다.
* JpaRepository에는 위의 코드와 같이 메서드를 추가할 수 있다.
* 메서드 선언문만 작성하면 그의 기능이 작동한다. 다만 JpaRepository에서 정의한 규칙대로 메서드 이름을 작성해야 한다.
