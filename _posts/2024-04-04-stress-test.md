---
title: Stress 명령어를 이용한 부하 테스트
author: bluearin
date: 2024-04-04 16:10:00 +0900
categories: [Dev, Operation]
tags: [stress,test,performance]
---

## 부하 테스트
부하테스트는 서버나 시스템이 얼마나 많은 요청이나 작업을 처리할 수 있는지, 극한 상황에서의 안정성과 효율성을 평가하기 위한 방법입니다. 이를 통해 시스템의 최적화된 성능을 확보하고, 잠재적인 문제점을 사전에 파악하여 미래의 운영에 큰 도움을 줄 수 있습니다. 리눅스 환경에서는 'stress'라는 강력한 도구를 활용해 부하테스트를 쉽게 진행할 수 있습니다.

물론 좀 더 체계적인 부하 테스트를 진행하고자 한다면 JMeter나 nGrinder와 같은 툴을 사용하는 것이 효율적입니다. 그러나 단일 서버 혹은 vm에 부하를 주는 것은 stress 명령어로도 충분합니다. 이 글에서는 stress 명령어의 기본 원리와 사용 방법 등을 살펴보도록 하겠습니다.


## Stress 사용법
### Stress 설치하기 (데비안 계열 설치 예제)
``` bash
[root@localhost ~]# apt-get install stress
```

### CPU에 부하 주기 (지정한 코어를 100% 사용하도록 함)
``` bash
[root@localhost ~]# stress -c <코어수>
ex) stress -c 4
```

### Memory에 부하 주기 (메모리 로드를 위해 프로세스 갯수와, 사용할 메모리 크기를 설정)
``` bash
[root@localhost ~]# stress –vm <프로세스 수> –vm-bytes <사용할 크기>
ex)stress -vm 2 –vm-bytes 2048m
```

### Disk에 부하 주기 (하드 디스크 로드를 위한 프로세스 수와 테스트 파일의 크기 지정)
``` bash
[root@localhost ~]# stress –hdd <hdd 수> –hdd-bytes <사용할 크기>
ex) stress –hdd 3 -hdd-bytes 1024m
```