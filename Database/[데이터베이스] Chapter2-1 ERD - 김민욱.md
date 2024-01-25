# DB Application 설계 방법

1. 무엇을 설계할지 선택한다. (요구사항 분석)
2. 무엇을 만들것인지 알아낸다. (ER model)
    → **ER diagram(ERD)**을 그린다
3. ER diagram을 relational scheam로 변환한다.
4. relational schema를 다듬는다. (**nomalization**)
5. schema를 구현하고 데이터를 적재한다.

# ERD
### 정의

**ERD(Entity Relationship Diagram): 릴레이션(테이블) 간의 관계들을 정의한 것**

즉 현실 세계의 정보를 모델링하여 **객체와 객체 간의 관계를 표현** (요구사항 분석 단계에서 얻은 엔티티와 속성들의 관계를 그림으로 표현)

- 테이터베이스를 구축할 때 가장 기초적인 뼈대 역할을 한다.
- 서비스를 구축한다면 가장 먼저 신경써야 할 부분

### ERD의 중요성

- ERD는 **시스템의 요구사항**을 기반으로 작성
- 작성된 ERD를 기반으로 데이터베이스를 구축
- 데이터베이스를 구축한 이후에도 디버깅 또는 비즈니스 프로세스 재설계가 필요한 경우 설계도 역할을 담당

<aside>
💡 ERD는 관계형 구조로 표현할 수 있는 데이터를 구성하는 데 유용할 수 있지만 비정형 데이터를 충분히 표현 할 수 없다.
</aside>

```
비정형 데이터
비구조화 데이터를 말하며, 미리 정의된 데이터 모델이 없거나 미리 정의된 방식으로 정리되지 않은 정보. ex) 동영상 파일, 오디오 파일, 사진, 보고서(문서), 메일 본문 등
```

### ERD vs UML

- 소프트웨어 개발 과정에서 활용되는 다이어그램 도구
- UML은 시스템 구조와 동작을 모델링하기 위해 사용
- ERD는 데이터베이스의 구조를 표현하기 위해 사용

### ER Model

- ER model은 데이터베이스 디자인을 위한 데이터 모델
    - Data structures ↔ table
    - Integrity constraints ↔ foreign key
    - Operation ↔ SQL
- ER model → (ER Diagram) Database Design → Relational database model

1. Data structures

![Untitled (6)](https://github.com/k-kmw/CS_study/assets/100478309/010b73e0-2149-4a41-bea5-6bf02e47d818)

2. **Integrity constraints**
- Cardinality ratios
    - entity가 참여할 수 있는 최대 관계 인스턴스의 수
        - Ex) 1:1, 1:N, N:1, M:N
    - EX)
        1. work_for 관계는 (employee, department)와 N:1
        2. manages 관계는 (employee, department)와 1:1
        3. work_on 관계는 (employee, project)와 M:N
        
        ER 다이어그램에서는 아래와 같이 Cardinality를 표기

        ![Untitled (7)](https://github.com/k-kmw/CS_study/assets/100478309/d3f29fbd-e97e-441d-999c-d00d01b3810f)

- Participation Constraint
    - 얼마나 많은 entity들이 relation type에 참여할 수 있는지
        - Total
            - 모든 인스턴스가 관계에 참여
            - Department는 모두 Employee에 의해 Manage 되어야 함
            - ERD에서 두 줄로 표현
        - Partial
            - 일부 인스턴스가 관계에 참여
            - 일부 Employee는 Department를 Manage한다.
            - ERD에서 한 줄로 표현

    ![Untitled (8)](https://github.com/k-kmw/CS_study/assets/100478309/d0aded90-a720-4ece-816b-ad8888c04aa1)

- Structural Constraint
    - Cardinality ratio (1:1, 1:N, M:N)과 Participation constraints(partial, total)을 (min, max) 표기법으로 대체할 수 있다.
    - Ex1) ![Untitled (9)](https://github.com/k-kmw/CS_study/assets/100478309/9f1b0649-3393-48c0-9c3c-c38a76c75ab7)

    - Ex2) ![Untitled (10)](https://github.com/k-kmw/CS_study/assets/100478309/7c3f3aea-47c6-4cef-a37a-7767da85ba3d)

    - (0, N): Partial participation, max=N
    - (0, 1): Partial participation, max=1
    - (1, N): Total participation, max=N

3. **Operation**
- ER 모델링은 개념적 설계에 유용하지만, 실제 데이터베이스 시스템을 구현하는데 사용되지 않는다.

### ERD → Relational Schema
![Untitled (11)](https://github.com/k-kmw/CS_study/assets/100478309/2350812b-c38c-4c3b-9726-553a9fc9ea4d)
- Entity set → Relation
- Attributes of an entity set → Attributes of relation
- Relationship R → Relation (R에 있는 Attributes와 연결된 entity set들의 key 속성들을 가짐)
- Weak entity sets
- Combining relations (M to 1 relationships)

1. **Strong Entity Types**
![Untitled (12)](https://github.com/k-kmw/CS_study/assets/100478309/5df5bb83-6cae-43e1-95df-b1533c412e3c)

2. **Weak Entity Types**
![Untitled (13)](https://github.com/k-kmw/CS_study/assets/100478309/449e824d-b93d-443d-b8ad-6343c3f70360)

3. **1:1 Relationship Types**
![Untitled (14)](https://github.com/k-kmw/CS_study/assets/100478309/fc3c3d6a-f136-423c-8ac6-f3fc2350f4de)
- total participation을  찾아서 연관된 entity의 foreign key를 추가
- relationship type의 attribute를 추가

4. 1:N Relationship Types

![Untitled (15)](https://github.com/k-kmw/CS_study/assets/100478309/4da183db-60e7-4b61-99d1-3b3352b41eae)
![Untitled (16)](https://github.com/k-kmw/CS_study/assets/100478309/06da132e-9495-4b99-b48d-8e10ffb2577b)
- N쪽에 foreign key(1쪽에 primary key)를 추가

5. M:N Relationship Types

![Untitled (17)](https://github.com/k-kmw/CS_study/assets/100478309/a6a33725-56cf-4b2c-ad00-9416d870be9d)
![Untitled (18)](https://github.com/k-kmw/CS_study/assets/100478309/d601d995-3952-4b30-ad76-b0cb0e548a07)
- M:N은 새로운 Relation을 생성해야 함
- 새로운 Relation에 연관된 Entity의 Primary key를 Foreign key로 추가
- Relationship Type의 속성 추가

6. Multivalued Attributes
![Untitled (19)](https://github.com/k-kmw/CS_study/assets/100478309/2b690c85-eeb8-419d-aa18-3011e34c6507)
- 마찬가지로 새로운 Relation을 생성해야 함
- multi-valued 속성과 entity의 key 속성을 추가

### Summary
![Untitled (20)](https://github.com/k-kmw/CS_study/assets/100478309/aa9aef0e-18b3-4b97-b291-01abb84a4903)