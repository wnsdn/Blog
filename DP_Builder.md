<a id="일반적인객체생성"></a>

# ⭐ 일반적인 객체 생성

**`🔻 MyPerson.h`**

```cpp
#pragma once
#include <string>

class MyPerson
{
private:
	std::string Name{};    //이름
	int Age = 0;           //나이
	int Height = 0;        //키
	int Weight = 0;        //몸무게
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

    > 보다시피 아주 간단하다.  
    >
    > 하지만, 속성이 늘어날수록 코드가 더러워질것이다.

<br>

# ⭐ 속성 추가

**`🔻 MyPerson.h`**

```cpp
std::string Name{};
int Age = 0;
int Height = 0;
int Weight = 0;
char add1{};           //추가된속성1
char add2{};           //추가된속성2
char add...{};         //추가된속성...

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

    > 새로 추가된 속성이 기존 객체에게 필요없더라도,  
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
	std::string Name{};
	int Age = 0;
	int Height = 0;
	int Weight = 0;
public:
	void SetName(const std::string& _Name);
	void SetAge(int _Age);
	void SetHeight(int _Height);
	void SetWeight(int _Weight);
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

    > `생성자`를 `기본생성자`로 대체하고,  
    >
    > 각 속성들에 대한 `Setter`를 만들어준다.

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
    > 하지만, 빌더만의 장점이 있다.  
    > 뒤에서 빌더의 장점을 알아보자,

<br>

# ⭐ 단순한 빌더 - 장점

**`🔻 MyPerson.h`**

```cpp
std::string Name{};
int Age = 0;
int Height = 0;
int Weight = 0;
std::string Address{};    //추가된 속성

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

    > 속성이 추가되어, 그에 대한 Setter도 만들어준다.  
    >
    > [일반적인 객체 생성](#일반적인객체생성) 방식과 달리,  
    > 기존의 어떤 함수에도 영향을 미치지 않는다.  
    > 그로인해, `속성의 추가,제거 용이`

<br>

**`🔻 main.cpp`**

```cpp
...

MyPerson NewPerson1;
NewPerson.SetHeight(173);
NewPerson.SetWeight(77);

MyPerson NewPerson2;
NewPerson.SetName("뱀파이어");
NewPerson.SetHeight(190);
NewPerson.SetWeight(85);
NewPerson.SetAddress("유럽 어느 산골속 성");
```

- 새로운 두 객체를 생성한다.

    > 첫 번째 사람은 기억을 잃어서 이름, 나이, 주소를 모른다.  
    > 그래서 키, 몸무게만으로 생성했다.
    >
    > 두 번째 사람은 뱀파이어라서 너무 오래 산 나머지 나이를 까먹었다.  
    > 그래서 나이 없이 생성했다.
- **`빌더방식의 장점`**
    1. 필요한 속성만 사용해서 깔끔하게 객체생성 가능
    2. 코드 수정 시, 다른 부분에 영향을 주지 않는다.


<br>

# ⭐ 흐름식 빌더

**`🔻 MyPerson.h`**

```cpp
#pragma once
#include <string>

class MyPerson
{
private:
	std::string Name{};
	int Age = 0;
	int Height = 0;
	int Weight = 0;
	std::string Address{};
public:
	MyPerson& SetName(const std::string& _Name);
	MyPerson& SetAge(int _Age);
	MyPerson& SetHeight(int _Height);
	MyPerson& SetWeight(int _Weight);
	MyPerson& SetAddress(const std::string& _Address);
};
```

**`🔻 MyPerson.cpp`**

```cpp
#include "MyPerson.h"

MyPerson& MyPerson::SetName(const std::string& _Name)
{
	Name = _Name;
	return *this;
}

MyPerson& MyPerson::SetAge(int _Age)
{
	Age = _Age;
	return *this;
}

MyPerson& MyPerson::SetHeight(int _Height)
{
	Height = _Height;
	return *this;
}

MyPerson& MyPerson::SetWeight(int _Weight)
{
	Weight = _Weight;
	return *this;
}

MyPerson& MyPerson::SetAddress(const std::string& _Address)
{
	Address = _Address;
	return *this;
}
```

- **`Setter`** 에서 **`자기자신의 참조`** 를 반환한다

    > MyPerson& 를 반환형으로 설정하고,  
    > *this 를 return 한다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPerson.h"

int main()
{
	MyPerson Person1;
	Person1.SetName("재석")
		.SetAge(13)
		.SetHeight(170)
		.SetWeight(60)
		.SetAddress("MBC");

	MyPerson Person2;
	Person2.SetName("명수")
		.SetAge(20)
		.SetHeight(182)
		.SetWeight(78)
		.SetAddress("MBC");

	MyPerson Person3;
	Person3.SetName("홍철")
		.SetAge(18)
		.SetHeight(153)
		.SetWeight(44)
		.SetAddress("MBC");

	MyPerson NewPerson1;
	NewPerson1.SetHeight(173)
		.SetWeight(77);

	MyPerson NewPerson2;
	NewPerson2.SetName("뱀파이어")
		.SetHeight(190)
		.SetWeight(85)
		.SetAddress("유럽 어느 산골속 성");

	return 0;
}
```

- **`Setter`** 를 이어서 사용할 수 있다.

    > 앞의 Setter 함수에서 자기자신의 참조를 반환하기 때문에,  
    > 이어서 Setter 를 사용할 수 있다.

<br>

# ⭐ 조금 더 깔끔하게

**`🔻 MyPerson.h`**

```cpp
#pragma once
#include <string>

class MyPersonBuilder; //전방선언
class MyPerson
{
	friend class MyPersonBuilder;	//빌더클래스를 친구클래스로 선언
private:
	std::string Name{};
	int Age = 0;
	int Height = 0;
	int Weight = 0;
	std::string Address{};

	MyPerson() = default;	//생성자를 숨겨준다.
public:
	static MyPersonBuilder Build();
};

class MyPersonBuilder
{
private:
	MyPerson Person{};	//친구클래스이기때문에 private 생성자에 접근 가능
public:
	MyPersonBuilder& SetName(const std::string& _Name);
	MyPersonBuilder& SetAge(int _Age);
	MyPersonBuilder& SetHeight(int _Height);
	MyPersonBuilder& SetWeight(int _Weight);
	MyPersonBuilder& SetAddress(const std::string& _Address);

	operator MyPerson() const	//형변환 operator
	{
		return Person;
	}
};
```

**`🔻 MyPerson.cpp`**

```cpp
#include "MyPerson.h"

//MyPerson ↓
MyPersonBuilder MyPerson::Build()
{
	return MyPersonBuilder();
}

//MyPersonBuilder ↓
MyPersonBuilder& MyPersonBuilder::SetName(const std::string& _Name)
{
	Person.Name = _Name;
	return *this;
}

MyPersonBuilder& MyPersonBuilder::SetAge(int _Age)
{
	Person.Age = _Age;
	return *this;
}

MyPersonBuilder& MyPersonBuilder::SetHeight(int _Height)
{
	Person.Height = _Height;
	return *this;
}

MyPersonBuilder& MyPersonBuilder::SetWeight(int _Weight)
{
	Person.Weight = _Weight;
	return *this;
}

MyPersonBuilder& MyPersonBuilder::SetAddress(const std::string& _Address)
{
	Person.Address = _Address;
	return *this;
}
```

1. 빌더를 다른 파일로 빼던지, 아래로 빼준다.
    > MyPerson(본체)와 MyPersonBuilder(빌더)가 각자 본연의 기능을 하게 하여 깔끔하게 만들어준다.
    >
    > `Setter` 또한 `빌더`의 역할이므로 옮겨준다.
1. `전방선언`을 하여 MyPerson에게 MyPersonBuilder를 알려준다.
    > Build 메서드 사용을 위해 알려준다.
1. `friend class`로 선언하여 빌더에서 본체의 private에 접근가능하게 해준다.
    > 빌더에 Person(본체)를 선언하면,  
    > 초기화를 위해 숨겨진 생성자에 접근할 수 있어야한다.  
    >
    > 그를 위해 friend class를 선언해준다.
1. 빌더를 본체로 `형변환해주는 operator`를 만들어준다.
    > 마지막에 완성된 Person(본체)를 건내주기위해 만들어준다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPerson.h"

int main()
{
	MyPerson Person1 = MyPerson::Build()
		.SetName("재석")
		.SetAge(13)
		.SetHeight(170)
		.SetWeight(60)
		.SetAddress("MBC");

	MyPerson Person2 = MyPerson::Build()
		.SetName("명수")
		.SetAge(20)
		.SetHeight(182)
		.SetWeight(78)
		.SetAddress("MBC");

	MyPerson Person3 = MyPerson::Build()
		.SetName("홍철")
		.SetAge(18)
		.SetHeight(153)
		.SetWeight(44)
		.SetAddress("MBC");

	MyPerson NewPerson1 = MyPerson::Build()
		.SetHeight(173)
		.SetWeight(77);

	MyPerson NewPerson2 = MyPerson::Build()
		.SetName("뱀파이어")
		.SetHeight(190)
		.SetWeight(85)
		.SetAddress("유럽 어느 산골속 성");

	return 0;
}
```

<<<<<<< HEAD
- 확실히 더 깔끔해졌다!
    > [흐름식 빌더](#⭐-흐름식-빌더)의 객체 생성부분과 비교해보면 확실하게 깔끔하다.


=======
    > [흐름식 빌더](#⭐-흐름식-빌더)의 객체 생성부분과 비교해보면 확실하게 깔끔하다.
>>>>>>> eae7cef5f379da9d8313a7fecc5b867d2e0abf88
