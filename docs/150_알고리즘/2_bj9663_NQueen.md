---
layout: default
title: BJ9663_NQueen
parent: 알고리즘
nav_order: 2
---

백준허브 깃허브: [링크](https://github.com/gubeomlee/algorithm)

알고리즘 스터디: [링크](https://github.com/yyoungl/SSAFY10-Class8-Algo)

# BJ96663_NQueen

---

[문제 링크](https://www.acmicpc.net/problem/9663)

- NQueen 문제를 처음 풀었을 때는 파이썬을 사용했다. 당시 풀이로는 문제 없이 통과되었는데, 자바로 풀자 메모리 초과 문제가 발생했다. 체스판 배열 자체를 백트래킹으로 전달했는데, 이것이 메모리 초과의 원인이었다. 이 문제를 해결하기 위해 2가지 풀이를 만들었다. 첫번째 풀이는 체스판 배열을 검사하는 과정 메모리 사용량을 줄였다.

```java
import java.util.Scanner;

public class Main {
	// 해당 위치에 퀸을 배치했을 때 대각선 방향으로 다른 퀸이 있는지 확인하는 메서드
	public static boolean isSafe(int[][] matrix, int len, int row, int col) {
		// 같은 열에 있는지 확인
		for (int i = 0; i < row; i++) {
			if (matrix[i][col] == 1) {
				return false;
			}
		}
		// 왼쪽 위 대각선에 있는지 확인
		for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
			if (matrix[i][j] == 1) {
				return false;
			}
		}
		// 오른쪽 위 대각선에 있는지 확인
		for (int i = row, j = col; i >= 0 && j < len; i--, j++) {
			if (matrix[i][j] == 1) {
				return false;
			}
		}
		return true;
	}

	// 퀸을 배치하는 백트래킹 메소드
	public static void backtrack(int[][] matrix, int len, int row, int[] result) {
		if (len == row) {
			result[0] += 1;
		} else {
			for (int col = 0; col < len; col++) {
				if (isSafe(matrix, len, row, col)) { // 현재 위치에 퀸을 배치할 수 있는 경우
					matrix[row][col] = 1;
					backtrack(matrix, len, row + 1, result); // 재귀 호출 시 다음 행으로 넘어간다.
					matrix[row][col] = 0; // 백트레킹
				}
			}
		}
	}

	public static void main(String args[]) {
		Scanner sc = new Scanner(System.in);
		int len = sc.nextInt();
		int[][] matrix = new int[len][len];

		int[] result = { 0 };
		backtrack(matrix, len, 0, result);

		System.out.println(result[0]);
		sc.close();
	}
}
```

- 두번째 풀이는 퀸이 위치한 좌표만 전달해 메모리 사용량을 줄였다. 1차원 배열의 인덱스를 row로 하고 값을 col로 했다.

```java
import java.util.Scanner;

public class Gubeomlee_20230804_9663_2 {
	public static boolean isSafe(int[] arr, int row, int col) {
		for (int i = 0; i < row; i++) {
			// 같은 열 또는 대각선 상에 퀸이 위치한 경우
			if (arr[i] == col || Math.abs(i - row) == Math.abs(arr[i] - col)) {
				return false;
			}
		}
		return true;
	}

	public static void backtrack(int[] arr, int len, int row, int[] result) {
		if (len == row) {
			result[0]++;
		} else {
			for (int col = 0; col < len; col++) {
				if (isSafe(arr, row, col)) {
					arr[row] = col;
					backtrack(arr, len, row + 1, result); // 재귀를 호출하며 다음 행으로 넘어간다.
					arr[row] = 0;
				}
			}
		}
	}

	public static void main(String args[]) {
		Scanner sc = new Scanner(System.in);
		int len = sc.nextInt();
		// 배열의 인덱스 값을 row로 하고, 값을 col로 한다.
		int[] arr = new int[len];

		int[] result = { 0 };
		backtrack(arr, len, 0, result);

		System.out.println(result[0]);
		sc.close();
	}
}
```
