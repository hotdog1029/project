<pre>
<code>
@Data
@AllArgsConstructor
@NoArgsConstructor
@ApiModel(value = "태스크 멤버 추가", description = "태스크 멤버 추가를 위한 dto")
public class AddTaskMembersDto {
    @ApiModelProperty(position = 1, value = "태스크 ID", required = true)
    private Integer TaskId;
    @ApiModelProperty(position = 2, value = "멤버 ID 목록", required = true)
    private List<Integer> memberIds;
}
</code>
</pre>

# DTO
DTO는 서버와 클라이언트 간 데이터 교환을 하기 위해 사용하는 객체로, DTO는 로직을 가지지 않는 순수한 데이터 객체이다.
