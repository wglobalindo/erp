# Dolibarr 24.0.2 업그레이드 진행 기록 (winsoft_dev)

이 파일은 작업을 이어서 할 때 가장 먼저 읽는 문서입니다. PR이 merge될 때마다 갱신합니다.

## 브랜치

| 레포 | 브랜치 | 역할 |
|---|---|---|
| `erp` | `winsoft_dev` | 작업 브랜치. Dolibarr 24.0.2 원본 그대로 (코어 수정 없음, `PATCHES_24.md`) |
| `erp` | `backup/winsoft_dev-20261027` | 작업 전 `winsoft_dev`(9e69197, 2022-08-15) 백업 |
| `erp_plugin` | `dolibarr24` | Dolibarr 24용 커스텀 모듈(`winsoft` + 기존 5종). `main`은 운영(13)용이라 건드리지 않음 |

- 커스텀 기준은 **운영 `winsoft` 브랜치**(037efd6)입니다. 옛 `winsoft_dev`에는 최근 커스텀 14개 파일이 빠져 있었습니다.
- 세부 작업 브랜치 `upgrade/24-<작업명>` → PR → 자동 merge.

## 구성

- 코어: `htdocs/`는 24.0.2 원본 그대로. **코어는 고치지 않고 모든 커스텀은 플러그인에서** 처리합니다(2026-10-28 결정). 13 커스텀이 옮겨간 위치는 `PATCHES_24.md`.
- 원본 24.0.2에서 제외: `.github`(CI 자동 실행 방지), `dev`, `test`, `doc`, `.agents`, `.phan`, `.tx` — 기존 포크도 `htdocs`와 루트 파일만 두었습니다.
- 커스텀 모듈: `erp_plugin`의 `dolibarr24` 브랜치를 `htdocs/custom/`에 둡니다.

## 진행 상황 (2026-10-28 코드 작업 완료)

| 단계 | 상태 | PR |
|---|---|---|
| 1. winsoft_dev → 24.0.2 원본 | 완료 | erp #63 |
| 2. winsoft 모듈 + PDF 템플릿 14종 + 서명 상수 + consumption hook | 완료 | erp_plugin #1 |
| 3. 코어 수정 13개 이관 | 완료 — 처음엔 코어 패치(#64), 이후 전부 플러그인으로 이동(erp_plugin #4)하고 코어 원복 | erp #64, erp_plugin #4, erp 코어 원복 PR |
| 4. import 3종, productstax0 | 완료 | erp_plugin #2 |
| 5. vptop (hook + 코어 페이지 복사본 재작성) | 완료 | erp_plugin #3 |
| 6. 실행 시험 (PHP 8.5.12 + MariaDB 11.4) | 완료 — 각 PR 본문에 결과 | |

Dolibarr 최신 릴리스는 2026-10-28 확인 기준 **24.0.2** 입니다.

## 남은 일 (코드 밖 / 오너 확인)

- 오너 확인: 공급사 주문의 **제품 없는 자유 입력 줄 입고** 사용 여부 (24 코어가 막음, erp_plugin #3).
- 오너 확인: vptop 버그로 생긴 **잘못된 공급사 견적 ↔ 고객 견적 연결** 정리 여부 (erp_plugin #3).
- 오너 확인: `*_test` PDF 템플릿 2개 실사용 여부 (안 쓰면 모듈에서 제외).
- `importorderlines`는 13에서도 가져오기 처리 코드가 없었음 (버튼만 있음).
- DB 마이그레이션 리허설(13→24), 운영 서버 전용 파일·모듈 확인, 전환 시 winsoft DB에 `WINSOFT_PDF_PRONEW_NO_TAX_SIGNATURE=0` 설정 — 인프라 쪽 작업.
- 배포 후 웹 화면 설정은 아래 "배포 후 웹에서 할 일".
- PDF 템플릿의 PHP 8 경고(미정의 변수, 13에서 Notice였던 것)는 출력에 영향 없어 남겨 둠.

## 이어서 할 때

1. 이 파일과 `PATCHES_24.md`, erp_plugin `dolibarr24` 브랜치의 PR #1~#4 본문을 읽습니다.
2. 열린 PR이 있으면 그것부터 마무리합니다.
3. 검증 환경 재구성 방법은 아래 "검증 환경"을 따릅니다.

## 검증 환경

- PHP 8.5.12: php-src 태그 `php-8.5.12`를 직접 빌드(Docker Hub·apt 차단 환경이라). 확장: mysqli, pdo_mysql, pgsql, gd, curl, mbstring, xml, zip, soap, bcmath, calendar.
- DB: MariaDB 11.4.13을 소스에서 빌드해 실행 시험.
- 테스트 인스턴스: `winsoft_dev` worktree + `htdocs/custom/<모듈>`에 erp_plugin `dolibarr24` 모듈을 연결, PHP 내장 서버로 실행. 더미 데이터(거래처·제품·견적·주문·공급사 문서)와 vptop이 쓰는 extrafields(cs, cp, cspo, vppo, unit, cn, sp, spd, spp …)를 스크립트로 생성.

## 배포 후 웹에서 할 일 (회사별 DB 설정)

| 화면 | 할 일 | 회사 |
|---|---|---|
| 설정 → 모듈 | `winsoft`, `vptop`, `importpropallines`, `importsupplierproposallines`, `importorderlines`, `productstax0` 활성화. 업그레이드한 DB에서는 **한 번 껐다 켜기** (hook·CSS·PDF 모델 등록) | 둘 다 |
| 설정 → 기타 설정 | `WINSOFT_PDF_PRONEW_NO_TAX_SIGNATURE` = `0` (세금 제외 견적서 서명 없음) | winsoft만 |
| 회사 문서 폴더 | `signature.jpg` (견적서 서명 이미지) 새 환경에 있는지 확인 | 둘 다 |
| 각 문서 모듈 설정 (견적·주문·송장·출하·공급사 문서) | 기본 PDF 모델이 기존 이름(pronew, invoice, pos …)으로 선택돼 있는지 확인 | 둘 다 |
| 설정 → 모듈 → 다중 통화 | 활성 상태와 환율 확인 (공급사 견적 단가 유지 기능이 사용) | 둘 다 |
| 설정 → 기타 설정 (선택) | 표준 동작으로 되돌릴 때만: `WINSOFT_PRODUCT_STATS_UNIT_PRICE=0`, `VPTOP_SUPPLIER_PROPOSAL_KEEP_MAIN_PRICE=0` | — |
