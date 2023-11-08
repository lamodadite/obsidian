## 시간복잡도
- 평균, 최악, 최선 모두 O(n)
- 빠르다는 정렬 알고리즘인 [[Quick Sort]] , [[Heap Sort]], [[Merge Sort]] 의 평균 시간복잡도가 O(nlongn)인 것에 비하면 엄청나게 빠름
## 장점
- 시간 복잡도면에서 원탑
- 데이터끼리 직접 비교하지 않음 -> 빠름
- 쓸 수 있는 상황이라면 가장 효율적인 알고리즘
## 단점
- 데이터의 범위가 크면 사용할 수 없음
- 음수인 경우에도 사용 불가
- 데이터의 범위만큼의 배열을 새로 만들어야 함 -> 공간효율성 낮음
- 실생활에서는 거의 쓸 수 없음
## 정렬 로직
- 데이터의 값이 몇번 나왔는지 세주는 것.
- 정렬할 데이터가 담긴 배열이 있고, 데이터의 범위와 같은 크기의 새로운 빈 배열(counting)을 생성
- 해당 배열을 순회하며 각 값을 index로 카운팅 배열의 값을 1 증가시킴
![[Pasted image 20231107204328.png]]
- 새로운 배열의 각 값을 누적합으로 변환
![[Pasted image 20231107204417.png]]
- 현재 카운팅 배열의 각 값은 (시작점 - 1)을 의미
- 즉, 해당 값에 대응되는 위치에 배정하면 됨
- 안정적으로 정렬하기 위해서는 배열의 마지막 인덱스부터 순회하는것이 좋음
![[Pasted image 20231108100610.png]]
## 코드
~~~c
int *counting_sort(int *list, int size)
{
int *count_arr;
int *result;
int value;

count_arr = (int *)malloc(100 * sizeof(int));
result = (int *)malloc(size * sizeof(int));

// list의 값을 인덱스로 count 배열의 값을 증가
for (int i = 0; i < size; i++)
	count_arr[list[i]]++;
// count 배열 누적합
for (int i = 1; i < 100; i++)
	count_arr[i] = count_arr[i - 1] + count_arr[i];
// count 배열의 값 - 1을 인덱스로 list에 값 삽입
for (int i = size - 1; i >= 0; i--)
{
	value = list[i];
	count_arr[value]--;
	result[count_arr[value]] = value;
}
return (result);
}
~~~
