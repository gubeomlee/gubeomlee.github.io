---
layout: default
title: LinkedList
parent: Algorithm Basic
nav_order: 10
---

# LinkedList

---

## 연결리스트

- 자료의 논리적인 순서와 메모리 상의 물리적인 순서가 일치하지 않고, 개별적으로 위치하고 있는 원소의 주소를 연결하여 하나의 전체적인 자료구조를 이룬다.
- 링크를 통해 원소에 접근하므로, 순차 리스트에서처럼 물리적인 순서를 맞추기 위한 작업이 필요하지 않다.
- 자료구조의 크기를 동적으로 조정할 수 있어, 메모리의 효율적인 사용이 가능하다.
- 노드: 연결 리스트에서 하나의 원소에 필요한 데이터를 갖고 있는 자료단위다. 노드는 데이터 필드와 링크 필드로 구성된다.
- 데이터 필드: 원소의 값을 저장하는 자료구조다. 저장할 원소의 종류나 크기에 다라 구조를정의하여 사용한다.
- 링크 필드: 다음 노드의 주소를 저장하는 자료구조다.
- 헤드: 리스트의 처음 노드를 가리키는 레퍼런스다.

## Singly Linked List

- 노드가 하나의 링크 필드에 의해 다음 노드와 연결되는 구조를 갖는다.
- 헤드가 가장 앞의 노드를 가리키고, 링크 필드가 연속적으로 다음 노드를 가리킨다.
- 최종적으로 NULL을 가리키는 노드가 리스크의 가장 마지막 노드다.

## Singly Linked List 구현

```java
public class Node {
	public String data;
	public Node link;

	public Node() {
	}

	public Node(String data) {
		this.data = data;
	}

	@Override
	public String toString() {
		return "Node [data=" + data + "]";
	}
}
```

```java
public class SinglyLinkedList {
	private Node head; // 노드의 시작점
	private int size; // 노드의 개수. 이론상 필수는 아니다.

	public Node get(int idx) {
		if (idx < 0 || idx >= size) {
			return null;
		}

		Node cur = head;
		for (int i = 0; i < idx; i++) {
			cur = cur.link;
		}
		return cur;
	}

	public void addFirst(String data) {
		Node node = new Node(data);
		node.link = head;
		head = node;
		size++;
	}

	public void addLast(String data) {
		if (size == 0) {
			addFirst(data);
			return;
		}

		Node node = new Node(data);

		Node cur = head;
		while (cur.link != null) {
			cur = cur.link;
		}

		cur.link = node;
		size++;
	}

	public void add(int idx, String data) {
		if (idx < 0 || idx > size) {
			System.out.println("유효하지 않은 인덱스다.");
			return;
		}
		if (idx == 0) {
			addFirst(data);
			return;
		}
		if (idx == size) {
			addLast(data);
			return;
		}

		Node pre = get(idx - 1);

		Node node = new Node(data);
		node.link = pre.link;
		pre.link = node;
		size++;
	}

	public String remove() {
		if (head == null) {
			return null;
		}

		String data = head.data;
		head = head.link;
		size--;

		return data;
	}

	public String remove(int idx) {
		if (idx < 0 || idx > size) {
			System.out.println("유효하지 않은 인덱스다.");
			return null;
		}
		if (idx == 0) {
			return remove();
		}

		Node pre = get(idx - 1);
		String data = pre.link.data;
		pre.link = pre.link.link;
		size--;

		return data;
	}

	public void printList() {
		Node cur = head;
		if (head == null) {
			System.out.println("공백 리스트다. 출력할 것이 없다.");
			return;
		}

		while (cur != null) {
			System.out.print(cur.data + " -> ");
			cur = cur.link;
		}
		System.out.println();
	}
}
```

## Doubly Linked List

- 양쪽 방향으로 순회할 수 있도록 노드를 연결한 리스트
- 두 개의 링크 필드와 한개의 데이터 필드로 구성된다.

## Doubly Linked List 구현

```java
public class Node {
	public int data;
	public Node pre;
	public Node next;

	public Node() {
	}

	public Node(int data) {
		this.data = data;
	}

	@Override
	public String toString() {
		return "Node [data=" + data + "]";
	}
}
```

```java
public class DoublyLinkedList {
	private Node head;
	private Node tail;
	private int size;

	public DoublyLinkedList() {
	}

	public Node get(int idx) {
		if (idx < 0 || size <= idx) {
			return null;
		}

		if (idx < size / 2) {
			Node cur = head;
			for (int i = 0; i < idx; i++) {
				cur = cur.next;
			}
			return cur;
		} else {
			Node cur = tail;
			for (int i = size - 1; i > idx; i--) {
				cur = cur.pre;
			}
			return cur;
		}

	}

	public void addFirst(int data) {
		Node node = new Node(data);
		node.next = head;

		if (head != null) {
			head.pre = node;
		}

		head = node;
		size++;

		if (head.next == null) {
			tail = head;
		}
	}

	public void addLast(int data) {
		if (size == 0) {
			addFirst(data);
			return;
		}

		Node node = new Node(data);

		tail.next = node;
		node.pre = tail;
		tail = node;
		size++;
	}

	public void add(int idx, int data) {
		if (idx < 0 || size <= idx) {
			return;
		}
		if (idx == 0) {
			addFirst(data);
			return;
		}
		if (idx == size) {
			addLast(data);
			return;
		}

		Node node = new Node(data);
		Node pre = get(idx - 1);
		Node next = pre.next;

		node.pre = pre;
		node.next = next;

		pre.next = node;
		next.pre = node;

		size++;
	}

	public int removeFirst() {
		int data = head.data;

		head = head.next;
		size--;

		return data;
	}

	public int removeLast() {
		if (size == 0) {
			return 0;
		}

		int data = tail.data;

		tail = tail.pre;
		size--;

		return data;
	}

	public int remove(int idx) {
		if (idx < 0 || size <= idx) {
			return 0;
		}
		if (idx == 0) {
			removeFirst();
		}
		if (idx == size) {
			removeLast();
		}

		Node removeNode = get(idx);
		Node pre = get(idx - 1);
		Node next = pre.next;

		pre.next = next;
		next.pre = pre;

		size--;

		return removeNode.data;
	}
}
```
