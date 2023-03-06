<pre>
<code>
@Service
@RequiredArgsConstructor
public class TaskMemberServiceImpl implements TaskMemberService {

    private final TaskMemberRepository taskMemberRepository;

    @Override
    public List<TaskMember> addTaskMembers(AddTaskMembersDto dto) {
        List<TaskMember> TaskMembers = dto.getMemberIds().stream()
                .map(memberId -> TaskMember.builder()
                        .task(Task.builder().id(dto.getTaskId()).build())
                        .member(Member.builder().id(memberId).build())
                        .build())
                .collect(Collectors.toList());
        return taskMemberRepository.saveAll(TaskMembers);
    }

    @Override
    public List<Task> getAllTasks(Integer memberId) {
        List<TaskMember> taskMemberList = taskMemberRepository.findByMemberIdOrderByIdDesc(memberId);

        return taskMemberList.stream()
                .map(e -> e.getTask())
                .collect(Collectors.toList());
    }

    @Override
    public List<Member> getAllMembers(Integer taskId) {
        List<TaskMember> taskMemberList = taskMemberRepository.findByTaskIdOrderByIdDesc(taskId);

        return taskMemberList.stream()
                .map(e -> e.getMember())
                .collect(Collectors.toList());
    }

    @Override
    public void deleteTaskMember(Integer taskId, Integer memberId) {
        taskMemberRepository.deleteByTaskIdAndMemberId(taskId, memberId);
    }
}

</pre>
</code>

* 서비스 인터페이스의 구현체를 만들어 주었다.
* 여기서는 기본적인 기능만 있으니 설명은 넘어감
