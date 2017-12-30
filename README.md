#include "stdafx.h"
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>
#include<iostream>
#include <iomanip>
#include <math.h>
using namespace std;


int gys(int x, int y)    /*定义求最大公约数函数*/
{
	return y ? gys(y, x%y) : x;  /*递归调用gys，返回最大公约数*/
}

struct fengshu { int a; int b; }fengshu01, fengshu02;
struct fengshu jisuan(struct fengshu  fengshu01, struct fengshu fengshu02, char op)//四则运算
{
	struct fengshu jieguo;
	int q;
	if (op == '+')
	{
		jieguo.a = fengshu01.a*fengshu02.b + fengshu02.a *fengshu01.b;
		jieguo.b = fengshu01.b*fengshu02.b;
	}
	else if (op = '-')
	{
		jieguo.a = fengshu01.a*fengshu02.b - fengshu01.b*fengshu02.a;
		jieguo.b = fengshu01.b*fengshu02.b;
	}

	else if (op == '*')
	{
		jieguo.a = fengshu01.a*fengshu02.a;
		jieguo.b = fengshu01.b*fengshu02.b;
	}

	else
	{
		jieguo.a = fengshu01.a*fengshu02.b;
		jieguo.b = fengshu01.b*fengshu02.a;

	}
	q = gys(jieguo.a, jieguo.b);
	jieguo.a = jieguo.a / q;
	jieguo.b = jieguo.b / q;
	return jieguo;
}


int main()
{
	srand((unsigned)time(NULL));//初始化随机
	int i, n, q;
	int ture = 0;

	char  op[4];//定义运算符
	cout << "输入题目数量" << endl;
	cin >> n;

	//生成一个算式并核对
	for (i = 1; i <= n; i++)
	{
		srand((unsigned)time(NULL));
		struct fengshu suijishu[4];
		for (int m = 0; m < 4; m++) {
			suijishu[m].a = rand() % 10+1;
			suijishu[m].b = rand() % 10+1;
		}
		for (int l = 0; l < 4; l++)
		{
			if (suijishu[l].a > suijishu[l].b)//控制生成真分数
			{
				int t;
				t = suijishu[l].a; suijishu[l].a = suijishu[l].b; suijishu[l].b = t;
			}

			q = gys(suijishu[l].a, suijishu[l].b);
			suijishu[l].a = suijishu[l].a / q;
			suijishu[l].b = suijishu[l].b / q;

		}
		/*suijishu[4]
		= { { rand() % 10 + 1, 1 }, { rand() % 10 + 1, 1 }, { rand() % 10 + 1, rand() % 10 + 1 }, { rand() % 10 + 1, rand() % 10 + 1 } };//定义数值*/

		int  j, s;
		//生成运算符


		for (s = 0; s < 3; s++)
		{

			j = rand() % 4;
			switch (j)
			{
			case 0:op[s] = '+'; break;
			case 1:op[s] = '-'; break;
			case 2:op[s] = '*'; break;
			case 3:op[s] = '/'; break;
			}
		}
		char op0 = op[0], op1 = op[1], op2 = op[2];


		cout << "第" << i << "题" << suijishu[0].a << '/' << suijishu[0].b << op0 << suijishu[1].a << '/' << suijishu[1].b << op1 << suijishu[2].a << '/' << suijishu[2].b << op2 << suijishu[3].a << '/' << suijishu[3].b << '=';

		//计算结果
		for (int n = 0; n < 3; n++)
		{
			if(op[n]=='*'||op[n]=='/')
			suijishu[n + 1] = jisuan(suijishu[n], suijishu[n + 1], op[n]);
			op[n] = '+';
			suijishu[n] = { 0,1 };
		}
		fengshu shuru, daan;
		for (int o = 0; o < 3; o++)
		{
			suijishu[n+1] = jisuan(suijishu[n], suijishu[n + 1], op[n]);
			daan = suijishu[3];
		}
		
		cin >> shuru.a >> shuru.b;
		
		if (shuru.a == daan.a&&shuru.b == daan.b)
		{
			cout << "正确" << endl;
			ture = ture + 1;
		}
		else
		{
			cout << "不正确，答案=" << daan.a << "/" << daan.b << endl;
		}
	}
	return 0;

}
