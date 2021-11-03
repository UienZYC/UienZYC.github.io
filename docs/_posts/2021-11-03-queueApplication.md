---
layout: post
title:  "队列应用（整数排序）"
date:   2021-11-03 17:11:00 +0800
categories: jekyll update
---
```c
/*写出P.101 例3-8的可运行完整程序，
包括队列结构体，各个函数以及主函数，并进行代码和算法的优化。*/
// 注意使用release编译

#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>		//头文件

typedef int DataType;			// 设置队列为整数型队列

//定义队列结构
typedef struct QUEUE
{
	DataType* queArray;
	int front;
	int rear;
	int maxLength;
}Queue;

//声明队列方法
Queue* CreateQueue(int length);			// 创建队列
int GetQueueLength(Queue* queue);		// 得到队列的长度
void EnQueue(Queue* queue, DataType data);		// 入队
DataType DlQueue(Queue* queue);					// 出队
DataType GetQueueHead(Queue* queue);			// 取队头元素

// 主函数
int main()
{
	const int QueueMax = 100;		// 队列最大容量
	// 创建队列
	Queue* queue[2];
	queue[0] = CreateQueue(QueueMax);
	if (queue[0] == NULL)
		return 1;		// 创建失败，程序退出
	queue[1] = CreateQueue(QueueMax);
	if (queue[1] == NULL)
		return 1;		// 创建失败，程序退出

	int t[10];
	printf("请输入5个待排序的整数：\n");
	for (int i = 0; i < 5; i++)
		scanf_s("%d", &t[i]);
	int k = 0, j;		// k，j均为队列编号，取值0或1
	EnQueue(queue[k], t[0]);
	for (int i = 1; i < 5; i++)
	{
		// 设置j为不同于k的另一个队列编号
		if (k == 0)  j = 1;
		else j = 0;

		// 小于t[i]的queue[k]中元素出队，再进入队列queue[j]
		while (queue[k]->front != queue[k]->rear && t[i] > GetQueueHead(queue[k]))
		{
			EnQueue(queue[j], DlQueue(queue[k]));
		}
		EnQueue(queue[j], t[i]);   // t[i]入队queue[j]

		// 其他大于t[i]的queue[k]中元素出队，再进队列queue[j]
		while (queue[k]->front != queue[k]->rear)
		{
			EnQueue(queue[j], DlQueue(queue[k]));
		}
		k = j;					// 设置k为下一次做出队操作的队列
	}
	printf("排序的结果为： ");
	while (queue[k]->front != queue[k]->rear)
	{
		printf("%d ", DlQueue(queue[j]));
	}
	printf("\n");
	return 0;
}

/**
* @brief 创建一个队列
* @param length 队列的容量
* @return 指向队列的指针，失败返回NULL
*/
Queue* CreateQueue(int length)
{
	Queue* queue = (Queue*)malloc(sizeof(Queue));
	if (queue)
	{
		// 申请内存
		queue->queArray = (DataType*)malloc(length * sizeof(DataType));
		// 失败返回NULL
		if (queue->queArray == NULL)
			return NULL;
		// 清空队列
		queue->front = 0;
		queue->rear = 0;
		// 记录最大容量
		queue->maxLength = length;
	}
	return queue;
}

/**
* @brief 得到队列长度
* @param queue 指向队列的指针
* @return 队列中的元素个数
*/
int GetQueueLength(Queue* queue)
{
	return queue->rear >= queue->front ?
		queue->rear - queue->front :
		queue->rear - queue->front + queue->maxLength;
}

/**
* @brief 入队
* @param queue 指向队列的指针
* @param data 要入队的元素
*/
void EnQueue(Queue* queue, DataType data)
{
	if ((queue->rear + 1) % queue->maxLength == queue->front)
	{
		printf("队列已满，无法完成入队操作");
	}
	else
	{
		// 队尾向后移动一位
		queue->rear = (queue->rear + 1) % queue->maxLength;
		// 入队
		queue->queArray[queue->rear] = data;
	}
}

/**
* @brief 出队
* @param queue 指向队列的指针
* @return 出队的元素值，如队空，返回0
*/
DataType DlQueue(Queue* queue)
{
	if (GetQueueLength(queue) > 0)
	{
		queue->front = (queue->front + 1) % queue->maxLength;
		return queue->queArray[queue->front];
	}
	else
	{
		return 0;
	}
}

/**
* @brief 取队头元素
* @param queue 指向队列的指针
* @return 队头元素值，如队空返回0
*/
DataType GetQueueHead(Queue* queue)
{
	if (GetQueueLength(queue) > 0)		// 队不空
	{
		int k = (queue->front + 1) % queue->maxLength;
		return queue->queArray[k];
	}
	else
	{
		return 0;					// 队是空的 返回0
	}
}
