# /new-crossword — 십자말풀이 퀴즈 앱 생성

힌트를 보고 빈칸을 채우는 크로스워드 퀴즈를 새로 만들 때 사용한다.
각 단계마다 사용자 확인 후 다음으로 넘어간다.

---

## Step 1. 기본 정보 확인

사용자에게 다음을 질문한다:

1. **과목 폴더명** — 예: `sports_C`
2. **단원 번호** — 예: `2` → `chap2` 폴더 생성

---

## Step 2. 이미지 분석 → 데이터 파일 저장

사용자가 퀴즈판 이미지를 제공하면 아래 순서로 분석한다:

1. 격자 크기(가로×세로 셀 수) 파악
2. 검정 칸(블로킹 셀) 위치 파악
3. 각 단어의 시작 좌표, 방향, 정답 파악
4. 힌트 번호 매핑 (가로 → 세로 순, 좌→우, 위→아래)

분석 결과를 사용자에게 텍스트로 보여주고 **확인을 받은 후**,
`{과목폴더}/chap{N}/puzzle_data.js` 파일로 즉시 저장한다.

```js
// puzzle_data.js — 이미지 분석 결과 (자동 생성)
const PUZZLE = {
  cols: N,
  rows: M,
  words: [
    { id: 1, direction: "across", row: 0, col: 0, answer: "단어", hint: "힌트" },
    // ...
  ]
};
```

> 이 파일을 먼저 저장해두면 Step 3이 중단되어도 분석 결과가 보존된다.

---

## Step 3. index.html 생성

`puzzle_data.js`의 PUZZLE 데이터를 기반으로 `{과목폴더}/chap{N}/index.html`을 생성한다.

- 해당 과목의 CLAUDE.md 규칙을 따른다
- PUZZLE 데이터는 index.html 안에 인라인으로 포함한다 (puzzle_data.js는 참조용)
- `APP_URL = "https://kiwili5820.github.io/quiz/{과목폴더}/chap{N}/index.html"`
- 이미지 경로는 `../img/` 기준으로 작성 (lock1.png, lock2.png)

생성 후 사용자에게 검토를 요청한다.

---

## Step 4. 커밋 & 푸시

사용자 승인 후 실행한다:

```
git add {과목폴더}/chap{N}/
git commit -m "feat: {과목폴더} chap{N} 십자말풀이 퀴즈 추가"
git push origin master
```

GitHub Pages 반영까지 1~2분 소요됨을 안내한다.
