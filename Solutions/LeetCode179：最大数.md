[**CSDN上的投稿文章，欢迎大家点击哦**](https://blog.csdn.net/qq_42403042/article/details/105696693):thumbsup:
## 题目描述
给定一组非负整数，重新排列它们的顺序使之组成一个最大的整数。
```c
示例 :
输入: [3,30,34,5,9]
输出: 9534330
```
==说明:== 输出结果可能非常大，所以你需要**返回一个字符串**而不是整数。
## 思路分析
首先肯定不能按照整数大小进行排序，比较的规则如下：

 1. 将整数a、b分别转化为字符串s1、s2;
 2. 比较`s1+s2`和`s2+s1`（+表示拼接）的大小，根据结果再对a、b进行排序。

## C语言代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
//设置字符数组最大（默认）长度
#define MAX_BUFFER 512
//设置比较器
//解释：传入参数*a、*b对应的数组下标前者大吗，当返回值为负值时二者交换位置
int cmpfunc(const void * a, const void * b)
{
	char s1[MAX_BUFFER];
	char s2[MAX_BUFFER];
	//发送格式化输出到 s1、s2 所指向的字符串
	sprintf(s1, "%d%d", *(int*)a, *(int*)b);
	sprintf(s2, "%d%d", *(int*)b, *(int*)a);
	return strcmp(s2, s1);
}
char * largestNumber(int* nums, int numsSize) {
	//根据比较器排序数组，好处在于排序前后数组元素类型都没有变化
	qsort(nums, numsSize, sizeof(int), cmpfunc);
	//特殊情况的处理
	if (nums[0] == 0) {
		return (char *)"0";
	}
	char *result = (char*)malloc(sizeof(char)*MAX_BUFFER);
	char *tem = result;
	for (int i = 0; i < numsSize; i++) {
		//在流指针之后将整数传递进tem字符串中
		sprintf(tem, "%d", nums[i]);
		//移动指针位置，相当于拼接
		tem += strlen(tem);
	}
	return result;
}
```
## 代码总结

 1. 很多类库函数如`sprintf、strcat`，由于安全性的问题，在VS中编译的时候会出现很出问题...建议测试的时候还是在网页中进行；
 2. 还有运用字符串拼接函数`strcat`解题的思路。问题在于控制台运行没有错误，一提交结果就莫名其妙多出来几个数字，如果有心人发现了错误，还请指出。:thumbsup:
```c
//只改变了最后几行代码
char result[MAX_BUFFER];
char tem[MAX_BUFFER];
char *a=result;
for (int i = 0; i < numsSize; i++) {
  sprintf(tem, "%d", nums[i]);
  strcat(result,tem);
}
return a;
```

## 复杂度分析

 1. 时间复杂度：`O(n lg n)`。由排序算法决定，`qsort`采用快速排序；
 2. 空间复杂度：不好描述，主要原因在于创建了很多辅助字符数组。

*欢迎交流~*
