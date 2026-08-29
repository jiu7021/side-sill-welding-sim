# 사이드실 용접 검사구 H값 최적화 디지털 트윈 시뮬레이터
> **Side-Sill Welding Inspection Gauge H-Value Digital Twin Simulator (v2.5 PRO)**

자동차 차체 조립 공정의 사이드실(Side-Sill) 용접 시 발생하는 열응력 변형 및 조립 누적 공차(H값: 단차/간극)를 검사구 기준 최적의 라이너(Shim Plate) 조합으로 보정 및 최적화하는 **산업용 스마트 팩토리 디지털 트윈 시뮬레이터**입니다.

---

## 📸 핵심 시각화 및 주요 기능

1. **사이드실 & 검사구 2D 디지털 트윈 실시간 연동 (Interactive SVG)**
   - 홀별 라이너(심 플레이트) 수동 가감 시 두께 적층 물리 그래픽 실시간 렌더링
   - 검사구 레이저 프로브 측정 빔 및 합격(OK) / 규격 이탈(NG) 실시간 상태 피드백
2. **H값과 라이너 개수의 비례 물리 모델링**
   - 라이너 개수가 증가할수록 검사구 접촉 높이(H값)가 **비례하여 증가**하는 물리적 특성 구현
   - 목표 기준치: **10.0mm (공차 ±5.0mm : 5.0 ~ 15.0mm)**
3. **용접 열변형 & 설비 열화 오차 합성 엔진 (Synthetic Data Generator)**
   - Box-Muller 가우시안 정규분포 ($\sigma$) 기반 열변형 산포
   - 연속 런 누적에 따른 전극/지그 마모 열화 드리프트 ($\Delta_{\text{degrade}}$)
   - 순간 용접 스패터 이상치(Outlier Spike) 모델
4. **실시간 SPC 통계 및 공정 능력 분석 (Chart.js)**
   - 5회 연속 생산 런 시계열 트렌드 관리도
   - 공정 수율(Yield %) 및 공정 능력 지수($C_{pk}$) 실시간 산출
   - 최적 설정(Best Run) 자동 하이라이트 및 CSV 데이터 내보내기
5. **AI 원클릭 자동 최적화 탐색 (Auto-Tuning)**
   - 각 홀의 공차 중심값(10.0mm)에 수렴하는 최적의 심 라이너 조합을 원클릭으로 자동 탐색

---

## 📐 수학 및 물리 모델링 명세

$$H_{\text{measured}}(i, n) = H_{\text{base}}(i, n) + \varepsilon_{\text{thermal}} + \Delta_{\text{degradation}} + \delta_{\text{spike}}$$

* **$H_{\text{base}}(i, n)$ (비례 기준 함수)**:
  라이너 개수 $n \in [0, 20]$에 따라 약 $3.8\text{mm} \to 21.2\text{mm}$로 비례 증가하는 비선형 기준 테이블 (최적 라이너: 홀A 8개, 홀B 10개, 홀C 11개, 홀D 9개)
* **$\varepsilon_{\text{thermal}}$ (열변형 산포)**:
  $z = \sqrt{-2\ln u_1}\cos(2\pi u_2) \implies \varepsilon \sim \mathcal{N}(0, \sigma_{\text{thermal}}^2)$
* **$\Delta_{\text{degradation}}$ (설비 열화 드리프트)**:
  $\Delta = k_{\text{drift}} \cdot \ln(1 + 0.05 \cdot N_{\text{total}}) \cdot (i - 1.5)$
* **$\delta_{\text{spike}}$ (순간 이상치)**:
  $P(\text{spike}) = 10\% \implies \pm (2.0 \sim 3.5)\text{mm}$

> ⚠️ **Synthetic Data Notice**: 본 시뮬레이터의 계측 데이터는 실제 차체 센서 데이터가 아닌, 용접 공정 열역학 및 기구 공차를 모사한 **물리 기반 합성 시뮬레이션 데이터**입니다.

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

## 🚀 GitHub 레포지토리 추천 이름 및 배포 방법

### 💡 추천 레포지토리 이름
1. `side-sill-welding-sim` *(가장 직관적이고 표준적)*
2. `welding-gauge-optimizer` *(용접 검사구 최적화 강조)*
3. `smart-welding-digital-twin` *(디지털 트윈 & 스마트 팩토리)*
4. `h-value-liner-optimizer` *(H값 심 라이너 튜닝)*

### 📦 GitHub Pages 1분 무료 배포 가이드
```bash
# 1. 로컬 저장소 초기화 및 커밋
git init
git add .
git commit -m "feat: 사이드실 용접 검사구 H값 최적화 디지털 트윈 시뮬레이터 구축"

# 2. 원격 저장소 연결 후 푸시 (GitHub에서 레포지토리 생성 후)
git branch -M main
git remote add origin https://github.com/당신의깃허브아이디/side-sill-welding-sim.git
git push -u origin main
```

1. GitHub 레포지토리 상단의 **Settings** 메뉴로 이동합니다.
2. 좌측 메뉴의 **Pages**를 클릭합니다.
3. **Build and deployment > Branch** 항목에서 `main` 브랜치 및 `/ (root)`를 선택하고 **Save**를 누릅니다.
4. 약 1분 후 `https://당신의아이디.github.io/side-sill-welding-sim/` 링크로 무료 배포가 완료됩니다.
