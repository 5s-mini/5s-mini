<div align="center">

### Backend Developer

# 오상민

<br>

문제를 먼저 정의하고, 대안을 비교해 선택 근거를 남깁니다.  
적용한 뒤에는 측정으로 확인하고 남은 과제를 기록합니다.

<br>

[![Email](https://img.shields.io/badge/dnclsehd123@naver.com-03C75A?style=flat&logo=naver&logoColor=white)](mailto:dnclsehd123@naver.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-333333?style=flat&logo=readthedocs&logoColor=white)](https://github.com/5s-mini/5s-mini/blob/main/Portfolio.pdf)

<br>
</div>

---

<br>

## Profile

여러 인스턴스가 함께 돌아갈 때 나타나는 문제를 먼저 살펴보는 백엔드 개발자입니다.
<br>
좌석 중복 할당, 외부 API 호출 병목, 커넥션 풀 소진과 같이 동시성과 자원 경합에서 비롯된 문제를
정의하고, 적용 가능한 대안들을 비교하여 어떤 근거로 무엇을 선택했는지 기록하며 해결해 왔습니다.

- **읽은 값과 쓰는 값이 다를 수 있는 지점을 찾습니다.** 잔고 검증, 체결 직전 상태, 배치 조회 결과가 모두 같은 성격의 문제였고, 한 곳을 고친 뒤 나머지를 같은 기준으로 점검했습니다.
- **증상에서 시작해 원인까지 좁힙니다.** 시세 API가 함께 멈추는 현상에서 공통 경로를 따라가 토큰 발급 구간을 찾았습니다.
- **가장 단순한 방법부터 의심합니다.** 왜 부족한지 확인한 뒤에 다음 선택지로 넘어갑니다.
- **한계를 함께 기록합니다.** 무엇을 포기했는지 적어 두어야 다음 사람이 판단할 수 있다고 생각합니다.
- **개선했다고 말하기 전에 측정합니다.** K6로 개선 전후를 측정하고, 기대만큼 줄지 않은 구간은 원인을 찾아 남은 과제로 정리합니다.

<br>

---

<br>

## Projects

### 실시간 예약 시스템 <sub>(2026.04 ~ 2026.05)</sub>
> 대규모 트래픽 상황을 가정한 식당 예약 시스템의 대기열과 동시성 제어 담당, 5인 팀  
> [2차 (Java)](https://github.com/prgrms-be-devcourse/NBE9-11-2-Team07) &nbsp;/&nbsp; [3차 (Redisson, Sentinel 고도화)](https://github.com/prgrms-be-devcourse/NBE9-11-3-Team07)

#### Tech Stack
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![JPA](https://img.shields.io/badge/JPA-59666C?style=flat&logo=hibernate&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![Redisson](https://img.shields.io/badge/Redisson-DC382D?style=flat&logo=redis&logoColor=white)
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat&logo=k6&logoColor=white)

#### Key Contributions
- **이중 방어 동시성 제어**: 같은 좌석에 동시에 들어온 요청을 Redis 분산 락으로 한 줄로 세워 DB까지 도달하는 경쟁을 줄이고, 락 만료나 예외를 대비해 DB 락(비관적 + 낙관적)으로 정합성을 한 번 더 보증하는 이중 구조로 설계
- **락 수명 관리**: 처리가 길어지면 락이 먼저 풀려 다른 요청이 끼어들던 문제를, 작업 중 Redisson Watchdog이 TTL을 연장하고 트랜잭션 완료 이후 락을 해제하도록 순서를 고정하여 해결
- **대기 방식 전환**: 락을 얻지 못한 요청을 즉시 취소하지 않고 해제 알림을 받을 때까지 대기시켜, 잠깐의 경합으로 취소되던 예약을 줄임

#### Results
- K6 동시성 테스트(재고 1석, 동시 요청 100건)에서 **확정 1건, 취소 99건, 초과 예약 0건**

<br>

### [스포츠 팀 매칭 플랫폼](https://github.com/prgrms-be-devcourse/NBE9-11-final-Team02) <sub>(2026.06 ~ 2026.07)</sub>
> 생활 스포츠 시설 예약 및 팀 매칭 플랫폼의 인증, 시설 검색, 실시간 동기화 담당, 4인 팀 **(팀장)**

#### Tech Stack
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![STOMP](https://img.shields.io/badge/WebSocket%20%2F%20STOMP-010101?style=flat&logo=socketdotio&logoColor=white)
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat&logo=k6&logoColor=white)

#### Key Contributions
- **인증 시스템 설계**: 토큰이 탈취되더라도 피해가 제한되도록, RefreshToken은 스크립트가 읽을 수 없는 HttpOnly Cookie로 분리 발급하고 AccessToken은 30분 만료로 노출 시간을 줄였으며, 저장된 것과 다른 RefreshToken이 제시되면 탈취로 간주해 강제 로그아웃. 토큰 상태를 Redis에 두어 서버가 여러 대여도 동일한 판단을 내리도록 구성
- **실시간 동기화**: WebSocket 연결은 접속한 서버 한 대에만 묶여 다른 서버의 사용자에게는 이벤트가 닿지 않음. 매치 참가 및 퇴장 이벤트를 Redis 채널로 발행하고 각 서버가 구독해 자기 사용자에게 STOMP로 전달하도록 구성
- **검색 캐싱**: 같은 지역 및 종목 조합의 검색이 반복되는데도 매번 DB를 조회해 커넥션을 점유하던 문제를, 결과를 Redis에 5분간 캐시하여 해결. 빈 결과는 캐시에서 제외하여 시설이 없던 조합에 새 시설이 등록되더라도 TTL 동안 검색되지 않는 문제를 방지
- **팀 리딩**: 데일리 스크럼과 PR 및 이슈 템플릿 기반 컨벤션을 도입. 인증, 회원, 시설 검색의 백엔드와 프론트엔드 개발을 병행

#### Results
- K6 부하 테스트에서 **DB 조회 약 1만 7천 회 → 조건당 1회**

<br>

### [모의주식 투자 플랫폼 ZeroRisk](https://github.com/ZeroRisk-Project/ZeroRisk_BackEnd) <sub>(2026.07 ~ 2026.09)</sub>
> KIS 오픈 API 기반 모의주식 투자 플랫폼의 외부 증권 API 연동과 주문, 포트폴리오 도메인 담당, 3인 팀

#### Tech Stack
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![JPA](https://img.shields.io/badge/JPA-59666C?style=flat&logo=hibernate&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle-F80000?style=flat&logo=oracle&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=flat&logo=socketdotio&logoColor=white)
![KIS Open API](https://img.shields.io/badge/KIS%20Open%20API-1E3A5F?style=flat)

#### Key Contributions
- **주문 정합성**: 잔고 1만 원인 계좌가 1만 원짜리 지정가 주문을 두 번 접수할 수 있었던 문제를 해결. 검증이 현재 잔고만 확인하고 대기 중인 주문이 쓸 금액을 반영하지 않는 것이 원인. 접수 시점에 잔고를 차감하는 방식은 체결, 취소, 미체결 종료마다 반환 처리가 필요해 누락 위험이 있다고 판단하여, 잔고 변경 지점을 체결 시점으로 유지한 채 대기 주문의 금액과 수량을 합산해 가용분을 계산하고, 계좌 행에 비관적 락을 건 뒤 검증을 수행하도록 구현
- **체결 시점 재검증**: 예약 주문 체결 배치가 대기 주문을 조회한 뒤 체결하기까지의 사이에 사용자가 주문을 취소할 수 있어, 이미 취소된 주문이 체결될 수 있었음. 조회 단계부터 락을 걸면 배치가 대상 전체를 오래 점유하므로, 체결 직전에만 주문과 계좌를 락과 함께 재조회해 상태와 잔고를 재확인하도록 변경
- **KIS 오픈 API 연동**: 현재가, 일/주/월/분봉 차트, 거래량 순위, 종목 통합 검색 등 시세, 주문, 포트폴리오 도메인을 구현. 증권사 API가 초당 2건까지만 조회를 허용하므로 배치 조회는 호출 간격을 설정하여 제한을 넘지 않도록 구성
- **실시간 시세 브로드캐스트**: 증권사 실시간 시세 연결을 사용자마다 맺지 않고 서버가 하나만 유지하도록 설계. 종목별 구독 사용자를 서버에서 관리하여 첫 사용자가 구독할 때만 증권사에 종목을 등록하고 마지막 사용자가 해지하면 해제
- **배치 분리와 장애 격리**: 예약 주문 체결, 목표가 알림 발송, 일별 자산 스냅샷 생성을 스케줄러로 분리하고, 자산 추이 조회는 미리 계산해 둔 스냅샷만 읽도록 구현. 초기에는 여러 건을 하나의 트랜잭션으로 묶어 한 건의 실패가 배치 전체를 롤백시켰으므로, 종목 마스터 동기화는 건별 독립 트랜잭션으로, 목표가 알림은 알림 단위 예외 격리로 변경
- **외부 연동 장애 격리**: 토큰 발급이 한 번에 한 요청만 통과하도록 묶여 있고 외부 응답 대기에 제한이 없어, 발급 하나의 지연이 시세 기능 전체의 중단으로 이어질 수 있었음. 대기에 3초 상한을 두고, 외부 호출에 연결 3초와 응답 5초 타임아웃을 설정

#### Results
- 담당 도메인 **19개 API와 4개 배치 스케줄러** 설계 및 구현
- 예외 상황을 먼저 정리하고 검증하는 방식으로 개발하여 **테스트 클래스 23개, 테스트 121개** 작성
- 종목 목록 조회 **40\~50초 → 20~30초** 단축 (페이지 단위 조회와 병렬 호출)

<br>

---

<br>

## Tech Stack

**Language**  
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)

**Framework**  
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-6DB33F?style=flat&logo=spring&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![Redisson](https://img.shields.io/badge/Redisson-DC382D?style=flat&logo=redis&logoColor=white)
![STOMP](https://img.shields.io/badge/WebSocket%20%2F%20STOMP-010101?style=flat&logo=socketdotio&logoColor=white)

**Database**  
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle-F80000?style=flat&logo=oracle&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)

**Test & Tools**  
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat&logo=k6&logoColor=white)
![JUnit](https://img.shields.io/badge/JUnit5-25A162?style=flat&logo=junit5&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)

<br>

---

<br>

## Education & Certificate

| 구분 | 내용 | 기관 | 기간 |
|:---|:---|:---|:---|
| 학력 | 소프트웨어융합학과 학사 | 홍익대학교 세종캠퍼스 | 2020.03 ~ 2026.08 |
| 교육 | 클라우드 기반 백엔드 엔지니어링 데브코스 | 프로그래머스 | 2026.01 ~ 2026.07 |
| 교육 | 메타버스 융합 SW 아카데미 | 홍익대학교 | 2025.06 ~ 2025.11 |
| 자격 | 데이터분석 준전문가 (ADsP) | 한국데이터산업진흥원 | 2026.06 |
| 어학 | OPIc IH (Intermediate High) | ACTFL | 2025.09 |
