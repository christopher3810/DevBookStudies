
> 구체적 -> 추상적
> 코드 변경을 위해 지저분한 코드를 마주칠때마다 적용할 수 있는 작은 설계 Move를 다룸.
> 코드 정리는 리팩터링의 부분 집합.

### 1. 보호 구문
---

```java
if (condition)
 //..code
```


```java
if (condition) {
	if (!OtherCondition){
		//code ...	
	}
}
```

중첩된 조건은 헷갈림.

```java
if(!condition) return
if(OtherCondition) return 
//code...
```

위와 같이 하면 읽기가 쉬워짐
코드의 세부 사항을 살펴보기 전에 염두에 두어야할 몇 가지 전제조건이 존재한다 라고 말하는 것 같음.

>[!Note]
>[보호 구문(Guard Clause)](https://refactoring.com/catalog/replaceNestedConditionalWithGuardClauses.html)이란? 🤔 (α)
>보호 구문이란 코드 내에서 조건을 검사하여 조건이 충족되지 않을 때 함수의 나머지 부분을 실행하지 않도록 조기에 함수에서 반환하게 만드는 패턴. 
>즉, 특정 조건에서 코드의 `추가 실행을 보호`하는 역할을 함.

7~8개의 보호구문이 있는 경우 읽기가 매우 까다롭기 때문에 조건에 부합 하는 경우를 만나면 보호 구문을 넣어 코드를 정리 할것.

```java
if (condition){
	//..code
}
//OtherCode ..
```

처음 두줄을 바로 **도우미 메서드**로 추출한 다음에 보호 구문으로 코드를 정리할 수 있지만
항상 그리고 반드시 **작은 단계**를 거쳐서 코드를 정리하자.

>[!Note]
>**다시 보는 리팩토링에서 작은 단계를 거치는 것의 장점**
>1. **위험 최소화**: 작은 변경을 하고 각 변경 후에 테스트를 함으로써 시스템을 망가뜨릴 수 있는 버그 도입의 위험을 줄임.
>2. **즉각적인 피드백**: 작은 변경을 통해 변경이 시스템에 미치는 영향에 대한 즉각적인 피드백을 받을 수 있으며, 이는 지속적인 개선에 중요함.
>3. **롤백 용이**: 변경사항이 기대한 것과 다른 경우, 큰 부분의 시스템에 영향을 주지 않고 쉽게 되돌릴 수 있음.
>
>커밋을 작은 단위로 하는게 중요함.
#### 보호 구문 예제 코드 (α)
---
```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Queue;
import java.util.LinkedList;

public class QueueManager {
    private HashMap<String, Queue<Object>> queues;
    private HashSet<String> delayQueues;

    public QueueManager() {
        this.queues = new HashMap<>();
        this.delayQueues = new HashSet<>();
    }

    public void declareQueue(String queueName) {
		//Guard Clause
        if (this.queues.containsKey(queueName)) {
            return;
        }

        emitBefore("declare_queue", queueName);
        this.queues.put(queueName, new LinkedList<>());
        emitAfter("declare_queue", queueName);

        String delayedName = dqName(queueName);
        this.queues.put(delayedName, new LinkedList<>());
        this.delayQueues.add(delayedName);
        emitAfter("declare_delay_queue", delayedName);
    }

    private void emitBefore(String event, String queueName) {
        // 이벤트 발행 전 로직
    }

    private void emitAfter(String event, String queueName) {
        // 이벤트 발행 후 로직
    }

    private String dqName(String queueName) {
        // 지연된 큐 이름을 생성하는 로직
        return "DQ_" + queueName;
    }
}

```

#### 도우메 메서드 예제 (α)
---

```java
public class InputValidator {
    public void validateInput(String inputData) {
        if (!isValid(inputData)) {
            throw new IllegalArgumentException("Invalid input");
        }
        // 입력 데이터 처리 로직
    }

	// 도우미 메서드
    private boolean isValid(String inputData) {
        return inputData != null && !inputData.isEmpty();
    }
}
```
### 2. 안쓰는 코드
---

>[!Note]
>실행되지 않는 코드는 지워 버리세요 그게 다입니다.

리플렉션을 여러번 사용한 코드와 같이 안쓰는 코드를 찾기 힘들 경우.

로그를 활용해서 지우고 나서 실행해 보자.

만약에
많은 코드가 당장 사용하지 않지만 미래에 사용되길 원하고 원래 작성된 방식과 정확히 동일한 방식으로 여전히 작동하는 경우에.
형상관리 도구를 활용해 가져오던지, 재작성이 빠르면 그때 작성하자.

>[!warning]
>단 정리 과정에서 코드는 조금씩만 삭제하자.
>비교적 쉽게 복구가 가능하다.

### 3. 대칭으로 맞추기
---

아직 계산하지 않았다면 foo의 값을 계산하고 임시보관 하라

```java
//if 문 활용
if (foo == null) {
    foo = computeFoo();
}

//optional 을 활용
Optional<Integer> fooOptional = Optional.ofNullable(foo);
foo = fooOptional.orElseGet(() -> computeFoo());

//synchronized 활용하여 thread safe 하게
if (foo == null) {
    synchronized (this) {
        if (foo == null) {
            foo = computeFoo();
        }
    }
}

//Concurrenthashmap의 key로 활용
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
foo = map.computeIfAbsent("key", k -> computeFoo());

```

서로 다른 스타일로 존재 할 수 있음. \
한가지 패턴의 코드를 읽다보면 금방 익숙해지나, 그 이상인 경우 혼란스러워 지며 기존과 다르면, \
다른 동작을 하는 코드라고 예단하게 됨.

>[!Note]
>한가지 방식을 선택해서 정하고 수정하자.
>세부사항에 묻혀 드러나지 않는다고 해도 같지 않은 루틴을 찾고 같은 부분속에 다른 부분들을 분리하자.

### 4. 새로운 인터페이스로 기존 루틴 부르기
---
