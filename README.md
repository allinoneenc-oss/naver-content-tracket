# 올인원 이엔씨 · 10~11월 콘텐츠 트래커

네이버 블로그(`kyk123146`)와 네이버 클립(`@allinone_enc`)에 **2026-10-01 ~ 2026-11-26** 사이에 올라온 콘텐츠와 반응 지표를 모아 만든 정적 사이트 + 데이터셋입니다.

- 대시보드: `index.html` (별도 빌드 없음, 정적 파일만으로 동작)
- 수집 시점: **2026-11-26 15:30 KST**
- 수집 결과: 블로그 **7건**, 클립 **8건** (총 15건)

## 폴더 구조

```
naver-content-tracker/
├── index.html          # 대시보드 (data/contents.json 을 fetch 해서 렌더링)
├── data/
│   ├── contents.json   # 전체 데이터 (메타 + 요약 + items)
│   ├── blog.json       # 블로그만
│   ├── clips.json      # 클립만
│   └── contents.csv    # 엑셀/구글시트용 CSV (UTF-8 BOM)
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

Claude 등에 붙여넣을 때는 `data/contents.json` 주소 하나면 됩니다(요약 통계까지 들어 있음).

## 데이터 스키마

### `contents.json`

```jsonc
{
  "meta": {
    "updated_at": "2026-11-26T15:30:00+09:00",
    "period": { "start": "2026-10-01", "end": "2026-11-26", "label": "2026년 10월 ~ 11월" },
    "account": {
      "naver_id": "kyk123146",
      "blog": { "id": "kyk123146", "name": "올인원 이엔씨", "url": "...", "total_posts_all_time": 869 },
      "clip": { "handle": "@allinone_enc", "name": "올인원 이엔씨", "url": "...", "total_clips_all_time": 43, "followers": 498, "following": 676 }
    },
    "summary": {
      "total": { "count": 15, "views": 642, "likes": 189, "comments": 49 },
      "by_platform": { "blog": {...}, "clip": {...} },
      "by_month": { "2026-10": {...}, "2026-11": {...} },
      "by_month_platform": { "2026-10": { "blog": {...}, "clip": {...} }, "2026-11": {...} }
    },
    "schema_version": "1.0",
    "notes": ["..."]
  },
  "items": [
    {
      "platform": "blog",            // "blog" | "clip"
      "id": "224491724626",          // 블로그 logNo / 클립 mediaId
      "title": "겨울철 콘크리트 바닥 '동해', ...",
      "content": "본문 요약 또는 클립 캡션",
      "category": "콘크리트 폴리싱",   // 클립은 "클립"
      "published_at": "2026-11-25",  // YYYY-MM-DD (게시일)
      "published_at_full": "2026-11-25T16:53:57+09:00",
      "url": "https://blog.naver.com/kyk123146/224491724626",
      "views": 9,
      "likes": 26,
      "comments": 4,
      "duration": null,              // 클립만 "00:43"
      "duration_seconds": null,      // 클립만 43
      "collected_at": "2026-11-26T15:30:00+09:00",
      "source": "수집 경로 설명",
      "note": "보정/추정이 있으면 기재, 없으면 null"
    }
  ]
}
```

> `meta` 의 내용은 최상위에도 한 번 더 펼쳐 두었습니다(`updated_at`, `period`, `account`, `summary`). `meta.summary` 와 `summary` 는 같은 값입니다.

### `contents.csv`

컬럼: `platform,id,title,category,published_at,url,views,likes,comments,duration,content,collected_at,note`
인코딩: **UTF-8 with BOM**(엑셀에서 한글 깨짐 없음), 구분자 쉼표, 값에 쉼표·줄바꿈이 있으면 따옴표로 감쌈.

### `blog.json` / `clips.json`

`{ "meta": {...}, "items": [...] }` 형태로, `items` 에 해당 플랫폼만 들어 있습니다.

## 지표 수집 기준 (중요)

| 지표 | 출처 | 비고 |
|---|---|---|
| 블로그 조회수 | 네이버 블로그 글 목록(`blog.naver.com/kyk123146`)의 조회수 컬럼 | 글별 누적값 |
| 블로그 댓글수 | 글 목록의 `(댓글 수)` 및 포스트 페이지 댓글 수 | 두 값 일치 확인 |
| 블로그 공감수 | 11월 게시글: 포스트 페이지 "공감 N" / 10월 게시글: **관리자 통계 → 공감수 순위**(2026.10 월간 + 11월 주간 합산) | 10월 글은 포스트 페이지에 공감 수가 0으로 표시되는 오류가 있어 관리자 통계 값으로 산출 |
| 클립 조회수·재생시간 | 클립 프로필 그리드 | 프로필 상단 콘텐츠 43개 중 10~11월 8개 |
| 클립 좋아요·댓글 | 클립 피드(개별 클립 열람 화면) | 그리드에는 표시되지 않음 |
| 클립 게시일 | 그리드 표기. 가장 최근 클립은 "3일 전" → 기준일 2026-11-26 로 환산해 **2026-11-23** | |

참고로 네이버 블로그의 글별 조회수와 공감수는 집계 기준이 달라, 공감수가 조회수보다 크게 표시되는 글도 있습니다(예: 11-25 글 조회수 9 / 공감 26). 원본 값 그대로 기록했습니다.

## 대시보드 기능

- KPI: 전체 콘텐츠 수, 총 조회수, 총 공감·좋아요, 총 댓글 (블로그/클립 분해 표시)
- 월별 조회수, 플랫폼별 반응 막대 차트
- 필터: 플랫폼(전체/블로그/클립) · 게시월(전체/10월/11월)
- 통합 검색(제목·본문·카테고리)
- 컬럼 정렬(제목·플랫폼·카테고리·게시일·조회수·공감·댓글)
- 원문 링크, 복사 버튼이 달린 데이터 URL(JSON/CSV)

## 로컬 실행

```bash
cd naver-content-tracker
python3 -m http.server 8000
# http://localhost:8000
```

`index.html` 을 파일로 직접 열면 브라우저 보안 정책 때문에 `fetch` 가 막혀 데이터가 보이지 않습니다. 반드시 로컬 서버로 열어 주세요.
