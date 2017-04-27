docker daemon이 `D`상태로 얼어있는 경우가 있었다. 이 때 /proc/sysrq-trigger를 통해 프로세스가 왜 멈춰 있는지 추적이 가능하다.

```
# ps OT -Leo state,pid,lwp,cmd,wchan | grep ^D
D  2758  2895 dockerd --graph=/naver/dock rtnetlink_rcv
D  2758  2961 dockerd --graph=/naver/dock rcu_barrier
D  2758  9927 dockerd --graph=/naver/dock rtnetlink_rcv
```

이 때 `w` key를 보내면 blocked state를 확인해 볼 수 있다. 
```
# echo w > /proc/sysrq-trigger

# dmesg -T 
```

http://m.blog.naver.com/seuis398/70095785826

