# 업그레이드 작업은 `wglobalindo/erp-platform`으로 옮겨졌습니다 (2026-10-28)

이 `winsoft_dev` 브랜치는 Dolibarr 24.0.2 **원본 그대로**(코어 수정 0)였습니다. 새 구조에서는 코어 소스를 레포에 두지 않고 `erp-platform`의 `core/VERSION`(공식 태그) + `core/patches/`로 관리합니다.

- 진행 기록·문서: `erp-platform`의 `docs/` (`docs/history/`에 이 브랜치의 `UPGRADE_24.md`, `PATCHES_24.md`).
- `winsoft`, `saudara`, `develop` 브랜치는 Dolibarr 13 운영용입니다. 13 운영이 끝날 때까지 긴급 수정만 합니다.
