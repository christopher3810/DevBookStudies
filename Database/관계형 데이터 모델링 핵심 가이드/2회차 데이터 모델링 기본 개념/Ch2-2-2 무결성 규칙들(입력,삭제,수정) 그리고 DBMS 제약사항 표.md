
### 참조/업무 무결성을 지키기 위한 다양한 규칙
---


### 입력 규칙

1. Dependent입력 규칙 
   상위(부모) 엔터티에 해당하는 주 식별자 값이 존재할 때만 하위(자식) 엔터티의 외래 식별자에 입력 가능
   
2. Automatic 입력 규칙 
   상위(부모) 엔터티에 해당하는 주 식별자 값이 없을 때 상위(부모) 엔터티에 주 식별자를 생성
   
3. Default 입력 규칙 
   상위(부모) 엔터티에 해당하는 주 식별자 값이 없을 때 하위(자식) 엔터티의 외래 식별자에 기본 값으로 입력.  
   외래 식별자에 기본 값이 설정돼 있어야 함.
   
4. Nullify 입력 규칙 
   상위(부모) 엔터티에 해당하는 주 식별자 값이 없을 때 하위(자식) 엔터티의 외래 식별자에 널(Null) 값으로 입력.  
   외래 식별자는 널(Null) 허용으로 설정해야 함.
  

##### 삭제 규칙

1. Restrict 삭제 규칙 
   상위(부모) 엔터티의 주 식별자를 삭제할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 없을 때만 상위(부모) 엔터티의 주 식별자 삭제를 허용.
   
2. Cascade 삭제 규칙 
   상위(부모) 엔터티의 주 식별자를 삭제할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 존재하면 해당 값을 모두 삭제하고 나서 상위(부모) 엔터티의 주 식별자 삭제
   
3. Default 삭제 규칙 
   상위(부모) 엔터ㅣ의 주 식별자를 삭제할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 존재하면 해당 값을 모두 기본 값으로 수정하고 나서 상위(부모) 엔터티의 주 식별자 삭제
   
4. Nullify 삭제 규칙 
   상위(부모) 엔터이의 주 식별자를 삭제할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 존재하면 해당 값을 모두 널(Null) 값으로 수정하고 나서 상위(부모) 엔터티의 주 식별자 삭제

  

##### 수정 규칙

1. Restrict 수정 규칙 
   상위(부모) 엔터티의 주 식별자를 업데이트 할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 없을 때만 상위(부모) 엔터티의 주 식별자를 수정

2. Cascade 수정 규칙 
   상위(부모) 엔터티의 주 식별자를 업데이트 할 때, 같은 값이 하위(자식) 엔터티의 외래 식별자에 존재하면 해당 값을 모두 업데이트한 후에 상위(부모) 엔터티의 주 식별자를 수정


### 예제
---

**예제 데이터 테이블**

```plaintext
Customers
| CustomerID | Name  |
|------------|-------|
| 1          | John  |
| 2          | Jane  |

Orders
| OrderID | CustomerID | Product |
|---------|------------|---------|
| 1       |            | Apples  |

```

### 입력 규칙
---

**Dependent입력 규칙**.

`Orders` 테이블 `CustomerID`에 값을 입력하려면 해당 `CustomerID`가 `Customers` 테이블에 존재해야 합니다. `1` 또는 `2`가 가능하지만 `3`은 불가능

**Automatic 입력 규칙**.

이 규칙은 자동 증가(auto-increment) 기능을 활용하는 경우가 대표적. \ 
새로운 레코드를 `Customers` 테이블에 추가할 때 `CustomerID`는 자동으로 생성.

**Default 입력 규칙**. 

`Orders` 테이블의 `CustomerID`에 기본 값이 설정되어 있다고 가정하겠습니다. `CustomerID`가 `Customers` 테이블에 없을 때, `Orders` 테이블에는 이 기본값이 입력됩니다.

**Nullify 입력 규칙**.

`Orders` 테이블의 `CustomerID`가 `Customers` 테이블에 없는 경우 `Null`로 설정됩니다.

### 삭제 규칙
---

**Restrict 삭제 규칙**

예를 들어 `Customers` 테이블에서 `CustomerID`가 `1`인 레코드를 삭제하려고 하면, `Orders` 테이블에 `CustomerID` `1`이 참조되고 있지 않아야 함.

**Cascade 삭제 규칙**

`Customers` 테이블에서 `CustomerID`가 `1`인 레코드를 삭제하면, `Orders` 테이블에서 `CustomerID`가 `1`인 모든 레코드도 삭제.

**Default 삭제 규칙**

`Customers` 테이블에서 `CustomerID`가 `1`인 레코드를 삭제하면, `Orders` 테이블에서 `CustomerID`가 `1`인 모든 레코드의 `CustomerID`가 기본 값으로 변경.

**Nullify 삭제 규칙**

`Customers` 테이블에서 `CustomerID`가 `1`인 레코드를 삭제하면, `Orders` 테이블에서 `CustomerID`가 `1`인 모든 레코드의 `CustomerID`가 `Null`로 변경.

### 수정 규칙과 예제
---

**Restrict 수정 규칙**

`Customers` 테이블에서 `CustomerID`가 `1`인 레코드의 `CustomerID`를 `3`으로 변경하려면, `Orders` 테이블에 `CustomerID` `1`이 참조되고 있지 않아야 합니다.

**Cascade 수정 규칙**

`Customers` 테이블에서 `CustomerID`가 `1`인 레코드의 `CustomerID`를 `3`으로 변경하면, `Orders` 테이블에서 `CustomerID`가 `1`인 모든 레코드의 `CustomerID`도 `3`으로 변경됩니다.


### DBMS 제약

|제약|특징|주요무결성|
|---|---|---|
|Primary Key|- 엔터티에서 PK로 지정된 속성에는 동일한 값을 가질 수 없다.<br>- PK로 지정된 속성에는 널(Null) 값을 가지면 안 된다.|엔터티 무결성|
|Unique|- 엔터티에서 Unique로 지정된 속성에는 동일한 값을 가질 수 없다.<br>- Unique로 지정된 속성에는 널(Null) 값을 가질 수 있다.<br>- 엔터티에 여러 개의 Unique 속성이 지정될 수 있다|엔터티무결성|
|Foreign Key|- 관계 속성의 FK값은 참조 엔터티의 PK 속성에 존재하거나 널(Null)이어야 한다.<br>- 참조 엔터티의 PK값이 수정.삭제되면 참조한 모든 값은 수정.삭제돼야 한다.|참조 무결성|
|Check|- 속성 값에는 특정한 범위의 값이나 특정 규칙을 따르는 값만이 존재할 수 있다.|도메인 무결성|
|Default|- 속성 값에는 특정한 범위의 값이나 특정 규칙을 따르는 값만이 존재할 수 있다.|도메인 무결성|
|Data Type|- 속성에 데이터 타입을 지정해 특정 형식을 유지할 수 있다.|도메인 무결성|
|Null/Not Null|- 속성 값이 널(Null)일 수 있는지, 값이 반드시 존재해야 하는지를 지정한다.|도메인 무결성|
|Trigger|- 속성 값이 입력되거나 수정.삭제 될 때 자동적으로 데이터를 처리할 수 있도록 지정|업무 무결성|