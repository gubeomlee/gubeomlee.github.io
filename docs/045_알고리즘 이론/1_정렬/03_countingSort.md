---
layout: default
title: countingSort
parent: 정렬 이론
grand_parent: 알고리즘 이론
nav_order: 3
---

# 카운팅정렬

---

```java
import java.util.Arrays;

public class CountingSort {
	public static void main(String[] args) {
		int[] arr = { 8, 8, 24, 12, 19, 3, 45, 60 };
		int maxNum = Integer.MIN_VALUE;
		for (int i = 0; i < arr.length; i++) {
			if (arr[i] > maxNum) {
				maxNum = arr[i];
			}
		}
		// 카운팅 배열을 구한다.
		int[] count = new int[maxNum + 1];
		for (int i = 0; i < arr.length; i++) {
			count[arr[i]]++;
		}
		// 카운팅 배열의 누적값을 구한다.
		for (int i = 1; i < count.length; i++) {
			count[i] += count[i - 1];
		}

		int[] result = new int[arr.length];
		for (int i = arr.length - 1; i >= 0; i--) {
			result[--count[arr[i]]] = arr[i];
		}
		System.out.println(Arrays.toString(result));
	}
}
// [3, 8, 8, 12, 19, 24, 45, 60]
```
