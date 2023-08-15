---
layout: default
title: selectionSort
parent: 정렬 이론
grand_parent: 알고리즘 이론
nav_order: 2
---

# 선택정렬

---

```java
import java.util.Arrays;

public class SelectionSort {
	public static void main(String[] args) {
		int[] arr = { 64, 25, 10, 22, 11 };
		for (int i = 0; i < arr.length - 1; i++) {
			int minIdx = i;
			for (int j = i + 1; j < arr.length; j++) {
				if (arr[minIdx] > arr[j]) {
					minIdx = j;
				}
			}
			int temp = arr[i];
			arr[i] = arr[minIdx];
			arr[minIdx] = temp;
		}
		System.out.println(Arrays.toString(arr));
	}
}

// [10, 11, 22, 25, 64]
```
