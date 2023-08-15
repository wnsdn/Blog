# ⭐ 일반적인 객체 생성
**`🔻 MyPerson.h`**
```cpp
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
**`🔻 MyPerson.cpp`**
```cpp
#include "MyPerson.h"

//생성자 정의
MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight)
{}
```
**`🔻 main.cpp`**
```cpp
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
- 4가지 **`속성`** 을 갖는, MyPerson **`클래스`** 를 정의하였다.

    > 보다시피 아주 간단하다.<br>
    >
    > 하지만, 속성이 늘어날수록 코드가 더러워질것이다.

<br>

# ⭐ 속성 추가
**`🔻 MyPerson.h`**
```cpp
std::string Name;
int Age;
int Height;
int Weight;
char add1;           //추가된속성1
char add2;           //추가된속성2
char add...;         //추가된속성...

...

MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight, char _add1, char _add2, char _add...)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight), add1(_add1), add2(_add2), ...(_add...)
{}
```
**`🔻 MyPerson.cpp`**
```cpp
MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight, char add1, char add2, char _add...);
```
- **`속성`** 추가, **`생성자`** 인자추가

    > MyPerson 클래스에 몇개의 속성이 추가된다. (add1, add2, add...)
    >
    > 그로인해, 생성자도 영향을 받는다.

<br>

**`🔻 main.cpp`**
```cpp
MyPerson Person1{ "재석", 13, 170, 60, add1, add2, ...};
MyPerson Person2{ "명수", 20, 182, 78, add1, add2, ...};
MyPerson Person3{ "홍철", 18, 153, 44, add1, add2, ...};
```
- **`기존 객체`** 도 영향을 받는다.

    > 새로 추가된 속성이 기존 객체에게 필요없더라도,<br>
    > 호출부를 수정해줘야한다.
    >
    > 반대로, 새로 만들어질 객체에게는 기존의 속성이 필요없을 수도 있다.

<br>

# ⭐ 단순한 빌더 - 소개
**`🔻 MyPerson.h`**
```cpp
#pragma once
#include <string>

class MyPerson
{
private:
	std::string Name;
	int Age;
	int Height;
	int Weight;
public:
	void SetName(const std::string& _Name);
	void SetAge(int _Age);
	void SetHeight(int _Height);
	void SetWeight(int _Weight);

	MyPerson() = default;
};
```
**`🔻 MyPerson.cpp`**
```cpp
#include "MyPerson.h"

void MyPerson::SetName(const std::string& _Name)
{
	Name = _Name;
}

void MyPerson::SetAge(int _Age)
{
	Age = _Age;
}

void MyPerson::SetHeight(int _Height)
{
	Height = _Height;
}

void MyPerson::SetWeight(int _Weight)
{
	Weight = _Weight;
}
```
- **`생성자`** to **`Setter`**

    > 생성자를 default로 바꿔주고,<br>
    >
    > 각 속성들에 대한 Setter를 만들어준다.

<br>

**`🔻 main.cpp`**
```cpp
#include "MyPerson.h"

int main()
{
	MyPerson Person1;
	Person1.SetName("재석");
	Person1.SetAge(13);
	Person1.SetHeight(170);
	Person1.SetWeight(60);

	MyPerson Person2;
	Person2.SetName("명수");
	Person2.SetAge(20);
	Person2.SetHeight(182);
	Person2.SetWeight(78);

	MyPerson Person3;
	Person3.SetName("홍철");
	Person3.SetAge(18);
	Person3.SetHeight(153);
	Person3.SetWeight(44);

	return 0;
}
```
- **`일반적인 객체 생성`** 과 같은 결과를 보여준다.

    > **"결과는 같은데, 괜히 코드만 길게 치는거 아닌가?"** 라는 생각이 들 수 있다.
    >
    > 뒤에서 빌더 방식의 장점을 알아보자,

<br>

# ⭐ 단순한 빌더 - 장점
**`🔻 MyPerson.h`**
```cpp
std::string Name;
int Age;
int Height;
int Weight;
std::string Address;    //추가된 속성

...

void SetName(const std::string& _Name);
void SetAge(int _Age);
void SetHeight(int _Height);
void SetWeight(int _Weight);
void SetAddress(const std::string& _Address);    //추가된 Setter - 선언
```
**`🔻 MyPerson.cpp`**
```cpp
...

//추가된 Setter - 정의
void MyPerson::SetAddress(const std::string& _Address)
{
	Address = _Address;
}
```
- **`속성`** 추가, **`Setter`** 추가

    > 속성이 추가되어, 그에 대한 Setter도 만들어준다.<br>
    >
    > `일반적인 객체 생성` 방식과 달리, 만들어져있는 어떤 함수에도 영향을 미치지 않는다.<br>
    > 그로인해, <ins style="color: #202020">속성의 추가, 제거</ins>가 용이하다!

<br>

**`🔻 main.cpp`**
```cpp
#include "MyPerson.h"

int main()
{
	MyPerson Person1;
	Person1.SetName("재석");
	Person1.SetAge(13);
	Person1.SetHeight(170);
	Person1.SetWeight(60);

	MyPerson Person2;
	Person2.SetName("명수");
	Person2.SetAge(20);
	Person2.SetHeight(182);
	Person2.SetWeight(78);

	MyPerson Person3;
	Person3.SetName("홍철");
	Person3.SetAge(18);
	Person3.SetHeight(153);
	Person3.SetWeight(44);

	return 0;
}
```
- **`일반적인 객체 생성`** 과 같은 결과를 보여준다.

    > **"결과는 같은데, 괜히 코드만 길게 치는거 아닌가?"** 라는 생각이 들 수 있다.
    >
    > 뒤에서 빌더 방식의 장점을 알아보자,

<br>
