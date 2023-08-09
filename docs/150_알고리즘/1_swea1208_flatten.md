---
layout: default
title: swea1208_flatten
parent: 알고리즘
nav_order: 1
---

백준허브 깃허브: [링크](https://github.com/gubeomlee/algorithm)

알고리즘 스터디: [링크](https://github.com/yyoungl/SSAFY10-Class8-Algo)

[문제 링크](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV139KOaABgCFAYh)

- 평탄화 작업은 4가지가 가능하다. 문제의 조건에 따라 적합한 풀이가 달라지기 때문에 어느 풀이가 가장 좋은 풀이라고 단정할 수 없다.

- 첫번째 방법은 정렬 후 높이가 가장 높은 쪽에서 가장 낮은 쪽으로 하나씩 이동시키는 것이다. 배열의 크기가 클 때 문제를 해결하기 위해 고안한 방법있었으나, 문제해결 과정 평탄화 작업의 조기종료를 해결 할 수 없다는 문제가 있었다. 이 문제를 해결하기 위해 반복문의 순회 방향을 바꾸었는데, 이로 인해 시간의 이점이 사라졌다.

```java
import java.util.Arrays;
import java.util.Scanner;

public class Flatten1 {
	public static void main(String args[]) throws Exception {
		Scanner sc = new Scanner(System.in);
		int T = 10;
		for (int test_case = 1; test_case <= T; test_case++) {
			int num = sc.nextInt();
			int len = 100;
			int[] arr = new int[len];
			for (int i = 0; i < len; i++) {
				arr[i] = sc.nextInt();
			}

			Arrays.sort(arr);
			int temp = 0; // 평탄화 작업 저장할 변수
			// 최대높이의 인덱스에서 시작하여 높이를 감소시킨다.
			outerLoop1: while (true) {
				// 평탄화 조기 종료 처리를 위해 순회 방식 및 높이 비교 부등호에 유의해야 한다.
				outerLoop2: for (int i = 0; i < len; i++) {
					if (arr[i] == arr[len - 1]) { // 현재 높이가 최대 높이보다 작은 경우
						for (int j = i; j < len; j++) {
							temp++; // 평탄화 작업 수 증가
							arr[j]--; // 평탄화
							if (temp == num) {
								break outerLoop1;
							}
						}
						break outerLoop2;
					}
				}
			}
			// 최소높이의 인덱스에서 시작하여 높이를 증가시킨다.
			outerLoop1: while (true) {
				outerLoop2: for (int i = len - 1; i >= 0; i--) {
					if (arr[i] == arr[0]) { // 현재 높이가 최대 높이보다 큰 경우
						for (int j = i; j >= 0; j--) {
							temp--; // 평탄화 작업 수 감소
							arr[j]++; // 평탄화
							if (temp == 0) {
								break outerLoop1;
							}
						}
						break outerLoop2;
					}
				}
			}
			System.out.printf("#%d %d\n", test_case, arr[len - 1] - arr[0]);
		}
		sc.close();
	}
}
```

- 두번째 방법은 카운팅 정렬을 차용한 방법이다. 최대 높이가 극단적으로 높아졌을 때 메모리 문제가 발생한다는 단점이 있다.

```java
import java.util.Scanner;

public class Flatten2 {
	public static void main(String args[]) throws Exception {
		Scanner sc = new Scanner(System.in);
		int T = 10;
		for (int test_case = 1; test_case <= T; test_case++) {
			int num = sc.nextInt();
			int len = 100;
			int[] arr = new int[len + 1];
			for (int i = 0; i < len; i++) {
				arr[sc.nextInt()]++; // 각 높이별 상자 수 저장
			}

			int left = 0;
			int right = len;

			while (arr[left] == 0) {
				left++;
			}
			while (arr[right] == 0) {
				right--;
			}

			for (int i = 0; i < num; i++) {
				if (right < left) { // 평탄화 조기종료 되는 경우
					break;
				}

				// 평탄화
				arr[left]--;
				arr[left + 1]++;
				arr[right]--;
				arr[right - 1]++;

				if (arr[left] == 0) {
					left++;
				}
				if (arr[right] == 0) {
					right--;
				}
			}
			System.out.printf("#%d %d\n", test_case, right - left);
		}
		sc.close();
	}
}
```

- 세번째, 네번째 풀이는 같은 논리를 공유한다. 매 평탄화 작업에서 최댓값과 최솟값을 찾는다.

```java
import java.util.Scanner;

public class Flatten3 {
	public static void main(String args[]) throws Exception {
		Scanner sc = new Scanner(System.in);
		int T = 10;
		for (int test_case = 1; test_case <= T; test_case++) {
			int num = sc.nextInt();
			int len = 100;
			int[] arr = new int[len];
			for (int i = 0; i < len; i++) {
				arr[i] = sc.nextInt();
			}

			int maxIdx = 0;
			int minIdx = 0;
			for (int i = 0; i < num; i++) {
				for (int j = 0; j < len; j++) {
					// 최대높이, 최소높이 인덱스 찾기
					if (arr[maxIdx] < arr[j]) {
						maxIdx = j;
					}
					if (arr[minIdx] > arr[j]) {
						minIdx = j;
					}
				}
				// 평탄화
				arr[maxIdx]--;
				arr[minIdx]++;
			}
			// 최대높이, 최소높이 구하기
			int max = Integer.MIN_VALUE;
			int min = Integer.MAX_VALUE;
			for (int i = 0; i < len; i++) {
				max = Math.max(max, arr[i]);
				min = Math.min(min, arr[i]);
			}

			System.out.printf("#%d %d\n", test_case, max - min);
		}
		sc.close();
	}
}
```

```java
import java.util.Arrays;
import java.util.Scanner;

public class Flatten4 {
	public static void main(String args[]) throws Exception {
		Scanner sc = new Scanner(System.in);
		int T = 10;
		for (int test_case = 1; test_case <= T; test_case++) {
			int num = sc.nextInt();
			int len = 100;
			int[] arr = new int[len];
			for (int i = 0; i < len; i++) {
				arr[i] = sc.nextInt();
			}

			Arrays.sort(arr);
			for (int i = 0; i < num; i++) {
				arr[0]++;
				arr[99]--;
				Arrays.sort(arr); // 배열 재정렬을 통해 최댓값, 최솟값 구하기
			}
			System.out.printf("#%d %d\n", test_case, arr[len - 1] - arr[0]);
		}
		sc.close();
	}
}
```
