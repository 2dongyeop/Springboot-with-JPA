# ������ �м� ����

## ������ �𵨰� ���̺� ����
<img width="482" alt="image" src="https://user-images.githubusercontent.com/106216912/213923601-f5bc6384-2daf-4640-ba10-34c5dbf6a462.png">

- **ȸ��, �ֹ�, ��ǰ�� ���� :** ȸ���� ���� ��ǰ�� �ֹ��� �� �ִ�.

    - �׸��� �� �� �ֹ��� �� ���� ��ǰ�� ������ �� �����Ƿ� �ֹ��� ��ǰ�� �ٴ�� �����.

    - ������ �̷� **�ٴ�� ����� ������ �����ͺ��̽��� �����̰� ��ƼƼ������ ���� ������� �ʴ´�!**

</br>

> **����, �ֹ���ǰ�̶�� ��ƼƼ�� �߰��Ͽ� �ٴ�� ���踦 �ϴ��, �ٴ��� ����� Ǯ��� �Ѵ�.**

</br>

> **�ܷ� Ű�� �ִ� ���� ���������� �������� ���ض�!**
- ���������� ������ ����Ͻ��� ������ �ִٰ� �������� ������ ����, �ܼ��� �ܷ� Ű�� ���� �����ϴ����� �����̴�.

- ���� �� �ڵ����� ������ ������, �ϴ�� ���迡�� �׻� ���ʿ� �ܷ� Ű�� �����Ƿ� �ܷ� Ű�� �ִ� ������ ���������� �������� ���ϸ� �ȴ�.

- ���� �ڵ����� ���������� �������� ���ϴ� ���� �Ұ��� �� ���� �ƴ�����, �ڵ����� ���������� �������� ���ϸ� �ڵ����� �������� �ʴ� ���� ���̺��� **�ܷ� Ű ���� ������Ʈ �ǹǷ� ������ ���������� ��ư�, �߰������� ������ ������Ʈ ������ �߻��ϴ� ���� ����**�� �ִ�.

</br>

## ��ƼƼ Ŭ���� ����
> **�ǹ������� ������ Getter�� ����ΰ�, Setter�� �� �ʿ��� ��쿡�� ����ϴ� ���� ��õ�Ѵ�!**
- �̷������δ� Getter, Setter ��� �������� �ʰ�, �� �ʿ��� ������ �޼��带 �����ϴ°� ����
�̻����̴�.

- �ǹ����� ��ƼƼ�� �����ʹ� ��ȸ�� ���� �ʹ� �����Ƿ�, Getter�� ��� ��� ����δ� ���� �����ϴ�.
     - Getter�� �ƹ��� ȣ���ص� ȣ�� �ϴ� �� ������ � ���� �߻������� �ʴ´�.

</br>

-> Setter�� ������ �ٸ���.
- **Setter�� ȣ���ϸ� �����Ͱ� ���Ѵ�.**
    - Setter�� �� ����θ� ����� �̷��� ��ƼƼ�� ����ü �� ����Ǵ��� �����ϱ� ���� ���������.

    - �׷��� ��ƼƼ�� ������ ���� Setter ��ſ� ���� ������ ��Ȯ�ϵ��� ������ ���� ����Ͻ� �޼��带 ������ �����ؾ� �Ѵ�.

</br>

- ȸ�� ��ƼƼ ����
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue // �⺻Ű ����, �� ���� ��Ʈ��!
    @Column(name = "member_id") // �Ӽ��� ����
    private Long id;

    private String name;

    @Embedded // �Ӻ���� Ÿ�� ���
    private Address address;

    @OneToMany(mappedBy = "member") // Order ���̺��� �ִ� member�� ���� ���ε� �ſ��� ��!
    private List<Order> orders = new ArrayList<>();
}
```

- ��ƼƼ�� �ĺ��ڴ� `id` �� ����ϰ� PK �÷����� `member_id` �� ����ߴ�.
    - ��ƼƼ�� Ÿ��(���⼭�� `Member`)�� �����Ƿ� `id` �ʵ常���� ���� ������ �� �ִ�.
    - ������, ���̺��� Ÿ���� �����Ƿ� ������ ��ƴ�.
- �׸��� ���̺��� ���ʻ� `���̺��� + id` �� ���� ����Ѵ�.
    - ������ ��ü���� `id` ��ſ� `memberId` �� ����ص� �ȴ�.
-> �߿��� ���� **�ϰ���**�̴�!

</br>

> **�ǹ������� `@ManyToMany` �� ������� ����!**

- `@ManyToMany` �� ������ �� ������, �߰� ���̺�(`CATEGORY_ITEM`)�� �÷��� �߰��� �� ����, �����ϰ� ������ �����ϱ� ��Ʊ� ������ �ǹ����� ����ϱ⿡�� �Ѱ谡 �ִ�.

- �߰� ��ƼƼ(`CategoryItem` �� ����� `@ManyToOne` , `@OneToMany` �� �����ؼ� �������.

-> �����ϸ� **�ٴ�� ������ �ϴ��, �ٴ��� �������� Ǯ��� �������!**

</br>

> **�� Ÿ���� ���� �Ұ����ϰ� �����ؾ� �Ѵ�.**

- **`@Setter` �� �����ϰ�, �����ڿ��� ���� ��� �ʱ�ȭ�ؼ� ���� �Ұ����� Ŭ������ ������.**

- JPA ����� ��ƼƼ�� �Ӻ���� Ÿ��(`@Embeddable`)�� �ڹ� �⺻ ������(default constructor)�� `public` �Ǵ� `protected` �� �����ؾ� �Ѵ�.

- `public` ���� �δ� �� ���ٴ� `protected` �� �����ϴ� ���� �׳��� �� �����ϴ�.
    - JPA������ `protected` = ��������!

- JPA�� �̷� ������ �δ� ������ **JPA ���� ���̺귯���� ��ü�� ������ �� ���÷��� ���� ����� ����� �� �ֵ��� �����ؾ� �ϱ� ����**�̴�.

</br>

## ��ƼƼ ����� ������
> **��ƼƼ���� ������ Setter�� ������� ����!**

- **���� ����Ʈ�� �ʹ� ���Ƽ�, ���������� ��ƴ�.**

</br>

> **��� ��������� �����ε����� ����!**

- **��÷ε�(`EAGER`)�� ������ ��ư�, � SQL�� ������� �����ϱ� ��ƴ�.**

- Ư�� JPQL�� ������ �� `N+1` ������ ���� �߻��Ѵ�.

- **�ǹ����� ��� ��������� �����ε�(`LAZY`)���� �����ؾ� �Ѵ�!**

- **`@XToOne(OneToOne, ManyToOne)` ����� �⺻�� ��÷ε��̹Ƿ� ���� �����ε����� �����ؾ�
�Ѵ�.**

</br>

> **�÷����� �ʵ忡�� �ʱ�ȭ ����!**

- **�÷����� �ʵ忡�� �ٷ� �ʱ�ȭ �ϴ� ���� ����**�ϴ�.
- `null` �������� �����ϴ�.

    - ���̹�����Ʈ�� ��ƼƼ�� ����ȭ �� ��, �÷����� ���μ� ���̹�����Ʈ�� �����ϴ� ���� �÷������� �����Ѵ�.
    
    - ���� **�ʵ巹������ �����ϴ� ���� ���� �����ϰ�, �ڵ嵵 �����ϴ�.**