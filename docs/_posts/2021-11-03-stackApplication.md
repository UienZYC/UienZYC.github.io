---
layout: post
title:  "栈应用（表达式求值）"
date:   2021-11-03 17:10:00 +0800
categories: jekyll update
---
```c
/*写出P.93 例3-6的可运行的完整程序，
包括栈结构体，各个函数以及主函数，并进行代码和算法的优化。*/

#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>		//头文件

// 定义一个实数栈
typedef double DataType;
// 定义栈 
typedef struct STACK
{
	DataType* stackArray;
	int top;
	int maxLength;
}Stack;

// 声明栈的函数
Stack* CreateStack(int length);		// 创建一个新的栈
int Pop(Stack* stack, DataType* data);		// 弹栈
void Push(Stack* stack, DataType data);	// 压栈

// 栈函数的实现
/**
* @brief 创建一个新的栈
* @param length 栈的大小
* @return 指向栈的指针，如果创建失败返回NULL
*/
Stack* CreateStack(int length)
{
	Stack* stack = (Stack*)malloc(sizeof(Stack));
	if (stack)
	{
		// 分配栈空间
		stack->stackArray = (DataType*)malloc(length * sizeof(DataType));
		if (stack->stackArray == NULL)
			return NULL;
		stack->maxLength = length;
		// 置为空栈
		stack->top = -1;
	}
	return stack;
}

/**
* @brief 弹栈
* @param stack 指向栈的指针
* @return 弹出的栈顶元素，如果弹栈失败返回0
*/
int Pop(Stack* stack, DataType* data)
{
	if (stack->top >= 0)
	{
		// 如果栈不空，则出栈
		*data = stack->stackArray[stack->top];
		stack->top -= 1;
		return 1;		// 成功返回1
	}
	else
	{
		return 0;		// 失败返回0
	}
}

/**
* @brief 压栈
* @param stack 指向栈的指针
* @param data 要入栈的元素
*/
void Push(Stack* stack, DataType data)
{
	if (stack->top < stack->maxLength - 1)
	{
		stack->top++;
		stack->stackArray[stack->top] = data;
	}
}

// 主函数
int main()
{
	// 定义栈大小
	const int MAXLENGTH = 100;
	// 创建一个栈
	Stack* stack = CreateStack(MAXLENGTH);
	double a, b, c, d;
	char exp[30];			// 表达式最长30个字符
	double num1, num2;
	printf("依次输入a、b、c、d的值： ");
	scanf("%lf%lf%lf%lf", &a, &b, &c, &d);		// 输入a、b、c、d的值
	printf("输入后缀表达式： ");
	scanf("%s", exp);							// 输入a、b、c、d组成的表达式
	// 计算后缀表达式
	for (int i = 0; exp[i] != 0; i++)
	{
		switch (exp[i])
		{
		case'+':
		case'-':
		case'*':
		case'/':
			Pop(stack, &num2);
			Pop(stack, &num1);
			if (exp[i] == '+')    Push(stack, num1 + num2);
			if (exp[i] == '-')    Push(stack, num1 - num2);
			if (exp[i] == '*')    Push(stack, num1 * num2);
			if (exp[i] == '/')    Push(stack, num1 / num2);
			break;
		default:
			if (exp[i] == 'a')    Push(stack, a);
			if (exp[i] == 'b')    Push(stack, b);
			if (exp[i] == 'c')    Push(stack, c);
			if (exp[i] == 'd')    Push(stack, d);
		}
	}
	Pop(stack, &num1);
	printf("结果为： %lf", num1);
	return 0;
}
