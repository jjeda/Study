## Transaction
데이터 무결성을 위한 논리적인 작업 단위

1. Atomicity: All or noting
2. Consistency: 트랜잭션의 시작되기전과 종료된 후가 일관된 상태어야 한다(질량보존의 법칙)
3. Isolation: commit or rollback 이 될 때까지 다른 트랜잭션에 고립된 상태
4. Durability: commit이 되면 영구적으로 저장되어야 한다.

상태
- Active: 트랜잭션 수행중
- Partially committed: 모든 연산 실행된 직후, 데이터 베이스에 반영되기 전 상태
- Committed
- Failed
- Aborted: rollback 연산을 실행한 상태 


분산 네트워크 데이터베이스: 다음 중 둘만 만족시킬 수 있다.
1. Consistency: 모든 읽기 작업에서는 가장 최근데 쓰여진 것을 반환
2. Accessibility: 모든 요청에 대한 응답이 따른다. 반드시 최근에 쓰인 내용이 반영되는 것은 아님
3. Partitionability: 시스템을 노드로 구분, 노드 사이에서 데이터가 유실되더라도 시스템은 계속 제 기능을 한다.

Isolation Level
- READ_UNCOMMITTED
- READ_COMMITTED
- REPEATABLE_READ
- SERIALIZABLE


propagation
- REQUIRED : 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성
- REQUIRES_NEW : 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성
- SUPPORT : 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 nontransactionally로 실행
- MANDATORY : 부모 트랜잭션 내에서 실행되며 부모 트랜잭션이 없을 경우 예외가 발생
- NOT_SUPPORT : nontransactionally로 실행하며 부모 트랜잭션 내에서 실행될 경우 일시 정지
- NEVER : nontransactionally로 실행되며 부모 트랜잭션이 존재한다면 예외가 발생
- NESTED : 해당 메서드가 부모 트랜잭션에서 진행될 경우 별개로 커밋되거나 롤백될 수 있음.
