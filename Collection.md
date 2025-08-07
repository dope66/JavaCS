# Java Collection

## Collection 왜 사용하지?
Java Collection 에는 List, Map, Set 인터페이스를 기준으로 여러 구현체가 존재한다. 이에 더해 Stack과 Queue 인터페이스도 존재한다. 왜 이러한 Collection 을 사용하는 것일까? 그 이유는 다수의 Data 를 다루는데 표준화된 클래스들을 제공해주기 때문에 DataStructure 를 직접 구현하지 않고 편하게 사용할 수 있기 때문이다. 또한 배열과 다르게 객체를 보관하기 위한 공간을 미리 정하지 않아도 되므로, 상황에 따라 객체의 수를 동적으로 정할 수 있다. 이는 프로그램의 공간적인 효율성 또한 높여준다.

**추가 장점:**
- **타입 안정성**: Generic을 통한 컴파일 타임 타입 체크
- **풍부한 API**: 다양한 알고리즘과 유틸리티 메서드 제공
- **표준화된 인터페이스**: 코드 일관성과 유지보수성 향상
- **메모리 관리**: 자동 크기 조절로 메모리 효율성

## List
**특징:** 순서가 있고, 중복을 허용하는 컬렉션

### ArrayList vs LinkedList 비교

#### 내부 구조
- **ArrayList**: 동적 배열 구조로 연속된 메모리 공간에 저장
- **LinkedList**: 이중 연결 리스트로 각 노드가 다음/이전 노드 참조

#### 성능 비교
| 연산 | ArrayList | LinkedList |
|------|-----------|------------|
| **접근(get)** | O(1) | O(n) |
| **끝에 추가** | O(1) 평균 | O(1) |
| **중간 삽입/삭제** | O(n) | O(1) |
| **메모리 사용량** | 효율적 | 3배 이상 사용 |

```java
// ArrayList 선택 기준
List<String> arrayList = new ArrayList<>();
// - 인덱스 기반 접근이 빈번한 경우
// - 메모리 효율성이 중요한 경우
// - 캐시 성능이 중요한 대용량 데이터

// LinkedList 선택 기준  
List<String> linkedList = new LinkedList<>();
// - 중간 삽입/삭제가 빈번한 경우
// - Queue/Deque로도 사용해야 하는 경우
```

#### ArrayList 용량 증가 메커니즘
- 초기 용량: 10개
- 증가율: **1.5배** (50% 증가)
- 기존 배열을 새 배열로 복사하는 과정에서 일시적으로 메모리 2배 사용

### Vector와의 차이점
| 특성 | ArrayList | Vector |
|------|-----------|--------|
| **동기화** | 비동기화 | 동기화 |
| **성능** | 빠름 | 느림 (동기화 오버헤드) |
| **용량 증가** | 50% | 100% |
| **권장도** | 권장 | 비권장 (레거시) |

## Map
**특징:** Key-Value 쌍으로 데이터 저장, Key는 중복 불가

### HashMap vs LinkedHashMap vs TreeMap 비교

#### 내부 구조
- **HashMap**: Hash Table (배열 + 연결 리스트/트리)
- **LinkedHashMap**: HashMap + 이중 연결 리스트 (순서 유지)
- **TreeMap**: Red-Black Tree (자가 균형 이진 탐색 트리)

#### 성능 및 특징 비교
| 특성 | HashMap | LinkedHashMap | TreeMap |
|------|---------|---------------|---------|
| **시간 복잡도** | O(1) 평균 | O(1) 평균 | O(log n) |
| **순서 보장** | X | 삽입/접근 순서 | Key 정렬 순서 |
| **null 허용** | Key 1개, Value 다수 | Key 1개, Value 다수 | Value만 |
| **메모리** | 가장 효율적 | HashMap의 1.5배 | HashMap의 1.2배 |

#### HashMap 내부 동작 원리
```java
// 1. 해시 함수로 버킷 위치 결정
int hash = key.hashCode();
int index = hash & (table.length - 1);

// 2. 해시 충돌 처리 (Java 8+)
// - 8개 미만: 연결 리스트
// - 8개 이상: Red-Black Tree로 변환 (O(n) → O(log n))
```

#### 사용 예제
```java
// HashMap: 일반적인 용도 (90% 경우)
Map<String, User> userMap = new HashMap<>();

// LinkedHashMap: 순서가 중요한 경우
Map<String, String> orderedMap = new LinkedHashMap<>();

// TreeMap: 정렬이 필요한 경우
Map<String, Integer> sortedMap = new TreeMap<>();
```

#### LRU Cache 구현 (LinkedHashMap 활용)
```java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int maxSize;
    
    public LRUCache(int maxSize) {
        super(16, 0.75f, true); // accessOrder = true
        this.maxSize = maxSize;
    }
    
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > maxSize;
    }
}
```

#### Hashtable vs HashMap
**Hashtable 사용을 피해야 하는 이유:**
- 모든 메서드가 synchronized로 성능 오버헤드
- null 값 불허용 (key, value 모두)
- JDK 1.0 레거시 클래스

**대안:**
```java
// 단일 스레드: HashMap
Map<String, String> map = new HashMap<>();

// 멀티 스레드: ConcurrentHashMap (권장)
Map<String, String> concurrentMap = new ConcurrentHashMap<>();
```

## Set
**특징:** 중복을 허용하지 않는 컬렉션

### HashSet vs LinkedHashSet vs TreeSet 비교

#### 내부 구조
- **HashSet**: HashMap을 내부적으로 사용 (value는 더미 객체)
- **LinkedHashSet**: LinkedHashMap 사용 (삽입 순서 유지)
- **TreeSet**: TreeMap 사용 (Red-Black Tree)

#### 성능 및 특징 비교
| 특성 | HashSet | LinkedHashSet | TreeSet |
|------|---------|---------------|---------|
| **시간 복잡도** | O(1) 평균 | O(1) 평균 | O(log n) |
| **순서 보장** | X | 삽입 순서 | 정렬된 순서 |
| **null 허용** | O | O | X |
| **메모리** | 가장 효율적 | 1.5배 | 1.2배 |

#### 사용 예제
```java
// HashSet: 빠른 중복 검사
Set<String> uniqueIds = new HashSet<>();

// LinkedHashSet: 순서 유지하며 중복 제거
Set<String> orderedTags = new LinkedHashSet<>();

// TreeSet: 자동 정렬
Set<String> sortedNames = new TreeSet<>();
```

#### TreeSet의 고급 기능 (NavigableSet)
```java
TreeSet<Integer> numbers = new TreeSet<>(Arrays.asList(1, 3, 5, 7, 9));

// 범위 검색
numbers.subSet(3, 8);     // [3, 5, 7]
numbers.headSet(5);       // [1, 3]
numbers.tailSet(5);       // [5, 7, 9]

// 근사치 검색
numbers.lower(5);         // 3 (5보다 작은 가장 큰 값)
numbers.ceiling(4);       // 5 (4보다 크거나 같은 가장 작은 값)
```

## Stack & Queue

### Stack 구현 방식
**권장하지 않음: java.util.Stack**
```java
Stack<String> oldStack = new Stack<>(); // 사용 금지!
```
**문제점:**
- Vector 상속으로 동기화 오버헤드
- 중간 접근 가능 (스택 원칙 위반)
- 성능 저하

**권장: ArrayDeque 사용**
```java
Deque<String> stack = new ArrayDeque<>();
stack.push("item");  // 스택에 추가
stack.pop();         // 스택에서 제거
```

### Queue 구현체 비교

#### ArrayDeque vs LinkedList vs PriorityQueue

| 특성 | ArrayDeque | LinkedList | PriorityQueue |
|------|------------|------------|---------------|
| **내부 구조** | 원형 배열 | 이중 연결 리스트 | Binary Heap |
| **양방향 지원** | O | O | X |
| **메모리 효율** | 높음 | 낮음 (3배 사용) | 보통 |
| **정렬** | X | X | 우선순위 정렬 |

#### 성능 비교
| 연산 | ArrayDeque | LinkedList | PriorityQueue |
|------|------------|------------|---------------|
| **앞에 추가** | O(1) | O(1) | 지원 안함 |
| **뒤에 추가** | O(1) | O(1) | O(log n) |
| **앞에서 제거** | O(1) | O(1) | O(log n) |
| **뒤에서 제거** | O(1) | O(1) | 지원 안함 |

#### 사용 예제
```java
// ArrayDeque: 일반적인 큐/스택 (권장)
Queue<String> queue = new ArrayDeque<>();
Deque<String> deque = new ArrayDeque<>();

// LinkedList: List도 함께 사용해야 하는 경우
List<String> list = new LinkedList<>();
Queue<String> queue2 = (Queue<String>) list;

// PriorityQueue: 우선순위 기반 처리
PriorityQueue<Task> taskQueue = new PriorityQueue<>(
    Comparator.comparing(Task::getPriority).reversed()
);
```

#### PriorityQueue 사용 예제
```java
// 최소 힙 (기본)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// 최대 힙
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// 커스텀 비교자
PriorityQueue<Person> personQueue = new PriorityQueue<>(
    Comparator.comparing(Person::getAge)
);
```


### 1. equals()와 hashCode() 계약
```java
// hashCode() 규칙
// 1. equals()가 true면 hashCode()도 같아야 함
// 2. hashCode()가 같다고 equals()가 true는 아님
// 3. 객체가 변경되지 않으면 hashCode()도 일정해야 함

public class Person {
    private String name;
    private int age;
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && Objects.equals(name, person.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

### 2. fail-fast vs fail-safe
```java
// fail-fast: ConcurrentModificationException 발생
List<String> list = new ArrayList<>();
for (String item : list) {
    list.remove(item); // 예외 발생!
}

// fail-safe: 복사본으로 안전한 순회
List<String> safeList = new CopyOnWriteArrayList<>();
for (String item : safeList) {
    safeList.remove(item); // 안전함
}
```

### 3. 동기화 방법들
```java
// 1. Collections.synchronizedXxx()
List<String> syncList = Collections.synchronizedList(new ArrayList<>());

// 2. Concurrent Collections (권장)
Map<String, String> concurrentMap = new ConcurrentHashMap<>();
List<String> concurrentList = new CopyOnWriteArrayList<>();

// 3. 직접 동기화
synchronized(list) {
    // 동기화 블록
}
```

### 4. 컬렉션 선택 가이드
```java
// List 선택
// - 인덱스 접근 많음: ArrayList
// - 중간 삽입/삭제 많음: LinkedList (드물게)
// - 스레드 안전성: CopyOnWriteArrayList

// Set 선택  
// - 빠른 검색: HashSet
// - 순서 유지: LinkedHashSet
// - 정렬 필요: TreeSet

// Map 선택
// - 일반적 용도: HashMap
// - 순서 유지: LinkedHashMap  
// - 정렬 필요: TreeMap
// - 스레드 안전성: ConcurrentHashMap

// Queue 선택
// - 일반적 큐/스택: ArrayDeque
// - 우선순위: PriorityQueue
// - 스레드 안전성: LinkedBlockingQueue
```
