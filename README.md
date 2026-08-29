# AutoFit-3D | 완성차 의장 공정 6축 로봇 3D 갭·단차(Gap & Flush) 디지털 트윈 & 힌지 자동 보정 시스템

> **Trim & Final Assembly In-line 3D Optical Metrology Digital Twin & Prescriptive Hinge Alignment System**  
> *현대자동차·기아 의장/조립 생산기술 및 스마트팩토리(HMGICS) 인라인 품질 최적화 디지털 트윈*

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-00f2fe?style=for-the-badge&logo=github)](https://jiu7021.github.io/side-sill-welding-sim/)
[![Tech Stack](https://img.shields.io/badge/Three.js-WebGL%203D-3b82f6?style=for-the-badge&logo=three.js)](https://threejs.org/)
[![Domain](https://img.shields.io/badge/Automotive-Trim%20%26%20Final%20Assembly-emerald?style=for-the-badge)](https://github.com/jiu7021/side-sill-welding-sim)

---

## 🎯 1. 프로젝트 배경 및 문제 정의 (Engineering Background)

완성차 제조의 최종 단계인 **의장 공정(Trim & Final Assembly)**은 도장된 차체(BIW)에 도어(Door), 후드(Hood), 테일게이트(Tailgate) 등 무빙파트(Moving Parts)를 장착하는 공정입니다.

### ⚠️ 현장의 핵심 페인포인트 (Pain Points)
1. **풍절음(NVH) 및 누수(Water Leak) 리스크**:
   * 도어와 펜더 간의 **간극(Gap: 틈새)**과 **단차(Flush: 높낮이 평탄도)**가 $\pm 0.5\text{mm}$ 이상 어긋나면 고속 주행 시 풍절음 및 웨더스트립 기밀성 저하로 인한 누수가 발생하여 완성차 감성 품질 클레임으로 직결됩니다.
2. **기존 수작업 단차 튜닝의 한계**:
   * 현장 작업자가 힌지 볼트를 풀고 수작업(고무망치 타격 등)으로 단차를 맞추던 주관적 튜닝 방식은 양산 택트타임(Cycle Time) 지연 및 높은 품질 산포를 유발합니다.

### 💡 디지털 트윈 해결책 (Solution)
* **6축 다관절 산업용 로봇 + 3D 레이저 슬릿 프로파일러(Laser Triangulation)**를 도입하여 8대 주요 포인트의 Gap & Flush를 $0.05$초 내에 비접촉 실시간 전수 계측.
* 단차 발생 시 **도어 힌지 상/하단 심(Shim) 라이너 보정량 및 로봇 볼팅 각도($\Delta \theta$)를 AI 알고리즘으로 자동 계산하여 처방(Prescriptive Alignment)**.

---

## 📸 2. 핵심 기능 및 아키텍처

```
┌────────────────────────────────────────────────────────────────────────┐
│  AUTOFIT-3D ROBOT METROLOGY DIGITAL TWIN                              │
│                                                                        │
│  [좌측 60% : 3D WebGL 캔버스]             [우측 40% : 실시간 계측 & 처방] │
│  ┌─────────────────────────────────┐   ┌─────────────────────────────┐│
│  │ 🚗 3D EV 차체 와이어프레임      │   │ 🎯 8개소 실시간 Gap/Flush   ││
│  │  - 도어/후드/바디 패널 분리     │   │  - Gap: 3.48mm (OK)         ││
│  │                                 │   │  - Flush: -0.05mm (OK)      ││
│  │ 🤖 6축 다관절 로봇 실시간 모션  │   ├─────────────────────────────┤│
│  │  - 티칭 경로 순차 이동          │   │ 🔴 3D 레이저 삼각측량 단면  ││
│  │  - 네온 레이저 빔 투사          │   │  - 2D Canvas Laser Profile  ││
│  │                                 │   ├─────────────────────────────┤│
│  │ 🟢🔴 8개소 3D 펄스 마커 핀     │   │ 💡 AI 힌지 자동 처방 제어   ││
│  │  (OK: 녹색 링, NG: 적색 링)     │   │  - 상단 힌지 심: ±0.00mm    ││
│  └─────────────────────────────────┘   └─────────────────────────────┘│
│  [하단 : 인라인 100대 연속 생산 Gap & Flush SPC 관리도 (Cpk 산출)]     │
└────────────────────────────────────────────────────────────────────────┘
```

1. **Three.js WebGL 3D 인터랙티브 디지털 트윈**:
   * 브라우저에서 마우스로 360° 자유 회전, 줌인/줌아웃, 탑/사이드/아이소메트릭 시점 전환 지원.
   * KUKA/현대로보틱스 스타일의 6축 로봇 팔이 8개 측정 포인트를 따라 움직이며 레이저 팬(Fan) 빔을 투사하는 역동적인 3D 애니메이션.
2. **의장 라인 8대 주요 포인트 실시간 계측**:
   * `PT-01~02`: 후드 - 헤드램프 좌/우
   * `PT-03~04`: 앞도어(FL/FR) - 프론트 펜더 단차
   * `PT-05~06`: 앞도어 - 뒷도어 B필러 단차
   * `PT-07~08`: 테일게이트 - 리어 쿼터/램프
   * **정밀 공차 관리**: **Gap $3.50\text{mm} \pm 0.50\text{mm}$**, **Flush $0.00\text{mm} \pm 0.50\text{mm}$**
3. **2D 레이저 삼각측량 단면 프로파일 (Laser Profile Slice)**:
   * 레이저 스캐너가 인식한 패널 단면의 높낮이 Step 파형을 실시간 Canvas 렌더링.
4. **💡 AI 힌지(Hinge) 자동 얼라인먼트 처방 (Prescriptive Engine)**:
   * 단차 이탈 시 레버 비율 기구학을 바탕으로 **상단 힌지 심(Shim) 라이너 보정량 및 하단 힌지 체결 각도** 자동 처방.
5. **통계적 공정관리(SPC) 및 공정능력지수($C_{pk}$)**:
   * 100대 연속 생산 시계열 관리도 (Chart.js) 연동, 단차 불량(NG) 강제 주입 및 복구 시뮬레이션.
6. **완성차 의장 공정 학습 가이드 & 면접 대비 모달 내장**.

---

## 📐 3. 광학 및 기구학 수학 모델링

### 3.1 3D 레이저 삼각측량법 (Laser Triangulation Metrology)
$$Z = \frac{B \cdot f}{x_{\text{sensor}} + f \cdot \tan(\theta)}$$
* $B$: 레이저 광원과 센서 간의 베이스라인 거리
* $f$: 수광 렌즈 초점 거리
* $x_{\text{sensor}}$: CMOS 센서 상의 레이저 빔 결상 위치

### 3.2 힌지 기구학 및 심(Shim) 라이너 보정식 (Prescriptive Kinematics)
$$\Delta t_{\text{upper}} = \Delta F_{\text{rear}} \cdot \left(\frac{L_{\text{hinge}}}{L_{\text{door}}}\right)$$
* $\Delta F_{\text{rear}}$: 도어 후단 B필러 단차 오차량
* $L_{\text{hinge}}$: 상/하단 힌지 사이의 간격 거리
* $L_{\text{door}}$: 도어 전체 전후 길이

---

## 🛠️ 4. 기술 스택 (Tech Stack)

- **3D Graphics & Engine**: Three.js (r128), WebGL, OrbitControls
- **Frontend Core**: Vanilla Modern JavaScript (ES6+), HTML5 Canvas & Dynamic SVG
- **Styling**: Tailwind CSS (Dark Cyber Industrial Theme), Glassmorphism
- **Data Visualization**: Chart.js (Real-time SPC Trend Chart)
- **Math Rendering**: KaTeX CDN
- **Icons & Sound**: Lucide Icons, Web Audio API (Synthesized Haptic Feedback)
- **Architecture**: Zero-Dependency Single HTML File (초경량 무설치 웹 애플리케이션)

---

## 🚀 5. 배포 링크

👉 **[https://jiu7021.github.io/side-sill-welding-sim/](https://jiu7021.github.io/side-sill-welding-sim/)**
