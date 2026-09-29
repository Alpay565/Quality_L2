# 12 – OPEN QUESTIONS

**Function:** Quality
**Version:** 1.1

> Çözülemeyen konular uydurulmamış, kayıt altına alınmıştır.

| ID | Question | Why It Matters | Owner | Blocking? |
|---|---|---|---|---|
| OQ-01 | 7 plan item'ın Standard Workload karşılığı nedir? | Tüm satırlar `WORKLOAD_STANDARD_GAP`. Merkez saatlerin SWL Calculations'ta faz bazında (NPA-Award / Kick Off-TOGO / TOGO-SOPR / SOP-PRCL) durduğunu bildirdi; fonksiyon faz alanlarını boş bıraktı (2026-09-29), eşleştirme merkezde. Eşleşme olmadan satırlar saatsiz görünür. | Merkezi mimari sahibi + Quality | **Evet** – workload hesabı için |
| OQ-02 | ~~PFMEA ve MSA / Gage R&R gerçekten Process L2'sinde mi tutuluyor?~~ | **KAPANDI (2026-09-24):** Merkezi karar — PFMEA → Process L2; MSA / Gage R&R → Quality L2 (QUA-L2-006). | Process + Quality | Kapandı |
| OQ-03 | Purchasing L2'sinde "supplier PPAP status" çıktısı tanımlı mı? | QUA-L2-003'ün girdisidir (`EXT-Q-03`). Purchasing tarafında karşılık yoksa PPAP dosyası eksik derlenir. | Purchasing + Quality | Hayır |
| OQ-04 | "R&D drawing release" hangi R&D L2 satırının çıktısı? | QUA-L2-002'nin start trigger'ı, QUA-L2-007'nin girdisi. R&D paketi geldi (`RND-L2-01` … `RND-L2-07`); drawing release'i üreten satırın ID'si R&D ile teyit edilince `RND_DRAWING_RELEASE` metni bu ID ile değiştirilecek ve "R&D gecikti" bilgisi Quality planında otomatik görünecek. | R&D + merkezi mimari | Hayır |
| OQ-05 | Gage / ölçüm ekipmanı için standart temin süresi kaç gün? | QUA-L2-002 due kuralı `L1 milestone − standart temin süresi` şeklindedir. Değer olmadan backward planning çalışmaz. | Quality (Metrology) | **Evet** – due hesabı için |
| OQ-06 | PPAP onay gecikmesi eşiği kaç gündür? | Hem `NOT-Q-01` bildirimi hem `EXC-Q-01` Task Follow-Up tetikleyicisi bu eşiğe bağlıdır. | Quality | **Evet** – bildirim/task tetikleyicisi için |
| OQ-07 | Önerilen Quick Action seti doğru mu? | Quick action'lar discovery'de sorulmadı; plan item çıktılarından türetildi. Gereksiz buton üretmemek için teyit gerekir. | Quality | Hayır |
| OQ-08 | `customer_ppap_required` ve `new_measurement_equipment_required` alanları L1 / project data'da mevcut mu? | Üç plan item'ın koşullu mantığı bu alanlara bağlıdır. Alan yoksa koşul otomatik değerlendirilemez, manuel girişe düşer (zero manual entry ihlali). | Merkezi mimari sahibi | **Evet** – koşullu mantık için |
| OQ-09 | Quality ana girdi olarak yalnızca müşteri CSR'ını seçti; R&D ve Process girdileri Critical Review ile eklendi. Bu doğru mu? | Girdi profili eksikse weekly review "Girdi bekleyenler" paneli boş çalışır. | Quality | Hayır |
| OQ-10 | Malzeme ve performans test raporları gerçekten QUA-L2-003 içinde mi kalmalı? | Ayrı takip gerekiyorsa yeni plan item gerekebilir; şu an bilinçli olarak alt detay sayıldı. | Quality | Hayır |
| OQ-11 | QUA-L2-006, QUA-L2-007 ve QUA-L2-003 aynı milestone'a (`PPAP_SUBMISSION`, offset 0) bağlı. Aradaki süreler kaç gün? | 003, 006 ve 007 bitmeden başlayamıyor; offset 0 ile PPAP dosyasını derlemeye süre kalmaz. 006 ve 007 için negatif offset gerekir. | Quality | Hayır – hesap çalışır, plan gerçekçi değil |
| OQ-12 | Pre-launch / production Control Plan QUA-L2-003 içinde alt detay olarak kabul edildi (QUA-L2-001 artık yalnız proto). Doğru mu? | Production CP bir PPAP elemanı ve QUA-L2-005'in girdisi. Ayrı takip gerekiyorsa yeni satır gerekir. | Quality | Hayır |
| OQ-13 | QUA-L2-007 için PPAP numune parçaları Process L2'de bir çıktı olarak tanımlı mı? (`EXT-Q-05`) | 007'nin start koşulu. Process planında karşılığı yoksa, OQ-02'deki gibi iki paket birbirine işaret eder. | Process + Quality | Hayır |
