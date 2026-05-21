# /new-wordsearch — 초성 찾기 퀴즈 앱 생성

워드크로스 격자에서 초성만 보고 단어를 찾는 퀴즈를 새로 만들 때 사용한다.
각 단계마다 사용자 확인 후 다음으로 넘어간다.

---

## Step 1. 기본 정보 확인

사용자에게 다음을 질문한다:

1. **과목 폴더명** — 예: `sports_S`
2. **단원 번호** — 예: `1` → `chap1` 폴더 사용

해당 과목의 CLAUDE.md를 읽어 유형별 규칙을 파악한다.

---

## Step 2. 이미지 분석 + hint.md 읽기 → 데이터 파일 저장

사용자가 퀴즈판 이미지를 제공하면 아래 순서로 분석한다:

1. 격자 크기(가로×세로 셀 수) 파악
2. 격자 내 초성 배치 위치 파악
3. 단어별 시작 좌표, 방향 파악
4. `{과목폴더}/chap{N}/hint.md` 읽어 각 단어의 초성·정답·힌트 텍스트 매핑

분석 결과를 아래 형식으로 사용자에게 보여주고 **확인을 받은 후**,
`{과목폴더}/chap{N}/puzzle_data.js`로 즉시 저장한다.

```
격자 크기: N×M
단어 목록:
  1 | 초성: "ㅇㄷㅈㅇ" | 정답: "운동전이" | 위치: (row, col) | 방향: 가로
  ...
```

```js
// puzzle_data.js — 이미지 분석 결과 (자동 생성)
const PUZZLE = {
  cols: N, rows: M,
  words: [
    { id: 1, answer: "운동전이", chosung: "ㅇㄷㅈㅇ",
      hint: "한 운동 기술을 배우면 다른 운동 기술 학습에 영향을 미치는 현상",
      row: 0, col: 0, direction: "across" },
    // ...
  ]
};
```

> 이 파일을 먼저 저장해두면 Step 3이 중단되어도 분석 결과가 보존된다.

---

## Step 3. index.html 생성

`puzzle_data.js`의 PUZZLE 데이터를 기반으로 `{과목폴더}/chap{N}/index.html`을 생성한다.

- 해당 과목의 CLAUDE.md 규칙을 따른다
- PUZZLE 데이터는 index.html 안에 인라인으로 포함 (puzzle_data.js는 참조용)
- `APP_URL = "https://kiwili5820.github.io/quiz/{과목폴더}/chap{N}/index.html"`
- 힌트 버튼: 단어 목록 각 항목 옆에 `<img src="../img/tip.png">` 버튼
  - 클릭 시 해당 단어의 힌트 텍스트 토글 (기본: 숨김)

생성 후 사용자에게 검토를 요청한다.

---

## Step 4. 커밋 & 푸시

사용자 승인 후 실행한다:

```
git add {과목폴더}/chap{N}/
git commit -m "feat: {과목폴더} chap{N} 초성 찾기 퀴즈 추가"
git push origin master
```

GitHub Pages 반영까지 1~2분 소요됨을 안내한다.
