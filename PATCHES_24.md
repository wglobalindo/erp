# Dolibarr 24.0.2 원본 수정 목록

**코어 수정 없음.** `htdocs/`는 Dolibarr 24.0.2 원본(upstream `f5ff00e`)과 같습니다.

원칙: 버그 수정이든 기능이든 Dolibarr 원본은 고치지 않고, 커스텀 플러그인(`erp_plugin` 레포 `dolibarr24` 브랜치, `htdocs/custom/`)에서 처리합니다.
확인: `git diff 74bf4582e5 -- htdocs` 결과가 비어 있어야 합니다.

## Dolibarr 13 코어 수정이 옮겨간 곳

| 13 코어 수정 | 24에서 | 위치 |
|---|---|---|
| `product/stats/{commande,facture,facture_fournisseur,propal,supplier_proposal}.php` 단가 열 | 플러그인 | winsoft 모듈 `product/stats/*.php` (24 원본 사본 + 단가 열). 표준 페이지는 winsoft hook이 넘김. 끄기: `WINSOFT_PRODUCT_STATS_UNIT_PRICE=0` |
| `supplier_proposal/class/supplier_proposal.class.php` 외화 단가 있을 때 원래 단가 0 처리 제거 | 플러그인 | vptop 모듈 `class/Utill.php` `SupplierProposal2::addline()/updateline()`. 끄기: `VPTOP_SUPPLIER_PROPOSAL_KEEP_MAIN_PRICE=0` |
| `core/class/commonobject.class.php` 추가필드 라벨 칸 `wordbreak` 제거 | 플러그인 | winsoft 모듈 `css/winsoft.css` |
| `societe/consumption.php` 참조번호 검색, 외화 금액 열 | 플러그인 | winsoft 모듈 hook (`thirdpartyconsumption`) |
| PDF 템플릿 14종 | 플러그인 | winsoft 모듈 `core/modules/<문서>/doc/` |
| `compta/paiement/card.php` 결제 화면 송장별 마진 열 | 24 원본 | 같은 기능 있음 |
| `compta/facture/list.php` 목록 하단 마진 합계 | 24 원본 | `totalarray['val']['total_margin']` |
| `fourn/facture/list.php` VAT 정렬 키 | 24 원본 | `f.total_tva`로 수정됨 |
| `supplier_proposal/list.php` VAT/TTC 정렬 키 | 24 원본 | `sp.total_tva`, `sp.total_ttc`로 수정됨 |
| `product/stats/commande_fournisseur.php` 단가 열 | 24 원본 | 단가 열 있음 |
| `install/.../llx_supplier_proposal_extrafields.key.sql` 인덱스 | 24 원본 | 20.0 마이그레이션에서 정리 |

## Dolibarr를 다시 올릴 때

플러그인 안의 **24 원본 페이지 사본**은 새 버전 원본에서 다시 복사하고 표시된 변경만 다시 적용합니다.

| 사본 | 원본 | 변경 표시 |
|---|---|---|
| winsoft `product/stats/*.php` (5개) | `htdocs/product/stats/` | `WINSOFT` |
| vptop `sproposal.php`, `sporder.php`, `sporder_list.php`, `reception_card.php`, `reception_list.php` | `supplier_proposal/card.php`, `fourn/commande/card.php`, `fourn/commande/list.php`, `reception/card.php`, `reception/list.php` | `VPTOP` |
