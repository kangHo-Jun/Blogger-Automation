# Google Blogger 건축자재 콘텐츠 자동화 시스템

구글 스프레드시트와 Claude AI를 활용하여 글과 이미지를 자동으로 생성하고, GitHub 이미지 호스팅을 거쳐 Google Blogger에 자동으로 발행하는 자동화 시스템입니다.

## 📁 주요 가이드 문서
- [00_개발체계.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/00_개발체계.md): 개발 목표 및 아키텍처
- [01_시스템가이드.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/01_시스템가이드.md): 전체 동작 구조 설명
- [02_글생성로직.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/02_글생성로직.md): Claude 프롬프트 설계 규칙
- [03_스타일가이드.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/03_스타일가이드.md): 톤앤매너 설정
- [05_이미지생성로직.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/05_이미지생성로직.md): OpenAI Image1 + SVG Image2 자동생성 규칙
- [blogger연결.md](file:///Users/zart/Library/Mobile%20Documents/com~apple~CloudDocs/프로젝트/Blogger-Automation/blogger연결.md): Blogger API 연동 및 GCP 설정 정보

## ⚙️ 기본 설정
- **GCP 프로젝트**: Antigravity (`122520894478`)
- **Blogger ID**: `3911627335922911456`
- **블로그 URL**: [Daesan Inside](https://daesan-inside.blogspot.com/)
- **컨트롤 시트 ID**: `1ln-FEi1W0ZPKmVFmBUp6iuQ8dm6ijoGVFB6qqCFP1Q0`

## 🚀 실행 프로세스
1. 스프레드시트 내 설정 (스타일 지정, 키워드 입력 등)
2. `runGenerateOnly()`로 글 본문 및 이미지 프롬프트, SVG 테이블 자동 생성
3. `generateOpenAIImage()`로 OpenAI DALL-E 3 활용 이미지 생성 후 GitHub 자동 업로드
4. `runPublishOnly()`로 최종 Blogger 발행

## 관련 사이트
- [대산 건축자재 공식 사이트](https://daesan.ai)
- [대산블로그] (https://blog.naver.com/daesan3833)
- [건축자재 인사이드 블로그](https://daesan-inside.blogspot.com/)
