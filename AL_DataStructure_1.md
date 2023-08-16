# 🌏 자료구조 - 1

✏️ 문제 - [백준 2605번: 줄 세우기](https://www.acmicpc.net/problem/2605)

```cpp
#include <iostream>

using namespace std;

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	short Student[100]{};//학생
	short Order[100]{};//뽑은번호 (만큼 앞으로 땡김)
	short Count{};//학생 수

	cin >> Count;//학생 수 입력받음

	for (short i = 0; i < Count; ++i)
	{
		Student[i] = i + 1;//학생 초기위치 세팅
		cin >> Order[i];//뽑은번호 세팅
	}

	for (short i = 1; i < Count; ++i)//i = 현재학생위치
	{
		short To = i - Order[i];//앞으로 땡길 위치(To) = 현재학생위치(i) - 해당학생이 뽑은번호(Order[i])
		for (short j = i; j > To; --j)//현재위치의 학생이 앞의 사람과 자리를 바꿔가며 To까지 감
		{
			if (j == 0)//내 위치가 0이면(앞 사람이 없으면) 탈출
			{
				break;
			}

			swap(Student[j], Student[j - 1]);//앞 사람과 자리를 바꿈
		}
	}

	for (short i = 0; i < Count; ++i)//출력
	{
		cout << Student[i] << ' ';
	}

	return 0;
}
```

***
|⏮️ Prev : 없음 |
|---------|
|⏭️ Next : 없음|
***
