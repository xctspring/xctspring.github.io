---
layout: post
title: Java实现排序
date: 2019-12-26
categories: Java
tags: Java
---

[TOC]
# 插入排序
## 直接插入排序
插入排序基本操作就是将一个数据插入到已经排好序的有序数据中，从而得到一个新的、个数加一的有序数据，**算法适用于少量数据的排序**，**时间复杂度为`$O(n^2)$`**。是**稳定**的排序方法。

插入排序的基本思想是：每步将一个待排序的纪录，按其关键码值的大小插入前面已经排序的文件中适当位置上，直到全部插入完为止。

```
	public static boolean insertSort(int[] arr) {
		// 参数合法性判断
		if (arr == null || arr.length == 0) {
			return false;
		}
		// 从第一个元素开始，该元素可以认为已经被排序 
		for (int i = 1; i < arr.length; i++) {
			// 取出下一个元素，在已经排序的元素序列中从后向前扫描 
			int key = arr[i], j;
			for (j = i - 1; j >= 0 && arr[j] > key; j--) {
				arr[j + 1] = arr[j];
				}
				System.out.println(Arrays.toString(arr));
				// 直到找到已排序的元素小于或者等于新元素的位置 ，将新元素插入到该位置中   
			arr[j + 1] = key;
			}
		return true;
	}
```

## 二分插入排序

## 希尔排序

# 选择排序
- 思想：每趟从待排序的记录序列中选择关键字最小的记录放置到已排序表的最前位置，直到全部排完。
- 关键问题：在剩余的待排序记录序列中找到最小关键码记录。
## 简单选择
- 基本思想：在要排序的一组数中，选出最小的一个数与第一个位置的数交换；然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。
- 不稳定的排序。
- 最好,有序，减少交换的次数`$O(n)$`; 最差，逆序，每个都要交换，`$O(n^2)$`
![image](https://note.youdao.com/yws/res/24448/8585E8A5AC5D4FDFBF8CB869A6208489)

```
	public static boolean selectSort(int[] arr) {
		if (arr == null || arr.length == 0) {
			return false;
		}
		for (int i = 0; i < arr.length; i++) {
			int min = arr[i],n = i; //记录最小值的下标
			for (int j = i; j < arr.length; j++) {
				if (arr[j] < min) {
					min = arr[j];
					n = j;
				}
			}
			arr[n] = arr[i];
			arr[i] = min;
		}
		return true;
	}
```
## 堆排序






# 交换排序
## 冒泡排序

 冒泡排序：比较两个相邻的元素，将值大的元素交换至右端。
 1. 依次比较相邻的两个数，将小数放在前面，大数放在后面。即在第一趟：首先比较第1个和第2个数，将小数放前，大数放后。
 2. 然后比较第2个数和第3个数，将小数放前，大数放后，如此继续，直至比较最后两个数，将小数放前，大数放后。
 3. 重复第一趟步骤，直至全部排序完成。
```
	public static boolean bubbleSort(int[] arr) {
		if (arr == null || arr.length == 0) {
			return false;
		}
		// 增加一个flag，如果不发生交换就是说后面的都已经排好序了
		int flag = 1;
		
		for (int i = 0; i < arr.length; i++) {
			for (int j = i + 1; j < arr.length; j++) {
				if (arr[j] > arr[i]) {
					int temp = arr[j];
					arr[j] = arr[i];
					arr[i] = temp;
					// 如果发生交换就改变flag
					flag = 0; 
				}
			}
			// 没有发生交换就直接跳出循环
			if (flag == 1) {
				break;
			}
			// 不要忘了每次循环要改回标志位
			flag = 0;
		}
		return true;
	}
```

## 快速排序
算法导论，P85
- 基于分治的思想
- 最差：如果是逆序就是最差，划分的两个子问题大小为n - 1和0，`$O(n^2)$`,
- 最好：最好情况划分的子问题的大小非常接近，为n/2 和 n/2 - 1，`$O(nlogn)$`
- 不稳定排序

基本思想：选择一个基准元素,通常选择第一个元素或者最后一个元素,通过一趟扫描，将待排序列分成两部分,一部分比基准元素小,一部分大于等于基准元素,此时基准元素在其排好序后的正确位置,然后再用同样的方法递归地排序划分的两部分。

![image](https://note.youdao.com/yws/res/24226/C12CD008708A4FC28B5E9915B1039EA0)

![image](https://note.youdao.com/yws/res/24441/2D3802FD71A54FCCB97982D7C7C0AEF0)

![image](https://note.youdao.com/yws/res/24444/BCC221F8636645ABBF5918D3F539F7CC)

```
/*
	 * 快速排序：基于分治的方法，选择一个基准元素,通常选择第一个元素或者最后一个元素,通过一趟扫描，
	 * 将待排序列分成两部分,一部分比基准元素小,一部分大于等于基准元素,此时基准元素在其排好序后的正确位置, 然后再用同样的方法递归地排序划分的两部分。
	 */
	public static boolean quickSort(int[] arr, int left, int right) {
		if (arr == null || arr.length == 0) {
			return false;
		}
		// 不断分解，直到只包含一个元素
		if (left < right) {
			int middle = getMiddle(arr, left, right);
			quickSort(arr, left, middle - 1);
			quickSort(arr, middle + 1, right);
		}
		System.out.println(Arrays.toString(arr));
		return true;
	}

	// 就是对数组进行partition操作，将数组按基准元素分为两块，左边小于基准元素，右边大于基准元素，返回基准元素的大小
	// 将数组分为三个区，i 指向 <=key区 的最后一个元素， j 指向 待查区的第一个元素，i 和 j 之间就是 >key 区
	public static int getMiddle(int[] arr, int left, int right) {
		// 选择数组最后一位为基准元素
		int key = arr[right];
		// 开始时，i在left的前一位，j指向left，因为整个数组为待查区，待查区首位的前一位就是 i
		int i = left - 1;
		for (int j = left; j <= right - 1; j++) {
			// 如果小于，i就增加一位，将待查区的第一位加入 <=key 区，就是将待查区的第一位与 i++的位置交换，添加到 <=key 的最后一位
			if (arr[j] <= key) {
				i++;
				int temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;
			}
		}
		// 此时，待查区已经没了，将最后一位（基准元素）与>key 的最后一位进行交换，将key放在数组中间，数组分割完成
		int temp = arr[i + 1];
		arr[i + 1] = arr[right];
		arr[right] = temp;
		
		// 返回的是 <=key区的下一位，就是分割点，也可以是 j - 1
		return i + 1;
	}
```
# 合并排序
 合并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。
 然后再把有序子序列合并为整体有序序列。
- 合并排序：同样是使用分治的方法
- 稳定
- `$O(nlogn)$`
- 一般用于对总体无序，但是各子项相对有序的数列。

![image](https://note.youdao.com/yws/res/24423/318D45316D2B407F93DEA2C0FFD30E9F)

![image](https://note.youdao.com/yws/res/24445/C7CA3235486B4FF2B5EF3447F17CA231)
```
	public static boolean mergeSort(int[] arr, int left, int right) {
		if (arr == null || arr.length == 0) {
			return false;
		}
		if (left < right) {
			int middle = (left + right) / 2;
			mergeSort(arr, left, middle);
			mergeSort(arr, middle + 1, right);
			// 相比于快排，这里需要增加一个merge操作，因为快排结束后，整个数组就是排好序的了，而合并排序，左右都是排好序的，但是合并起来不一定是排好序的
			merge(arr, left, middle, right);
			System.out.println(Arrays.toString(arr));
		}
		return true;
	}
	
	public static void merge(int[] arr, int left, int middle, int right) {
		int lengthOfL = middle -left + 1; // 数组l的长度 
		int lengthOfR = right - middle; //数组r的长度
//		// 长度 + 1 ，设置一个哨兵元素，遇到哨兵元素，说明数组结尾
//		int[] l = new int[lengthOfL + 1]; 
//		int[] r = new int[lengthOfR + 1];
		int[] l = new int[lengthOfL]; 
		int[] r = new int[lengthOfR];
		//复制arr 到 l 和 r 中
		for (int i = 0; i < lengthOfL; i++) {
			l[i] = arr[left + i];
		}
		for (int j = 0; j < lengthOfR; j++) {
			r[j] = arr[middle + j + 1];
		}
		int i = 0, j = 0, k;
		// 选择l 和 r中小的那一个加入arr
		for (k = left; k < right && i < lengthOfL && j < lengthOfR; k++) {
			if (l[i] <= r[j]) {
				arr[k] = l[i++];
			} else {
				arr[k] = r[j++];
			}
		}
		// 将某一个数组的剩余部分放入arr
		while (i < lengthOfL) {
			arr[k++] = l[i++];
		}
		while (j < lengthOfR) {
			arr[k++] = r[j++];
		}
	}
```
# 基数排序


# 总结

稳定性：就是能保证排序前两个相等的数据其在序列中的先后位置顺序与排序后它们两个先后位置顺序相同。再简单具体一点，如果A i == A j，Ai 原来在 Aj 位置前，排序后 Ai  仍然是在 Aj 位置前。


![image](https://note.youdao.com/yws/res/24248/7D2313115D0341FBB266D759DE727793)
