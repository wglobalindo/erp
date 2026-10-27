# Dolibarr 24.0.2 업그레이드 진행 기록 (winsoft_dev)

이 파일은 작업을 이어서 할 때 가장 먼저 읽는 문서입니다. PR이 merge될 때마다 갱신합니다.

## 브랜치

| 레포 | 브랜치 | 역할 |
|---|---|---|
| `erp` | `winsoft_dev` | 작업 브랜치. Dolibarr 24.0.2 원본 + 코어 패치(`PATCHES_24.md`) |
| `erp` | `backup/winsoft_dev-20261027` | 작업 전 `winsoft_dev`(9e69197, 2022-08-15) 백업 |
| `erp_plugin` | `dolibarr24` | Dolibarr 24용 커스텀 모듈(`winsoft` + 기존 5종). `main`은 운영(13)용이라 건드리지 않음 |

- 커스텀 기준은 **운영 `winsoft` 브랜치**(037efd6)입니다. 옛 `winsoft_dev`에는 최근 커스텀 14개 파일이 빠져 있었습니다.
- 세부 작업 브랜치 `upgrade/24-<작업명>` → PR → 자동 merge.

## 구성

- 코어: `htdocs/`는 24.0.2 원본. 원본을 고친 곳은 `PATCHES_24.md`에 모두 적고 코드에 `// WINSOFT PATCH` 주석을 답니다.
- 원본 24.0.2에서 제외: `.github`(CI 자동 실행 방지), `dev`, `test`, `doc`, `.agents`, `.phan`, `.tx` — 기존 포크도 `htdocs`와 루트 파일만 두었습니다.
- 커스텀 모듈: `erp_plugin`의 `dolibarr24` 브랜치를 `htdocs/custom/`에 둡니다.

## 진행 상황

| 단계 | 상태 | PR |
|---|---|---|
| 1. winsoft_dev → 24.0.2 원본 | 진행 중 | |
| 2. winsoft 모듈 뼈대 + PDF 템플릿 14종 + 서명 상수 | 대기 | |
| 3. 코어 수정 13개 이관 | 대기 | |
| 4. import 3종, productstax0 | 대기 | |
| 5. vptop (hook + 페이지 9개) | 대기 | |
| 6. 실행 시험 (PHP 8.5.12) | 대기 | |

## 이어서 할 때

1. 이 파일과 `PATCHES_24.md`, erp_plugin `dolibarr24` 브랜치의 `UPGRADE_24.md`를 읽습니다.
2. 열린 PR이 있으면 그것부터 마무리합니다.
3. 검증 환경 재구성 방법은 아래 "검증 환경"을 따릅니다.

## 검증 환경

- PHP 8.5.12: php-src 태그 `php-8.5.12`를 직접 빌드(Docker Hub·apt 차단 환경이라). 확장: mysqli, pdo_mysql, pgsql, gd, curl, mbstring, xml, zip, soap, bcmath, calendar.
- DB: 작업 환경에 MariaDB가 없어 PostgreSQL 16으로 실행 시험. MySQL 전용 SQL은 정적으로 따로 확인.
