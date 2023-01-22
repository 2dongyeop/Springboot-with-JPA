# ȸ�� ������ ����

## ȸ�� �������丮 ����
```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;
import jpabook.jpashop.domain.Member;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class MemberRepository {

    @PersistenceContext
    private EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }

    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}
```

</br>

> **��� ����**

- `@Repository` : ������ ������ ���, JPA ���ܸ� ������ ��� ���ܷ� ���� ��ȯ
- `@PersistenceContext` : ��ƼƼ �Ŵ���(`EntityManager`) ����
- `@PersistenceUnit` : ��ƼƼ �޴��� ���丮(`EntityManagerFactory`) ����

</br>

> **��������**

- `persist()` : ���Ӽ� ���ؽ�Ʈ�� ��ü�� �ְ�, ���߿� Ʈ������� Ŀ�Ե� �� DB�� �ݿ��ȴ�.
- `find()` : �� ���� ��ȸ�� �� ����ϰ�, `(Ÿ��, PK)`�� ���ڷ� ���´�.
- JPQL : SQL�� ������ �ٸ���, `FROM`�� ����� ���̺��� �ƴ� ��ƼƼ�̴�.
- ���� ���� ��ȸ�� ���� ������ �̿�����!

</br>

## ȸ�� ���� ����
```java
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class MemberService {

    private final MemberRepository memberRepository;

    /**
     * ȸ�� ����
     */
    @Transactional
    public Long join(Member member) {
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        // EXCEPTION
        List<Member> findMembers = memberRepository.findByName(member.getName());
        if (!findMembers.isEmpty()) {
            throw new IllegalStateException("�̹� �����ϴ� ȸ���Դϴ�.");
        }
    }

    // ȸ�� ��ü ��ȸ
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Member findOne(Long memberId) {
        return memberRepository.findOne(memberId);
    }
}
```

</br>

> **��� ����**

- `@Transactional` : Ʈ�����, ���Ӽ� ���ؽ�Ʈ
    - `readOnly=true` : �������� ������ ���� �б� ���� �޼��忡 ����ϰ�, �ణ�� ���� ����� ���� �� �ִ�.
    - �б� ������ �ƴ� �޼��忡�� `@Transactional`�� �ٽ� ���� �߰��ϸ� �ȴ�.
- `@Autowired`
    - ������ ���� ���� ���, �����ڰ� �ϳ��� ���� ����

</br>

> **��������**

- ������ ������ JPA�� ����ϸ� `EntityManager` �� `@Autowired` �� ���� �����ϴ�.
- �ǹ������� ���� ������ �־ ��Ƽ ������ ��Ȳ�� ����ؼ� ȸ�� ���̺��� ȸ���� �÷��� ����ũ
���� ������ �߰��ϴ� ���� �����ϴ�.
- �ʵ� ���� ���, ������ ������ �������!

</br>

## ȸ�� ��� �׽�Ʈ
```java
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;

import static org.junit.jupiter.api.Assertions.*;

@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class MemberServiceTest {

    @Autowired MemberService memberService;
    @Autowired MemberRepository memberRepository;

    @Test
    public void ȸ������() throws Exception {
        // given
        Member member = new Member();
        member.setName("kim");

        // when
        Long saveId = memberService.join(member);

        // then
        assertEquals(member, memberRepository.findOne(saveId));
    }

    @Test(expected = IllegalStateException.class)
    public void �ߺ�_ȸ��_����() throws Exception {
        // given
        Member member1 = new Member();
        member1.setName("kim");

        Member member2 = new Member();
        member2.setName("kim");

        // when
        memberService.join(member1);
        memberService.join(member2);

        // then
        fail("���ܰ� �߻��ؾ� �Ѵ�.");
    }
}
```

</br>

> **��� ����**

- `@RunWith(SpringRunner.class)` : �������� �׽�Ʈ ����
- `@SpringBootTest` : ������ ��Ʈ ���� �׽�Ʈ(�̰� ������ `@Autowired` �� ����)
- `@Transactional` : �ݺ� ������ �׽�Ʈ ����, ������ �׽�Ʈ�� ������ ������ Ʈ������� �����ϰ�, **�׽�Ʈ�� ������ Ʈ������� ������ �ѹ�** (�� ������̼��� �׽�Ʈ ���̽����� ���� ���� �ѹ�)

</br>

> **��������**
- `@Test(expected = IllegalStateException.class)` : `try-catch`�� ��� ����� �����ϴ�!
- `fail()` : ������ ����!

</br>

## �׽�Ʈ ���̽��� ���� �߰� ����

</br>

> **�׽�Ʈ ���̽��� ���� ����**
 
- �׽�Ʈ�� ���̽� �ݸ��� ȯ�濡�� �����ϰ�, ������ �����͸� �ʱ�ȭ�ϴ� ���� ����.
    - �׷� �鿡�� �޸� DB�� ����ϴ� ���� ���� �̻����̴�.

- �߰��� �׽�Ʈ ���̽��� ���� ������ ȯ���, �Ϲ������� ���ø����̼��� �����ϴ� ȯ���� ���� �ٸ��Ƿ� ���� ������ �ٸ��� ����ϸ� ����.

- �׽�Ʈ ���丮�� �߰������� ���� ������ �����ϸ� �ȴ�.

</br>

-> ������ ��Ʈ�� datasource ������ ������, �⺻������ �޸� DB�� ����ϰ�, driver-class�� ���� ��ϵ� ���̺귯���� ���� ã���ֹǷ� ������ �߰� ������ ���� �ʾƵ� �ȴ�.