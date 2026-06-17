Viewed %5B%EB%A1%9C%EB%B3%B4%ED%8B%B0%EC%A6%88%5D_%ED%9C%B4%EB%A8%B8%EB%85%B8%EC%9D%B4%EB%93%9C_%EC%8B%9C%EC%8A%A4%ED%85%9C_%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EC%97%94%EC%A7%80%EB%8B%88%EC%96%B4_%EC%9E%90%EA%B8%B0%EC%86%8C%EA%B0%9C%EC%84%9C.md:15-31

상세 포트폴리오 웹사이트 링크가 파란색 글씨로 노출되고 이미지가 불러와지지 않는 이유는 **마크다운의 이미지 렌더링 대체(Fallback) 메커니즘**과 **배지 생성 API(`shields.io`)의 잘못된 옵션 지정** 때문입니다.

---

### 🔍 원인 분석

1. **대체 텍스트(Alt Text) 노출 원리**:
   마크다운에서 이미지 링크는 `[![대체 텍스트](이미지 URL)](이동할 링크 URL)` 형태로 작성됩니다. 만약 **이미지 URL에서 이미지를 성공적으로 가져오지 못하면**, 브라우저는 이미지 대신 대괄호 안의 `"상세 포트폴리오 대시보드 보기"`라는 대체 텍스트를 파란색 링크 형태로 띄우게 됩니다.
2. **잘못된 스타일 파라미터 (`style=flat-for-the-badge`)**:
   상단 연락처 배지의 이미지 URL에 작성된 `style=flat-for-the-badge`는 `shields.io`가 지원하지 않는 잘못된 옵션 명칭입니다. 올바른 명칭은 **`style=for-the-badge`**입니다. 옵션 오류로 인해 이미지 자체가 깨졌습니다.
3. **존재하지 않는 로고 이름 (`logo=phone`, `logo=robot`, `logo=server`)**:
   `shields.io`는 브랜드 로고 라이브러리인 **Simple Icons**에서 로고를 가져옵니다. `gmail`, `github`은 정식 브랜드명이기에 동작하지만, `phone`(전화기), `robot`(로봇), `server`(서버)는 브랜드가 아닌 일반 명사이기 때문에 Simple Icons에 존재하지 않아 이미지 로드에 실패하거나 긴 지연이 발생하게 됩니다.
4. **일시적인 AdBlock(광고 차단기) 또는 VPN의 차단**:
   간혹 개인 브라우저 환경에서 `img.shields.io` 서버를 광고 이미지 공급처로 인식하여 로드가 차단되는 경우도 발생합니다.

---

### 🛠️ 해결 방법 (수정된 코드)

잘못 지정된 **`style` 옵션을 정상 값으로 복구**하고, Simple Icons에 존재하지 않는 **`logo` 파라미터들을 안전하게 제거**했습니다. (로고를 지정하지 않으면 아이콘 없이 깔끔하게 텍스트 배지만 정상 렌더링됩니다.)

아래 코드를 복사해서 포트폴리오 리드미나 메인 파일에 덮어씌우시면 깨짐 없이 고품질 배지가 정상 작동합니다.

```markdown
# 윤재현

<div align="left">
  <a href="mailto:yjhyun0613@gmail.com"><img src="https://img.shields.io/badge/Email-yjhyun0613@gmail.com-4f46e5?style=for-the-badge&logo=gmail&logoColor=white"/></a>
  <a href="tel:010-6211-7382"><img src="https://img.shields.io/badge/Phone-010--6211--7382-0f172a?style=for-the-badge&logoColor=white"/></a>
  <a href="https://github.com/yjhyun0613" target="_blank"><img src="https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white"/></a>
</div>

## 🛠️ Technical Skills
| 분류 | 상세 스택 및 핵심 기술 역량 |
| :--- | :--- |
| **Robot Control** | Doosan Robotics API (`DSR_ROBOT2`), DRL Script, TurtleBot 4, ROS 2 Humble, Nav2, 순운동학(Forward Kinematics), 유연 순응 제어(Compliance Control), 원주구속 제어, AMR Fleet 스케줄링 (동시 기동 제약), JIT 인터로킹 제어 |
| **Vision & Space** | YOLOv8 Inference, Intel RealSense D435i, OpenCV, 다시점 3D 포인트 Cloud 병합(Point Cloud Stitching), 벡터 선형대수(Vector Cross Product) |
| **Data & Twin** | Python, C++, Fast DDS Network Middleware, PostgreSQL, Redis (ZSET Cache), WebSockets, FastAPI, Firebase Realtime DB, Plotly.js 3D Telemetry Web Visualizer (Zero-Latency Streaming) |

---

## 💼 Core Projects (Click Badge to View Technical Report)

인사담당자 및 기술면접관분들을 위해 프로젝트별 상세 트러블슈팅, 현장 엔지니어링 이슈 해결 과정 및 구동 미디어가 포함된 **독립 대시보드 포트폴리오**를 연동해 두었습니다. 아래 배너를 클릭하시면 해당 포트폴리오 웹사이트로 즉시 이동합니다.

### 📌 1. AI 비전 및 LiDAR 센서 융합 기반 AMR 자율주행 안전 시스템
- **소속 인프라:** TEAM 오라이봇 | TurtleBot 4 & ROS 2 Humble & YOLOv8
- **전담 담당 업무:** YOLOv8 GPU 가속 실시간 객체(사람) 감시 노드 및 RGB-D 뎁스 매핑 결합, LiDAR TF(좌표계) 변환 조향 제어, Nav2 Action 강제 취소(Cancel) 및 캐싱된 목적지 재배포(Resume) 관제 통신부 설계
- **상세 포트폴리오 웹사이트:** [![상세 포트폴리오 대시보드 보기](https://img.shields.io/badge/Oraibot__AMR__Project-Click_to_View_Dashboard-0984e3?style=for-the-badge&logo=github&logoColor=white)](https://yjhyun0613.github.io/Oraibot/)

<br>

### 📌 2. 토크 센서와 3D Point Cloud를 활용한 능동형 표면 탐색 로봇
- **소속 인프라:** TEAM 더듬이 | Doosan M0609 & ROS 2 Humble
- **전담 담당 업무:** 로봇 제어부(정밀 원주 구속제어), 데이터 분석부(불량 판단 방법), 시각화부(사용자용 실시간 디지털 트윈 렌더링 최적화)
- **상세 포트폴리오 웹사이트:** [![상세 포트폴리오 대시보드 보기](https://img.shields.io/badge/Surface__Inspection__Project-Click_to_View_Dashboard-4f46e5?style=for-the-badge&logoColor=white)](https://yjhyun0613.github.io/Deodeumi_Bot/)

<br>

### 📌 3. AI 컴퓨터 비전 및 3D 공간 맵핑 기반 자동 정비 시스템
- **소속 인프라:** TEAM Joingo | Doosan M0609 & Intel RealSense D435i & YOLOv8
- **전담 담당 업무:** 3D 벡터 선형대수 기반 Look-at 수직 보간 정렬 제어, 다시점 5면 3D 포인트 클라우드 병합(Stitching) 알고리즘 파이프라인 수립
- **상세 포트폴리오 웹사이트:** [![상세 포트폴리오 대시보드 보기](https://img.shields.io/badge/Screw__Fastening__Project-Link_Pending_(Slot_Reserved)-64748b?style=for-the-badge&logo=codeforces&logoColor=white)](https://yjhyun0613.github.io/JoinGo/)

<br>

### 📌 4. NVIDIA Isaac Sim & ROS 2 Humble 기반 지능형 다중 로봇 물류창고 관제 시스템
- **소속 인프라:** TEAM SKFC | NVIDIA Isaac Sim & ROS 2 Humble & PostgreSQL & Redis
- **전담 담당 업무:** ROS 2 Humble 기반 관제 백엔드 스케줄러(MultiThreadedExecutor) 설계, A/B double buffering & Look-ahead 예비 작업대(3/7슬롯) 동적 호출 스케줄링, 143개 바닥 QR 격자망 생성 및 USD Instancing VRAM 최적화, AMR 액션 비동기 호출 타임아웃 예외 대응 DB 복구(Rollback) 트랜잭션 수립
- **상세 포트폴리오 웹사이트:** [![상세 포트폴리오 대시보드 보기](https://img.shields.io/badge/SkynetFC__Warehouse__Project-Click_to_View_Dashboard-00f5d4?style=for-the-badge&logoColor=black)](https://yjhyun0613.github.io/Skynet_FC/)
```
