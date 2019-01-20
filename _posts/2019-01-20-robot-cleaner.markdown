---
layout: post
title:  "로봇 청소기"
date:   2019-01-20 22:34:53 +0900
categories: [codetest]
---

[문제링크]

정답률도 높은 편이고 꽤 쉬운 문제인 것 같다.

처음에 청소를 전진 후진시 다하는 줄 알고 애먹었지만. 

디버깅을 단계별로 해보며 문제를 해결했다.

풀이 시간 : 1시간 11분

{% highlight ruby %}
#include <iostream>
using namespace std;

int n, m;
int y, x, d;
int map[50][50];
bool visited[50][50];
int sum = 0;

void turn_left();
int last_direct(int d, int f_b);
bool dirty_nowall(int f_b);
void go(int f_b);
void clean()
{
	map[y][x] = 2;
	sum++;
	/*cout << "direct: " << d << endl;
	for (int i = 0;i < m;++i)
	{
		for (int j = 0;j < n;++j)
		{
			cout << map[i][j] << " ";
		}
		cout << endl;
	}
	cout << endl;*/
}
int robot()
{
	clean();//현재위치 청소
	while (1)
	{
		
		for (int i = 0;i < 4;i++)
		{
			turn_left();//1. 왼쪽으로 돈다.
			if (dirty_nowall(1))//앞쪽에 벽이 아니고 더러운지 확인
			{
				go(1);//앞쪽으로 전진
				clean();
				i = -1;
			}
		}
		if (dirty_nowall(2))//후진 가능하면
		{
			go(2);
			continue;
		}
		else
			break;
	}
	return sum;
}
void turn_left()
{
	if (d == 0)
		d = 3;
	else
		d = d - 1;
}
int last_direct(int d,int f_b)
{
	int result = d*10+f_b;
	if (result == 1 || result == 22)
	{
		return 0;
	}
	if (result == 11 || result == 32)
	{
		return 1;
	}
	if (result == 21 || result == 2)
	{
		return 2;
	}
	if (result == 31 || result == 12)
	{
		return 3;
	}
	else
	{
		cout << "error" << endl;
		return -1000000;
	}
}
bool dirty_nowall(int f_b)
{//전진 1 후진 2
	int real=last_direct(d, f_b);

	if (real == 0)
	{//0 북쪽 
		if (y - 1 < 0 )
			return false;
		if (f_b == 2&& map[y - 1][x] != 1)
			return true;
		if (map[y - 1][x] == 0)//dirty
			return true;
		else
			return false;
	}
	else if (real == 1)
	{//1 동쪽
		if (x + 1 > n-1)
			return false;
		if (f_b == 2 && map[y][x+1] != 1)
			return true;
		if (map[y][x+1] == 0)//dirty
			return true;
		else
			return false;
	}
	else if (real == 2)
	{//2 남쪽
		if (y + 1 > m-1)
			return false;
		if (f_b == 2 && map[y + 1][x] != 1)
			return true;
		if (map[y + 1][x] == 0)//dirty
			return true;
		else
			return false;
	}
	else if (real == 3)
	{//3 서쪽
		if (x - 1 < 0)
			return false;
		if (f_b == 2 && map[y][x-1] != 1)
			return true;
		if (map[y][x-1] == 0)//dirty
			return true;
		else
			return false;
	}
}
void go(int f_b)//전진
{//1 전진
// 2 후진
	if (f_b == 2)
		f_b = -1;
	if (d == 0)
	{//0 북쪽 
		y = y + (-1)*f_b;
	}
	else if (d == 1)
	{//1 동쪽
		x = x + (1)*f_b;
	}
	else if (d == 2)
	{//2 남쪽
		y = y + (1)*f_b;
	}
	else if (d == 3)
	{//3 서쪽
		x = x + (-1)*f_b;
	}
	
}

int main()
{
	cin >> m >> n;
	
	
	cin >> y >> x >> d;
	for (int i = 0;i < m;++i)
	{
		for (int j = 0;j < n;++j)
		{
			cin >> map[i][j];
		}
	}

	cout << robot() << endl;
}
{% endhighlight %}

[문제링크]: https://www.acmicpc.net/problem/14503