---
layout: post
title:  "单链表应用（通讯录管理）"
date:   2021-10-23 13:04:00 +0800
categories: jekyll update
---
```c
/**
 * 通讯录管理程序 
 * @author UienZYC
 * @date 10-23-2021 13:01
 * 单链表，没有头结点
 * 对课本中struct LNode（即数据类型）进行了修改，删除了其中的指针域
 * 删除了主函数中tmp.next=NULL一行
 * 一些其他修改
 */

//#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>		//头文件

// 定义通讯录记录信息结构
typedef struct addressBook {
	char name[20];		// 姓名
	char tel[15];		// 电话
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
int GetLinkListLength(Head head);
int FindElement(Head head, char* name);
void PrintList(Head head);
int InsertRear(Head* head, DataType data);
int DeleteFromList(Head* head, int pos);

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

/* 修改查找函数，按名称查找，成功则返回位置下标*/
int FindElement(Head head, char* name)
{
	int i = 0;
	while (head)
	{
		if (strcmp(head->data.name, name) == 0)				// 如果找到了元素
			return i;
		head = head->next;		// 看下一个
		i++;
	}
	return -1;					// 失败返回-1
}

/* 修改输出函数，输出全部记录*/
void PrintList(Head head)
{
	printf("联系人 \t 电话 \n");
	printf("===================\n");
	while (head)
	{
		printf("%s\t%s\n", head->data.name, head->data.tel);
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
	Node* pDeleteNode=NULL;
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

// 主函数
int main()
{
	Head head = NULL;		// 定义头指针（无头结点）
	int select = 1;			// 操作选择 0-结束 1-录入 2-删除 3-显示
	char Name[20];			// 临时存储姓名
	DataType tmp;
	int pos;
	while (select != 0)
	{
		printf("\n 请输入操作选择：1-录入 2-删除 3-显示 0-结束\n");
		scanf_s("%d", &select);
		switch (select)
		{
		case 1:
			printf("输入联系人的姓名：");
			scanf_s("%s", &tmp.name,20);
			printf("输入联系人的电话：");
			scanf_s("%s", &tmp.tel,15);
			InsertRear(&head, tmp);
			break;
		case 2:
			printf("请输入待删除联系人的姓名：");
			scanf_s("%s", Name,20);
			pos = FindElement(head, Name);
			if (pos >= 0)
			{
				DeleteFromList(&head, pos);
			}
			break;
		case 3:
			PrintList(head);
			break;
		case 0:
			printf("使用结束，再见！");
			break;
		}
	}
	return 0;
}
```

