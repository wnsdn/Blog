# ⭐ 일반적인 객체 생성
**`▼ MyPerson.h`**
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
**`▼ MyPerson.cpp`**
```cpp
#include "MyPerson.h"

//생성자 정의
MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight)
{}
```
**`▼ main.cpp`**
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

**`▼ MyPerson.h`**
```cpp
std::string Name;
int Age;
int Height;
int Weight;
char add1;           //추가된속성1
char add2;           //추가된속성2
char add...;         //추가된속성...

...
...
...

MyPerson::MyPerson(const std::string& _Name, int _Age, int _Height, int _Weight, char _add1, char _add2, char _add...)
	: Name(_Name), Age(_Age), Height(_Height), Weight(_Weight), add1(_add1), add2(_add2), ...(_add...)
{}
```
**`▼ MyPerson.cpp`**
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