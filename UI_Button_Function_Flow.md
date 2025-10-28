# BatteryDataTool UI 버튼 기능 및 함수 호출 Flow

## 목차
1. [UI 구조 개요](#ui-구조-개요)
2. [탭별 버튼 분류](#탭별-버튼-분류)
3. [버튼별 함수 Flow](#버튼별-함수-flow)
4. [공통 데이터 처리 Flow](#공통-데이터-처리-flow)

---

## UI 구조 개요

### 메인 윈도우
- **클래스**: `Ui_sitool`
- **프레임워크**: PyQt6
- **버전**: BatteryDataTool v250819
- **해상도**: 1913 x 1005 (1920x1080 기준)

### 탭 구조 (총 8개)

```
TabWidget
├── Tab 1: 현황 (충방전기 현황)
├── Tab 2: 사이클데이터 (Cycle/Profile 분석)
├── Tab 3: 패턴 수정
├── Tab 4: SET 로그
├── Tab 5: dQdV 분석
├── Tab 6: 수명 예측 (EU)
├── Tab 7: 승인 수명
└── Tab 8: Full Cell Fitting
```

---

## 탭별 버튼 분류

### 1️⃣ Tab 1: 현황 (Cycler Status)

#### 충방전기 연결 버튼
| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| TOYO | `mount_toyo` | `mount_toyo_button()` | TOYO 충방전기 연결 |
| PNE 1 | `mount_pne_1` | `mount_pne1_button()` | PNE 충방전기 1 연결 |
| PNE 2 | `mount_pne_2` | `mount_pne2_button()` | PNE 충방전기 2 연결 |
| PNE 3 | `mount_pne_3` | `mount_pne3_button()` | PNE 충방전기 3 연결 |
| PNE 4 | `mount_pne_4` | `mount_pne4_button()` | PNE 충방전기 4 연결 |
| PNE 5 | `mount_pne_5` | `mount_pne5_button()` | PNE 충방전기 5 연결 |
| 전체 연결 | `mount_all` | `mount_all_button()` | 모든 충방전기 연결 |
| 전체 해제 | `unmount_all` | `unmount_all_button()` | 모든 충방전기 연결 해제 |

#### 콤보박스
| 객체명 | 이벤트 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| `tb_room` | `currentIndexChanged` | `tb_room_combobox()` | 실험실 선택 (R5 15F/3F B-1/B-2/A/전체) |
| `tb_cycler` | `currentIndexChanged` | `tb_cycler_combobox()` | 충방전기 선택 (Toyo1-5/PNE1-5) |
| `tb_info` | `currentIndexChanged` | `tb_info_combobox()` | 표시 정보 선택 (채널/파일명/스케쥴명/온도 등) |

---

### 2️⃣ Tab 2: 사이클데이터

#### 2-1. Cycle 탭

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| Tab Reset | `cycle_tab_reset` | `cycle_tab_reset_confirm_button()` | 사이클 탭 초기화 |
| 개별 Cycle | `indiv_cycle` | `indiv_cyc_confirm_button()` | 개별 사이클 분석 |
| 통합 Cycle | `overall_cycle` | `overall_cyc_confirm_button()` | 통합 사이클 분석 |
| 연결 Cycle | `link_cycle` | `link_cyc_confirm_button()` | 연결 사이클 분석 |
| 연결 Cycle 여러개 개별 | `link_cycle_indiv` | `link_cyc_indiv_confirm_button()` | 연결 사이클 개별 표시 |
| 연결 Cycle 여러개 통합 | `link_cycle_overall` | `link_cyc_overall_confirm_button()` | 연결 사이클 통합 표시 |
| 신뢰성 Cycle | `AppCycConfirm` | `app_cyc_confirm_button()` | 신뢰성 데이터 분석 |

#### 2-2. Profile 탭

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| 충전 Step 확인 | `StepConfirm` | `step_confirm_button()` | Step 충전 프로파일 분석 |
| 율별 충전 확인 | `RateConfirm` | `rate_confirm_button()` | Rate별 충전 프로파일 |
| 충전 분석 | `ChgConfirm` | `chg_confirm_button()` | 충전 프로파일 상세 분석 |
| 방전 분석 | `DchgConfirm` | `dchg_confirm_button()` | 방전 프로파일 상세 분석 |
| HPPC/GITT | `ContinueConfirm` | `continue_confirm_button()` | 연속 프로파일 분석 |
| DCIR | `DCIRConfirm` | `dcir_confirm_button()` | DCIR 프로파일 분석 |
| 사이클 통합 | `CycProfile` | - | 사이클별 프로파일 통합 |
| 셀별 통합 | `CellProfile` | - | 셀별 프로파일 통합 |

---

### 3️⃣ Tab 3: 패턴 수정

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| 패턴 리스트 Load | `ptn_load` | `ptn_load_button()` | PNE 패턴 리스트 로드 |
| 전류 일괄 변경 | `chg_ptn` | `ptn_change_pattern_button()` | C-rate 기준 전류 변경 |
| 충방전 전류 바꾸기 | `chg_ptn_refi` | `ptn_change_refi_button()` | 특정 전류값 변경 |
| 충전 전압 바꾸기 | `chg_ptn_chgv` | `ptn_change_chgv_button()` | 충전 CV 전압 변경 |
| 방전 전압 바꾸기 | `chg_ptn_dchgv` | `ptn_change_dchgv_button()` | 방전 CV 전압 변경 |
| Step 끝전류 바꾸기 | `chg_ptn_endi` | `ptn_change_endi_button()` | Step 종료 전류 변경 |
| Step 끝전압 바꾸기 | `chg_ptn_endv` | `ptn_change_endv_button()` | Step 종료 전압 변경 |
| Step 시간 바꾸기 | `chg_ptn_step` | `ptn_change_step_button()` | Step 시간 변경 |

---

### 4️⃣ Tab 4: SET 로그

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| Tab Reset | `SETTabReset` | `set_tab_reset_button()` | SET 탭 초기화 |
| ACT Log 확인 | `SetlogConfirm` | `set_log_confirm_button()` | ACT 로그 프로파일 확인 |
| Profile 확인 | `SetConfirm` | `set_confirm_button()` | SET 프로파일 확인 |
| Cycle 확인 | `SetCycle` | `set_cycle_button()` | SET 사이클 확인 |
| ECT 단락 확인 | `ECTShort` | `ect_short_button()` | ECT 단락 체크 |
| ECT SOC 확인 | `ECTSOC` | `ect_soc_button()` | ECT SOC 에러 확인 |
| ECT Profile 확인 | `ECTSetProfile` | `ect_set_profile_button()` | ECT 프로파일 확인 |
| ECT Cycle 확인 | `ECTSetCycle` | `ect_set_cycle_button()` | ECT 사이클 확인 |

---

### 5️⃣ Tab 5: dQdV 분석

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| 재료 dQdV 선택 | `mat_dvdq_btn` | `dvdq_material_button()` | 양극/음극 재료 dQdV 파일 로드 |
| Profile dQdV 선택 | `pro_dvdq_btn` | `dvdq_profile_button()` | 실측 프로파일 dQdV 계산 |
| 초기값 Reset | `dvdq_ini_reset` | `dvdq_ini_reset_button()` | Fitting 초기값 리셋 |
| Fitting | `dvdq_fitting` | `dvdq_fitting_button()` | Full Cell Fitting 실행 |
| Fitting 2 | `dvdq_fitting_2` | `dvdq_fitting2_button()` | Full Cell Fitting 실행 (개선) |

---

### 6️⃣ Tab 6: 수명 예측 (EU)

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| Tab Reset | `TabReset_eu` | `eu_tab_reset_button()` | EU 탭 초기화 |
| Parameter Reset | `ParameterReset_eu` | `eu_parameter_reset_button()` | 파라미터 초기화 |
| Load Parameter | `load_cycparameter_eu` | `eu_load_cycparameter_button()` | 파라미터 로드 |
| Save Parameter | `save_cycparameter_eu` | `eu_save_cycparameter_button()` | 파라미터 저장 |
| Fitting 확인 | `FitConfirm_eu` | `eu_fitting_confirm_button()` | 수명 예측 Fitting |
| 상수 Fitting | `ConstFitConfirm_eu` | `eu_constant_fitting_confirm_button()` | 상수 기반 Fitting |
| 개별 상수 Fitting | `indivConstFitConfirm_eu` | `eu_indiv_constant_fitting_confirm_button()` | 개별 상수 Fitting |

---

### 7️⃣ Tab 7: 승인 수명

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| Tab Reset | `AppCycleTabReset` | `app_cycle_tab_reset_button()` | 승인 수명 탭 초기화 |
| Load Parameter | `load_cycparameter` | `load_cycparameter_button()` | 파라미터 로드 |
| Folder 수명 예측 | `folderappcycestimation` | `folder_approval_cycle_estimation_button()` | 폴더 기반 수명 예측 |
| Path 수명 예측 | `pathappcycestimation` | `path_approval_cycle_estimation_button()` | 경로 기반 수명 예측 |

---

### 8️⃣ Tab 8: Full Cell Fitting

| 버튼명 | 객체명 | 연결 함수 | 설명 |
|--------|--------|-----------|------|
| Tab Reset | `SimulTabResetConfirm` | `simulation_tab_reset_confirm_button()` | Fitting 탭 초기화 |
| Simulation 확인 | `SimulConfirm` | `simulation_confirm_button()` | Full Cell 시뮬레이션 실행 |

---

## 버튼별 함수 Flow

### 🔵 개별 Cycle 분석 Flow

**버튼**: `indiv_cycle` → **함수**: `indiv_cyc_confirm_button()`

```
[사용자 입력]
  ↓
indiv_cyc_confirm_button()
  ↓
cyc_ini_set()  ← 초기 설정 (C-rate, 용량, 축 범위)
  ↓
pne_path_setting()  ← 경로 설정
  ↓
[폴더 순회]
  ├─ check_cycler()  ← PNE/TOYO 구분
  │
  ├─ [TOYO인 경우]
  │   └─ toyo_cycle_data()
  │       ├─ toyo_cycle_import()
  │       ├─ toyo_min_cap()
  │       └─ [용량/효율/온도/DCIR 계산]
  │
  └─ [PNE인 경우]
      └─ pne_cycle_data()
          ├─ pne_min_cap()
          ├─ pne_data() 또는 pne_continue_data()
          ├─ pne_search_cycle()
          └─ [용량/효율/온도/RSS/DCIR 계산]
  ↓
graph_output_cycle()  ← 6개 그래프 생성
  ├─ graph_cycle() × 4
  ├─ graph_cycle_empty() × 2
  └─ graph_base_parameter()
  ↓
[저장 옵션 체크시]
  └─ output_data()  → Excel 저장
  ↓
[PyQt6 TabWidget에 그래프 추가]
  └─ FigureCanvas + NavigationToolbar
```

**주요 데이터 흐름**:
1. 폴더 선택 → 채널별 순회
2. CSV 파일 읽기 (PNE: Restore 폴더, TOYO: 직접)
3. 사이클 데이터 추출 및 계산
4. 6개 그래프 생성 (용량/효율/온도/DCIR/전압/RSS)
5. Excel 저장 (옵션)

---

### 🔵 통합 Cycle 분석 Flow

**버튼**: `overall_cycle` → **함수**: `overall_cyc_confirm_button()`

```
overall_cyc_confirm_button()
  ↓
cyc_ini_set()
  ↓
pne_path_setting()
  ↓
[모든 폴더 데이터 통합]
  ├─ [폴더별 처리]
  │   ├─ check_cycler()
  │   ├─ toyo_cycle_data() or pne_cycle_data()
  │   └─ [데이터 병합]
  ↓
[1개 그래프에 모든 데이터 표시]
  ├─ graph_cycle() × N개 데이터
  └─ 범례: extract_text_in_brackets() 또는 사용자 지정
  ↓
output_data()  ← Excel 저장 (옵션)
```

**개별 vs 통합 차이점**:
- **개별**: 채널마다 별도 그래프 생성
- **통합**: 모든 채널을 1개 그래프에 오버레이

---

### 🔵 충전 Step 프로파일 Flow

**버튼**: `StepConfirm` → **함수**: `step_confirm_button()`

```
step_confirm_button()
  ↓
[입력 파라미터 읽기]
  ├─ 사이클 번호 (stepnum)
  ├─ 용량 (mincapacity)
  ├─ Cutoff (cutoff)
  └─ 전압 범위 (volrngyhl, volrngyll, volrnggap)
  ↓
[경로 설정]
  └─ pne_path_setting()
  ↓
[충방전기별 처리]
  ├─ [TOYO]
  │   └─ toyo_step_Profile_data()
  │       ├─ toyo_Profile_import()
  │       └─ [Step별 전압/시간/용량 추출]
  │
  └─ [PNE]
      └─ pne_step_Profile_data()
          ├─ pne_data()
          ├─ pne_search_cycle()
          └─ [Step별 데이터 분리]
  ↓
graph_step()  ← Step 프로파일 그래프
  ├─ 시간 vs 전압
  ├─ 시간 vs 전류
  └─ 용량 vs 전압
  ↓
output_para_fig()  ← 그래프 저장 (옵션)
```

**Step 프로파일 특징**:
- CC, CV 구간 구분
- Step별 색상 다르게 표시
- 사용자 지정 Step 선택 가능 (예: "2 3 5-8")

---

### 🔵 충전/방전 분석 Flow

**버튼**: `ChgConfirm` / `DchgConfirm` → **함수**: `chg_confirm_button()` / `dchg_confirm_button()`

```
chg/dchg_confirm_button()
  ↓
[입력 파라미터]
  ├─ 사이클 범위 (stepnum: "1-5 10 15-20")
  ├─ Smooth 계수 (smooth)
  ├─ Cutoff (cutoff)
  └─ dQdV 스케일 (dqdvscale)
  ↓
convert_steplist()  ← "1-5" → [1,2,3,4,5]
  ↓
[충방전기별 처리]
  ├─ [TOYO]
  │   └─ toyo_chg_Profile_data() / toyo_dchg_Profile_data()
  │       ├─ toyo_Profile_import()
  │       ├─ [충전 CC/CV 구분]
  │       └─ [dQdV 계산]
  │
  └─ [PNE]
      └─ pne_chg_Profile_data() / pne_dchg_Profile_data()
          ├─ pne_data()
          ├─ [충전/방전 구간 분리]
          └─ [dQdV 미분 계산]
  ↓
[4개 그래프 생성]
  ├─ graph_profile() - 전압 프로파일
  ├─ graph_profile() - 전류 프로파일
  ├─ graph_profile() - dQdV (용량 vs 전압 미분)
  └─ graph_profile() - 용량 vs 전압
  ↓
output_para_fig()  ← 저장 (옵션)
```

**dQdV 계산**:
- 충전: `dQ/dV = ΔCapacity / ΔVoltage`
- Smoothing: Savitzky-Golay filter 또는 이동평균
- Peak 분석: 전극 반응 특성 파악

---

### 🔵 HPPC/GITT 분석 Flow

**버튼**: `ContinueConfirm` → **함수**: `continue_confirm_button()`

```
continue_confirm_button()
  ↓
[입력 파라미터]
  ├─ 시작 사이클 (inicycle_continue)
  ├─ 종료 사이클 (endcycle_continue)
  └─ 용량 (mincapacity)
  ↓
[연속 프로파일 데이터 읽기]
  ├─ [TOYO]
  │   └─ toyo_Profile_continue_data()
  │       └─ [inicycle ~ endcycle CSV 병합]
  │
  └─ [PNE]
      └─ pne_Profile_continue_data()
          ├─ pne_search_cycle()
          └─ [SaveData CSV 연속 읽기]
  ↓
[프로파일 분석]
  ├─ 전압 vs 시간
  ├─ 전류 vs 시간
  ├─ SOC 계산 (적분)
  └─ Pulse 구간 검출
  ↓
graph_continue()  ← 연속 그래프
  ├─ 전압 프로파일
  ├─ SOC vs 전압
  └─ SOC vs 저항
  ↓
output_para_fig()
```

**HPPC vs GITT**:
- **HPPC**: Hybrid Pulse Power Characterization (짧은 펄스)
- **GITT**: Galvanostatic Intermittent Titration Technique (긴 휴지)

---

### 🔵 DCIR 프로파일 Flow

**버튼**: `DCIRConfirm` → **함수**: `dcir_confirm_button()`

```
dcir_confirm_button()
  ↓
[입력 파라미터]
  ├─ 사이클 범위
  └─ 용량
  ↓
[PNE 전용 함수]
  └─ pne_dcir_Profile_data()
      ├─ pne_search_cycle()
      ├─ [Pulse 구간 검출]
      │   ├─ 충전 Pulse (10s)
      │   ├─ 방전 Pulse (10s)
      │   └─ Rest 구간
      ├─ [전압 변화 계산]
      │   └─ R = ΔV / I
      ├─ [SOC 계산]
      │   └─ 용량 적분
      └─ [SOC별 DCIR 매핑]
  ↓
[그래프 생성]
  ├─ graph_soc_dcir() - SOC vs DCIR
  ├─ graph_dcir() - OCV vs DCIR
  └─ graph_continue() - 전압/전류 프로파일
  ↓
output_para_fig()
```

**DCIR 계산 방식**:
1. **Pulse DCIR**: `R = (V_pulse_end - V_pulse_start) / I_pulse`
2. **RSS DCIR**: SOC 30/50/70에서 1s Pulse 저항
3. **OCV DCIR**: Open Circuit Voltage 기반 저항

---

### 🔵 패턴 수정 Flow

**버튼**: `ptn_load` → **함수**: `ptn_load_button()`

```
ptn_load_button()
  ↓
[PNE Access DB 연결]
  └─ pyodbc.connect("Cycler_Schedule_2000.mdb")
  ↓
[패턴 리스트 쿼리]
  └─ SELECT * FROM Schedule_List
  ↓
[UI 리스트 박스에 표시]
  └─ 패턴 선택 가능
```

**패턴 변경 Flow**:

```
ptn_change_pattern_button()  ← C-rate 기준 일괄 변경
  ↓
[입력 파라미터]
  ├─ 방전 최대 C-rate (ptn_crate)
  ├─ 변경할 용량 (ptn_capacity)
  └─ 선택된 패턴 리스트
  ↓
[Access DB 업데이트]
  ├─ 충전 전류 = 용량 × C-rate
  ├─ 방전 전류 = 용량 × C-rate
  └─ UPDATE Schedule_Step SET ...
  ↓
[Coin Cell 처리]
  └─ 소수점 전류 (4.5mAh 등)
  ↓
disconnect_change()  ← 버튼 색상 빨간색 (수정됨)
```

**패턴 수정 종류**:
1. **전류 일괄 변경**: C-rate × 용량
2. **특정 전류 변경**: 기존값 → 신규값
3. **충전 전압 변경**: CV 전압 조정
4. **방전 전압 변경**: Cut-off 전압 조정
5. **Step 종료 조건 변경**: 전류/전압/시간

---

### 🔵 SET 로그 확인 Flow

**버튼**: `SetlogConfirm` → **함수**: `set_log_confirm_button()`

```
set_log_confirm_button()
  ↓
[파일 선택]
  └─ filedialog.askdirectory()
  ↓
[SET 로그 파일 읽기]
  ├─ ACT_Log.csv
  ├─ ECT_BatteryStatus_Log.csv
  └─ set_act_log_Profile()
      ├─ 사이클 번호 추출
      ├─ 실시간 프로파일 파싱
      └─ 데이터 정리
  ↓
[그래프 생성]
  ├─ graph_soc_set() - SOC vs 전압
  ├─ graph_set_profile() - 시간 프로파일
  └─ graph_set_guide() - 기준선
  ↓
output_para_fig()
```

**SET 로그 특징**:
- **ACT**: Accelerated Cycle Test (가속 사이클)
- **ECT**: Endurance Cycle Test (내구 사이클)
- 실시간 데이터 → SOC 추정 → 에러 계산

---

### 🔵 ECT SOC 에러 확인 Flow

**버튼**: `ECTSOC` → **함수**: `ect_soc_button()`

```
ect_soc_button()
  ↓
[ECT 로그 읽기]
  └─ set_act_ect_battery_status_cycle()
      ├─ 실제 SOC (OCV 기반)
      ├─ ECT 추정 SOC (알고리즘)
      └─ 에러 계산 = |실제 - 추정|
  ↓
[통계 계산]
  ├─ 최대 에러 (socerrormax)
  ├─ 평균 에러 (socerroravg)
  └─ SOC별 에러 분포
  ↓
graph_soc_err()  ← SOC별 에러 막대 그래프
  ├─ X축: SOC (0~100%)
  ├─ Y축: 에러 (%)
  └─ 5% 단위 평균
  ↓
UI 텍스트 박스 업데이트
  ├─ socerrormax.setText()
  └─ socerroravg.setText()
```

---

### 🔵 dQdV Fitting Flow

**버튼**: `dvdq_fitting` → **함수**: `dvdq_fitting_button()`

```
dvdq_fitting_button()
  ↓
[재료 데이터 로드]
  ├─ 양극 dQdV (ca_mat_dvdq_path)
  ├─ 음극 dQdV (an_mat_dvdq_path)
  └─ 실측 프로파일 (pro_dvdq_path)
  ↓
[초기 파라미터]
  ├─ ca_mass (양극 질량)
  ├─ ca_slip (양극 슬립)
  ├─ an_mass (음극 질량)
  └─ an_slip (음극 슬립)
  ↓
generate_params()  ← 파라미터 범위 생성
  ↓
[최적화 루프]
  └─ FOR EACH param_set:
      ├─ generate_simulation_full()
      │   ├─ 양극 dQdV × ca_mass
      │   ├─ 음극 dQdV × an_mass
      │   ├─ Slip 보정
      │   └─ Full Cell dQdV 합성
      │
      ├─ RMS 계산 (실측 vs 시뮬레이션)
      └─ 최소 RMS 파라미터 저장
  ↓
[결과 출력]
  ├─ 최적 파라미터 표시
  ├─ RMS 값 (dvdq_rms)
  └─ 그래프 오버레이
      ├─ 실측 dQdV
      └─ Fitting dQdV
```

**Fitting 원리**:
- Full Cell dQdV = Cathode dQdV + Anode dQdV (전압 축 이동 고려)
- 최소 제곱법으로 최적 Mass/Slip 찾기

---

### 🔵 수명 예측 (EU) Flow

**버튼**: `FitConfirm_eu` → **함수**: `eu_fitting_confirm_button()`

```
eu_fitting_confirm_button()
  ↓
[사이클 데이터 로드]
  └─ pne_simul_cycle_data() or toyo_cycle_data()
      ├─ 용량 비율 (Capacity Ratio)
      ├─ 사이클 번호
      └─ 온도 정보
  ↓
[수명 예측 모델]
  ├─ Power Law Model
  │   └─ Cap(N) = a - b × N^c
  │
  ├─ Exponential Model
  │   └─ Cap(N) = a × exp(-b × N) + c
  │
  └─ Polynomial Model
      └─ Cap(N) = a + b×N + c×N^2 + d×N^3
  ↓
[Fitting 실행]
  └─ scipy.optimize.curve_fit()
      ├─ 초기값: 사용자 입력 또는 자동
      ├─ 최적화: Levenberg-Marquardt
      └─ R² 계산
  ↓
[예측]
  ├─ EOL 사이클 (80% 도달)
  ├─ 신뢰구간 (±σ)
  └─ 외삽 (Extrapolation)
  ↓
graph_eu_set()  ← EU 그래프
  ├─ 실측 데이터
  ├─ Fitting 곡선
  └─ 예측 구간
  ↓
[파라미터 저장]
  └─ JSON or Excel
```

**EU 수명 예측 특징**:
- EU 규정: 1000 사이클 또는 80% 용량 유지
- 온도/C-rate별 개별 Fitting
- Arrhenius 법칙 적용 가능

---

### 🔵 승인 수명 예측 Flow

**버튼**: `folderappcycestimation` → **함수**: `folder_approval_cycle_estimation_button()`

```
folder_approval_cycle_estimation_button()
  ↓
[다중 폴더 선택]
  └─ multi_askopendirnames()
  ↓
[각 폴더 사이클 데이터 로드]
  ├─ pne_simul_cycle_data_file() or toyo_cycle_data()
  └─ [최소 사이클 기준 정렬]
  ↓
[Hybrid 수명 계산]
  ├─ 초기 급격 열화 구간 (0~50 사이클)
  ├─ 선형 열화 구간 (50~현재)
  └─ 외삽 (현재~EOL)
  ↓
[통계 계산]
  ├─ 평균 EOL 사이클
  ├─ 표준편차
  ├─ 최소/최대 EOL
  └─ 신뢰구간 (95%)
  ↓
graph_default()  ← 통합 그래프
  ├─ 모든 셀 데이터
  ├─ 평균 곡선
  └─ 예측 구간
  ↓
[Excel 출력]
  └─ output_data()
      ├─ 셀별 EOL
      ├─ 통계값
      └─ 그래프
```

**승인 수명 특징**:
- 다수 셀 데이터 통합 분석
- 가장 짧은 수명 기준 (최악 케이스)
- Hybrid 모델: 초기 + 선형

---

### 🔵 Full Cell Simulation Flow

**버튼**: `SimulConfirm` → **함수**: `simulation_confirm_button()`

```
simulation_confirm_button()
  ↓
[재료 데이터 로드]
  ├─ Cathode Half Cell (CSV)
  ├─ Anode Half Cell (CSV)
  └─ Real Full Cell (CSV)
  ↓
[파라미터 입력]
  ├─ Cathode Mass & Slip
  ├─ Anode Mass & Slip
  └─ Full Cell 최대 용량
  ↓
generate_simulation_full()
  ├─ [양극 Half Cell 보정]
  │   └─ V_ca = V_ca_raw + ca_slip
  │
  ├─ [음극 Half Cell 보정]
  │   └─ V_an = V_an_raw + an_slip
  │
  ├─ [Full Cell 전압 계산]
  │   └─ V_full = V_ca - V_an
  │
  ├─ [용량 정규화]
  │   ├─ Q_ca = ca_mass × Q_ca_raw
  │   └─ Q_an = an_mass × Q_an_raw
  │
  └─ [Full Cell 프로파일 합성]
      └─ 용량 제한: min(Q_ca, Q_an)
  ↓
[RMS 계산]
  └─ RMS = sqrt(Σ(V_real - V_sim)² / N)
  ↓
graph_simulation()  ← 시뮬레이션 그래프
  ├─ Real Full Cell (검은색)
  ├─ Simulated Full Cell (빨간색)
  └─ Cathode/Anode Half Cell (파란색/녹색)
  ↓
[최적화 옵션]
  └─ scipy.optimize.minimize()
      ├─ 목적함수: RMS 최소화
      └─ 제약조건: Mass/Slip 범위
```

**Full Cell Fitting 활용**:
- 전극 설계 최적화
- 용량 밸런싱
- Slip 효과 분석

---

## 공통 데이터 처리 Flow

### 📊 PNE 데이터 처리 파이프라인

```
[PNE 폴더 구조]
테스트폴더/
├── Pattern/          ← PNE 식별자
├── Restore/          ← CSV 데이터
│   ├── SaveData0001.csv
│   ├── SaveData0002.csv
│   ├── SaveEndData.csv
│   └── savingFileIndex_start.csv
└── 채널폴더/

[데이터 읽기 Flow]
pne_min_cap()  ← 용량 자동 추출
  ↓
pne_search_cycle()  ← 사이클 범위 → 파일 인덱스
  ├─ SaveEndData.csv 읽기 (사이클 정보)
  ├─ savingFileIndex_start.csv 읽기 (파일 매핑)
  └─ binary_search() - 파일 인덱스 찾기
  ↓
pne_data() or pne_continue_data()
  ├─ SaveData CSV 병합
  ├─ 컬럼 파싱 (Time/Vol/Curr/Temp/Step...)
  └─ DataFrame 생성
  ↓
pne_cycle_data()
  ├─ [용량 계산]
  │   ├─ 충전 용량 = ∫ I_chg dt
  │   └─ 방전 용량 = ∫ I_dchg dt
  │
  ├─ [효율 계산]
  │   ├─ 충방 효율 = Q_dchg / Q_chg
  │   └─ 방충 효율 = Q_chg / Q_dchg
  │
  ├─ [온도 추출]
  │   └─ 사이클 평균 온도
  │
  ├─ [DCIR 계산]
  │   ├─ Pulse DCIR (10s 방전)
  │   ├─ RSS DCIR (SOC별 1s Pulse)
  │   └─ R = ΔV / I
  │
  └─ [Rest End Voltage]
      └─ 방전 후 휴지 전압
  ↓
DataFrame.NewData
  ├─ Dchg: 방전 용량 비율
  ├─ Chg: 충전 용량 비율
  ├─ Eff: 충방 효율
  ├─ Eff2: 방충 효율
  ├─ Temp: 온도
  ├─ dcir: DCIR 또는 RSS
  ├─ dcir2: 1s DCIR (옵션)
  ├─ RndV: Rest End Voltage
  └─ AvgV: Average Voltage
```

---

### 📊 TOYO 데이터 처리 파이프라인

```
[TOYO 폴더 구조]
테스트폴더/
└── 채널번호/
    ├── 000001  (사이클 1 CSV)
    ├── 000002  (사이클 2 CSV)
    └── ...

[데이터 읽기 Flow]
toyo_min_cap()  ← 용량 추출
  ↓
toyo_cycle_import()
  ├─ 각 사이클 파일 읽기
  └─ 컬럼 파싱 (Time/V/I/Ah/T...)
  ↓
toyo_cycle_data()
  ├─ [용량 계산]
  │   ├─ Step별 용량 합산
  │   ├─ CC 충전 용량
  │   ├─ CV 충전 용량 ← **TOYO 특징**
  │   └─ CC 방전 용량
  │
  ├─ [효율 계산]
  │   └─ (CC+CV) / 방전
  │
  ├─ [온도 추출]
  │   └─ 사이클 평균
  │
  └─ [DCIR 계산]
      └─ Pulse 구간 검출 (전류 변화 ±10% 기준)
  ↓
DataFrame.NewData (PNE와 동일 구조)
```

**PNE vs TOYO 차이점**:

| 항목 | PNE | TOYO |
|------|-----|------|
| 파일 구조 | Restore/*.csv (통합) | 채널/사이클별 개별 CSV |
| 사이클 검색 | savingFileIndex + 이진탐색 | 파일명 순차 읽기 |
| CV 용량 | Step 구분 명확 | CC/CV 수동 구분 필요 |
| DCIR | RSS 옵션 다양 | 기본 Pulse만 |
| 속도 | 느림 (대용량 병합) | 빠름 (개별 파일) |

---

### 📊 그래프 생성 Flow

```
[그래프 타입별 함수]

1. 사이클 그래프
   graph_cycle() / graph_cycle_empty()
   ├─ scatter plot
   ├─ 색상: graphcolor 리스트 순환
   └─ X축 자동 스케일링

2. 프로파일 그래프
   graph_profile()
   ├─ line plot
   ├─ X축: 시간 or 용량
   └─ Y축: 전압 or 전류

3. SOC 그래프
   graph_soc_continue() / graph_soc_dcir()
   ├─ X축: SOC (0~100%)
   └─ Y축: 전압/저항

4. SET 그래프
   graph_soc_set() / graph_soc_err()
   ├─ scatter: 전체 데이터
   └─ bar: 평균 에러

[공통 처리]
graph_base_parameter()
  ├─ xlabel / ylabel (굵게, 12pt)
  ├─ grid (점선)
  └─ tick direction (안쪽)

[PyQt6 통합]
FigureCanvas(fig) → TabWidget
  ├─ matplotlib.figure.Figure
  ├─ NavigationToolbar2QT (확대/이동/저장)
  └─ QtWidgets.QVBoxLayout
```

---

### 📊 Excel 저장 Flow

```
[저장 옵션 체크시]
filedialog.asksaveasfilename()
  ↓
pd.ExcelWriter(filename, engine="xlsxwriter")
  ↓
FOR EACH 데이터셋:
  output_data()
    ├─ df.to_excel(writer, sheet_name)
    ├─ 열 위치: writecolno
    ├─ 행 위치: start_row
    └─ 헤더: colname + headername
  ↓
writer.close()

[저장 데이터]
├─ Sheet "방전용량": Dchg
├─ Sheet "충방효율": Eff
├─ Sheet "DCIR": dcir
├─ Sheet "RSS": (옵션)
└─ Sheet "온도": Temp
```

---

### 📊 진행률 계산

```
progress(count1, max1, count2, max2, count3, max3)
  ├─ 3단계 중첩 계산
  ├─ 폴더 (count1/max1)
  ├─ 채널 (count2/max2)
  └─ 데이터 (count3/max3)
  ↓
progressBar.setValue(int(progressdata))
  └─ PyQt6 ProgressBar (0~100%)
```

---

## 데이터 구조 요약

### PNE CSV 구조 (SaveData)

```csv
[컬럼]
0: Index
1: Time (datetime or seconds)
2: Voltage (mV)
3: Current (mA)
4: Step
5: Capacity (mAh)
6: Temperature (°C)
...
27: Cycle Number  ← 중요: 사이클 구분
```

### TOYO CSV 구조

```csv
[컬럼]
Time, V, I, Ah, Wh, T, Step, ...
```

### NewData DataFrame 구조

```python
df.NewData (pd.DataFrame)
├─ index: 사이클 번호 (1, 2, 3, ...)
├─ Dchg: 방전 용량 비율 (0.0~1.0)
├─ Chg: 충전 용량 비율
├─ Eff: 충방 효율 (0.99~1.01)
├─ Eff2: 방충 효율
├─ Temp: 온도 (°C)
├─ dcir: DCIR 또는 RSS (mΩ)
├─ dcir2: 1s DCIR (옵션)
├─ RndV: Rest End Voltage (V)
├─ AvgV: Average Voltage (V)
├─ rssocv: RSS OCV (옵션)
├─ rssccv: RSS CCV (옵션)
├─ soc70_dcir: SOC 70% DCIR (옵션)
└─ soc70_rss_dcir: SOC 70% RSS (옵션)
```

---

## 주요 설정 파라미터

### 사이클 분석 파라미터

| 파라미터 | 객체명 | 기본값 | 설명 |
|----------|--------|--------|------|
| 용량 설정 방식 | `inicaprate` | 라디오 버튼 | 1) Cyclepath 용량, 2) 테스트 이름 용량, 3) Crate 기준 |
| C-rate | `ratetext` | 0.2 | 초기 C-rate |
| 용량 직접 입력 | `capacitytext` | 58 | 용량 (mAh) |
| X축 최대 | `tcyclerng` | 0 (자동) | 사이클 X축 범위 |
| Y축 최소 | `tcyclerngyll` | 0.65 | 용량 Y축 하한 |
| Y축 최대 | `tcyclerngyhl` | 1.10 | 용량 Y축 상한 |
| DCIR scale | `dcirscale` | 0 (자동) | DCIR 축 배율 |

### 프로파일 분석 파라미터

| 파라미터 | 객체명 | 기본값 | 설명 |
|----------|--------|--------|------|
| 사이클 선택 | `stepnum` | "2" | 분석할 사이클 (띄어쓰기 or -) |
| Smooth | `smooth` | 0 (자동) | Smoothing 계수 |
| Cutoff | `cutoff` | 0 | 전류/전압 cutoff |
| 전압 Y축 하한 | `volrngyll` | 2.5 | 전압 범위 (V) |
| 전압 Y축 상한 | `volrngyhl` | 4.7 | 전압 범위 (V) |
| 전압 Y축 간격 | `volrnggap` | 0.1 | 전압 눈금 간격 |
| dQdV 축 | `dqdvscale` | 1 | dQdV 스케일 |

### 패턴 수정 파라미터

| 파라미터 | 객체명 | 기본값 | 설명 |
|----------|--------|--------|------|
| 패턴 경로 | `ptn_ori_path` | PNE DB 경로 | Access DB 파일 |
| 방전 C-rate | `ptn_crate` | 1.0 | 최대 C-rate |
| 용량 기준 | `ptn_capacity` | 4000 | 용량 (mAh) |
| 기존 전류 | `ptn_refi_pre` | 2000 | 변경 전 전류 (mA) |
| 변경 전류 | `ptn_refi_after` | 1000 | 변경 후 전류 (mA) |
| 기존 충전 전압 | `ptn_chgv_pre` | 4470 | 변경 전 전압 (mV) |
| 변경 충전 전압 | `ptn_chgv_after` | 4500 | 변경 후 전압 (mV) |

---

## 체크박스 옵션

### 사이클 분석 옵션

| 체크박스 | 객체명 | 설명 |
|----------|--------|------|
| 지정Path사용 | `chk_cyclepath` | cyclepath 파일로 경로 지정 |
| PNE DCIR | `dcirchk` | SOC100 10s 방전 Pulse |
| PNE 10s DCIR | `pulsedcir` | SOC5, 50 10s 방전 Pulse |
| PNE RSS DCIR | `mkdcir` | SOC별 1s Pulse/RSS |
| DCIR 고정 해제 | `dcirchk_2` | DCIR 자동 스케일링 |
| 저장 | `saveok` | Excel 파일 저장 |
| 그래프 저장 | `figsaveok` | PNG 파일 저장 |

### 프로파일 옵션

| 체크박스 | 객체명 | 설명 |
|----------|--------|------|
| dQdV X/Y축 변환 | `chk_dqdv` | dQdV 축 전환 |
| Coin cell | `chk_coincell` | 코인셀 모드 (소수점 용량) |

---

## 단축키 및 팁

### 사이클 번호 입력 방식
- **연속**: `1-10` → 1,2,3,...,10
- **개별**: `1 5 10` → 1,5,10
- **혼합**: `1-5 8 10-15` → 1,2,3,4,5,8,10,11,12,13,14,15

### Cyclepath 파일 형식
```
폴더경로1	[사이클이름1]
폴더경로2	[사이클이름2]
폴더경로3	[사이클이름3]
```
- Tab으로 구분
- 대괄호 안에 범례 이름 입력

### 색상 코드
```python
graphcolor = [
    '#1f77b4',  # 파란색
    '#ff7f0e',  # 주황색
    '#2ca02c',  # 녹색
    '#d62728',  # 빨간색
    '#9467bd',  # 보라색
    '#8c564b',  # 갈색
    '#e377c2',  # 분홍색
    '#7f7f7f',  # 회색
    '#bcbd22',  # 노란녹색
    '#17becf'   # 청록색
]
```
- 9개 순환 → 10번째부터 반복

---

## 오류 처리

### 일반적인 오류 메시지

```python
err_msg(title, msg)  ← PyQt6 MessageBox
```

| 오류 상황 | 메시지 |
|-----------|--------|
| 파일 없음 | "선택한 폴더에 데이터가 없습니다" |
| 용량 오류 | "용량을 확인하세요" |
| 사이클 없음 | "해당 사이클이 존재하지 않습니다" |
| 패턴 오류 | "패턴 파일을 찾을 수 없습니다" |
| DB 연결 오류 | "Access DB 연결 실패" |

### 버튼 비활성화

```python
self.indiv_cycle.setDisabled(True)  # 실행 중 비활성화
# ... 작업 수행 ...
self.indiv_cycle.setEnabled(True)   # 완료 후 활성화
```

---

## 성능 최적화

### 대용량 데이터 처리

1. **파일 읽기**: `engine="c"` (C parser 사용)
2. **에러 무시**: `on_bad_lines='skip'`
3. **진행률 표시**: `progressBar.setValue()`
4. **비동기 처리**: 버튼 비활성화로 중복 실행 방지

### 메모리 관리

```python
# 임시 DataFrame 삭제
del df.Profilerawtemp

# 그래프 메모리 해제
plt.close(fig)

# Excel Writer 닫기
writer.close()
```

---

## 버전 히스토리 주요 변경사항

- **250819**: PyQt6로 변경, RSS rest time 반영
- **250805**: Pattern 충방전 확인 반영
- **250728**: TOYO 충전 용량 산정 오류 수정
- **250724**: UI 사이즈 축소 (윈도우11 대응)
- **250722**: dQdV 축 바꾸기 기능 추가
- **250707**: 15F Toyo, PNE IP 변경 반영
- **250527**: PNE21 추가, EU 수명 예측 추가
- **241203**: 수명 예측 tab 추가
- **241107**: 개별 사이클 범례/색상 수정
- **240731**: HPPC 데이터 확인, 오류 메세지 추가

---

## 참고 자료

### 관련 파일
- `BatteryDataTool.py`: 메인 소스 코드 (19,000+ 줄)
- `BatteryDataTool_Examples.ipynb`: 함수 사용 예제
- `Cycler_Schedule_2000.mdb`: PNE 패턴 DB (Access)

### 데이터 경로
- **Rawdata**: `D:/Rawdata/` (기본값)
- **PNE DB**: `C:/Program Files/PNE CTSPro/DataBase/`

### 충방전기 IP (예시)
- **R5 15F Toyo**: `192.168.10.xxx`
- **R5 15F PNE**: `192.168.10.yyy`
- **R5 3F PNE1-5**: 별도 IP

---

## 문의 및 개선 사항

본 문서는 BatteryDataTool v250819 기준으로 작성되었습니다.
추가 기능 요청이나 버그 리포트는 개발팀에 문의하세요.

**작성일**: 2025-10-27
**작성자**: Claude (AI Assistant)
