---
layout: post
title:  "单链表应用（复数）"
date:   2021-10-24 17:10:00 +0800
categories: jekyll update
---
```c
/**
 * 复数单链表
 * @author UienZYC 
 * @date 10-24-2021 17:10
 * 单链表，没有头结点
 * 在Visual Studio中请使用release运行，Debug会报毒，目前还不知道原因（10.24）
 * 交互！
 * 功能：新增、在指定位置插入、查找删除、显示全部元素
 * 主函数attached
 */

 //#define _CRT_SECURE_NO_WARNINGS		// 根据需要
#include<stdio.h>
#include<stdlib.h>
#include<string.h>		//头文件

// 定义复数data结构
typedef struct complexNumber {
	double Real;
	double Imaginary;
}DataType;

// 单链表定义结点
struct NODE {
	DataType data;				// 数据域
	struct NODE* next;					// 指针域
};
typedef struct NODE Node;

// 定义头指针类型
typedef Node* Head;

// 链表的方法声明

int GetLinkListLength(Head head);				// 获取表长（有用元素的数量）
int FindElement(Head head, double Re,double Im);	// 查找元素返回下标（从0开始）
void PrintList(Head head);						// 全打印
int InsertRear(Head* head, DataType data);		// 表尾插入（返回新表长）
int InsertHead(Head* head, DataType data);
int DeleteFromList(Head* head, int pos);		// 删除pos处元素，pos从0开始
int InsertToList(Head* head, int pos, DataType data);	// 将data插入到pos（下标）处

// 链表的方法实现

/**
* @brief 求表长
* @param head 链表的头指针
* @return 链表的长度
*/
int GetLinkListLength(Head head)
{
	if (head == NULL)
		return 0;
	int i = 1;
	Node* pNode = head;
	while (pNode->next)
	{
		i++;
		pNode = pNode->next;
	}
	return i;
}

/* 修改查找函数，输入实部和虚部查找，返回位置下标，否则返回-1*/
int FindElement(Head head, double Re,double Im)
{
	int i = 0;
	while (head)
	{
		if (head->data.Real==Re && head->data.Imaginary==Im)   // 如果找到了元素
			return i;
		head = head->next;		// 看下一个
		i++;
	}
	return -1;					// 失败返回-1
}

/* 修改输出函数，输出全部记录*/
void PrintList(Head head)
{
	while (head)
	{
		printf("%g%+gi\n", head->data.Real, head->data.Imaginary);
		head = head->next;
	}
}

/**
* @brief 从表尾插入元素
* @param head 指向线性表的头指针
* @param data 插入的数据
* @return 插入成功返回新的表长，否则返回-1
*/
int InsertRear(Head* head, DataType data)
{
	// 准备新数据
	Node* pNewNode = (Node*)malloc(sizeof(Node));
	if (pNewNode == NULL)				// 内存分配失败
		return -1;
	pNewNode->data = data;
	pNewNode->next = NULL;
	if (*head == NULL)						// 如果表是空表
	{
		*head = pNewNode;
		return 1;							// 表长为1
	}
	// 找到表尾
	Node* pNode = *head;
	while (pNode->next)
		pNode = pNode->next;
	// 插入到表尾
	pNode->next = pNewNode;
	// 返回新的表长
	return GetLinkListLength(*head);
}

/**
* @brief 从表头插入元素
* @param head 指向线性表的头指针
* @param data 插入的数据
* @return 插入成功返回新的表长，否则返回-1
*/
int InsertHead(Head* head, DataType data)
{
	// 准备新数据
	Node* pNewNode = (Node*)malloc(sizeof(Node));
	if (pNewNode == NULL)
		return -1;				// 分配内存失败
	pNewNode->data = data;
	// 插入
	pNewNode->next = (*head);		// 新结点指向原来的第一个结点
	*head = pNewNode;				// 修改头指针，指向新结点
	// 返回新的表长
	return GetLinkListLength(*head);
}

/**
* @brief 删除线性表上位置为pos的元素
* @param head 指向线性表的头指针
* @param pos 删除的位置
* @return 删除成功返回新的表长，否则返回-1
*/
int DeleteFromList(Head* head, int pos)
{
	Node* pNode = *head;
	int length = GetLinkListLength(*head);		// 得到表长
	if (pos<0 || pos>length - 1)						// 删除的位置不对
		return -1;
	Node* pDeleteNode = NULL;
	if (pos > 0)
	{
		for (int i = 0; i < pos - 1; ++i)
			pNode = pNode->next;		//得到pos的前驱指针
		pDeleteNode = pNode->next;		// 暂存待删除节点指针
		pNode->next = pNode->next->next;	// 修改前驱结点指针
	}
	if (pos == 0)
	{
		pDeleteNode = *head;			// 暂存待删除结点指针
		*head = (*head)->next;
	}
	free(pDeleteNode);					// 释放删除结点的空间
	return --length;
}

/**
* @brief 将data插入到pos处
* @param head 指向线性表的头指针
* @param pos 插入的位置
* @return 插入成功返回新的表长，否则返回-1
*/
int InsertToList(Head* head, int pos, DataType data)
{
	Node* pNode = *head;
	int length = GetLinkListLength(pNode);		// 得到表长
	if (pos<0 || pos>length)
		return -1;								// 插入位置错误
	if (pos == 0)
		return InsertHead(head, data);
	if (pos == length - 1)
		return InsertRear(head, data);
	// 定位到pos前一位
	for (int i = 0; i < pos - 1; ++i)
		pNode = pNode->next;
	// 生成一个新的结点
	Node* pNewNode = (Node*)malloc(sizeof(Node));
	if (pNewNode == NULL)
		return -1;				// 分配内存失败
	pNewNode->data = data;		// 存入要插入的数据
	pNewNode->next = pNode->next;		// 插入链表
	pNode->next = pNewNode;
	// 返回新的列表长度
	return ++length;
}

// 主函数
int main()
{
	Head head = NULL;		// 定义头指针（无头结点）
	int select = 1;			// 操作选择 0-结束 1-录入 2-删除 3-显示
	double Re,Im;			// 临时存储虚数，供查找
	DataType tmp;
	int pos;
	while (select != 0)
	{
		printf("\n请输入操作选择：1-新增复数 2-删除 3-显示 4- 插入 0-结束\n");
		printf(">>>");
		scanf_s("%d", &select);
		switch (select)
		{
		case 1:
			printf("请依次输入新增复数的实部，虚部（空格分隔）：\n");
			scanf_s("%lf%lf", &tmp.Real, &tmp.Imaginary);
			InsertRear(&head, tmp);
			break;
		case 2:
			printf("请输入待删除复数的实部和虚部（空格分隔）：\n");
			scanf_s("%lf%lf",&Re,&Im);
			pos = FindElement(head, Re,Im);
			if (pos >= 0)
			{
				DeleteFromList(&head, pos);
			}
			else
				printf("未找到该复数！");
			break;
		case 3:
			PrintList(head);
			break;
		case 4:
			printf("请输入要插入的位置（从0开始）：");
			scanf_s("%d", &pos);
			printf("请依次输入新增复数的实部，虚部（空格分隔）：\n");
			scanf_s("%lf%lf", &tmp.Real, &tmp.Imaginary);
			if (InsertToList(&head, pos, tmp) == -1)
				printf("插入位置错误，插入失败");
			break;
		case 0:
			printf("使用结束，再见！");
			break;
		}
	}
	return 0;
}
```

