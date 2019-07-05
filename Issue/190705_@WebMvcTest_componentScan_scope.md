## 190705
- RestAPI를 설계하는 과정에서 Controller를 테스트하고 싶어졌음
- 열심히 그동안에 공부해왔던 TDD방식을 써보려고함
- 컨트롤러 테스트를 하니까 당연히 @WebMvcTest를 붙임
- ㅎㅎ 테스팅이 안됨 JpaRepository Bean을 못찾음
- 근데 어플리케이션 실행은되고 메시지도 잘 주고받음
- 컴포넌트 스캔범위가 다른게 생각이남..

## 0.결론
- @SpringBootTest는 패키지 이하 모든 컴포넌트를 스캔함
- @WebMVcTest는 단위테스트임
- 그렇다고 컴포넌트스캔 범위를 확장하는 것은 너무 비효율적
- @MockBean을 쓰자!

## 1.공부하자
- @WebMvcTest, @SpringBootTest
- Mock
- TDD

