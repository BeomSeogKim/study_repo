## Collection

![image](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20200811210521/Collection-Framework-1.png)

### List

객체들의 순서가 있으며 중복을 허용

##### ArrayList

- 배열을 사용해서 데이터를 관리
- 기본 CAPACITY : 10 (CAPACITY를 넘기면 50% 씩 증가시킴)
- 메모리 고속 복사 연산을 사용함
  - 배열의 요소 이동과 관련하여 System.arraycopy()를 사용해 고속 복사 연산을 활용
  - 비교적 빠른 수행 시간

##### LinkedList

- 이중 연결 리스트 구조
- 첫 번째 노드와 마지막 노드 둘다 참조

시간 복잡도

| 기능            | ArrayList | LinkedList |
| --------------- | --------- | ---------- |
| 앞에 추가(삭제) | O(N)      | O(1)       |
| 중간 추가(삭제) | O(N)      | O(N)       |
| 뒤에 추가(삭제) | O(1)      | O(1)       |
| 인덱스 조회     | O(1)      | O(N)       |
| 검색            | O(N)      | O(N)       |

이론적으로 중간 추가 및 삭제 관련하여 LinkedList 중간 삽입 연산이 ArraList보다 빠를 순 있음

그러나 실제로는 순차적 접근 속도, 메모리 할당 및 해제 비용, CPU 캐시 활용도 등 다양한 요소에 의해 영향을 받음

### Queue

- Queue의 대표적인 구현체는 ArrayDeque, LinkedList가 존재함

##### Deque

- Double Ended Queue의 약자로 양쪽 끝에서 요소를 추가하거나 제거할 수 있음
- 큐와 스택의 기능을 모두 포함하고 있음

### Set

중복을 허용하지 않고 순서를 보장하지 않음

##### HashSet

- 해시 자료 구조를 사용해 요소를 저장
- 요소들은 특정한 순서 없이 저장됨
- 주요 연산들의 경우 O(1) 시간 복잡도를 가짐
- 데이터의 유일성만 중요하고, 순서가 중요하지 않은 경우에 적합

##### LinkedHashSet

- HashSet 자료구조에 LinkedList를 추가해 요소들의 순서를 유지함
- 순서대로 조회 시 추가된 순서대로 반환됨
- 주요 연산들의 경우 O(1) 시간 복잡도를 가짐
- 데이터 유일성과 함께 삽입 순서를 유지해야 할 때 적합

##### TreeSet

- 이진 탐색 트리를 개선한 레드-블랙 트리를 내부에서 사용
- 요소들은 정렬된 순서로 저장됨 
  - 순서의 기준은 Comparator로 변경할 수 있음
- 주요 연산들의 경우 O(log N) 시간 복잡도를 가짐 
- 데이터를 정렬된 순서로 유지하면서 집합의 특성을 유지해야 할 때 사용

### Map

key - value 쌍을 저장하는 자료 구조

##### HashMap

- 해시를 사용하여 요소를 저장함
- 주요 연산들의 경우 O(1) 시간 복잡도를 가짐

##### LinkedHashMap

- HashMap과 유사하지만 LinkedList를 사용해 삽입 순서 또는 최근 접근 순서에 따라 요소를 유지
- 입력 순서에 따라 순회가 가능함
- 주요 연산들의 경우 O(1) 시간 복잡도를 가짐

##### TreeMap

- 레드 - 블랙 트리를 기반으로 한 구현
- 모든 키는 자연 순서 혹은 생성자에 제공된 Comparator를 기반으로 정렬됨
- 주요 연산들의 경우 O(log N) 시간 복잡도를 가짐

---

### Comparable Comparator

Comparator

```java
public interface Comparator<T> {
  int compare(T o1, T o2);
}
```

두 인수를 비교해서 결과 값을 반환하면 됨

- 첫번째 인수가 더 작으면 음수 
- 두 값이 같으면 0 
- 첫번째 인수가 더 크면 양수

Comparable 

```java
public interface Comparable<T> {
	public int compareTo(T o);
}
```

자기 자신과 인수로 넘어온 객체를 비교해 반환하면 됨

- 현재 객체가 인수로 주어진 객체보다 더 작으면 음수
- 두 객체의 크기가 같으면 0 
- 현재 객체가 인수로 주어진 객체보다 더 크면 양수

### equals & hashCode의 중요성

Collection을 사용할 때 (특히 Set, Map) 사용자가 정의한 객체를 사용하는 경우 hashCode와 equals 메서드를 재정의 해야함

- hashCode
  - hash값을 기반으로 Collection에 저장하거나 값을 찾음
  - 만약 재정의를 하지 않을 경우 Collection에 저장한 객체를 중복으로 저장하거나 값을 찾지 못하는 문제 발생
- equals
  - hash index가 충돌할 경우 해시 인덱스에 있는 데이터들을 하나하나 비교해서 찾아야 함 
  - 이 때 equals 메서드를 사용하여 비교함
  - 만약 재정의를 하지 않을 경우 값을 찾지 못하는 문제 발생 