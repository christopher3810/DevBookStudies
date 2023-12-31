
>무결성(Integrity)은 데이터가 `정확하고 완전해야 한다` 는 것을 의미하는 말.

**정합성은?**

> `데이터들의 값이 일치`할 때 데이터 정합성이 맞는다고 볼 수 있음.
> 데이터가 `모순 없이 일관되게 일치` 해야 함을 의미.

특정 데이터는 <span style="color:#00FFCC">정합성은 만족하지만 무결성을 만족하지 못하는 케이스</span>가 존재 할 수 있음.

ex) 중복 데이터가 전부 틀린 값으로 일치하고 있는 경우에는 `정합성`은 만족하나 `무결성`은 만족하지 못함

무결성이 더 광범위 하며 `무결성을 지키는 것`이 데이터 모델링의 최고 목표임.

## 무결성을 보장하는 방법
---

### 엔터티 무결성(Entity Integrity)
---

> 엔터티 무결성은 엔터티가 되기 위한 최소한의 조건이므로 지켜지지 않으면 엔터티가 아님.

엔터티 무결성은 데이터베이스의 각 행이 유일하게 식별되어야 한다는 원칙 

즉 `식별자`에 의해서 지켜질 수 있음.

특히, `주 식별자(primary key)`에 해당하는 열은 `중복`되거나 `null 값`이 있으면 안 됨. 

**예시**

|StudentID|Name|Age|
|---|---|---|
|1|John Doe|22|
|2|Jane Doe|24|
|3|Mary Jane|21|

위 테이블에서 `StudentID`가 각 학생을 고유하게 식별하는 주 식별자. 

이 값은 `중복`되거나 `null`이 될 수 `없음.

### 참조 무결성(Referential Integrity) 
---

참조 무결성은 `외래 키(foreign key)가 참조하는 테이블의 주 키`를 항상 참조하도록 하는 원칙. 

외래 키 값이 `참조하는 테이블의 주 키` 값 중 하나와 `일치`하거나 `null`이어야 함. 

>관계형 데이터 베이스에 `집합(엔터티) 사이에 연산`할 수 있도록 하는 개념.

**예시**

|CourseID|CourseName|StudentID|
|---|---|---|
|1|Math|1|
|2|English|1|
|3|History|2|

위 테이블에서 `StudentID`는 외래 키로, 각각의 코스가 어떤 학생에 의해 수강 되고 있는지 를 나타냄. 

이 값은 학생 테이블의 `StudentID`와 일치해야 함.

### 도메인 무결성(Domain Integrity) 
---

도메인 무결성은 `특정 속성의 값이 그 속성이 가질 수 있는 범위나 조건에 부합`해야 한다는 원칙. 

**예시**

|StudentID|Name|Age|
|---|---|---|
|1|John Doe|22|
|2|Jane Doe|24|
|3|Mary Jane|21|

위 테이블에서 `Age`는 학생의 나이를 나타내며, 이 값은 일반적으로` 양의 정수`여야 함.

### 업무 무결성(Business Integrity)
---

업무 무결성은 데이터베이스가 비즈니스 규칙을 준수해야 한다는 원칙. 

이는 더 복잡하며, 데이터베이스 스키마나 제약 조건만으로는 해결할 수 없는 경우가 많음.

업무 무결성은 범위가 넓어 주로 프로그램에서 체크함.


**예시**

|OrderID|Product|Quantity|Price|
|---|---|---|---|
|1|Book|3|30|
|2|Pen|2|5|
|3|Paper|10|20|

위 테이블에서 `Price`는 `Product`와 `Quantity`에 의해 결정됩니다. 

이는 비즈니스 규칙에 의해 결정되며, 이러한 규칙은 데이터베이스에서 트리거 등을 사용해 관리됩니다.


>**트리거란?**
>데이터베이스에서 특정 이벤트(삽입, 삭제, 업데이트 등)가 발생할 때 자동으로 실행되도록 설정된 `프로시저`
