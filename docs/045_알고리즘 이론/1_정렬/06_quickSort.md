---
layout: default
title: quickSort
parent: 정렬 이론
grand_parent: 알고리즘 이론
nav_order: 6
---

# 퀵정렬

---

#### Hoare Partition

```java
import java.util.Arrays;

public class 퀵정렬_Hoare {
	public static void swap(int[] arr, int l, int r) {
		int temp = arr[l];
		arr[l] = arr[r];
		arr[r] = temp;
	}

	public static int hoarePartition(int[] arr, int start, int end) {
		int pivot = arr[start];

		int l = start + 1;
		int r = end;

		while (l <= r) {
			while (l <= r && arr[l] <= pivot) {
				l++;
			}
			while (pivot < arr[r]) {
				r--;
			}
			if (l < r) {
				swap(arr, l, r);
			}
		}
		swap(arr, start, r);
		return r;
	}

	public static void quickSort(int[] arr, int start, int end) {
		if (start < end) {
			int pivot = hoarePartition(arr, start, end);
			quickSort(arr, start, pivot - 1);
			quickSort(arr, pivot + 1, end);
		}
	}

	public static void main(String[] args) {
		int[] arr = { 10, 3, 2, 4, 6, 9, 1, 1, 2, 8, 7, 5 };
		quickSort(arr, 0, arr.length - 1);
		System.out.println(Arrays.toString(arr));
	}
}
```

#### Lomuto Partition

```java
import java.util.Arrays;

public class 퀵정렬_Lomuto {
	public static void swap(int[] arr, int l, int r) {
		int temp = arr[l];
		arr[l] = arr[r];
		arr[r] = temp;
	}

	public static int lomutoPartition(int[] arr, int start, int end) {
		int pivot = arr[end];
		int i = start;

		for (int j = start; j < end; j++) {
			if (arr[j] <= pivot) {
				swap(arr, i++, j);
			}
		}
		swap(arr, i, end);
		return i;
	}

	public static void quickSort(int[] arr, int start, int end) {
		if (start < end) {
			int pivot = lomutoPartition(arr, start, end);
			quickSort(arr, start, pivot - 1);
			quickSort(arr, pivot + 1, end);
		}
	}

	public static void main(String[] args) {
		int[] arr = { 10, 3, 2, 4, 6, 9, 1, 1, 2, 8, 7, 5 };
		quickSort(arr, 0, arr.length - 1);
		System.out.println(Arrays.toString(arr));
	}
}
```
