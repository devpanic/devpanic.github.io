---
title: Core Affinity
date: 2019-08-09 20:55:00 +0800
categories: [Blogging, Tutorial]
tags: [getting started]
---

1. taskset
   - command line으로 mask를 이용해 task를 코어에 할당할 수 있다.
   - taskset ‘mask’ ‘task’
   - kernel thread & user process
2. cpuset
   - 코드 작성 시에, 원하는 부분을 마스킹을 통해 task 할당이 가능하다.
   - kernel thread & user process
3. making new cgroup
   - kernel thread & user process
4. workqueue
   - 위의 방법들로 옮겨지지 않는 kernel thread task들은 workqueue의 마스킹을 통해 제어 가능하다.
   - kernel thread & user process
5. kernel patch
   - dynamic하게 생성되는 kernel thread들의 경우, 위의 방법을 적용한 뒤에 생성되면 의도하지 않은 코어에 task가 할당 될 수 있다. 따라서 아래와 같은 커널 패치를 통해 해결 가능하며, boot argument 설정을 통해 바꾸어 줄 수 있다.
   - 패치 내용
   - kernel_default_mask=ff
6. isolcpus
   - boot argument
   - u-boot
   - boot.ini에 다음과 같은 포맷으로 작성하면 된다.
     - isolcpus=0-7
     - isolcpus=1,2,3
   - user process에 한해서만 가능한 동작이다.
