# 올인원 이엔씨 · CFS KOREA · 10~11월 콘텐츠 트래커

두 개의 네이버 계정 — 올인원 이엔씨(`kyk123146`)와 CFS KOREA / 씨에프에스코리아(`kyk123159`) — 의 **네이버 블로그**와 **네이버 클립**에 2026-10-01 ~ 2026-11-26 사이에 올라온 콘텐츠와 반응 지표를 한곳에 모은 정적 사이트 + 데이터셋입니다.

- 대시보드: `index.html` (별도 빌드 없음, 정적 파일만으로 동작)
- 수집 시점: **2026-11-26 16:10 KST**
- 수집 결과 (총 **24건**)

| 계정 | 블로그 | 클립 | 합계 | 조회수 | 공감·좋아요 | 댓글 |
|---|---|---|---|---|---|---|
| 올인원 이엔씨 (`kyk123146`) | 7 | 8 | 15 | 642 | 189 | 49 |
| CFS KOREA (`kyk123159`) | 6 | 3 | 9 | 295 | 196 | 45 |
| **합계** | **13** | **11** | **24** | **937** | **385** | **94** |

## 폴더 구조

```
naver-content-tracker/
├── index.html          # 대시보드 (data/contents.json 을 fetch 해서 렌더링)
├── strategy.html       # 브랜드 전략·경쟁사·업계 인텔리전스 (data/intelligence.json + contents.json)
├── data/
│   ├── contents.json   # 전체 데이터 (메타 + 요약 + items, 두 계정 통합)
│   ├── blog.json       # 블로그만 (두 계정)
│   ├── clips.json      # 클립만 (두 계정)
│   ├── contents.csv    # 엑셀/구글시트용 CSV (UTF-8 BOM)
│   └── intelligence.json # 전략 페이지 데이터 (액션·키워드·경쟁사·트렌드·사이트 진단)
└── README.md
```

## 엔드포인트

GitHub Pages 기준:

| 파일 | URL |
|---|---|
| 대시보드 | `https://allinoneenc-oss.github.io/naver-content-tracket/` |
| 전체 JSON | `https://allinoneenc-oss.github.io/naver-content-tracket/data/contents.json` |
| 전체 CSV | `https://allinoneenc-oss.github.io/naver-content-tracket/data/contents.csv` |
| 블로그 JSON | `https://allinoneenc-oss.github.io/naver-content-tracket/data/blog.json` |
| 클립 JSON | `https://allinoneenc-oss.github.io/naver-content-tracket/data/clips.json` |
| 전략 페이지 | `https://allinoneenc-oss.github.io/naver-content-tracket/strategy.html` |

Claude 등에 붙여넣을 때는 `data/contents.json` 주소 하나면 됩니다(요약 통계까지 들어 있음).

## 전략 페이지 (`strategy.html`)

같은 저장소의 별도 페이지로, 상단 내비게이션(`📊 콘텐츠 트래커` ↔ `🧭 브랜드 전략·시장 분석`)으로 오갑니다.

- 데이터는 전부 `data/intelligence.json`에서 옵니다: `actions`(실행 항목 20개), `keywords`(10개, `match` 배열로 contents.json과 대조), `competitors.items`(13곳), `trends`(10건), `site_audit`, `channel_plan`, `glossary`.
- 키워드별 '10~11월 콘텐츠' 건수와 KPI는 `data/contents.json`을 실시간으로 읽어 계산합니다(키워드 추가 시 `match` 동의어만 채우면 됨).
- 액션 체크 상태는 브라우저 `localStorage` 키 `strategy-actions-v1`에 저장됩니다(사람별·기기별).
- 내용을 고칠 때는 `intelligence.json`만 수정하면 페이지 코드는 건드릴 필요가 없습니다. `meta.updated_at`도 함께 갱신하세요.

## 데이터 스키마 (schema_version 2.0)

### `contents.json`

```jsonc
{
  "meta": {
    "updated_at": "2026-11-26T16:10:00+09:00",
    "period": { "start": "2026-10-01", "end": "2026-11-26", "label": "2026년 10월 ~ 11월" },
    "accounts": [                       // 계정 목록 (2개)
      {
        "account_id": "kyk123146",
        "account_name": "올인원 이엔씨",
        "blog": { "id": "kyk123146", "name": "올인원 이엔씨", "url": "...", "total_posts_all_time": 869 },
        "clip": { "handle": "@allinone_enc", "name": "올인원 이엔씨", "url": "...", "total_clips_all_time": 43, "followers": 498, "following": 676 }
      },
      {
        "account_id": "kyk123159",
        "account_name": "CFS KOREA",
        "blog": { "id": "kyk123159", "name": "CFS KOREA (씨에프에스코리아)", "url": "...", "total_posts_all_time": 252 },
        "clip": { "handle": "cfs_korea_", "name": "씨에프에스코리아", "url": "...", "total_clips_all_time": 23, "followers": 488, "following": 468 }
      }
    ],
    "account": { ... },                 // 예전 단일 계정(kyk123146) 블록 — 하위 호환용으로 유지
    "summary": {
      "total": { "count": 24, "views": 937, "likes": 385, "comments": 94 },
      "by_platform": { "blog": {...}, "clip": {...} },
      "by_month": { "2026-10": {...}, "2026-11": {...} },
      "by_month_platform": { "2026-10": { "blog": {...}, "clip": {...} }, "2026-11": {...} },
      "by_account": {                   // 계정별 요약 (+ by_platform, by_month 포함)
        "kyk123146": { "account_id": "kyk123146", "account_name": "올인원 이엔씨", "count": 15, "views": 642, "likes": 189, "comments": 49, "by_platform": {...}, "by_month": {...} },
        "kyk123159": { "account_id": "kyk123159", "account_name": "CFS KOREA", "count": 9, "views": 295, "likes": 196, "comments": 45, "by_platform": {...}, "by_month": {...} }
      },
      "by_account_month":    { "kyk123146": { "2026-10": {...}, "2026-11": {...} }, "kyk123159": {...} },
      "by_account_platform": { "kyk123146": { "blog": {...}, "clip": {...} }, "kyk123159": {...} }
    },
    "schema_version": "2.0",
    "notes": ["..."]
  },
  "items": [
    {
      "platform": "blog",              // "blog" | "clip"
      "id": "224484438867",            // 블로그 logNo / 클립 mediaId
      "account_id": "kyk123159",       // "kyk123146" | "kyk123159"
      "account_name": "CFS KOREA",     // "올인원 이엔씨" | "CFS KOREA"
      "title": "콘크리트 연마 실패 원인 1순위, ...",
      "content": "본문 요약 또는 클립 캡션",
      "category": "그라인딩패드",        // 클립은 "클립"
      "published_at": "2026-11-20",    // YYYY-MM-DD (게시일)
      "published_at_full": "2026-11-20T10:24:59+09:00",
      "url": "https://blog.naver.com/kyk123159/224484438867",
      "views": 18,
      "likes": 35,
      "comments": 7,
      "duration": null,                // 클립만 "01:10"
      "duration_seconds": null,        // 클립만 70
      "collected_at": "2026-11-26T16:10:00+09:00",
      "source": "수집 경로 설명",
      "note": "보정/추정이 있으면 기재, 없으면 null"
    }
  ]
}
```

> `meta` 의 내용은 최상위에도 한 번 더 펼쳐 두었습니다(`updated_at`, `period`, `accounts`, `account`, `summary`). `meta.summary` 와 `summary` 는 같은 값입니다.
> `account_id` / `account_name` 은 **모든 item 에 반드시** 들어 있습니다. 이 두 필드로 계정을 구분합니다.

### `contents.csv`

컬럼: `platform,id,account_id,account_name,title,category,published_at,url,views,likes,comments,duration,content,collected_at,note`
인코딩: **UTF-8 with BOM**(엑셀에서 한글 깨짐 없음), 구분자 쉼표, 값에 쉼표·줄바꿈이 있으면 따옴표로 감쌈.

### `blog.json` / `clips.json`

`{ "meta": {...}, "summary": {...}, "items": [...] }` 형태로, `items` 에 해당 플랫폼만 들어 있습니다(두 계정 모두 포함).

## 지표 수집 기준 (중요)

| 지표 | 출처 | 비고 |
|---|---|---|
| 블로그 조회수 | 올인원 이엔씨: 블로그 글 목록의 조회수 컬럼 / CFS KOREA: 글별 통계 페이지 `blog.stat.naver.com/blog/article/{logNo}/cv` 의 **누적 조회수** | 글별 누적값 |
| 블로그 댓글수 | 글 목록의 `(댓글 수)` 및 포스트 페이지 / CFS KOREA 는 글별 통계의 누적 댓글수 | 두 값 일치 확인 |
| 블로그 공감수 | 올인원 이엔씨 11월: 포스트 페이지 "공감 N" / 10월: 관리자 통계 '공감수 순위' · CFS KOREA: 글별 통계의 누적 공감수 | 네이버 집계 기준상 공감이 조회수보다 큰 글도 있음(원본 값 유지) |
| 클립 조회수·재생시간 | 클립 프로필 그리드 (올인원 이엔씨 43개 / CFS KOREA 23개 중 10~11월분) | 그리드 표시값 |
| 클립 좋아요·댓글 | 클립 피드(개별 클립 열람 화면) | 그리드에는 표시되지 않음 |
| 클립 게시일 | 프로필 그리드 표기 | 올인원 이엔씨 최근 클립은 '3일 전' → 2026-11-23 환산 |

참고로 네이버 블로그의 글별 조회수와 공감수는 집계 기준이 달라, 공감수가 조회수보다 크게 표시되는 글도 있습니다(예: CFS KOREA 11-20 글 조회수 18 / 공감 35). 원본 값 그대로 기록했습니다.

## 계정별 수집 메모

- **CFS KOREA (`kyk123159`)**
  - 블로그: 10~11월 게시글은 **6건** — 10-19, 10-21, 10-30, 11-05, 11-12, 11-20. RSS(`rss.blog.naver.com/kyk123159.xml`)와 블로그 글 목록을 대조해 이 기간에 추가 글이 없음을 확인했습니다(직전 글 2026-09-21, 다음 글 없음).
  - 클립: 채널 `cfs_korea_`(씨에프에스코리아, 콘텐츠 23개·팔로워 488)가 있으며, 10~11월 게시 클립은 **3건** — 10-20, 11-10, 11-16. 11-16 클립은 프로필 그리드 조회수가 0으로 표시되었습니다.
- **올인원 이엔씨 (`kyk123146`)**: 기존 수집분(블로그 7건 / 클립 8건)을 그대로 유지하고 `account_id`·`account_name` 필드만 추가했습니다.

## 대시보드 기능

- KPI: 전체 콘텐츠 수, 총 조회수, 총 공감·좋아요, 총 댓글 (블로그/클립 분해 표시, **필터에 따라 자동 재계산**)
- 계정별 요약 표 (콘텐츠 수·조회수·공감·댓글 비교)
- 월별 조회수(계정별), 플랫폼별 반응 막대 차트
- 필터: **계정(전체/올인원 이엔씨/CFS KOREA)** · 플랫폼(전체/블로그/클립) · 게시월(전체/10월/11월)
- 통합 검색(제목·본문·카테고리·계정)
- 컬럼 정렬(제목·계정·플랫폼·카테고리·게시일·조회수·공감·댓글), 표에 **계정 배지** 표시
- 원문 링크, 복사 버튼이 달린 데이터 URL(JSON/CSV)

## 배포 (GitHub Pages)

- 저장소: <https://github.com/allinoneenc-oss/naver-content-tracket>
- 사이트: <https://allinoneenc-oss.github.io/naver-content-tracket/>
- Pages 는 **`gh-pages` 브랜치 루트(`/`)** 에서 배포됩니다. (Pickle 의 GitHub 연결 권한으로는 Pages 설정 API 의 소스 변경이 403 으로 막혀, `gh-pages` 브랜치 푸시로 Pages 가 켜진 상태입니다. 소스를 `main` 으로 바꾸려면 저장소 Settings → Pages 에서 직접 변경하면 됩니다.)
- 내용을 갱신할 때는 두 브랜치를 함께 올려 주세요:

```bash
git add -A && git commit -m "update"
git push origin main
git push origin main:gh-pages
```

## 로컬 실행

```bash
cd naver-content-tracker
python3 -m http.server 8000
# http://localhost:8000
```

`index.html` 을 파일로 직접 열면 브라우저 보안 정책 때문에 `fetch` 가 막혀 데이터가 보이지 않습니다. 반드시 로컬 서버로 열어 주세요.
