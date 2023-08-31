---
layout: default
title: mergeSort
parent: 정렬 이론
grand_parent: 알고리즘 이론
nav_order: 5
---

# 병합정렬

---

```java
import java.util.Arrays;

public class 병합정렬 {
	public static void merge(int[] arr, int start, int mid, int end) {
		int l = start;
		int r = mid + 1;
		int idx = 0;
		int[] sortedArr = new int[end - start + 1];
		while (l <= mid && r <= end) {
			if (arr[l] <= arr[r]) {
				sortedArr[idx++] = arr[l++];
			} else {
				sortedArr[idx++] = arr[r++];
			}
		}

		while (l <= mid) {
			sortedArr[idx++] = arr[l++];
		}
		while (r <= end) {
			sortedArr[idx++] = arr[r++];
		}

		for (int i = start; i <= end; i++) {
			arr[i] = sortedArr[i - start];
		}
	}

	public static void mergeSort(int[] arr, int start, int end) {
		if (start < end) {
			int mid = (start + end) / 2;
			mergeSort(arr, start, mid);
			mergeSort(arr, mid + 1, end);
			merge(arr, start, mid, end);
		}
	}

	public static void main(String[] args) {
		int[] arr = { 69, 10, 30, 2, 16, 8, 31, 22 };
		mergeSort(arr, 0, arr.length - 1);
		System.out.println(Arrays.toString(arr));
	}
}
```
