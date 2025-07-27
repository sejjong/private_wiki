# W3 팀 활동 요구사항

# **W3M2b 팀 활동 요구사항**

4개의 xml files들을 살펴 보고 각 화일의 셋팅 중에 중요하거나 유용하다고 생각되는 것들을 각자 골라서 용법을 파악해 보세요. 그런 다음 팀과 함께 토의한 다음, 위키에 정리합시다.

### **core-site.xml**

- Change fs.defaultFS to hdfs://namenode:9000. 주환
    - Hadoop에서 기본으로 사용하는 파일 시스템의 URI를 지정. 대부분의 hdfs 작업( -ls, -put 등)은 이 경로를 기준으로 실행되며, 클러스터 내의 **NameNode 주소와 포트**를 포함해야 정상 작동함
- Change hadoop.tmp.dir to /hadoop/tmp. 하연
    - mapreduce 작업 수행 중 로컬 디스크에 중간 데이터를 저장해야할 때 사용한다.
- Change io.file.buffer.size to 131072. 세종
    - io.file.buffer 는 데이터 셋을 보고 결정하는 것이 좋다, 버퍼 크기를 크게 잡으면 속도는 빠를 수 있지만 job의 수가많아진다면 그만큼 잡아먹은 메모리도 많을 것이고 너무 작게한다면 그것또한 많은 cpu, io호출을 할 것이기 때문이다.

### **hdfs-site.xml**

- Change dfs.replication to 2. 세종
    - 저장하고자 하는 block의 복제 개수를 결정한다, job을 처리하는 container와 가까이 사용하고자 하는 block 있을 수록 속도가 빠르기 때문에 최대한 같은 rack에서 데이터를 처리하기 위해 복제 개수는 default로 3을 지정하고 있다.
- Change dfs.blocksize to 134217728 (128 MB). 주환
    - HDFS에 저장되는 파일이 나뉘는 블록 크기를 설정
    - 블록 크기가 크면 파일당 블록 수가 줄어들어 **NameNode의 메타데이터 부하가 감소**하지만, **작은 파일이나 병렬성에는 불리**할 수 있음
- Change dfs.namenode.name.dir to /hadoop/dfs/name. 하연
    - namenode의 메타데이터가 저장되는 경로이다.

### **mapred-site.xml**

- Change mapreduce.framework.name to yarn. 하연
    - mapreduce 작업을 어떤 리소스 매니저 위에서 실행할지 결정한다.
- Change mapreduce.jobhistory.address to namenode:10020. 세종
    - mapreduce 작업을 하다보면 알수 없는 오류들이 발생하는데 이를 기록하는 서버와 포트번호이다, 추가적으로 ui 포트를 열어 ui로 간편하게 확인한다.
- Change mapreduce.task.io.sort.mb to 256. 주환
    - Map 작업의 출력 데이터를 디스크에 기록하기 전에 메모리에서 정렬하는 버퍼 크기(MB)를 설정합니다.
    - 이 값을 늘리면 **디스크 I/O를 줄이고 성능을 향상**시킬 수 있지만, **메모리 사용량이 증가**하므로 노드 자원과 병렬 작업 수를 고려해 조정해야 함

### **yarn-site.xml**

- Change yarn.resourcemanager.address to namenode:8032. 주환
    - YARN 클러스터의 ResourceManager가 수신하는 주소를 지정합니다.
    - ResourceManager는 각 NodeManager로부터 리소스를 관리하고, Job 실행을 스케줄링합니다.
    - 모든 YARN 작업은 이 주소로 리소스 요청을 보내게 됩니다.
- Change yarn.nodemanager.resource.memory-mb to 8192. 하연
    - nodemanager가 전체 컨테이너에 할당할 수 있는 총 메모리 크기이다.
- Change yarn.scheduler.minimum-allocation-mb to 1024. 세종
    - resourcemanager가 허용하는 가장 작은 컨테이너의 메모리 크기이다. 애플리케이션이 이 값보다 작은 메모리를 가진 컨테이너를 요청하면 이 값으로 변경된 컨테이너를 할당한다

# W3M4 **팀 활동 요구사항**

'predefined keywords'가 아닌 다른 방법으로 나누고자 한다면 어떤 방법이 있을까요? 팀과 함께 논의해서 다른 방법을 시도해 보세요.

- 트윗 데이터를 predefined keywords로 나눈다면 부정 단어가 함께 있을 때(예를 들어 not good) 텍스트의 감성을 잘못 분류할 수 있다.
- 다른 방식으로는 단어를 긍정/부정으로 분류하고 감성 점수를 매기는 사전 기반 방식을 사용할 수 있다. 텍스트 단어마다의 감성 점수를 총합해서 텍스트 감성을 분류할 수 있다.
- 또한 머신러닝 기법도 사용할 수 있다. 단어를 벡터 값으로 나타내는 임베딩 처리를 통해서 유사한 의미의 단어는 벡터 공간 상에 가깝게 위치하도록 한다. 임베딩된 텍스트 데이터셋을 머신러닝 모델에 학습하여 감성 분류할 수 있다. 다만 이 방법은 사전에 텍스트가 레이블된 데이터셋이 존재해야 한다는 단점이 있다.
- 기존에는 predefined keywords를 기반으로 텍스트를 분류했지만, 이 방식은 유연성이 떨어지고 새로운 유형의 데이터에 대응하기 어렵기 때문에 TextBlob을 활용하여 감정 분석 및 명사 추출을 통해 텍스트를 동적으로 분류하는 대안을 생각했음
