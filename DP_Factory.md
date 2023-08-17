# 🌏 생성 - 팩토리 (Factory)
**아래 예시는 전적으로 [모던 C++ 디자인패턴](http://www.apress.com/9781484236024) 책을 참조하였습니다.**
<!-- TOC tocDepth:2..3 chapterDepth:2..6 -->

- ⭐ [상황 설정](#-상황-설정)
- ⭐ [인자를 추가하여 해결!](#-인자를-추가하여-해결)
- ⭐ [팩토리 메서드](#-팩토리-메서드)
- ⭐ [팩토리](#-팩토리)
- ⭐ [내부 팩토리](#-내부-팩토리)
- ⭐ [실험 1](#-실험-1)

<!-- /TOC -->

## ⭐ 상황 설정

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>//cosf(), sinf()를 위해 include

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;
public:
	Point(float _X, float _Y)    //데카르트좌표 생성자
		: X{ _X }, Y{ _Y }
	{}
	Point(float _R, float _Degree)    //극좌표 생성자
	{
        //아래는 극좌표 계산 코드로 중요한 부분은 아니다.
		const float PI = 3.141592f;
		const float Radius = _Degree * PI / 180.0f;

		X = _R * cosf(Radius);
		Y = _R * sinf(Radius);
	}
};
```
- 좌표(Point) 클래스

    > X, Y를 설정한다.
    > 
    > 데카르트, 극 좌표계를 위한 두개의 생성자를 만든다.  
    > 그러나, 두 생성자의 인자 타입이 같아 오류가 발생한다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian{ 2.0f, 6.0f };	//데카르트 좌표
	Point Polar{ 5.0f, 30.0f };		//극 좌표

	return 0;
}
```

- 컴파일에 `실패`한다.

<br>

## ⭐ 인자를 추가하여 해결!

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>

enum class PointType
{
	Cartesian,
	Polar,
};

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;
public:
	Point(float _A, float _B, PointType _Type = PointType::Cartesian)
	{
		if (_Type == PointType::Cartesian)
		{
			X = _A;
			Y = _B;
		}
		else
		{
			const float PI = 3.141592f;
			const float Radius = _B * PI / 180.0f;

			X = _A * cosf(Radius);
			Y = _A * sinf(Radius);
		}
	}
};
```
- `인자`를 하나 추가하고, `생성자`를 하나로 합친다!

    > std::string, enum class, 등등... 인자를 추가해준다.  
    > 위에선 enum class를 추가했다.
    > 
    > 추가된 인자에 따라 데카르트좌표인지, 극좌표인지 결정한다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian{ 2.0f, 6.0f, PointType::Cartesian };//데카르트 좌표
	Point Polar{ 5.0f, 30.0f, PointType::Polar };//극 좌표

	return 0;
}
```

- 컴파일에 성공한다.
    > 그러나 좌푯값을 지정하는 변수명이 A, B로 변경되었다.  
    > 이것은 사용자가 보기에 직관적이지 못하다.

<br>

## ⭐ 팩토리 메서드

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;

	Point(float _X, float _Y)//생성자를 숨겨준다.
		: X{ _X }, Y{ _Y }
	{}
public:
	static Point Cartesian(float _X, float _Y)//데카르트좌표 팩토리메서드
	{
		return { _X, _Y };
	}
	static Point Polar(float _R, float _Degree)//극좌표 팩토리메서드
	{
		const float PI = 3.141592f;
		const float Radius = _Degree * PI / 180.0f;

		return { _R * cosf(Radius), _R * sinf(Radius) };
	}
};
```
- `생성자`를 숨기고, 각각의 `팩토리메서드`를 만든다.

    > 팩토리메서드 사용을 유도하기위해 생성자를 숨겨준다. 
    >  
    > static 멤버함수로 각각의 팩토리 메서드를 만들어준다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian = Point::Cartesian(3.0f, 4.0f);
	Point Polar = Point::Polar(5.0f, 90.0f);

	return 0;
}
```

- 아주 직관적이다!
    > 생성을 위한 함수들을 따로 클래스로 빼주면 좋을 것 같다!  
    > 바로 해보자,

<br>

## ⭐ 팩토리

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>

class PointFactory;//전방선언
class Point
{
	friend class PointFactory;//팩토리를 친구클래스로!
private:
	float X = 0.0f;
	float Y = 0.0f;

	Point(float _X, float _Y)
		: X{ _X }, Y{ _Y }
	{}
};

class PointFactory//팩토리메서드를 따로 빼서 팩토리클래스로 만든다!
{
public:
	static Point Cartesian(float _X, float _Y)
	{
		return { _X, _Y };
	}
	static Point Polar(float _R, float _Degree)
	{
		const float PI = 3.141592f;
		const float Radius = _Degree * PI / 180.0f;

		return { _R * cosf(Radius), _R * sinf(Radius) };
	}
};
```
- 전방선언, friend class, `팩토리 빼기`

    > 전방선언과 freind class는 [빌더패턴](DP_Builder.md)에서 다뤘으므로 넘어간다.
    >  
    > 팩토리 빼기는 단순히 안에 있던 팩토리메서드를 밖에서 클래스로 만들어준것이다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian = PointFactory::Cartesian(3.0f, 4.0f);
	Point Polar = PointFactory::Polar(5.0f, 90.0f);

	return 0;
}
```

- 클래스가 깔끔해서 좋지만, 애매하다...
    > 사용자 입장에선, PointFactory가 있다는 것을 모를 가능성이 높다!

<br>

## ⭐ 내부 팩토리

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;

	class PointFactory//단순히 밖에있던 팩토리클래스를 안으로 복붙했다.
	{
	public:
		static Point Cartesian(float _X, float _Y)
		{
			return { _X, _Y };
		}
		static Point Polar(float _R, float _Degree)
		{
			const float PI = 3.141592f;
			const float Radius = _Degree * PI / 180.0f;

			return { _R * cosf(Radius), _R * sinf(Radius) };
		}
	};

	Point(float _X, float _Y)
		: X{ _X }, Y{ _Y }
	{}
public:
	static PointFactory Factory;//팩토리 static 멤버변수
};
```
- `inner 클래스`로 바꿔주고, `정적멤버변수`를 하나 만들어준다.

    > 밖에 있던 클래스를 안에 복붙해준다.
    >  
    > 정적멤버변수로 선언한 이유는 굳이 Point 마다 Factory 멤버변수를 가지고 있을 필요가 없기 때문이다.  
    > 모든 Point가 공유하는 정적멤버변수로 만들어준다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian = Point::Factory.Cartesian(3.0f, 4.0f);
	Point Polar = Point::Factory.Polar(5.0f, 90.0f);

	return 0;
}
```

- 사용자 입장에서 추론가능하지만, 이번엔 클래스 내부가 복잡하다...
    > 이에대해 여러가지 실험을 해보자,

<br>

## ⭐ 실험 1

**`🔻 MyPoint.h`**

```cpp
#pragma once
#include <cmath>

class Point
{
	friend class PointFactory;//팩토리를 친구클래스로,
private:
	float X = 0.0f;
	float Y = 0.0f;

	Point(float _X, float _Y)
		: X{ _X }, Y{ _Y }
	{}
public:
	static PointFactory Factory;
};

class PointFactory//다시 밖으로 빼보자,
{
public:
	static Point Cartesian(float _X, float _Y)
	{
		return { _X, _Y };
	}
	static Point Polar(float _R, float _Degree)
	{
		const float PI = 3.141592f;
		const float Radius = _Degree * PI / 180.0f;

		return { _R * cosf(Radius), _R * sinf(Radius) };
	}
};
```
- `팩토리`를 다시 밖으로 빼고, `friend class` 선언

    > 펙토리를 그대로 다시 밖으로 뺀다.  
    >  
    > friend class 선언은 Factory에서 Point생성자에 접근하기 위함이다.
    >
    > static 멤버변수를 사용하기에 전방선언은 할 필요없다.

<br>

**`🔻 main.cpp`**

```cpp
#include "MyPoint.h"

int main()
{
	Point Cartesian = Point::Factory.Cartesian(3.0f, 4.0f);
	Point Polar = Point::Factory.Polar(5.0f, 90.0f);

	return 0;
}
```

- 잘 동작한다.

***
|⏮️ Prev : [빌더(Builder)](DP_Builder.md) |
|---------|
|⏭️ Next : [프로토타입(Prototype)](DP_Prototype.md)|
***
