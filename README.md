# 100-Prisoners-Problem

A simulation-based implementation of the 100 Prisoners Problem, comparing random guessing vs the loop-following strategy and estimating the overall success probability through repeated trials.

```md
# 100 Prisoners Problem Simulator

100 Prisoners Problem을 **몬테카를로 시뮬레이션**으로 검증하는 프로젝트입니다.  
두 전략을 비교합니다.

- **Random Strategy**: 죄수가 서랍을 무작위로 최대 50개 열어본다
- **Optimal Cycle Strategy (Wikipedia)**: “사이클 추적” 방식으로 서랍을 연다

---

## 1. Problem Summary

- 죄수 100명(1~100)이 존재
- 서랍 100개(1~100)가 있고, 카드 1~100이 **무작위로 1장씩** 들어있음
- 죄수는 **최대 50개**의 서랍만 열 수 있음
- 자기 번호 카드를 찾으면 성공
- **100명 전원이 성공해야** 전체가 사면

---

## 2. Strategies

### 2.1 Random Strategy

- 각 죄수는 100개 중 **중복 없이 50개**의 서랍을 무작위로 선택해 확인

### 2.2 Optimal Cycle Strategy (Wikipedia)

- 죄수 번호를 `p`라고 할 때:
  1. `p`번 서랍을 연다
  2. 나온 카드 번호가 `x`이면 다음에 `x`번 서랍을 연다
  3. 최대 50번 반복하며 자기 번호를 찾는다

> 이 전략은 “서랍 번호 → 카드 번호”를 하나의 순열로 보고 **사이클을 따라가도록** 만든다.

---

## 3. Expected Results (Intuition)

- **Random Strategy**

  - 개인 성공확률은 약 50%지만,
  - 100명 전원이 동시에 성공할 확률은 사실상 0에 수렴
  - 시뮬레이션에서도 수천~수만 번 시행해도 `0회 성공`이 정상

- **Optimal Cycle Strategy**
  - 전체 성공확률은 약 **31%** 근처로 수렴하는 것으로 알려져 있음

---

## 4. Project Structure

예시 폴더 구조 (권장)
```

.
├─ core/
│ ├─ model.py # permutation 생성, 공통 데이터 구조
│ ├─ engine.py # run_trial(), simulate()
│ └─ types.py # Strategy 인터페이스
├─ strategies/
│ ├─ random_strategy.py
│ └─ cycle_strategy.py
├─ report/
│ ├─ cli.py # 실행 엔트리포인트
│ └─ summary.py # 결과 출력/저장
├─ tests/
│ └─ test_small_cases.py
├─ requirements.txt
└─ README.md

````

---

## 5. How to Run

### 5.1 Setup
```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
# .venv\Scripts\activate    # Windows

pip install -r requirements.txt
````

### 5.2 Run Simulation

```bash
python -m report.cli --trials 20000 --seed 42
```

옵션 예시:

```bash
python -m report.cli --trials 50000 --seed 1 --N 100 --K 50 --out results.json
```

---

## 6. Output Example

예시 출력:

```
Random Strategy:  wins=0/20000,    p=0.0
Cycle Strategy:   wins=6320/20000, p=0.316
```

저장 파일(JSON/CSV)을 지원하면 결과는 다음 정보를 포함하도록 권장합니다.

- trials
- wins_random / p_random
- wins_cycle / p_cycle
- N, K, seed
- timestamp

---

## 7. Correctness Rules (Important)

이 프로젝트의 시뮬레이션이 의미 있으려면 아래 조건을 반드시 지켜야 합니다.

- 카드 배치는 “무작위 순열”

  - `perm[drawer-1] = card_number`

- Random Strategy는 **중복 없는 50개 서랍 선택**
- 각 서랍은 열고 다시 닫아야 함(정보 공유 X)
- 성공 판정은 “개별 성공”이 아니라 **100명 전원 성공**

---

## 8. Development Workflow (3 People)

권장 역할 분리(동시 개발용)

- Core/Engine: `core/` 담당
- Strategies: `strategies/` 담당
- Runner/Report: `report/` 담당

공통 인터페이스만 고정하면 충돌이 거의 없습니다.

---

## 9. License

MIT (원하면 변경 가능)

---

## 10. References

- Wikipedia: _100 prisoners problem_

```

```
