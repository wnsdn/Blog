---
title: "디자인패턴: 빌더"
summary: "디자인패턴: 빌더"
---

# $${⭐\color{orange}일반적인\space객체\space생성}$$
> ❤️ MyPerson.h & MyPerson.cpp

```cpp
//MyPerson.h
#pragma once
#include <string>

class MyPerson
{
private:
	std::string Name;    //이름
	int Age;             //나이
	int Height;          //키
	int Weight;          //몸무게
public:
        //생성자 선언
	MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight);
};
```

```cpp
//MyPerson.cpp
#include "MyPerson.h"

//생성자 정의
MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight)
{
}
```

> ❤️ `main.cpp`

```cpp
//main.cpp
#include "MyPerson.h"

int main()
{
        //Person1 생성
	MyPerson Person1{ "재석", 13, 170, 60 };

        //Person2 생성
	MyPerson Person2{ "명수", 20, 182, 78 };

        //Person3 생성
	MyPerson Person3{ "홍철", 18, 153, 44 };

	return 0;
}
```
4가지 `속성`을 갖는, Person(사람) `클래스`를 정의하였다.<br>
**보다시피 아주 간단하다.**

`하지만,` 속성이 늘어날수록 코드가 더러워질것이다.<br>

`예를들어보자,`<br>
Ⅰ. `속성`이 늘어난다.<br>
```cpp
//MyPerson.h
private:
	std::string Name;    //이름
	int Age;             //나이
	int Height;          //키
	int Weight;          //몸무게
    void add1;               //추가된속성1
    void add2;               //추가된속성2
    void ...;                //...
```
Ⅱ. `생성자의 선언`을 수정한다.<br>
```cpp
//MyPerson.h
MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight, void add1, void add2, void ...);
```
Ⅲ. `생성자의 정의`를 수정한다.<br>
```cpp
//MyPerson.cpp
MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight, void _add1, void _add2, void _...)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight), add1(_add1), add2(_add2), ...(_...)
{
}
```
Ⅳ. `기존에 생성`한 Person1, Person2, Person3의 호출부를 수정한다.<br>
```cpp
//main.cpp
MyPerson Person1{ "재석", 13, 170, 60, add1, add2, ...};
MyPerson Person2{ "명수", 20, 182, 78, add1, add2, ...};
MyPerson Person3{ "홍철", 18, 153, 44, add1, add2, ...};
```
Ⅴ. `새로운 객체` 생성 또한 위와 같아진다.<br><br>
`또한,` 다른 무언가와 엮여있을 가능성까지 생각해보면 머리가 어지러워진다...
