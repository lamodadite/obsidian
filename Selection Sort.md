##  시간복잡도
- 최선, 최악, 평균 모두 O(N^2)
- 거품정렬 다음으로 느린 정렬 알고리즘
## 장점
- 추가적인 메모리 소모가 (거의) 없음
- 난이도 낮음
## 단점
- 매우 느림 (사실상 사용 불가)
- 불안정 정렬
## 정렬 로직
1. 주어진 리스트에서 최솟값을 찾음
2. 최솟값을 맨 앞자리와 swap
3. 맨 앞자리를 제외한 나머지 값들 중 최솟값을 찾아 위와 같이 반복
![[Pasted image 20231108125611.png]]
## 코드
~~~C
int *selection_sort(int *list, int size)
{
	int min;
	int min_idx;
	// list에서 최솟값을 찾아 맨 앞자리와 swap -> 반복
	for (int i = 0; i < size - 1; i++)
	{
		min = INT_MAX;
		min_idx = 0;
		for (int j = i; j < size; j++)
		{
			if (list[j] < min)
			{
				min = list[j];
				min_idx = j;
			}
		}
		swap(list, i, min_idx);
	}
	return (list);

}
~~~
