# 사이드실 용접 검사구 H값 최적화 디지털 트윈 시뮬레이터
> **Side-Sill Welding Inspection Gauge H-Value Digital Twin Simulator (v2.5 PRO)**

자동차 차체 조립 공정의 사이드실(Side-Sill) 용접 시 발생하는 열응력 변형 및 조립 누적 공차(H값: 단차/간극)를 검사구 기준 최적의 라이너(Shim Plate) 조합으로 보정 및 최적화하는 **산업용 스마트 팩토리 디지털 트윈 시뮬레이터**입니다.

---

## 💡 현장 실측 데이터 연계 및 확장성 로드맵 (Field Scalability)

본 시뮬레이터는 현재 **용접 열변형 특성 곡선, Box-Muller 가우시안 노이즈, 설비 열화 오차 모델**을 기반으로 생성된 합성 데이터(Synthetic Simulation Data)를 사용하여 검증 및 튜닝을 수행합니다.

실제 양산 제조 현장에 적용 시, 다음과 같은 **실측 데이터 기반 스마트 팩토리 솔루션**으로 즉시 연계·확장할 수 있습니다:
1. **인라인 3차원 계측기(CMM) / 레이저 비전 센서 실측 데이터 연동 (CSV / MQTT / REST API)**: 실시간 차체 측정치를 수신하여 런별 단차 품질 추세를 관리도(SPC) 상에 실시간 시각화.
2. **설비 열화 및 마모 예지 보전(PdM)**: 용접 지그 변형, 전극 팁 마모로 인한 H값의 점진적 드리프트 추세를 감지하여 사전 캘리브레이션 알림 제공.
3. **최적 심(Shim) 라이너 자동 추천 처방(Prescriptive Maintenance)**: 공차 이탈 발생 시 작업자에게 홀별 필요 라이너 증감 매수를 AI 알고리즘으로 자동 계산하여 가이드.

---

## 📸 핵심 기능 및 사양

1. **사이드실 & 검사구 2D 디지털 트윈 실시간 연동 (Interactive SVG)**
   - 홀별 라이너(심 플레이트) 수동 가감 시 두께 적층 물리 그래픽 실시간 렌더링
   - 검사구 레이저 프로브 측정 빔 및 합격(OK) / 규격 이탈(NG) 실시간 상태 피드백
2. **H값과 라이너 개수의 비례 물리 모델링**
   - 라이너 개수가 증가할수록 검사구 접촉 높이(H값)가 **비례하여 증가**하는 물리적 특성 구현
   - **정밀 공차 관리 기준**: **목표 10.0mm (공차 ±1.0mm : 9.0 ~ 11.0mm)**
   - 최적 라이너 조합: **홀A 8개(10.1mm), 홀B 10개(9.9mm), 홀C 11개(10.2mm), 홀D 9개(10.0mm)**
3. **용접 열변형 & 설비 열화 오차 합성 엔진 (Synthetic Data Engine)**
   - Box-Muller 가우시안 정규분포 ($\sigma$) 기반 열변형 산포
   - 연속 런 누적에 따른 전극/지그 마모 열화 드리프트 ($\Delta_{\text{degrade}}$)
   - 순간 용접 스패터 이상치(Outlier Spike) 모델
4. **실시간 SPC 통계 및 공정 능력 분석 (Chart.js)**
   - 5회 연속 생산 런 시계열 트렌드 관리도 (LSL: 9.0mm / USL: 11.0mm)
   - 공정 수율(Yield %) 및 공정 능력 지수($C_{pk}$) 실시간 산출
   - 최적 설정(Best Run) 자동 하이라이트 및 CSV 데이터 내보내기
5. **AI 원클릭 자동 최적화 탐색 (Auto-Tuning)**
   - 각 홀의 공차 중심값(10.0mm)에 수렴하는 최적의 심 라이너 조합을 원클릭으로 자동 탐색

---

## 📐 수학 및 물리 모델링 명세

$$H_{\text{measured}}(i, n) = H_{\text{base}}(i, n) + \varepsilon_{\text{thermal}} + \Delta_{\text{degradation}} + \delta_{\text{spike}}$$

* **$H_{\text{base}}(i, n)$ (비례 기준 함수)**:
  라이너 개수 $n \in [0, 20]$에 따라 약 $3.8\text{mm} \to 21.2\text{mm}$로 비례 증가하는 비선형 기준 테이블
* **$\varepsilon_{\text{thermal}}$ (열변형 산포)**:
  $z = \sqrt{-2\ln u_1}\cos(2\pi u_2) \implies \varepsilon \sim \mathcal{N}(0, \sigma_{\text{thermal}}^2)$
* **$\Delta_{\text{degradation}}$ (설비 열화 드리프트)**:
  $\Delta = k_{\text{drift}} \cdot \ln(1 + 0.05 \cdot N_{\text{total}}) \cdot (i - 1.5)$
* **$\delta_{\text{spike}}$ (순간 이상치)**:
  $P(\text{spike}) = 10\% \implies \pm (2.0 \sim 3.5)\text{mm}$

---

## 🛠️ 기술 스택 (Tech Stack)

- **Frontend Core**: Vanilla Modern JavaScript (ES6+), HTML5, Canvas & Dynamic SVG
- **Styling**: Tailwind CSS (Dark Cyber Industrial Theme), Glassmorphism UI
- **Data Visualization**: Chart.js (Real-time SPC Trend Chart)
- **Math Rendering**: KaTeX CDN
- **Icons**: Lucide Icons
- **Audio FX**: Web Audio API (Synthesized Haptic Feedback)
- **Architecture**: Zero-Dependency Single HTML File (초경량 무설치 웹 앱)

---

## 🚀 배포 링크

👉 **[https://jiu7021.github.io/side-sill-welding-sim/](https://jiu7021.github.io/side-sill-welding-sim/)**
