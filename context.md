# 프로젝트 기본 정보

- 프로젝트명: Blogger 건축자재 콘텐츠 자동화 시스템
- 컨트롤 시트 ID: `1ln-FEi1W0ZPKmVFmBUp6iuQ8dm6ijoGVFB6qqCFP1Q0`
- Apps Script ID: `11Oqwz80T8dmsZoYbP4Xh7EsMQMDodK8BwIqQx5V2sltSiZlD4Ji4-et_`
- Blogger ID: `3911627335922911456`
- 블로그 URL: `https://daesan-inside.blogspot.com/`
- GCP 프로젝트: Antigravity (`122520894478`)

# 완료된 기능

- 글 생성 파이프라인
  - `runGenerateOnly()`
  - 입력 파일 전처리 → Claude SEO 글 생성 → `_final_seo.json` 저장
- 발행 파이프라인
  - `runPublishOnly()`
  - 이미지 준비 → 사진 매핑 → Blogger HTML 변환 → Blogger 발행
- 전체실행 원클릭
  - `runAll_Auto()` (자동생성 모드 전용)
- 이미지 모드 분기
  - `자동생성`: OpenAI image1 + SVG image2 자동 생성
  - `직접업로드`: Drive 폴더에 사진 업로드 후 발행
- 발행모드 분기
  - `자동` = LIVE 공개 게시
  - `수동승인` = DRAFT 임시저장
- 예약발행 H열 연동
  - H열 시간 입력 시 ISO 8601 변환 후 `payload.published` 반영
- 중복 발행 방지
  - I열 URL 존재 행 스킵
  - 발행 대상 행 없음 즉시 종료
- 카테고리 자동 분류
  - `석고보드` / `목자재` / `단열재` / `기타`
  - SEO 키워드 + 강조키워드 + 제목 기준 자동 라벨 추가
- OpenAI image1 생성 연동
  - `generateImageWithOpenAI_()`, `generateOpenAIImageOnly_()`
  - OpenAI 생성 이미지 → GitHub Raw URL로 공개 호스팅 → M열 저장
- SVG image2 자동 생성
  - `generateSvgTableFromData_()`, `generateImage2AsSvg_()`
  - `image2_table_data` 기반 비교표/매트릭스 SVG 생성 → GitHub Raw URL → R열 저장
- GitHub 이미지 파일명 고유화
  - 파일명 규칙: `yyyyMMdd_HHmmss_{title앞10자정리}_01.png`
- 이미지 URL 검증
  - 발행 전 이미지 URL 렌더링 검증
  - Blogger 게시 HTML 이미지 검증
  - 검증 실패 시 `발행완료(검증주의)` 상태 기록
- 이미지 프롬프트 개선
  - 9개 유형별 image1/image2 role_map 적용 (`05_이미지생성로직.md` 참고)
  - `image2_table_data`로 SVG 구조형 이미지 생성
- 프롬프트 개선
  - AI 금지패턴 추가, 오프닝 후킹 강제, 인간적 문체 지시 추가
- 블로그 디자인 개선
  - H2 구분선, 이미지 풀와이드 + 캡션
  - 하이라이트 박스, 수치 카드, 인용구 박스
  - CTA 섹션, 회사 서명, 관련글 섹션, FAQ 섹션
- 직접업로드 모드 검증
  - 파일명/사진수/폴더 유효성 검사
  - 사진 부족 시 팝업 후 발행 중단
- 디버깅 강화
  - OpenAI 재시도 3회
  - GitHub 오류 세분화
  - 시트 설정값 검증
- MHTML 파일 지원 (`.mhtml`, `.mht`)
- 사진 설명 영어 자동 생성 (`photo_guides` 영문 생성)

# 현재 브랜치 상태

- `main` — v1 Unsplash 자동삽입 완료 기준선
- `v2_imagen` — 현재 개발 브랜치 (OpenAI + SVG image2 자동화 완성)
- 태그
  - `v2_시스템안정화_완성`
  - `v2_디버깅_1_4순위_완성`
  - `v2_openai_완성`
  - `v2_openai_이미지분리`
  - `v2_gemini_openai_이미지생성` (Gemini 실험 → OpenAI 전환 기점)
  - `v1_unsplash`
  - `baseline_모듈화_전`
  - `baseline_이미지개선_전`
  - `0511_프롬프트개선전`

# 남은 작업

- `clasp run` 불가 — Execution API 미배포 상태, Apps Script 편집기 직접 실행 필요
- 관련글: 실제 Blogger LIVE 글/라벨 상태에 따라 빈 배열 가능
  - 카테고리 조회 실패 시 최근 LIVE 글 폴백 코드는 반영됨
- 실제 UI 발행/렌더링 최종 확인은 Blogger에서 직접 필요
- 이미지 품질 개선
  - OpenAI image1 프롬프트 튜닝
  - SVG image2 시각 스타일 일관성 개선
- 블로그 스타일 프롬프트 추가 개선
  - 본문 문체, CTA, 사진 배치 미세 조정
- 사용자 가이드 작성 예정

# 컨트롤 시트 구조

| 열 | 항목 | 입력/자동 |
|----|------|----------|
| A | 강조키워드 | 입력 |
| B | 스타일 | 입력 |
| C | 검색최적화 키워드 | 입력 |
| D | 템플릿 | 입력 |
| E | 최신반영여부 | 입력 |
| F | 발행모드 | 입력 |
| G | 상태 | 자동 |
| H | 발행시간 | 입력 |
| I | Blogger URL | 자동 |
| J | 이미지소스 | 입력 |
| K | 예비 | — |
| L | 예비 | — |
| M | image1_url | 자동 |
| N | 예비 | — |
| O | 업로드폴더URL | 자동 |
| P | 필요사진수 | 자동 |
| Q | 파일명가이드 | 자동 |
| R | image2_svg_url | 자동 |

# 기술적 제약 사항

- `clasp run` 사용 불가
  - Execution API 제약으로 원격 함수 실행 검증이 막혀 있음
  - 실제 테스트는 Apps Script 편집기에서 직접 실행 필요
- GCP 프로젝트: Antigravity (`122520894478`)
- 필수 API 활성화: Blogger API, Drive API
- API 키는 모두 Properties Service 관리
  - `CLAUDE_API_KEY`
  - `OPENAI_API_KEY`
  - `GEMINI_API_KEY` (현재 미사용 — 향후 Gemini 이미지 활용 시 사용)
  - `GITHUB_TOKEN`
  - `UNSPLASH_ACCESS_KEY` (v1 main 브랜치 전용)

# 카테고리 체계

- `석고보드`
- `목자재`
- `단열재`
- `기타`

# 주요 폴더 ID

- INPUT: `1J_wn9JIilhkyfOBvxkEB1C5B0f5LzNj8`
- JSON 출력: `1wr_0xqWOqStu7AFw3NP9RktXA-f7AR0o`
- 이미지 상위폴더: `1-QkVAQf8O5vSV4ndaXEHAD1soKmhQj5t`
- 테스트 이미지 폴더: `1wpVU90Cg7DZ1G8syZK4V1CxH0PuVV5oT`

# 이미지 호스팅 방식

Drive URL(`lh3.googleusercontent.com`, `drive.google.com/uc?export=view`)은 Blogger 본문에서 렌더링 실패함. 반드시 GitHub Raw URL 사용.

| 모드 | 이미지 소스 | 호스팅 방식 |
|------|------------|------------|
| `자동생성` | OpenAI 생성 (image1) + SVG (image2) | GitHub Raw URL |
| `직접업로드` | Drive 업로드 파일 | GitHub Raw URL |

GitHub 업로드 적용 사항:
- Blob → GitHub Contents API → Raw URL 반환 → 본문 `<img>` 삽입
- 저장소: `kangHo-Jun/Blog` / 이미지 폴더: `/images/`
- Drive 저장 불필요

참고 — v1(main 브랜치) 방식:
- `자동생성`: Unsplash CDN URL 직접 삽입 (Drive 저장 불필요)

# 이미지 관련 주의사항

- GitHub 이미지 파일명 고유화 완료 (타임스탬프 + 제목 일부 + 순번)
- 파일명 중복 시 기존 블로그 이미지 덮어쓰기 발생 가능 — 반드시 고유 파일명 사용
- 기존 이미지 복구 경로: `https://github.com/kangHo-Jun/Blog/commits/main/images/`

## 2026-06-23 작업 내역

### Blogger-Automation (main 브랜치)

#### 1. 폴더 스캔 제거 (커밋: 871fa59)
- `processNextSEOFile()`: savedFinalFile 반환 추가
- `runFullPipelineOneByOne()`: 반환값 전달
- `runGenerateOnly()`: getLatestGeneratedSeoFile_() 스캔 제거 → runFullPipelineOneByOne() 직접 반환값 사용

#### 2. 브랜드 자연 노출 원칙 (커밋: 5de7489)
- 시스템 프롬프트에 [브랜드 노출 원칙] 섹션 추가
- 대산 유통사 포지셔닝 명시
- 글 1편당 1~2회 자연 노출 규칙

#### 3. 자재별 실패 사례 동적 주입 (커밋: be575a7)
- Drive 폴더 1YhX-ubbEe6sFKPvh-in8dBm_y2SOrfTj 에 material-cautions.md 생성
- loadMaterialCautions_() 함수 추가 (6hr 캐시)
- 시스템 프롬프트에 동적 주입

#### 4. FAQ 섹션 자동 삽입 (커밋: 9289321)
- 고정 표준 템플릿에 FAQ 섹션 추가 (9번)
- details/summary HTML 형식
- 실제 검색 질문 형태 작성 규칙

#### 5. 지식 출처 표시 규칙
- 수치/사실 뒤 출처 근거 괄호 명시 규칙 추가
- 출처 불명확 시 "업계 일반 기준" 표기 방법 명시

# 실행 메모

- `runGenerateOnly()`
  - 최신 입력 파일 기준 `_final_seo.json` 생성
  - 자동생성 모드: SVG image2 생성 → R열 저장 포함
  - 직접업로드 모드: Drive 폴더 생성 + O/P/Q열 기록
- `generateOpenAIImage()` (자동생성 모드 전용)
  - `_final_seo.json`의 image1 prompt 읽기 → OpenAI 생성 → GitHub 업로드 → M열 저장
- `runPublishOnly()`
  - 자동생성: M열 image1_url + R열 image2_svg_url 사용
  - 직접업로드: Drive 업로드 파일 사용
  - 발행 전 이미지 URL 검증 수행
- `runAll_Auto()` — 자동생성 모드 원클릭 전체실행
- `clasp push --force`로만 코드 반영 가능
