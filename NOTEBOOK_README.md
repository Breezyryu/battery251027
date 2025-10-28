# BatteryDataTool Jupyter Notebook 예제

UI_Button_Function_Flow.md를 기반으로 구현한 Jupyter Notebook 예제입니다.

## 📁 파일 구조

```
battery251027/
├── BatteryDataTool.py              ← 원본 소스 코드 (19,000+ 줄)
├── BatteryDataTool_Examples.ipynb  ← Jupyter Notebook 예제 (이 파일)
├── UI_Button_Function_Flow.md      ← 전체 기능 및 Flow 문서
├── NOTEBOOK_README.md              ← 사용 설명서 (이 파일)
└── Rawdata/                        ← 실제 배터리 데이터 폴더
    ├── [테스트명_용량mAh_조건]/
    │   ├── Pattern/                (PNE 식별자)
    │   ├── Restore/                (PNE 데이터)
    │   │   ├── SaveEndData.csv
    │   │   └── SaveData####.csv
    │   └── [채널번호]/             (TOYO 데이터)
    │       ├── 000001
    │       └── 000002
    └── ...
```

## 🚀 빠른 시작

### 1. 환경 설정

```bash
# 필수 패키지 설치
pip install pandas numpy matplotlib scipy xlsxwriter
```

### 2. Jupyter Notebook 실행

```bash
cd c:\Users\Ryu\Python_project\data\battery251027
jupyter notebook BatteryDataTool_Examples.ipynb
```

### 3. 데이터 모드

노트북은 **2가지 모드**로 실행됩니다:

#### A. 실제 데이터 모드 (권장)
- **조건**: `BatteryDataTool.py`가 있고 `Rawdata/` 폴더에 데이터가 있을 때
- **동작**: 실제 배터리 데이터 분석
- **표시**: `✅ BatteryDataTool.py 함수 Import 성공`

#### B. 샘플 데이터 모드
- **조건**: 실제 데이터가 없거나 Import 실패할 때
- **동작**: 시뮬레이션 데이터로 모든 기능 테스트
- **표시**: `⚠️ BatteryDataTool.py 함수 Import 실패`

## 📚 구현된 Flow

### ✅ 완전 구현됨

| 섹션 | Flow | 실제 함수 | 설명 |
|------|------|----------|------|
| 1 | 기본 유틸리티 | `progress`, `name_capacity` 등 | 9개 함수 |
| 2 | 파일 시스템 | `check_cycler`, `remove_end_comma` | 3개 함수 |
| 3 | PNE 데이터 처리 | `pne_min_cap`, `pne_search_cycle` | 용량 산정, 사이클 검색 |
| 4 | 그래프 생성 | `graph_cycle`, `graph_profile` 등 | 6종류 그래프 |
| 5-6 | 데이터 탐색 | Rawdata 자동 분석 | 폴더 구조 탐색 |
| 7 | 종합 파이프라인 | 통합 워크플로우 | 완전한 분석 흐름 |
| 9 | 통합 Cycle 분석 | `load_cycle_data_real()` | 여러 조건 비교 |
| 10 | 충방전 프로파일 | `load_profile_data_real()` | dQdV, Peak 분석 |
| 11 | HPPC/DCIR | 펄스 패턴 분석 | SOC별 저항 특성 |

## 🎯 사용 예제

### 1. 단일 폴더 사이클 분석

```python
# 설정
settings = CycleAnalysisSettings()
settings.capacity = 0  # 자동 산정
settings.cycle_range_x = 0  # 자동 범위

# 실행
folder_path = os.path.join(RAWDATA_PATH, "4500mAh_Test")
result = indiv_cycle_analysis(folder_path, settings)
```

**출력**:
- 6개 그래프 (용량/효율/온도/DCIR/전압)
- 통계 요약
- Excel 저장 (옵션)

### 2. 여러 조건 통합 비교

```python
# 여러 폴더 선택
folders = [
    os.path.join(RAWDATA_PATH, "Test_23C"),
    os.path.join(RAWDATA_PATH, "Test_45C"),
    os.path.join(RAWDATA_PATH, "Test_0C")
]

# 통합 분석
result = overall_cycle_analysis(folders, settings)
```

**출력**:
- 조건별 오버레이 그래프
- 성능 비교 표

### 3. 충전 프로파일 분석

```python
# 설정
settings = ProfileSettings()
settings.cycle_nums = "2 10 50"  # 사이클 선택
settings.smooth_window = 51

# 실행
result = charge_discharge_profile_analysis(
    folder_path, settings, profile_type='charge'
)
```

**출력**:
- 전압/전류 프로파일
- dQdV 그래프
- Peak 검출 결과

### 4. HPPC/DCIR 분석

```python
# 설정
settings = HPPCSettings()
settings.start_cycle = 1
settings.end_cycle = 5

# 실행
result = hppc_dcir_analysis(folder_path, settings)
```

**출력**:
- 펄스 패턴 그래프
- SOC vs DCIR 곡선
- DCIR 통계

## 🔧 실제 데이터 사용 방법

### 자동 전환 방식 (권장)

노트북이 자동으로 실제 데이터를 시도하고, 실패하면 샘플 데이터로 전환합니다.

```python
# 이 함수가 자동으로 처리합니다
capacity, cycle_df, cycler_type = load_cycle_data_real(
    folder_path=folder_path,
    capacity=0,  # 0이면 자동 산정
    ini_crate=0.2,
    chkir=True
)

# 결과 확인
print(f"충방전기: {cycler_type}")  # "PNE", "TOYO", 또는 "SAMPLE"
print(f"용량: {capacity} mAh")
print(f"사이클 수: {len(cycle_df)}")
```

### 실제 데이터 필수 조건

1. **PNE 데이터**:
   - `Pattern/` 폴더 존재 (식별자)
   - `Restore/SaveEndData.csv` 파일 필수
   - `Restore/SaveData####.csv` 파일들

2. **TOYO 데이터**:
   - 채널 폴더 (001, 002 등)
   - 사이클 파일 (000001, 000002 등)

3. **용량 정보**:
   - 폴더명에 "mAh" 포함 (예: `4500mAh_Test`)
   - 또는 자동 산정 (첫 사이클 전류 기반)

## 📊 데이터 구조

### NewData DataFrame (사이클 데이터)

```python
cycle_df.columns = [
    'Dchg',   # 방전 용량 비율 (0.0~1.0)
    'Chg',    # 충전 용량 비율
    'Eff',    # 충방 효율 (0.99~1.01)
    'Eff2',   # 방충 효율
    'Temp',   # 온도 (°C)
    'dcir',   # DCIR 또는 RSS (mΩ)
    'RndV',   # Rest End Voltage (V)
    'AvgV'    # Average Voltage (V)
]
cycle_df.index = [1, 2, 3, ...]  # 사이클 번호
```

### Profile Dictionary (프로파일 데이터)

```python
profile = {
    'time': np.array([...]),      # 시간 (h)
    'voltage': np.array([...]),   # 전압 (V)
    'current': np.array([...]),   # 전류 (mA 또는 C-rate)
    'capacity': np.array([...]),  # 용량 (mAh 또는 비율)
    'dqdv': np.array([...])       # dQ/dV
}
```

## 🎨 그래프 커스터마이징

### 스타일 변경

```python
# 색상 변경
colors = ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728']

# 폰트 변경
plt.rcParams["font.family"] = "Arial"  # 영문
plt.rcParams["font.family"] = "Malgun Gothic"  # 한글

# 그래프 크기
plt.rcParams['figure.figsize'] = (15, 10)

# 해상도
plt.savefig('output.png', dpi=300, bbox_inches='tight')
```

### Y축 범위 조정

```python
settings = CycleAnalysisSettings()
settings.capacity_y_min = 0.70  # 용량 최소
settings.capacity_y_max = 1.05  # 용량 최대
settings.dcir_scale = 1.5       # DCIR 스케일
```

## 💾 Excel 저장

모든 분석 함수는 Excel 저장 기능을 포함합니다:

```python
settings.save_excel = True  # 저장 활성화

# 결과 파일
# - [폴더명]_cycle_analysis.xlsx
# - Sheet: 방전용량, 충방효율, DCIR, 온도, 전압
```

## 🐛 문제 해결

### Import 오류

```python
⚠️ BatteryDataTool.py 함수 Import 실패
```

**원인**: `BatteryDataTool.py` 파일이 없거나 경로 오류

**해결**:
1. `BatteryDataTool.py`가 같은 폴더에 있는지 확인
2. 경로 확인: `BASE_PATH = r"c:\Users\Ryu\Python_project\data\battery251027"`
3. 샘플 데이터 모드로 계속 사용 가능

### 데이터 로딩 실패

```python
⚠️ 실제 데이터 로딩 실패: [오류 메시지]
📝 샘플 데이터로 전환합니다.
```

**원인**: 데이터 파일 구조 문제 또는 손상된 CSV

**해결**:
1. `Rawdata/` 폴더 구조 확인
2. PNE: `Pattern/` 폴더와 `Restore/SaveEndData.csv` 확인
3. TOYO: 채널 폴더와 사이클 파일 확인
4. 샘플 데이터로 기능 테스트 가능

### 빈 그래프

```python
⚠️ 데이터가 비어있습니다.
```

**원인**:
- CSV 파일이 비어있음
- 필터 조건이 너무 엄격함 (cutoff, 용량 범위)

**해결**:
1. CSV 파일 내용 확인
2. cutoff 값 줄이기: `settings.cutoff = 0`
3. 용량 범위 확인

## 📖 추가 구현 가능한 Flow

UI_Button_Function_Flow.md에 있지만 노트북에 미포함:

- 📋 dQdV Fitting (Gaussian Peak Fitting)
- 📋 수명 예측 알고리즘 (EU/장수명)
- 📋 SET 로그 분석
- 📋 Full Cell Simulation
- 📋 패턴 수정 기능

필요하면 동일한 방식으로 추가 구현 가능합니다.

## 💡 유용한 팁

1. **대용량 데이터**: 사이클 범위를 제한하여 메모리 절약
   ```python
   # 처음 100 사이클만
   cycle_df = cycle_df.iloc[:100]
   ```

2. **진행률 표시**: `progress()` 함수 활용
   ```python
   prog = progress(file_num, total_files, cycle, total_cycles, 1000, 1000)
   print(f"진행률: {prog:.2f}%")
   ```

3. **에러 처리**: Try-except로 견고하게
   ```python
   try:
       result = load_cycle_data_real(folder_path)
   except Exception as e:
       print(f"오류: {e}")
   ```

4. **배치 처리**: 여러 폴더 자동 분석
   ```python
   for folder in rawdata_folders:
       try:
           result = indiv_cycle_analysis(folder, settings)
       except:
           continue
   ```

5. **결과 비교**: Pandas로 통합
   ```python
   all_results = pd.concat([result1['cycle_data'], result2['cycle_data']],
                          keys=['Test1', 'Test2'])
   ```

## 📞 참고 문서

- **UI_Button_Function_Flow.md**: 전체 기능 및 Flow 상세 설명
- **BatteryDataTool.py**: 원본 소스 코드 (19,000+ 줄)
- **BatteryDataTool_Examples.ipynb**: 이 노트북

## ✅ 체크리스트

실행 전 확인 사항:

- [ ] Python 3.7+ 설치
- [ ] 필수 패키지 설치 (pandas, numpy, matplotlib, scipy)
- [ ] `BatteryDataTool.py` 파일 존재 (같은 폴더)
- [ ] `Rawdata/` 폴더 존재 (실제 데이터 사용 시)
- [ ] 폴더명에 용량 정보 포함 (예: "4500mAh")
- [ ] Jupyter Notebook 실행 가능

샘플 데이터 모드는 모든 조건에서 실행 가능합니다!

---

**버전**: 1.0
**최종 수정**: 2025-10-28
**작성자**: Based on BatteryDataTool.py and UI_Button_Function_Flow.md
