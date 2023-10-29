## 파티션이란?
- 디스크를 더 작은 논리적인 단위로 분할하는 것
- 분할된 각각을 파티션이라고 함
- 파티션을 사용하면 데이터를 서로 다른 논리적 공간에 저장하여 관리를 용이하게 할 수 있음
- 각 파티션은 별도의 파일 시스템을 가질 수 있음
## LVM
- Logical Volume Manager
- 여러 물리적 디스크를 하나의 논리적 공간으로 결합하고, 이 공간을 조절하고 관리
- 예로 2TB 디스크 2개를 합친 후에 다시 1TB와 3TB로 나눠서 사용 가능
#### Physical Volume(물리 볼륨)
- /dev/sda1 과 같은 파티션
#### Volume Group(볼륨 그룹)
- 물리 볼륨을 합쳐서 1개의 물리 그룹으로 만든 것
#### Logical Volume(논리 볼륨)
- 볼륨 그룹을 1개 이상으로 나눠서 논리 그룹으로 나눈 것
[![LVM 논리 볼륨 구성 요소](https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-9-Configuring_and_managing_logical_volumes-ko-KR/images/31bd96635c4120abe3e771a423f61cd6/basic-lvm-volume-components.png)](https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-9-Configuring_and_managing_logical_volumes-ko-KR/images/31bd96635c4120abe3e771a423f61cd6/basic-lvm-volume-components.png "LVM 논리 볼륨 구성 요소")
## LVM의 장점
- 유연하게 논리 볼륨을 확장하거나 줄일 수 있음
	- 예를 들어 30GB 논리 볼륨에 40GB짜리 프로그램을 설치해야 한다면, 10GB만큼의 논리 볼륨을 확장하면 됨
	- 물리 디스크라면 40GB의 새로운 디스크가 필요한 작업
- 백업을 위해 스냅샷을 찍거나 실제 데이터에 영향을 주지 않고 변경의 영향을 테스트 할 수 있음