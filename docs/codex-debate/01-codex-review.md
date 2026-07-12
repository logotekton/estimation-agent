검토 결론부터 말하면, **계획은 진행 가능하지만 “엄격한 0단계 게이트 후 조건부 진행”이어야 합니다.** 현재 문서 그대로 5~6개월 파일럿 개발에 들어가면 DWG 충실도와 평가 통계력이 가장 먼저 무너질 가능성이 큽니다.

**치명적 결함**

1. **LibreDWG 전제가 과신에 가깝습니다.**  
   문서는 DWG를 “LibreDWG 우선”으로 두고, 미달 시 ODA 전환을 분기한다고 합니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:81), [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:205). 그런데 LibreDWG 공식 문서는 아직 beta 성격이고, 고급 R2010+ 엔티티 일부 skip, PROXY_ENTITY 미지원, TABLE 관련 제약, 미검증 엔티티 목록을 명시합니다.([gnu.org](https://www.gnu.org/software/libredwg/)) ([gnu.org](https://www.gnu.org/software/libredwg/manual/LibreDWG.html)) 실무 견적에는 “파일을 열 수 있음”이 아니라 **치수·MTEXT·TABLE·BLOCK/XREF·SHX 한글 폰트·proxy/custom object가 조용히 누락되지 않음**이 필요합니다. 현재 N=50 스파이크는 맞는 방향이지만, 엔티티별 recall/precision, silent-drop 허용치, 한글 텍스트/치수 무결성 기준이 없습니다.

2. **GPL 격리 판단이 법무 리스크를 닫지 못합니다.**  
   문서는 “별도 프로세스(CLI) 격리로 온프레미스 배포 시에도 전파 차단”이라고 단정합니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:81). SaaS에서 서버 내부 실행만 하는 것은 GPL상 대체로 문제가 작지만, 온프레미스 패키지로 같이 배포하면 별도 프로세스라는 형식만으로 충분하지 않을 수 있습니다. FSF FAQ도 GPL 프로그램과 proprietary 시스템이 실질적으로 하나로 결합되면 전체가 GPL 영향을 받으며, arm’s-length로 분리되어야 한다고 설명합니다.([gnu.org](https://www.gnu.org/licenses/gpl-faq.html)) GPL 프로그램을 단순 실행만 할 때는 의무가 없다는 점도 별도입니다.([gnu.org](https://www.gnu.org/licenses/gpl-faq.html)) 온프레미스가 중요한 세그먼트라면 ODA 비용을 초기 원가 구조에 넣어야 합니다. ODA는 DWG 데이터 접근과 최신 DWG 버전 지원을 내세우지만, SaaS/Web 허용 플랜은 연 $7,500 수준부터입니다.([opendesign.com](https://www.opendesign.com/products/drawings)) ([opendesign.com](https://www.opendesign.com/pricing))

3. **평가체계는 방향은 좋지만 통계적으로 아직 성립하지 않습니다.**  
   escaped defects를 페어 지표로 둔 점은 좋습니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:17). 그러나 non-inferiority라면 허용 마진, 분석 단위, 검정 방법, 표본수 산정이 먼저 정의돼야 합니다. FDA 비열등성 가이드도 NI margin을 사전 지정하고 신뢰구간으로 판정해야 한다고 설명합니다.([fda.gov](https://www.fda.gov/media/78504/download)) 현재 “GT 10건+ 후 프로젝트 단위 판정” [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:14)은 너무 작습니다. 극단적으로 10개 프로젝트에서 escaped defect가 0건이어도 95% 상한은 약 26%라서 “수작업 대비 비열등”을 주장하기 어렵습니다. 라인 수가 수백 개여도 같은 프로젝트·같은 도면·같은 검토자에 묶인 군집 자료라 독립 표본처럼 취급하면 안 됩니다. paired binary defect는 McNemar류 접근이 필요하고, 소표본에서는 방법 선택에 따라 power와 보수성이 크게 달라집니다.([bmcmedresmethodol.biomedcentral.com](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-13-91))

**중요하지만 치명적이지 않은 결함**

1. **룰 기반 엔진은 가능하지만 현재 스코프가 넓습니다.**  
   룰 ID, 기준 조문 링크, 오버라이드 레이어 방향은 맞습니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:104). 다만 표준품셈·LH·조달청 기준은 “정본” 하나로 깔끔히 합쳐지지 않습니다. 발주처 특기시방, 물량산출 유의서, 사무소 관행, 공제 기준이 충돌합니다. 특히 철근 ±10% 목표 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:30)는 배근상세, 이음 위치, 정착, 후크, 커플러, 가공손실 없이는 과감합니다. 2a 파일럿에서는 철근을 “정미량 초안/고위험 플래그”로 낮추는 편이 안전합니다.

2. **벽식 구조 반자동 전략은 맞지만 UX 단위가 낙관적입니다.**  
   구간별 전개와 전이층 별도 확정은 좋은 수정입니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:127). 그러나 “기준층 반복 레버리지”는 동별 타입, 코어, 세대 조합, 전이층, 지하주차장 접합부, 벽두께 변화, 개구부가 섞이면 빠르게 약해집니다. “개입 1회당 UX 시간”보다 **층·동·타입당 필요한 개입 수, carry-forward 후 재검증 시간, 미검토 carry-forward 오류율**이 더 핵심 지표입니다.

3. **용도 c, 발주 내역 검증의 시장성은 아직 가설 수준입니다.**  
   내역입찰 구조상 검증 니즈가 있다는 판단은 타당합니다 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:59). 하지만 구매 이유는 “물량 오류 발견” 자체가 아니라 입찰 리스크 회피, 클레임/설계변경 근거, CM 감수비 절감 같은 경제 사건입니다. 시공사 견적팀, CM, 발주처는 구매 예산·책임·도입 장벽이 다릅니다. 파일럿 계약에 유료 전환 트리거를 넣는다는 문장 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:64)은 좋지만, 세그먼트별 ROI와 법적 사용 가능성 검증이 선행돼야 합니다.

**사소한 개선사항**

1. HANDOFF는 “5라운드”라고 쓰고, 계획서는 “4라운드”라고 씁니다 [HANDOFF.md](C:/Logotekton/Estimate-Agent/docs/HANDOFF.md:7), [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:3). 문서 신뢰도를 위해 맞추는 게 좋습니다.

2. 0단계 종료 조건에 “통과/실패 기준 수치”가 없습니다. DWG는 엔티티군별 최소 보존율, 텍스트 glyph 정확도, dimension override 보존율, XREF 해석률, silent-drop 0 허용 같은 기준이 필요합니다.

3. 뷰어 제3 경로는 합리적이지만 [estimation-automation-plan.md](C:/Logotekton/Estimate-Agent/docs/estimation-automation-plan.md:149), 원본 래스터 배경과 자체 벡터 오버레이의 좌표 정합 오차 허용치가 빠져 있습니다.

**최종 판단**

진행은 가능합니다. 다만 바로 제품 개발이 아니라 **DWG/뷰어/평가설계/고객검증을 0단계의 실제 중단 가능 게이트로 운영**해야 합니다. LibreDWG가 N=50 실무 도면에서 실패하거나, 비열등성 평가 설계가 표본수상 불가능하거나, 용도 c 구매자가 유료 전환 의사를 보이지 않으면 범위를 DXF·벡터PDF 중심 검증 도구로 축소하는 게 맞습니다.