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

### 평균 부하란?
단위 시간 동안 실행 가능 또는 중단 불가능한 상태의 프로세스의 평균 수
이는 활성 프로세스의 평균 수라고도 하며 CPU 사용률과 직접적인 관련이 없다
<br>
단위 시간당 CPU 사용률 이라고 생각할 수 있고
부하 평균 0.63은 CPU 사용률 63%를 의미라고 생각했지만 그렇지 않다

#### 실행 가능한 프로세스
CPU를 사용하거나 CPU time을 기다리는 프로세스
ps 명령어에서 Running 또는 Runnable로 표시
