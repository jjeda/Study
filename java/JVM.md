## Java Garbage Collection
- [D2 이상민님의 게시글](https://d2.naver.com/helloworld/1329)
- stop-the-world : GC를 실행하기 위해 JVM이 애플리 케이션 실행을 멈추는 것 
- GC 튜닝 -> stop-the-world 줄이기

#### weak generational hypothesis
- 대부분의 객체는 곧 접근 불가능 상태(unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.
- 이 두가지 가설(전제 조건)을 최대한 살리기 위해 
- HotSpot VM에서 Young / Old 영역으로 나눔
- Yonng Generation
  - 새롭게 생성한 객체의 대부분이 여기에 위치
  - 대부분의 객체가 곧 unreachable -> 매우 많은 객체가 생성 -> 소멸
  - Young에서 객체가 사라진다 == Minor GC가 발생한다
- Old Generation
  - Young 영역에서 살아남은 (unreachable 상태가 되지 않은) 객체가 복사 됨
  - 대부분 Young 영역보다 크게 할당됨 -> GC는 적게 발생
  - Major GC(Full GC)가 발생
