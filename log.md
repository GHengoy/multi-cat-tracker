# Operations Log

## [2026-08-21] LAUNCH | Niche A pipeline (Multi-Cat Household Health & Litter Tracking)

- Selected niche: Multi-Cat Household Health & Litter Tracking (see decisions/niche-selection.md)
- Gumroad product live: https://kimnet8.gumroad.com/l/ecpqxa ($14.99, tracker CSV + guide, cover image included)
- Site live: https://ghengoy.github.io/multi-cat-tracker/
- 7 posts published, all passed content-safety validation (word count, no diagnostic language, real product link)
- Gumroad has no confirmed public API for product creation as of this launch (see decisions/gumroad-integration-decision.md) — product was listed manually by the user; future updates will follow the same manual-assist path unless that changes
- Success metric being tracked going forward: first paid sale (no fixed deadline — organic SEO/discovery takes time)

## [2026-08-25] FEATURE | Amazon 제휴 링크 도입 (검증 1주차 시작)

- 신규 포스트 게시: multi-cat-household-essentials (다묘 가정 실용 용품 14개, 카테고리 검색형 Amazon Associates 링크)
- 허브-스포크 연결: pillar-guide.html에서 링크, 새 포스트에서 pillar-guide.html로 역링크
- 검증 기준: 게시일로부터 1주 고정 기간, 성과와 무관하게 니치 B/C 복제 예정
- 복제 예정일: 2026-09-01 (니치 B/C의 구체적 제품 범위는 그 시점에 별도 브레인스토밍 필요 — 특히 니치 B는 "오디오 제작 기술 콘텐츠 금지" 제약 확인 필요)

## [2026-08-26] FEATURE | 뉴스레터 레이어 도입 (beehiiv, 니치 A)

- `templates/base.html` 푸터에 beehiiv 구독 링크 추가 (https://multi-cat-tracker.beehiiv.com/subscribe)
- 전체 10개 페이지(포스트 9개 + index)에 자동 반영, 별도 콘텐츠/스크립트 변경 없음
- 목적: 무료 재방문 채널 — 새 포스트 게시 시 재방문 유도, 구독자 수 게이팅 없이 니치 B/C로 복제 예정

## [2026-08-27] FEATURE | 결제 프로세서 마이그레이션 준비 (이용약관 페이지 신설)

- `build_site.py`에 unlisted 페이지 지원 추가, `posts/terms.html` 신설 — 사이트에 배포되지만 index엔 미노출
- Gumroad → Paddle 전환의 1단계: Paddle 심사 시 요구되는 라이브 이용약관 페이지 준비 완료
- 실제 결제 링크 교체(Task 5)는 Paddle 계정 승인 후 별도로 진행
