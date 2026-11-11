# 개발팀 오피스

GitHub 저장소의 push·PR·커밋 기록만으로 **누가 지금 어떤 브랜치를 하고 있는지**를 픽셀 오피스로 보여 주는 한 파일짜리 현황판입니다.
서버가 없습니다. 브라우저가 GitHub API를 직접 읽습니다.

![미리보기](preview.png)

## 무엇이 보이나

- **사무실**: 개발자마다 책상과 캐릭터. 30분 안에 push가 있으면 *작업 중*(모니터 켜짐·타이핑), 3시간 안이면 *조금 전까지*, 그 밖엔 *자리 비움*(커피 머신·화분 사이를 돌아다님). 말풍선에 이름과 지금 브랜치, main에 올렸으면 `→ main`.
- **누가 무엇을**: 사람별 브랜치·마지막 커밋(링크)·오늘 커밋 수·열린 PR.
- **열린 PR**, **최근 활동(7일)**: 커밋·PR 열림·병합을 날짜별로.
- Claude Code 웹 세션처럼 커밋 작성자가 전부 "Claude"여도, GitHub push 기록의 actor(실제로 밀어 넣은 사람)로 사람을 붙입니다.

## 쓰는 법

1. 이 저장소의 Settings → Pages → Source "Deploy from a branch", `main` / `/ (root)` → Save.
2. 열기: `https://<org>.github.io/team-office/` — 다른 저장소를 보려면 `?repo=owner/name`.
3. 파일을 그냥 더블클릭해 열어도(`file://`) 똑같이 동작합니다.

### 설정 (index.html 맨 위 `<script>`)

```js
const REPO = ... || 'jiil-bid-monitor/Negotiate-Contract-Logic-First-try';  // 기본 저장소
const PEOPLE = {            // GitHub 로그인 → 보일 이름·캐릭터 색 (없는 사람은 자동 추가)
  'iamhiki': {name: '히키', color: '#3b82f6'},
  'kdh-007': {name: 'HJE', color: '#e0a53a'},
};
const POLL_MS = 10 * 60 * 1000;   // 갱신 주기
```

### 조회 한도

토큰 없이는 GitHub가 **IP당 시간당 60회**만 허용합니다(한 번 갱신에 5~7회). 같은 사무실에서 여러 명이 열어 두면 한도에 닿을 수 있고, 그때 화면에 안내가 뜹니다.
화면의 **토큰** 버튼에 읽기 전용 PAT(공개 저장소면 권한 없이도 됨)를 넣으면 시간당 5,000회로 늘어납니다. 토큰은 브라우저 localStorage에만 저장됩니다.

비공개 저장소를 보려면 그 저장소를 읽을 수 있는 토큰이 필요합니다.
