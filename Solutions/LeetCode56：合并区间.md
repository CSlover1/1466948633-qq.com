## 思路分析：

 **1. 首先进行给定区间的排序：**
 题目中我选择了快速排序，需要注意的是要根据二维数组每行的首元素大小进行排序，并且每次需同时交换两个元素。
 
 **2. 遍历排序好的二维数组：**
每次需要比较intervals[i][1]和intervals[i+1][0],如果后者大，证明区间不重叠，此时不做特殊处理，否则合并区间。
##  代码实现
**关于题目中给的形参：**
我个人认为int* intervalsColSize是没有必要的，因为intervals的行数由intervalSize给定，列数固定为2。
```c
#include <stdio.h>
#include <stdlib.h>
//找到划分区间时所用元素位置
int Partition(int** intervals, int low, int high) {
	int pivot = intervals[low][0];
	int secpivot = intervals[low][1];
	while (low < high) {
		while (low < high&&intervals[high][0] >= pivot)
			high--;
		intervals[low][0] = intervals[high][0];
		intervals[low][1] = intervals[high][1];
		while (low < high&&intervals[low][0] <= pivot)
			low++;
		intervals[high][0] = intervals[low][0];
		intervals[high][1] = intervals[low][1];
	}
	intervals[low][0] = pivot;
	intervals[low][1] = secpivot;
	return low;
}
//快速排序
void QuickSort(int** intervals, int low, int high) {
	if (low < high) {
		int pivotpos = Partition(intervals, low, high);
		QuickSort(intervals, low, pivotpos);
		QuickSort(intervals, pivotpos+1, high);
	}
}
//合并区间
int** merge(int** intervals, int intervalsSize, int* intervalsColSize, int* returnSize, int** returnColumnSizes) {
	QuickSort(intervals, 0, intervalsSize - 1);
	//申请内存，此时不确定合并之后的数组大小，只能按照intervalSize进行申请
	int **target = (int **)malloc(sizeof(int *)*intervalsSize);
	*returnColumnSizes = (int *)malloc(sizeof(int) * intervalsSize);
	//引入两个中间变量，一来表述方便，二来起到记忆比较元素的效果
	int start = 0, end = 0;
	*returnSize = 0;
	//遍历需要合并的数组
	for (int i = 0; i < intervalsSize; i++) {
		start = intervals[i][0];
		end = intervals[i][1];
		//这里一定要把i < intervalsSize - 1写到前面，否则将会出错！
		while (i < intervalsSize - 1 && intervals[i + 1][0] <= end) {
			end = intervals[i + 1][1] > end ? intervals[i + 1][1] : end;
			i++;
		}
		//动态内存分配，避免内存浪费
		target[*returnSize] = (int *)malloc(sizeof(int) * 2);
		//注意前面的括号，下标引用的优先级高于间接访问
		(*returnColumnSizes)[*returnSize] = 2;
		//将比较好的元素加入到目标数组中
		target[*returnSize][0] = start;
		target[*returnSize][1] = end;
		(*returnSize)++;
	}
	//减小已分配的内存大小，因为刚开始申请的内存大于等于修改后的内存
	target=(int **)realloc(target,sizeof(int *)*(*returnSize));
	return target;
}
```

**复杂度分析**：

 1. 时间复杂度：O ( n log n )
 主要来源于排序的复杂与否
 
 2. 空间复杂度：O ( n )
 我的空间复杂度打败了100%的提交解法，有点骄傲，嘻嘻。

##  避免bug的几个注意点：

 - 内存分配的几个地方，注意返回指针的类型转换；
 - for循环遍历数组时，while语句一定要对i-1的大小进行限定，且写在前面，即先判断，避免判断到intervals[i + 1][0] <= end时下标超出界限；
 - 最后调用realloc（）函数进行申请内存的修改可减小程序的占用空间。
