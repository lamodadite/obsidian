## 시간복잡도
최선 : nlogn
평균 : nlogn
최악 : nlogn
## 장점
- 항상 두 부분 리스트로 쪼개어 들어가기 때문에 어떤 상황에서도 O(nlogn)을 보장
- 안정정렬임
- 성능에 비해 구현이 비교적 쉬움
## 단점
- 정렬과정에서 추가적은 보조 배열 공간을 사용하기 때문에 메모리 사용량이 많음
- 보조 배열에서 원본배열로 복사하는 과정은 시간을 많이 소비하기 때문에 데이터가 많을경우 상대적으로 시간이 많이 소요될 수 있음

## 정렬 로직
1. 주어진 리스트를 절반으로 분할하여 부분리스트로 나눔 (Divide : 분할)
2. 해당 부분리스트의 길이가 1이 아니라면 1번 과정을 되풀이
3. 인접한 부분리스트끼리 정렬하여 합침 (Conqure : 정복)
![[Pasted image 20231108221512.png]]
- 합치는 과정에서는 정렬 알고리즘이 따로 필요 없음. 각 원소들을 순차적으로 비교만 하면 됨
	- 각각의 리스트는 이미 정렬된 상태이기 때문
	- ![[Pasted image 20231108221810.png]]
	- ![[Pasted image 20231108222858.png]]
## 코드
~~~C
void merge(int *list, int *sorted, int lo, int mid, int hi)
{
	int l = lo;
	int r = mid + 1;
	int idx = lo;
	
	while(l <= mid && r <= hi)
	{
		if(list[l] <= list[r])
			sorted[idx++] = list[l++];
		else
			sorted[idx++] = list[r++];
	}
	if (l > mid)
	{
		while(r <= hi)
			sorted[idx++] = list[r++];
	}
	else
	{
		while(l <= mid)
			sorted[idx++] = list[l++];
	}
	for (int i = lo; i <= hi; i++)
	list[i] = sorted[i];
}

  

void merge_sort(int *list, int *sorted, int lo, int hi)

{
	if (lo == hi)
		return ;
	int mid = (lo + hi) / 2;
	merge_sort(list, sorted, lo, mid);
	merge_sort(list, sorted, mid + 1, hi);
	merge(list, sorted, lo, mid, hi);
}
~~~
