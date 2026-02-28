# 数据结构

## 两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

### 普通法：时间复杂度O（n^2),空间复杂度O(1)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
    int len = nums.size();
    int i,j;
    for(i = 0;i<len;i++)
    {
        for(j = i+1;j<len;j++)
        {
            if(nums[i] + nums[j] == target)
            {
                return {i,j};
            }
        }
    }
    return{};
    }//&为引用的意思，起别名
    
};
```

### 哈希查找表法时间复杂度O（1）

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> hashtable;//定义一个unordered_map,它是一个容器类,用于存储元素集合，它不保障元素的排序但是提供更快的查找，插入和删除操作

    for(int i = 0;i<nums.size();i++)
    {
       auto it = hashtable.find(target - nums[i]);//unordered_map.find()提供一种返回方法，返回的是元素的iterator，hashtable.begin指向第一个元素的iterator，end则是一个不存在的元素位置。
       if(it != hashtable.end())//it查找的是一整个元素，it->first查找的是元素的key，->second查找的是元素的值
       {
         return{it->second,i}; 
       }
       hashtable[nums[i]] = i;
    }
    return{};
    }//&为引用的意思，起别名
    
};
```

![image-20250421132137937](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20250421132137937.png)

## 两数相加（链表）

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

### 链表

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //初始化链表头指针和进位
        ListNode *head = nullptr;
        ListNode *p = nullptr;
        int carry = 0;
        //遍历链表直达两个链表都处理完
        while(l1 || l2)
        {
            //取当前节点的值，直到两个链表都处理完
            int n1 = l1?l1->val: 0;
            int n2 = l2?l2->val: 0;
            int sum = n1 + n2 + carry;
            
            
            if(!head)
            {
                head = p = new ListNode(sum % 10);//初始化头节点
            }
            else
            {
                p->next = new ListNode(sum % 10);//链接后续节点
                p = p -> next;
            }                                
            carry = sum/10;

            if(l1)
            {
                l1 = l1 -> next;
            }
            if(l2)
            {
                l2 = l2 -> next;
            }
        }
        if(carry > 0)
        {
            p->next = new ListNode(carry);
        }
        return head;
    }

};
```

![image-20250421141600362](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20250421141600362.png)

# 数据结构

## 数据结构各个特点

顺序表：数组，链表，栈，队列

栈：后进先出

队列：先进先出

树：二叉树

![image-20250520152255964](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520152255964.png)

![image-20250520152500726](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520152500726.png)

 ![image-20250520153026268](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520153026268.png)

## 两种不同的定义方式

### 使用void方法初始化（栈内存自动管理）

使用void方法初始化，调用初始化方法时则需要传入地址

初始化方法内也不需要手动开辟新空间

```c
typedef struct Queue
{
	ElemType data[MAXSIZE];
	int front;
	int rear;
	
}Queue;

//初始化
void initQueue(Queue* Q)
{
	Q->front = 0;
	Q->rear = 0;
}

//入队
int equeue(Queue* Q, ElemType e)
{
	if (Q->rear >= MAXSIZE && !queueFull(Q))
	{
			return 0;
	}
	Q->data[Q->rear] = e;
	Q->rear++;
	return 1;
}

//出队
ElemType dequeue(Queue* Q)
{
	if (Q->front == Q->rear)
	{
		printf("空的\n");
		return 0;
	}
	ElemType e = Q->data[Q->front];
	Q->front++;
	return e;
}

int main(int argc, char const* argv[])
{

	Queue q;
	initQueue(&q);

	equeue(&q, 10);
	equeue(&q, 20);
	equeue(&q, 30);
	equeue(&q, 40);
	equeue(&q, 50);

	printf("%d\n", dequeue(&q));
	printf("%d\n", dequeue(&q));

	return 0;
}
```



### 使用结构定义初始化（堆内存手动管理）

使用这种定义方法则无需在使用时传入地址

但是在初始化方法内需要手动开辟空间，并且return 该节点；

```c
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 100
typedef int ElemType;

typedef struct
{
	ElemType* data;
	int front;
	int rear;

}Queue;

//初始化
Queue* initQueue()
{
	Queue* q = (Queue*)malloc(sizeof(Queue));
	q->data = (ElemType*)malloc(sizeof(ElemType) * MAXSIZE);
	q->front = 0;
	q->rear = 0;
	return q;
}

//判断队列是否为空
int isEmpty(Queue* Q)
{
	if (Q->front == Q->rear)
	{
		printf("空的\n");
		return 1;
	}
	else
	{
		return 0;
	}
}

//出队
ElemType dequeue(Queue* Q)
{
	if (Q->front == Q->rear)
	{
		printf("空的\n");
		return 0;
	}
	ElemType e = Q->data[Q->front];
	Q->front++;
	return e;
}

//队尾满了调整呢队列
int queueFull(Queue* Q)
{
	if (Q->front > 0)
	{
		int step = Q->front;
		for (int i = Q->front; i <= Q->rear; ++i)
		{
			Q->data[i - step] = Q->data[i];
		}
		Q->front = 0;
		Q->rear = Q->rear - step;
		return 1;
	}
	else
	{
		printf("真的满了\n");
		return 0;
	}
}

//入队
int equeue(Queue* Q, ElemType e)
{
	if (Q->rear >= MAXSIZE)
	{
		return 0;
	}
	Q->data[Q->rear] = e;
	Q->rear++;
	return 1;
}

//获取队头元素
int getHead(Queue* Q, ElemType *e)
{
	if (Q->front == Q->rear)
	{
		printf("空的\n");
		return 0;
	}
	*e = Q->data[Q->front];
	return 1;
}

int main(int argc, char const* argv[])
{

	Queue* q = initQueue();

	equeue(q, 10);
	equeue(q, 20);
	equeue(q, 30);
	equeue(q, 40);
	equeue(q, 50);

	printf("%d\n", dequeue(q));
	printf("%d\n", dequeue(q));
	ElemType e;
	getHead(q, &e);
	printf("%d\n", e);

	return 0;
}
```

## 循环队列判断是否为满

这里应该选A

![image-20250519220125853](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250519220125853.png)

![image-20250519220618796](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250519220618796.png)

## 递归概念

就相当于找到an 的表达式，如斐波那契数列的表达式就是a【n】 = a【n-1】+a【n-2】

![image-20250519224322050](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250519224322050.png)

### ✅ 递归的本质是：

> **一个函数调用自己，并向着一个明确的结束条件前进。**

## 树

完全二叉树

![image-20250520160156671](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520160156671.png)



### 前序遍历

![image-20250520233135174](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520233135174.png)

### 中序遍历

![image-20250520233613262](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520233613262.png)

### 后序遍历

![image-20250520233730132](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250520233730132.png)

### 二叉树的遍历性质

已知前后序遍历不能唯一确定一棵二叉树

![image-20250521093923054](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250521093923054.png)

### 线索二叉树

![image-20250521104918751](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250521104918751.png)

![image-20250521104937919](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250521104937919.png)

### 树转换为二叉树

![a1e21717474fe41e2500629dd18209eb](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/a1e21717474fe41e2500629dd18209eb.png)

![66b7da9722fe0a00bdc2c1bed669b6d5](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/66b7da9722fe0a00bdc2c1bed669b6d5.png)

### 二叉树转换为树



![image-20250522155601238](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155601238.png)

![image-20250522155632712](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155632712.png)

![image-20250522155642503](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155642503.png)

### 森林与二叉树的转换

![image-20250522155818236](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155818236.png)

![image-20250522155806097](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155806097.png)

![image-20250522155917523](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155917523.png)

![image-20250522155924955](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155924955.png)

### 二叉树转森林

![image-20250522155947542](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522155947542.png)

![image-20250522160002078](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522160002078.png)

![image-20250522160020680](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522160020680.png)

![image-20250522160027796](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522160027796.png)

![image-20250522160033631](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522160033631.png)

![image-20250522195734537](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250522195734537.png)

### 克鲁斯卡尔和普瑞姆算法总结

![image-20250524140038666](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250524140038666.png)

### 关键路径

![image-20250525205213777](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250525205213777.png)

![image-20250525205231060](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250525205231060.png)

![image-20250525205244624](C++%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20250525205244624.png)

 
