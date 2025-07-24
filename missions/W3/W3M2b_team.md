## 팀 활동 요구사항
4개의 xml files들을 살펴 보고 각 화일의 셋팅 중에 중요하거나 유용하다고 생각되는 것들을 각자 골라서 용법을 파악해 보세요. 

그런 다음 팀과 함께 토의한 다음, 위키에 정리합시다.


## 나의 생각
fs.defaultFS
- namenode가 돌아가기 위한 필수!

dfs.namenode.name.dir
- namenode 의 메타데이터들이 저장되는 directory이기 때문에 hdfs의 데이터를 유지하기 위해서 경로를 알고있는것이 필수적으로 보인다

dfs.replication
- 수업에서 들었듯 replication이 많을 수록 Rack에서의 복사가 이루어질 확률이 올라가니, 속도관련해서 큰 영향을 미치기때문에 중요하다고 생각한다.

dfs.blocksize
- block size가 크다면 cpu 호출 회수가 줄어들고, 메모리 낭비는 커지는 현상이 나타나고
- block size가 작다면 cpu 호출 회수가 늘고, 메모리 낭비는 덜해지는 현상이 나타날 것이다
- 상황에 맞게 잘 선택하는 것이 중요
