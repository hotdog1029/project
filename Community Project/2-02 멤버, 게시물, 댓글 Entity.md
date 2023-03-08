# Member

<pre>
<code>
@Entity
@Getter
@Setter
public class Member extends CommonEntity {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE,
    generator = "member_seq")
  private Integer id;

  @NotNull
  private String name;

  @NotNull
  private String email;
  
  @OneToMany(mappedBy = "member", cascade = CascadeType.ALL)
  private List<Posting> postingList;

  @OneToMany(mappedBy = "member", cascade = CascadeType.ALL)
  private List<Comments> commentsList;
}
</code>
</pre>

# Posting

<pre>
<code>
@Entity
@Getter
@Setter
public class Posting extends CommonEntity {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE,
    generator = "posting_seq")
  private Integer id;

  @NotNull
  private String name;

  @NotNull
  private String Content;

  @ManyToOne
  @NotNull
  private Member member;
}
</code>
</pre>

# Comments

<pre>
<code>
@Entity
@Getter
@Setter
public class Comments extends CommonEntity {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE,
    generator = "comments_seq")
  private Integer id;

  @NotNull
  private String Content;

  @ManyToOne
  private Member member;

  @ManyToOne
  private Posting posting;
}
</code>
</pre>
