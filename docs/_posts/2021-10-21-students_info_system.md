---
layout: post
title:  "线性表顺序表应用（学号姓名表）"
date:   2021-10-21 17:48:00 +0800
categories: jekyll update
---
```c
/**
 * @author UienZYC
 * @date 10-21-2021 16：57
 * 本程序在课本基础上大大提高了可交互性
 * 本程序参考了大量课本代码
 * 部分函数没有被调用
 */

#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>		//头文件
// 定义结构体STU，学生的学号和姓名是它的元素
// 并且给结构体STU改个名字为DataType，用DataType替代 struct STU
typedef struct STU {
	int id;				// 学号
	char name[20];		// 姓名
}DataType;

// 定义线性表的结构
typedef struct List
{
	DataType* list;
	int length;
	int maxLength;
}ListType;

// 声明线性表具有的方法
ListType* CreatList(int length);				// 创建长度为length的线性表
void DestroyList(ListType* pList);				// 销毁线性表
void ClearList(ListType* pList);				// 置空线性表
int IsEmptyList(ListType* pList);				// 检测线性表是否为空
int GetListLength(ListType* pList);				// 获取线性表长度
int GetListElement(ListType* pList, int n, DataType* data);		// 获取线性表中第n个元素
int FindElement(ListType* pList, int pos, int id);				// 从pos起查找data第一次出现的位置
int InsertToList(ListType* pList, int pos, DataType data);		// 将data插入到pos处
int DeleteFromList(ListType* pList, int pos);					// 删除pos处的元素
void PrintList(ListType* pList);

//线性表方法实现
/**
 * @brief 创建一个新的线性表
 * @param length 线性表的最大容量
 * @return 成功返回指向该表的指针，否则返回NULL
 */
ListType* CreatList(int length)
{
	ListType* sqList = (ListType*)malloc(sizeof(ListType));
	if (sqList != NULL) 
	{
		// 为线性表分配内存
		sqList->list = (DataType*)malloc(length * sizeof(DataType));
		// 如果分配失败，返回NULL
		if (sqList->list == NULL)
			return NULL;
		// 置为空表
		sqList->length = 0;
		// 最大长度
		sqList->maxLength = length;
	}
	return sqList;  //如果if前面那一行成功分配给sqList内存，那么返回的仍然是NULL
}

/**
 * @brief 销毁线性表
 * @param pList 指向需要销毁的线性表的指针
*/
void DestroyList(ListType* pList)
{
	free(pList->list);
}

/**
 * @brief 置空线性表
 * @param pList 指向需要置空线性表的指针
*/
void ClearList(ListType* pList)
{
	pList->length = 0;
}

/**
 * @brief 检测线性表是否为空
 * @param pList 指向线性表的指针
 * @return 如果线性表为空，返回1；否则返回0
*/
int IsEmptyList(ListType* pList)
{
	return pList->length == 0 ? 1 : 0;
}

/**
 * @brief 获取线性表长度
 * @param pList 指向线性表的指针
 * @return 返回线性表的长度
 */
int GetListLength(ListType* pList)
{
	return pList->length;
}

/**
 * @brief 获取线性表中第n个元素(这里应该是下标n）
 * @param pList 指向线性表的指针
 * @param n 要获取元素在线性表中的位置
 * @param data 获取成功，取得元素存放于data中
 * @return 获取成功返回1，失败返回0
 */
int GetListElement(ListType* pList, int n, DataType* data)
{
	if (n<0 || n>pList->length - 1)  
		return 0;
	*data = pList->list[n];
	return 1;
}

/*下面是输入学号id进行查找的函数*/
/**
 * @brief 从pos起查找id第一次出现的位置
 * @param pList 指向线性表的指针
 * @param pos 查找的起始位置
 * @param data 要查找的元素
 * @return 找到则返回该位置（角标），未找到则返回-1
 */
int FindElement(ListType* pList, int pos, int id)
{
	for (int n = pos; n < pList->length; ++n)
	{
		if (id == pList->list[n].id)
			return n;
	}
	return -1;
}
/**
 * @brief 将data 插入到线性表的pos位置处
 * @param pList 指向线性表的指针
 * @param pos 插入的位置
 * @param data 要插入的元素放在data中
 * @return 成功则返回新的表长（原表长+1），失败则返回-1
 */
int InsertToList(ListType* pList, int pos, DataType data)
{
	// 如果插入的位置不正确或者线性表已满，则插入失败
	if (pos<0 || pos>pList->length || pList->length == pList->maxLength)
		return -1;
	// 从pos起，所有的元素向后移动1位(n是下标)
	for (int n = pList->length; n > pos; --n)
	{
		pList->list[n] = pList->list[n - 1];
	}
	// 插入新的元素
	pList->list[pos] = data;
	// 表长增加1
	return ++pList->length;
}
/*输出每个学生的学号、姓名*/
void PrintList(ListType* pList)
{
	for (int n = 0; n < pList->length; ++n)
		printf("%d  %s\n", pList->list[n].id, pList->list[n].name);
}

/**
 * @brief 将pos位置处的元素删除
 * @param pList 指向线性表的指针
 * @param pos 删除元素的位置（下标）
 * @return 成功则返回新的表长（原表长-1），失败返回-1
 */
int DeleteFromList(ListType* pList, int pos)
{
	if (pos<0 || pos>pList->length-1) //我认为这里应该是length-1而不是书上的length,因为这里的pos实际上是从0开始的下标
		return -1;
	//将pos后的元素向前移动一位
	for (int n = pos; n < pList->length - 1; ++n)
		pList->list[n] = pList->list[n + 1];
	return --pList->length;
}
/*主函数*/
int main()
{
	const int MAXLENGTH = 1000;
	ListType* sqList = CreatList(MAXLENGTH);  //先建一个表
	int i;	//feedback
	int j;  //循环变量
	int number;   //初始化人数
	int pos;		//插入位置
	int id;			//要删除的学号
	printf("------------------------------------------------------------------------\n");
	printf("欢迎使用学生名册管理程序，请问您想要做点什么？\n");
	printf("0-初始您的名册表 1-插入新的信息  2-删除信息  3-查看当前名单  88-结束程序\n");
	printf("------------------------------------------------------------------------\n");
	printf(">>>");
	while (1)
	{
		scanf("%d", &i);
		switch (i)
		{
		case 0:
		{
			DataType temp;
			printf("几个人呀？\n");
			scanf("%d", &number);
			printf("请依次输入这些人的学号和姓名（学号和姓名间使用空格分隔）\n");
			for (j = 0; j <= number - 1; j++)
			{
				scanf("%d%s", &temp.id, &temp.name);
				InsertToList(sqList, j, temp);
			}
			printf("您的名单初始化完毕");
			break;
		}
		case 1:
		{				//这里注意要有大括号，不然DataType temp会报错
			DataType temp;
			printf("请输入插入位置：\n");
			scanf("%d", &pos);
			printf("请输入学号和姓名（空格分隔）\n");
			scanf("%d%s", &temp.id, &temp.name);
			InsertToList(sqList, pos, temp);
			printf("插入完毕");
			break;
		}
		case 2:
			printf("输入将要删除的学生的学号：\n");
			scanf("%d", &id);
			int pos = FindElement(sqList, 0, id);
			DeleteFromList(sqList, pos);
			printf("已删除学号为%d的学生信息", id);
			break;
		case 3:
			PrintList(sqList);
			break;
		case 88: return 0;
		}
		printf("\n--------------------------------------------------------------------------\n");
		printf("请问您还要做点什么？\n");
		printf("0-初始您的名册表 1-插入新的信息  2-删除信息  3-查看当前名单  88-结束程序\n>>>");
	}
}
```

