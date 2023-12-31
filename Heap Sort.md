## 시간복잡도
최선 : nlogn
평균 : nlogn
최악 : nlogn
## 장점
- 최악의 경우에도 O(nlogn)으로 유지됨
- 힙의 특징상 부분 정렬을 할 때 효과가 좋음
## 단점
- 일반적인 O(nlogn) 정렬 알고리즘에 비해 성능은 약간 떨어짐
- 한 번 최대 힙을 만들면서 불안정 정렬 상태에서 최댓값만 갖고 정렬하기 때문에 안정정렬이 아님
## 정렬 로직
- 최대 힙을 먼저 구성하여 리스트의 원소들을 넣어 root 노드가 항상 큰 값을 갖게 만듦
- root 노드를 하나씩 삭제하며 뒤에서부터 배치하며 정렬
- 별도의 메모리 공간을 사용하지 않고 최대 힙을 만들기 위해 가장 작은 서브트리부터 최대 힙을 만족하도록 순차적으로 진행
![[Pasted image 20231109103921.png]]
- 위와 같이 힙을 만드는 과정을 'heapify'라고 함
- 이렇게 힙을 만든 후, root 원소를 배열의 뒤로 보내면서 뒤에 채운 원소를 제외한 나머지 배열 원소들을 다시 최대 힙을 만족하도록 재구성
![[Pasted image 20231109104208.png]]
![[Pasted image 20231109104215.png]]
## 코드
