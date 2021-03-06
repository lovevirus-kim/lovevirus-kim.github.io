---
layout: post
title: "리눅스 시스템 상태 확인"
description: "리눅스 시스템 상태 확인하는 간단한 명령어."
date: 2018-07-03
tags: [리눅스, LINUX, SystemMonitoring]
comments: true
share: true
---

# top

* 시스템의 상태를 전반적으로 가장 빠르게 파악할 수 있는 명령어
* VIRT : 프로세스에 할당된 가상 메모리 전체 크기
* RES : 그중 실제로 메모리에 올려서 사용하고 있는 물리 메모리의 크기
* SHR : 다른 프로세스와 공유하고 있는 메모리의 크기
* VIRT는 실제로는 할당되지 않은 가상의 공간이기 때문에 해당 값이 크다고 문제가 되진 않는다.
* 실제 사용하고 있는 메모리는 RES 영역이기 때문에 메모리 점유율이 높은 프로세스를 찾기 위해서는 RES 영역이 높은 프로세스를 찾아야 한다.
* 프로세스 상태 중
  1. D :  I/O 대기중인 프로세스
  2. R : 실제 싱행 중인 프로세스
  3. S : sleep 상태의 프로세스
  4. T : tracing 중인 프로세스
  5. Z : 좀비 상태의 프로세스

# load average

* TOP 명령어을 통해 확인한 프로세스 상태 중 R과 D 상태에 있는 프로세스 개수의 1분, 5분, 15분마다의 평균 값
* 얼마나 많은 프로세스가 실행 중 혹은 실행 대기 중이냐를 의미하는 수치
* 수치가 높다면 많은 수의 프로세스가 실행 중이거나 I/O등ㅇ을 처리하기 위한 대기상태에 있다는 것
* 프로세스의 수를 세는 것이기 때문에 시스템에 있는 CPU Core 수가 몇 개냐에 따라 의미가 상대적이다.
* 수치가 높다는건 단순하게 프로세스가 많다는 것을 의미하는 건 아니다. I/O 병목이 생겨서 I/O 작업을 대기하는 프로세스가 많을 수도 있다는 의미이다.

# vmstat

* 시스템의 부하를 측정하는데 사용될 수 있다.
* r과 b열을 눈여겨봐야하고 특히 b열의 경우 I/O 작업 등의 이유로 대기 상태에 있는 프로세스의 수를 의미하며 전체적인 시스템의 성능을 떨어뜨릴 수 있는 프로세스들이다.
* b열의 수가 높다면 I/O 처리 과정에서 문제가 있지 않은지 봐야한다.

# free

* 리눅스에서 메모리의 전체적인 현황을 가장 빠르게 살펴볼 수 있는 명령어
* 전체 메모리 용량, 사용 중인 용량, 그리고 버퍼와 캐시 영역을 용량을 확인할 수 있다.
* mem
  1. -/+ buffers/cache
  2. used : 버퍼와 캐시 영역을 제외하고 사용하고 있는 영역
  3. free : 버퍼와 캐시 영역을 제외하고 사용하지 않는 영역
* 캐시는 파일의 내용을 저장하고 있는 캐시, 버퍼는 파일 시스템의 메타 데이터를 담고 있는 블록을 저장하고 있는 캐시
* 메모리가 부족한 상황이 되면 커널은 해당 영역을 자동으로 반환한다.
* 스왑은 디스크의 일부분을 메모리처럼 사용하기 위해 만들어 놓은 공간
  1. 스왑을 사용한다는 것 자체가 시스템의 메모리가 부족할 수 있다는 의미이다.
  2. 스왑의 사용 여부도 중요하지만 누가 사용하는지도 매우 중요하다.
  3. 전체 프로세스별로 사용중인 메모리를 확인하는데 유용한 툴은 smem이다.(smem은 각 프로세스들의 메모리 사용 현황을 보여준다.)

# dstat

* dstat를 이용하면 간단하게 실시간으로 모니터링이 가능하다. (끝판왕정도..)
  1. CPU : **dstat -c**
  2. Network : **dstat -n**
  3. Memory : **dstat -m**
  4. Disk : **dstat -d**
  5. Load Avrage : **dstat -l**
  6. 페이지 입출력 : **dstat -g**
* 그리고 모니터링하고 싶은 항목들을 아래와 같이 조합하면 한 화면에서 확인이 가능하다.
  1. **dstat -cnmdlg**
* 파일에도 기록하고 싶으면 아래 옵션을 사용하면 된다. csv 형태로 기록된다.
  1. **dstat -cnmdlg --output test.log**
* dstat 관련하여 자세한 정보는 [링크](http://dag.wiee.rs/home-made/dstat/)에서 확인이 가능하다.
