---
layout: default
title: bubbleSort
parent: 정렬
nav_order: 1
---

# 버블정렬

---

```java
import java.util.Arrays;

public class BubbleSort {
	public static void main(String[] args) {
		int[] nums = { 24, 99999, 99, 31, 213124, 7, 35 };

		while (true) {
			int flag = 0;
			for (int i = 0; i < nums.length - 1; i++) {
				if (nums[i] > nums[i + 1]) {
					int temp = nums[i];
					nums[i] = nums[i + 1];
					nums[i + 1] = temp;
					flag++;
				}
			}
			if (flag == 0) {
				break;
			}
		}
		System.out.println(Arrays.toString(nums));
	}
}

// [7, 24, 31, 35, 99, 99999, 213124]
```
