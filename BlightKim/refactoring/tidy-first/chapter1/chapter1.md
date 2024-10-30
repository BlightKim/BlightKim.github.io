---
title: tidy-first(1)
layout: default
parent: 리팩토링
---

# 보호구문

> 보호 구문은 하나의 루틴에 하나의 반환문이 있어야 한다.
> ```java
> if (조건) {
> ...루틴의 나머지 모든 코드...
> }
> ```

* 예시코드(리팩토링 전)

```java
public class QueueManager {
    private final Map<String, Queue<String>> queues = new HashMap<>();
	private final Set<String> delayQueues = new HashSet<>();
    
    // 리팩토링 전
	public void declareQueue(String queueName) {
		emitBefore("declareQueue", queueName);
		queues.put(queueName, new LinkedList<>());
		emitAfter("declareQueue", queueName);

		String delayedName = dqName(queueName);
		queues.put(delayedName, new LinkedList<>());
		delayQueues.add(delayedName);
		emitAfter("declareDelayQueue", delayedName);
	}
}
```

* 예시코드(리팩토링 후)
```java
public class QueueManager {
	private final Map<String, Queue<String>> queues = new HashMap<>();
	private final Set<String> delayQueues = new HashSet<>();
    
    // 리팩토링 후
	public void declareQueue(String queueName) {
        // 보호구문 추가
		if (queues.containsKey(queueName)) {
			return;
		}

		emitBefore("declareQueue", queueName);
		queues.put(queueName, new LinkedList<>());
		emitAfter("declareQueue", queueName);

		String delayedName = dqName(queueName);
		queues.put(delayedName, new LinkedList<>());
		delayQueues.add(delayedName);
		emitAfter("declareDelayQueue", delayedName);
	}
}
```

