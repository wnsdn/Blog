# 🌏 생성 - 프로토타입 (Prototype)
<!-- TOC tocDepth:2..3 chapterDepth:2..6 -->

- [⭐ 상황 설정](#-상황-설정)
    - [⭐ 프로토타입](#-프로토타입)
- [⭐ 원의 이동](#-원의-이동)
    - [⭐ 프로토타입 - 원의 이동](#-프로토타입---원의-이동)

<!-- /TOC -->
## ⭐ 상황 설정

1. `데카르트 좌표계` 상의 `원`을 만들고 싶다.
1. `원 클래스`를 만들것이다.
1. `원 클래스`는 `좌표 클래스`와 `반지름`으로 구성된다.

**`🔻 Point_Circle.h`**

```cpp
#pragma once

class Point    //좌표 클래스
{
private:
	float X = 0.0f;    //X,Y로 구성됨
	float Y = 0.0f;
public:
	Point()    //일반생성자
		: X(0.0f), Y(0.0f)
	{}
	Point(float _X, float _Y)    //좌표 두개를 받는 생성자
		: X(_X), Y(_Y)
	{}
};

class Circle    //원 클래스
{
private:
	Point Coord{};    //좌표(중심점)
	float Radius = 0.0f;    //반지름
public:
	Circle(const Point& _Coord, float _Radius)    //좌표와 반지름을 받는 생성자
		: Coord(_Coord), Radius(_Radius)
	{}
};
```
- 좌표(Point) 클래스
    > X, Y 값으로 구성됨,  
    > 
    > 아무 인자도 없는 생성자와,  
    > X, Y 값을 받는 생성자가 있다.
- 원(Circle) 클래스
    > 좌표(중심점)과 반지름을 갖는다.  
    > 
    > 그 두 값을 받는 생성자가 있다.

<br>

**`🔻 main.cpp`**

```cpp
#include "Point_Circle.h"

int main()
{
	Circle C1{ Point{0, 0}, 5.0f };

	return 0;
}
```

- 임시객체(좌표)를 만들어 넣는 방식을 사용해봤다.
    > 다음은 프로토타입 방식을 사용해보자,

<br>

## ⭐ 프로토타입

**`🔻 Point_Circle.h`**

```cpp
#pragma once

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;
public:
	static Point Origin;    //static 멤버변수: 원점 프로토타입

	Point()
		: X(0.0f), Y(0.0f)
	{}
	Point(float _X, float _Y)
		: X(_X), Y(_Y)
	{}
};

...
```
**`🔻 Point_Circle.cpp`**
```cpp
#include "My.h"

Point Point::Origin{ 0.0f, 0.0f };    //static 멤버변수를 cpp에 정의해준다.
```
- `프로토타입` 만들기
    > 좌표 클래스에 static 멤버변수로 만들어준다.  
    > cpp에 정의도 해준다.

<br>

**`🔻 main.cpp`**

```cpp
#include "Point_Circle.h"

int main()
{
	Circle C1{ Point::Origin, 5.0f};

	return 0;
}
```

- 프로토타입을 넣어주는 방식을 사용해봤다.
    > 임시객체를 넣어주는 것과 길이상 차이가 없어보인다.  
    > 또, 이런식이면 생성자에서 처리해주는게 더 편해보인다.
    >
    > 하지만, 클래스가 좀 더 복잡하거나   
    > 자주 사용되는 의미있는 좌표라면  
    > 프로토타입은 유용하게 사용될것이다.

<br>

## ⭐ 원의 이동
이번엔 원에 좌표값을 더해 이동시켜보자.

**`🔻 Point_Circle.h`**

```cpp
#pragma once

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;
public:
	static Point Origin;

	void operator+=(const Point& _Dir)    //"좌표 += 좌표" 를 처리해준다.
	{
		X += _Dir.X;
		Y += _Dir.Y;
	}

	Point()
		: X(0.0f), Y(0.0f)
	{}
	Point(float _X, float _Y)
		: X(_X), Y(_Y)
	{}
};

class Circle
{
private:
	Point Coord{};
	float Radius = 0.0f;
public:
	void operator+=(const Point& _Dir)    //"원 += 좌표" 를 처리해준다.
	{
		Coord += _Dir;    //"좌표 += 좌표" 로 넘겨준다.
	}

	Circle(const Point& _Coord, float _Radius)
		: Coord(_Coord), Radius(_Radius)
	{}
};
```
- 원 이동시키기
    > 원에 좌표값을 더해준다.  
    > 원은 자신의 좌표값과 들어온 좌표값과의 계산으로 넘겨준다.
    >
    > 최종적으로 좌표클래스에서 계산을 처리한다.  
    > (과정을 줄이기 위해 좌표클래스의 좌표를 public 으로 바꾸고 원클래스에서 처리해도된다.)

<br>

**`🔻 main.cpp`**

```cpp
#include "Point_Circle.h"

int main()
{
	Circle C1{ Point::Origin, 5.0f};

    C1 += Point{ -1.0f, 0.0f };
    C1 += Point{ -1.0f, 0.0f };
    C1 += Point{ -1.0f, 0.0f };

	return 0;
}
```

- 왼쪽으로 세번 움직여보자.
    > 좌표를 임시객체로 생성해 원에 더해준다.  
    > -값이기에 더해줘도 왼쪽으로 움직이는 결과를 볼 수 있다.
    >
    > 초기값 `(0.0f, 0.0f)` → 이동후 `(-3.0f, 0.0f)`
    >
    > 뭔가 방향값도 프로토타입으로 만들면 편할것같다..!

<br>

## ⭐ 프로토타입 - 원의 이동

**`🔻 Point_Circle.h`**

```cpp
#pragma once

class Point
{
private:
	float X = 0.0f;
	float Y = 0.0f;
public:
	static Point Origin;
	static Point Left;    //좌표 좌
	static Point Right;   //좌표 우
	static Point Up;      //좌표 상
	static Point Down;    //좌표 하

	void operator+=(const Point& _Dir)
	{
		X += _Dir.X;
		Y += _Dir.Y;
	}

	Point()
		: X(0.0f), Y(0.0f)
	{}
	Point(float _X, float _Y)
		: X(_X), Y(_Y)
	{}
};

class Circle
{
private:
	Point Coord{};
	float Radius = 0.0f;
public:
	void operator+=(const Point& _Dir)
	{
		Coord += _Dir;
	}

	Circle(const Point& _Coord, float _Radius)
		: Coord(_Coord), Radius(_Radius)
	{}
};
```
**`🔻 Point_Circle.cpp`**

```cpp
#include "My.h"

Point Point::Origin{ 0.0f, 0.0f };
Point Point::Left{ -1.0f, 0.0f };    //정의 좌
Point Point::Right{ 1.0f, 0.0f };    //정의 우
Point Point::Up{ 0.0f, 1.0f };       //정의 상
Point Point::Down{ 0.0f, -1.0f };    //정의 하
```
- 방향값 프로토타입
    > 원점 프로토타입을 만들듯이 방향값도 만들어준다.

<br>

**`🔻 main.cpp`**

```cpp
#include "Point_Circle.h"

int main()
{
	Circle C1{ Point::Origin, 5.0f};

	C1 += Point::Left;
    C1 += Point::Left;
    C1 += Point::Left;
    C1 += Point::Right;
    C1 += Point::Up;
    C1 += Point::Up;

	return 0;
}
```

- 프로토타입을 이용해 움직여보자
    > 좌로 세번, 우로 한번, 위로 두번 움직인다.
    >
    > 초기값 `(0.0f, 0.0f)` → 이동후 `(-2.0f, 2.0f)`

위의 방식 말고도 아주 다양하고 유용한 사용방식이 있을것 같다.

***
|⏮️ Prev : [팩토리(Factory)](DP_Factory.md)|
|---------|
|⏭️ Next : 없음|
***
