---
layout: default
title: insertSort
parent: 정렬 이론
grand_parent: 알고리즘 이론
nav_order: 4
---

# 삽입 정렬

---

```java
import java.util.Arrays;

public class TEST {
	public static void main(String[] args) {
		int[] arr = { 69, 10, 30, 2, 16, 8, 31, 22 };

		for (int i = 1; i < arr.length; i++) {
			int currentElement = arr[i];
			int j;
			for (j = i - 1; j >= 0 && arr[j] > currentElement; j--) {
				arr[j + 1] = arr[j];
			}
			arr[j + 1] = currentElement;
		}
		System.out.println(Arrays.toString(arr));
	}
}
// [2, 8, 10, 16, 22, 30, 31, 69]
```
