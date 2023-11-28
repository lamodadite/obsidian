## 시간복잡도
- 최선 : O(nlongn)
- 평균 : O(nlongn)
- 최악 : O(n^2)
- 대부분의 정렬들에 비해 속도가 빠름
## 장점
- 평균 시간 복잡도가 nlogn으로 안정적이며, 다른 nlogn 알고리즘에 비해 속도가 매우 빠름
	- 같은 nlogn인 merge sort에 비해 2~3배나 빠름
- 추가 메모리가 필요 없음
- 재귀 호출 스택프레임에 의한 공간복잡도도 logn으로 메모리를 적게 소비
## 단점
- 특정 조건하에 성능이 급격히 하락
- 재귀를 사용하지 못하는 환경일 경우 구현이 매우 복잡해짐
## 정렬 로직
1. 임의의 피벗을 선택 (왼쪽, 중간, 오른쪽 등)
2. 피벗을 기준으로 양쪽에서 피벗보다 큰 값, 혹은 작은 값을 찾음
3. 양 방향에서 찾은 두 원소를 교환
4. 왼쪽에서 탐색하는 위치와 오른쪽에서 탐색하는 위치가 엇갈리지 않는 동안 2번으로 돌아가 위 과정 반복
5. 엇갈린 기점을 기준으로 두 개의 부분리스트로 나누어 1번으로 돌아가 해당 부분리스트의 길이가 1이 아닐 대까지 1번 과정 반복 (Divide : 분할)
6. 인접한 부분리스트끼리 합침 (Conqure : 정복)
- 왼쪽 피벗 선택 방식
- ![[Pasted image 20231108211115.png]]

## 코드
~~~C
int *quick_sort(int *list, int start, int end)
{
	int pivot;
	int hi;
	int lo;
	
	if (start >= end)
		return (list);
	pivot = list[start];
	lo = start;
	hi = end;
	while (lo < hi)
	{
		while (list[hi] > pivot && lo < hi)
			hi--;
		while (list[lo] <= pivot && lo < hi)
			lo++;
		swap(list, hi, lo);
	}
	swap(list, hi, start);
	quick_sort(list, start, lo - 1);
	quick_sort(list, lo + 1, end);
	return (list);
}
~~~
	

## nlogn이 보장되는 이유
![[Pasted image 20231108202702.png]]
- 만약 이상적으로 피벗이 중앙에 위치되어 리스트를 절반으로 나누면, 이진트리 형태가 됨
	- N개 노드에 대한 이진트리의 높이는 logn
- 피벗을 잡고 피벗보다 큰 요소와 작은 요소를 탐색하며 만족하지 못하는 경우 swap을 하는 과정은 현재 리스트의 요소들을 탐색하기 때문에 O(N)
- O(N)의 비교작업을 트리의 높이인 logn - 1 번 수행하므로  O(N) * O(logn)  -> O(nlogn)
## 최악의 경우 n^2인 이유
- 정렬된 상태의 배열을 정렬하고자 할 때
- 왼쪽을 기준으로 피벗을 잡는다고 할 때, 만약 오름차순으로 정렬되어 있다면?
	- 왼쪽의 부분 리스트는 없고, N - 1 개의 요소를 갖는 오른쪽 부분리스트만 생성됨
	- 이를 반복적으로 재귀호출하면
	- ![[Pasted image 20231108204631.png]]
	- 공간복잡도까지 O(N)이 되어버림
- 중간 피벗이 선호되는 이유도 이와 같은 이유 때문
	- 중간을 피벗으로 잡으면 거의 정렬된 배열이더라도 거의 중간의 위치에서 왼쪽과 오른쪽 리스트가 균형에 가까운 트리를 얻어낼 수 있음
## Quick Sort가 유독 빠른 이유
- '멀리 떨어진 요소와 교환'되는 정렬 방식이기 때문
- Quick Sort는 양 끝에서 피벗을 기준으로 피벗보다 작은 값을 갖는 위치에 있어야 할 원소가 피벗보다 크거나, 그 반대인 경우 서로 원소를 교환
- 이러한 방식은 해당 원소가 최종적으로 있어야 할 위치에 거의 근접하게 위치하도록 하기 때문에 효율이 좋음
- 다만, 이런 방식의 정렬은 따로 처리를 해주지 않는 한 안정정렬은 안된다고 봐야함