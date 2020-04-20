[**CSDN上的投稿文章，欢迎大家点击哦**](https://blog.csdn.net/qq_42403042/article/details/105634963):thumbsup:
## 题目描述
在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序。
```c
示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
```
## 思路分析
在排序算法中，时间复杂度满足要求的并不多，而单链表的特殊之处又是无法从后往前遍历，所以想到了2路归并排序。

<img src="https://img-blog.csdnimg.cn/20200420150216819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNDAzMDQy,size_16,color_FFFFFF,t_70" width = "406" height = "496" alt="图片名称" 
align=center>

逐级的分裂和合并是由递归完成的，只需要写出找到链表中间节点的程序`findMid(struct ListNode* head)`和将两个链表合并的程序`merge(struct ListNode* head, struct ListNode* midNode)`即可
## C语言代码
```c
//建立链表结点
struct ListNode
{
	int val;
	struct ListNode *next;
};
//找到中间节点
struct ListNode* findMid(struct ListNode* head) {
	//一次遍历找到midNode，往往需要两个指针
	struct ListNode* fastNode = head;
	struct ListNode* slowNode = head;
	//注意非空的判断
	while (fastNode != NULL && fastNode->next != NULL) {
		fastNode = fastNode->next->next;
		if (fastNode != NULL)
			slowNode = slowNode->next;
	}
	//对应数组中的第（length-1）/2个元素
	return slowNode;
}
//对两个链表进行合并
struct ListNode* merge(struct ListNode* head, struct ListNode* midNode) {
	//增加头节点便于对表头进行操作
	struct ListNode* result = (struct ListNode*)malloc(sizeof(struct ListNode));
	struct ListNode* copy = (struct ListNode*)malloc(sizeof(struct ListNode));
	result = copy;
	while (head != NULL && midNode != NULL) {
		if (head->val < midNode->val) {
			copy->next = head;
			head = head->next;
		}
		else {
			copy->next = midNode;
			midNode = midNode->next;
		}
		copy = copy->next;
	}
	//循环结束之后可能还有剩余节点没有加入
	if (head != NULL)
		copy->next = head;
	if (midNode != NULL)
		copy->next = midNode;
	return result->next;
}
//合并排序
struct ListNode* sortList(struct ListNode* head) {
	if (head == NULL)
		return NULL;
	struct ListNode* midNode = findMid(head);
	//递归结束判断条件
	if (midNode->next == NULL)
		return midNode;
	struct ListNode* one = sortList(midNode->next);
	//一定要断开节点，便于merge
	midNode->next = NULL;
	struct ListNode* two = sortList(head);
	return merge(one, two);
}
```
## 复杂度分析

 1. 时间复杂度：`O(n log n)`
 2. 空间复杂度：这个有点迷惑，因为用到了递归，应该不是常数级，而是`O(log n)`，所以没有达到题目的要求，非递归的方法我暂时没想出来:sweat:
 
欢迎大家交流指正
