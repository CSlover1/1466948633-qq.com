[**CSDN上的投稿文章，欢迎大家点击哦**](https://blog.csdn.net/qq_42403042/article/details/105484790):thumbsup:
## 1、题目描述
插入排序算法：

 1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
 2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
 3. 重复直到所有输入数据插入完为止。

```c
示例 ：

输入: 4->2->1->3
输出: 1->2->3->4
```

## 2、示意图分析题目
<img src="https://img-blog.csdnimg.cn/20200413113017464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNDAzMDQy,size_16,color_FFFFFF,t_70" width = "467" height = "515" alt="图片名称" 
align=center>
## 3、C语言代码

```c
#include <stdio.h>
#include <stdlib.h>
//建立链表结点
struct ListNode
{
	int val;
	struct ListNode *next;
};
//链表插入排序
struct ListNode* insertionSortList(struct ListNode* head) {
	//创建需要的一些节点指针
	struct ListNode* result = (struct ListNode*)malloc(sizeof(struct ListNode));
	result->next = head;
	struct ListNode* index;
	struct ListNode* second;
	//外循环，遍历所有元素
	while (head != NULL) {
		if (head->next != NULL){
			second = head->next;
			//处理顺序不对的元素
			if (second->val < head->val) {
				//孤立second指针节点
				head->next = second->next;
				index = result;
				//从头遍历，找到合适的插入位置
				while (index->next != NULL) {
					if (index->next->val > second->val) {
						second->next = index->next;
						index->next = second;
						break;
					}
					else
						index = index->next;
				}
			}
			else head = head->next;
		}
		//便于最后一次跳出循环，否则程序无法结束
		else break;
	}
	return result->next;
}
```
### （1）易错点

 1. 对于数组的插入排序而言，是从目标元素前进行遍历，但单链表只能从前往后查找节点，所以index指针的目的在于确立开始遍历的节点；
 
 2. ` first = first -> next`的写入时机要注意，调整元素顺序时不用等于next，因为孤立second的时候first的下一个节点元素的值已经改变，需要重复比较的步骤；
 3. 由于` first = first -> next`的写入处理，当first指向尾节点时，程序会一直循环下去，所以加上`else break`。
### （2）复杂度分析
 1. 时间复杂度：O（n^2） 
 2. 空间复杂度：O（1）
