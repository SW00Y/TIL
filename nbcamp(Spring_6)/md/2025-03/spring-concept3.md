# 2025-03-28~

### [ Spring 숙련 3주차 강의]

1. JPA
2. 영속성 컨텍스트
3. Entity 만들기
4. 연관관계 Mapping
5. Spring Data JPA

<hr>

<details> 
<summary style="font-size: 16px;"><strong>1. JPA</strong></summary>

1. 개념
- Java ORM 기술 표준
- 객체와 관계형 DB를 매핑해주는 도구
2. 장점
- SQL 없이 CRUD 가능
- 객체 중심 개발 → 생산성 & 유지보수 향상
- 데이터베이스 독립성 확보
- 생산성, 유지보수성, 성능 개선에 기여

</details>

<hr>

<details> <summary style="font-size: 16px;"><strong>2. 영속성 컨텍스트</strong></summary>

1. 개념
- JPA에서 엔티티 객체를 저장하고 관리하는 공간
- EntityManager가 관리
2. 특징
- 1차 캐시: 같은 엔티티는 캐시에서 재사용
- 변경 감지 (Dirty Checking): 변경된 필드만 UPDATE 쿼리 생성
- 지연 로딩 (LAZY): 필요한 시점에 조회
- 동일성 보장: 같은 트랜잭션 내 동일 엔티티는 동일 객체

</details>

<hr>

<details> <summary style="font-size: 16px;"><strong>3. Entity 만들기</strong></summary>

1. 코드

```java
@Entity
@Table(name = "member")
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
}
```

2. 주요 어노테이션
- @Entity: JPA 관리 대상
- @Id: 기본 키
- @GeneratedValue: 자동 생성 전략
- @Table, @Column: 매핑 설정

</details>

<hr>

<details> <summary style="font-size: 16px;"><strong>4. 연관관계 Mapping</strong></summary>

1. 연관관계 방향
- 단방향: 한 쪽만 참조
- 양방향: 양쪽 모두 참조 (주인 설정 필요)

2. 주요 어노테이션
- @ManyToOne, @OneToMany, @OneToOne, @ManyToMany
- @JoinColumn: 외래키 설정

3. Fetch 전략
- LAZY (기본): 지연 로딩 (권장)
- EAGER: 즉시 로딩 (주의)

</details>

<hr>

<details> <summary style="font-size: 16px;"><strong>5. Spring Data JPA</strong></summary>

1. 개념
- 인터페이스만 정의하면 기본 CRUD 자동 구현
- JPA를 더 쉽게 사용할 수 있게 도와주는 모듈

1. Repository 코드
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    List<Member> findByUsername(String username);
}
```

1. 기능
- 기본 CRUD
- 메서드 이름 기반 쿼리 생성 (findByUsername)
- @Query 사용 시 JPQL/Native Query 가능

1. 주의사항
- N+1 문제 발생 가능
- 연관 객체 조회 시 추가 쿼리 발생
- 해결: Fetch Join, EntityGraph, Batch Size 등

</details>
