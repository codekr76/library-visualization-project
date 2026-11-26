# library-visualization-project

Vue 3와 TypeScript를 활용해 **2D SVG 도서관 평면도**와 **3D 서가 시각화**를 단계별로 구현하는 실습 프로젝트입니다.

## 주요 기술 스택
- **Frontend:** Vue 3 (`<script setup>`), TypeScript, Vite
- **2D 시각화:** SVG (`<svg>`, `<rect>`, `<text>`, `<g>`)
- **3D 시각화 (예정):** Three.js (`three`, `@types/three`)

## 진행 단계

### STEP 1. 2D 도서관 평면도 만들기 (완료)
- [x] **STEP 1-1:** Vue 3 + Vite + TypeScript 프로젝트 구성
- [x] **STEP 1-2:** SVG 좌표 기반 도서관 영역(A/B/C구역), 출입구, 서가 배치
- [x] **STEP 1-3:** 서가 ID 부여, 클릭 선택 상태 관리 및 선택된 서가 정보 패널 구현

### STEP 2. Three.js로 3D 서가 모델 불러오기 (진행 예정)
- [ ] `three` 및 `@types/three` 설치
- [ ] Scene, Camera, Renderer 기본 3D 뷰어 구성
- [ ] 3D 서가 모델(GLB) 로드 및 카메라 조작/상호작용 연동
