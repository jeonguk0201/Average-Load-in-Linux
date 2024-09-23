# Average-Load-in-Linux

## 

![image](https://github.com/user-attachments/assets/3e10cfc3-ea5a-40a0-abb6-8a2ef3523998)

Linux uptime 명령어의 의미
```
12:23:46      //current time
up 3:15       //system uptime
4 users       //number of logged-in users
load average  //1분, 5분, 15분 동안의 평균 부하
```
지난 1분동안의 avrage load, 지난 5분동안의 avrage load, 지난 15분동안의 avrage load

### average load
단위 시간 동안 실행 가능 또는 중단 불가능한 상태의 프로세스의 평균 수
이는 활성 프로세스의 평균 수라고도 하며 CPU 사용률과 직접적인 관련이 없다
<br>
단위 시간당 CPU 사용률 이라고 생각할 수 있고
부하 평균 0.12은 CPU 사용률 12%를 의미라고 생각했지만 그렇지 않다

### Runnable processes
CPU를 사용하거나 CPU time을 기다리는 프로세스
ps 명령어에서 Running 또는 Runnable로 표시

### Uninterruptible processes
시스템이 프로세스와 하드웨어 장치를 보호하는 메커니즘
하드웨어 장치의 I/O 응답을 기다리는 프로세스로 중요한 커널 프로세스에 속하며 중단될 수 없는 프로세스
Uninterruptible Sleep 또는 Disk Sleep로 표시
<br>
프로세스가 디스크에서 데이터를 읽거나 쓸 때, 데이터 일관성을 보장하기 위해
디스크에서 응답을 받을 때까지 중단될 수 없다
만약, 프로세스가 중단되면 디스크와 프로세스 간의 데이터가 일관되지 않을 수 있다

### average load=2 의 의미
2개의 CPU가 있는 시스템에서 모든 CPU가 사용중이다
<br>
4개의 CPU가 있는 시스템에서는 50%의 유휴 CPU 용량이 있다
<br>
1개의 CPU가 있는 시스템에서 프로세스의 절반이 CPU time을 놓고 경쟁 중이다

### 적절한 avrage load
이상적인 상황은 average load = CPU의 개수
<br>
시스템에 CPU 개수를 알아내는 법
```
grep 'model name' /proc/cpuinfo | wc -l
```

![image](https://github.com/user-attachments/assets/138ccaca-c595-4b64-a699-f234a4006543)

### Average load vs. CPU usage
Average load는 시스템의 부하를 대기 중인 프로세스의 수로 측정하며, CPU가 일을 얼마나 할 수 있는지와 관련이 있다
CPU Usage는 CPU가 현재 얼마나 바쁜지를 보여준다

## average load 이해를 위한 test
### iostat, mpstat, pidstat 명령어를 통해 average load 증가 원인 파악
명령어 사용을 위한 패키지 설치
```
apt install stress sysstat
```
mpstat는 멀티코어 CPU 성능을 분석하는 데 일반적으로 사용되는 도구로, 각 CPU에 대한 실시간 성능 지표와 모든 CPU에 대한 평균 지표를 보는 데 사용
<br>
pidstat는 프로세스 성능을 분석하는 데 일반적으로 사용되는 도구로, 프로세스의 CPU, 메모리, I/O, 컨텍스트 전환에 대한 실시간 성능 지표를 확인하는 데 사용
<br>

![image](https://github.com/user-attachments/assets/ad083c49-ea0c-44c2-9bfc-d775d95794ff)

<br>
root 사용자로 전환

```
sudo su root
```

## CPU 사용률 100% 인 상황
### 1. 부하를 주기위한 stress 명령어
첫 번째 터미널에서 실행
```
stress --cpu 1 --timeout 600
```

![image](https://github.com/user-attachments/assets/c559326d-cb46-4ada-a309-fae7ef089e9b)

### 2. average load의 변화를 살펴보기 위한 uptime 명령어
두 번째 터미널에서 실행
```
watch -d uptime
```

![image](https://github.com/user-attachments/assets/5858f20a-5c4a-44a6-be22-4826edebe75e)

### 3. CPU 사용량의 변화를 살펴보기 위한 mpstat 명령어
세 번째 터미널에서 실행
```
mpstat -P ALL 5
```

![image](https://github.com/user-attachments/assets/5c214020-c599-4d3d-9265-33afeaa5d307)

두 번째 터미널에서 uptime 으로는 average load가 1.00으로 점점 증가하는 것을 확인할 수 있고
<br>
세 번째 터미널에서 mpstat 로는 CPU 사용량이 100%에 가깝고 iowait가 0에 가까운 것을 확인 가능



