
sort()函数有三个参数：

- 第一个是要排序的容器的**起始迭代器**。

- 第二个是要排序的容器的**结束迭代器**。

- 第三个参数是**排序的方法，是可选的参数**。默认的排序方法是从小到大排序，也就是`less<Type>()`，还提供了`greater<Type>()`进行从大到小排序。这个参数的类型是`函数指针`，`less和greater`实际上都是`类/结构体`，内部分别`重载了()运算符`，称为仿函数，所以实际上`less<Type>()和greater<Type>()`都是函数名，也就是函数指针。我们还可以用普通函数来定义排序方法。

如果容器内元素的类型是`内置类型或string类型`，我们可以直接用`less<Type>()或greater<Type>()`进行排序。但是如果数据类型是我们自定义的结构体或者类的话，我们需要自定义[排序函数](https://so.csdn.net/so/search?q=%E6%8E%92%E5%BA%8F%E5%87%BD%E6%95%B0&spm=1001.2101.3001.7020)，有三种写法：

- 重载 < 或 > 运算符：重载 `<` 运算符，传入`less<Type>()`进行升序排列。重载 `>` 运算符，传入`greater<Type>()`进行降序排列。这种方法只能针对一个维度排序，**不灵活**。

- 普通函数：写普通函数cmp，传入`cmp`按照指定规则排列。这种方法可以对多个维度排序，**更灵活**。

- 仿函数：写仿函数cmp，传入`cmp<Type>()`按照指定规则排列。这种方法可以对多个维度排序，**更灵活**。


### 重写 < 或 > 运算符 (推荐写法)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Person {
	int id;
	int age;
	Person(int id,int age):id(id),age(age){}
	//重载<运算符，进行升序排列
	bool operator < (const Person& p2) const {
		return id < p2.id;
	}
	//重载>运算符，进行降序排列
	bool operator > (const Person& p2) const {
		return id > p2.id;
	}
};

int main()
{
	Person p1(1, 10), p2(2, 20), p3(3, 30);
	vector<Person> ps;
	ps.push_back(p2);
	ps.push_back(p1);
	ps.push_back(p3);
	sort(ps.begin(), ps.end(), less<Person>());
	for (int i = 0; i < 3; i++) {
		cout << ps[i].id << " " << ps[i].age << endl;
	}
	cout << endl;
	sort(ps.begin(), ps.end(), greater<Person>());
	for (int i = 0; i < 3; i++) {
		cout << ps[i].id << " " << ps[i].age << endl;
	}
	cout << endl;
}
```

### 普通函数

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Person {
	int id;
	int age;
	Person(int id,int age):id(id),age(age){}
	//重载<运算符，进行升序排列
	bool operator < (const Person& p2) const {
		return id < p2.id;
	}
	//重载>运算符，进行降序排列
	bool operator > (const Person& p2) const {
		return id > p2.id;
	}
};

int main()
{
	Person p1(1, 10), p2(2, 20), p3(3, 30);
	vector<Person> ps;
	ps.push_back(p2);
	ps.push_back(p1);
	ps.push_back(p3);
	sort(ps.begin(), ps.end(), less<Person>());
	for (int i = 0; i < 3; i++) {
		cout << ps[i].id << " " << ps[i].age << endl;
	}
	cout << endl;
	sort(ps.begin(), ps.end(), greater<Person>());
	for (int i = 0; i < 3; i++) {
		cout << ps[i].id << " " << ps[i].age << endl;
	}
	cout << endl;
}
```

### 仿函数

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Person {
	int id;
	int age;
	Person(int id, int age) :id(id), age(age) {}
};

//仿函数
struct cmp {
	bool operator()(const Person& p1, const Person& p2) {
		if (p1.id == p2.id) return p1.age >= p2.age;
		return p1.id < p2.id;
	}
};

int main()
{
	Person p1(1, 10), p2(2, 20), p3(3, 30), p4(3, 40);
	vector<Person> ps;
	ps.push_back(p2);
	ps.push_back(p1);
	ps.push_back(p3);
	ps.push_back(p4);
	sort(ps.begin(), ps.end(), cmp()); //传入函数指针cmp()
	for (int i = 0; i < 4; i++) {
		cout << ps[i].id << " " << ps[i].age << endl;
	}
}
```

