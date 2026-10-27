# Dolibarr 24.0.2 원본 수정 목록 (WINSOFT PATCH)

원본 파일을 고친 곳은 모두 여기에 적고, 코드에는 `// WINSOFT PATCH` 주석을 답니다.
Dolibarr를 다시 올릴 때는 이 목록대로 다시 적용합니다: `grep -rn "WINSOFT PATCH" htdocs`

## 적용한 패치

| 파일 | 내용 | 13 커스텀 대응 |
|---|---|---|
| `htdocs/product/stats/commande.php` | 제품 통계(고객 주문)에 단가(PriceUHT) 열 추가, 금액 정렬을 라인 금액(`d.total_ht`)으로 | product/stats 단가 열 |
| `htdocs/product/stats/facture.php` | 제품 통계(고객 송장)에 단가 열 추가 | 〃 |
| `htdocs/product/stats/facture_fournisseur.php` | 제품 통계(공급사 송장)에 단가(`d.pu_ht`) 열 추가 | 〃 |
| `htdocs/product/stats/propal.php` | 제품 통계(견적)에 단가 열 추가 | 〃 |
| `htdocs/product/stats/supplier_proposal.php` | 제품 통계(공급사 견적)에 단가 열 추가 | 〃 |
| `htdocs/supplier_proposal/class/supplier_proposal.class.php` | 외화 단가가 있을 때 본 통화 단가를 0으로 바꾸는 코드 2곳(addline, updateline)을 끔 | 공급사 견적 단가 0 처리 제거 |

## 패치 없이 해결된 13 커스텀

| 13 커스텀 | 24에서 |
|---|---|
| `compta/paiement/card.php` 결제 화면 송장별 마진 열 | 24 원본에 같은 기능이 들어 있음 |
| `compta/facture/list.php` 목록 하단 마진 합계 | 24 원본에 있음 (`totalarray['val']['total_margin']`) |
| `fourn/facture/list.php` VAT 정렬 키 | 24에서 `f.total_tva`로 수정됨 |
| `supplier_proposal/list.php` VAT/TTC 정렬 키 | 24에서 `sp.total_tva`, `sp.total_ttc`로 수정됨 |
| `product/stats/commande_fournisseur.php` 단가 열 | 24 원본에 단가 열 있음 |
| `install/.../llx_supplier_proposal_extrafields.key.sql` 인덱스 | 24 마이그레이션이 인덱스를 직접 관리 (20.0에서 정리) |
| `societe/consumption.php` 참조번호 검색, 외화 금액 열 | erp_plugin `winsoft` 모듈의 hook으로 이관 (원본 수정 없음) |

## 적용하지 않은 것 (확인 필요)

| 13 커스텀 | 이유 |
|---|---|
| `core/class/commonobject.class.php` extrafield 라벨 셀의 `wordbreak` CSS 클래스 제거 | 24에서 해당 HTML 구조가 바뀌어 같은 수정이 의미가 없음. 화면에서 라벨 줄바꿈이 문제되면 모듈 CSS로 처리 |
