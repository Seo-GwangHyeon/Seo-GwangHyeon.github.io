---
layout: post
title:  "적녹색약"
date:   2019-01-22 23:01:53 +0900
categories: [codetest]
---

[문제링크]

지난 번에 풀었던 [단지문제]나 [안전영역] 문제와 비숫했다.

결국 같은 속성을 방문하여 방문 여부를 체크하고
그 갯수를 세거나 영역의 갯수를 세는 문제 이다.

코드를 줄이기위해 2가지 경우를 하나의 함수로 작성했다.

1. 색약이 아닌 경우, 같은 색만 방문하면된다.
2. 색약인 경우, 적색과 녹색이 인접한 경우에는 같은 색으로 봐야한다.
	1. 시작 색이 적색이고, 새로 탐색한 색이 녹색인 경우
	2. 시작 색이 녹색이고, 새로 탐색한 색이 적색인 경우
	와 같이 2가지 경우를 추가했다.

처음에는 어떻게 같은 색으로 인지하는지 몰라서 헤매기도 했다.

풀이 시간 : 23분

~~~c++

#include <iostream>
#include <queue>
using namespace std;


int n;
int visit[100][100] = { 0, };
char map[100][100]= { 0, };
int dx[] = {1,0,-1,0};
int dy[] = {0,1,0,-1};

//bfs 사용할 것

int bfs(int a,int b,int eye) //색약이 아닌사람
{// eye 0 색약 X
// eye 1  색약 O
	queue< pair<int, int> > q;
	char state= map[a][b];
	q.push(make_pair(a, b));//처음에 0,0을 넣는다.
	visit[a][b] = 1;

	while (!q.empty())
	{
		auto t = q.front();
		q.pop();
		int y = t.first;
		int x = t.second;
		for(int i=0;i<4;++i)
		{
			int ny = y + dy[i];
			int nx = x + dx[i];
			if (ny<0 || ny>n - 1 || nx <0 || nx>n - 1)
			{
				continue;
			}

			if (eye==0 && map[ny][nx] == state && !visit[ny][nx])
			{
				q.push(make_pair(ny, nx));
				visit[ny][nx] = 1;
			}
			if (eye == 1 && !visit[ny][nx] &&( map[ny][nx] == state
				|| (state=='R' && map[ny][nx] == 'G')
				|| (state == 'G' && map[ny][nx] == 'R'))   )
			{
				q.push(make_pair(ny, nx));
				visit[ny][nx] = 1;
				state = map[ny][nx];
			}
		}
	}
	return 1;
}



int main()
{
	cin >> n;
	for (int i = 0;i < n;++i)
	{
		for (int j = 0;j < n;++j)
		{
			cin >> map[i][j];
		}
	}
	int sum1 = 0;
	for (int i = 0;i < n;++i)
	{
		for (int j = 0;j < n;++j)
		{
			if (!visit[i][j])
			{
				sum1+=bfs(i, j, 0);
			}
		}
	}

	for (int i = 0;i < n;++i)
		for (int j = 0;j < n;++j)
			visit[i][j] = 0;

	int sum2 = 0;
	for (int i = 0;i < n;++i)
	{
		for (int j = 0;j < n;++j)
		{
			if (!visit[i][j])
			{
				sum2 += bfs(i, j, 1);
			}
		}
	}
	cout << sum1 << " " << sum2;
}
~~~

[문제링크]: https://www.acmicpc.net/problem/10026
[단지문제]: https://www.acmicpc.net/problem/2667
[안전영역]: https://www.acmicpc.net/problem/2468
