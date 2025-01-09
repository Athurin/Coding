朴素版的KMP和改进版的KMP:
KMP.h:
```cpp
#pragma once

#include<iostream>
#include<string>

using namespace std;

//朴素KMP算法
void GetNext(int next[], string t)//模式串t和next[]数组
{
	int j = 0, k = -1;
	next[0] = -1;
	while (j < t.size()-1)
	{
		if (k == -1 || t[j] == t[k])//只要相等或者开头
		{
			j++;
			k++;
			next[j] = k;//j跳到下一位，对于下一位来说，加速匹配的信息是k
		}
		else
		{
			k = next[k];
		}
	}
}

bool KMP(string s, string t, int next[], int &val)//val是匹配成功的第一个字母在主串中的下标
{
	int i = 0;//扫描主串s
	int j = 0;//扫描模式串t
	int ls = s.size(), lt = t.size();

	GetNext(next, t);
	//for (int i = 0; i < lt; i++) cout << next[i] << " ";

	while (i < ls && j < lt)
	{
		if (j == -1 || s[i] == t[j])
		{
			i++;
			j++;
		}
		else
		{
			j = next[j];
		}
	}
	if (j >= lt)
	{
		val = i - lt;
		return true;
	}
	else
		return false;
}


//改进版KMP

void GetNextval(string t, int nextval[])
{
	int j = 0, k = -1;
	nextval[0] = -1;

	while (j < t.size() - 1)
	{
		if (k == -1 || t[j] == t[k])
		{
			j++, k++;
			if (t[j] == t[k])
			{
				nextval[j] = nextval[k];
			}
			else
			{
				nextval[j] = k;
			}
		}
		else
		{
			k = nextval[k];
		}
	}
}

bool KMP_pro(string s, string t, int nextval[], int &val)
{
	int i = 0;//扫描主串s
	int j = 0;//扫描模式串t
	int ls = s.size(), lt = t.size();

	GetNextval(t, nextval);
	//for (int i = 0; i < lt; i++) cout << next[i] << " ";

	while (i < ls && j < lt)
	{
		if (j == -1 || s[i] == t[j])
		{
			i++;
			j++;
		}
		else
		{
			j = nextval[j];
		}
	}
	if (j >= lt)
	{
		val = i - lt;
		return true;
	}
	else
		return false;
}
```

KMP_Main.cpp
```cpp
#include"KMP.h"
#include<iostream>
#include<string>

using namespace std;
const int Maxsize = 30;

int main()
{
	string s, t;
	int next[Maxsize];
	int val = -1;

	cout << "请输入主串：" << endl;
	cin >> s;
	cout << "请输入模式串：" << endl;
	cin >> t;

	if (KMP_pro(s, t, next, val))
	{
		cout << "存在子串模式匹配成功！下标为：" << " " << val << endl;
	}
	else
	{
		cout << "模式匹配不成功！" << endl;
	}

	return 0;
}
```
![[Screenshot 2024-03-28 183100.png]]


