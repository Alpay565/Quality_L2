# 12 – OPEN QUESTIONS

**Function:** Quality
**Version:** 1.0

> Çözülemeyen konular uydurulmamış, kayıt altına alınmıştır.

| ID | Question | Why It Matters | Owner | Blocking? |
|---|---|---|---|---|
| OQ-01 | 5 plan item'ın Standard Workload karşılığı nedir? | Standard Workload dosyası bu çalışmada erişilebilir değildi; tüm satırlar `WORKLOAD_STANDARD_GAP`. Eşleşme olmadan Planned Workload hesaplanamaz. | Merkezi mimari sahibi + Quality | **Evet** – workload hesabı için |
| OQ-02 | PFMEA ve MSA / Gage R&R gerçekten Process L2'sinde mi tutuluyor? | Quality bunları ana sorumluluk olarak saymış ama plan item açmamıştır. Process planında karşılığı yoksa bu iş hiçbir planda görünmez. | Process + Quality | **Evet** – kapsam boşluğu riski |
| OQ-03 | Purchasing L2'sinde "supplier PPAP status" çıktısı tanımlı mı? | QUA-L2-003'ün girdisidir (`EXT-Q-03`). Purchasing tarafında karşılık yoksa PPAP dosyası eksik derlenir. | Purchasing + Quality | Hayır |
| OQ-04 | "R&D drawing release" hangi L1 milestone / R&D L2 çıktısına karşılık geliyor? | QUA-L2-002'nin start trigger'ıdır. Kesin karşılığı tanımlanmadan otomatik başlatma kurulamaz. | R&D + merkezi mimari | Hayır |
| OQ-05 | Gage / ölçüm ekipmanı için standart temin süresi kaç gün? | QUA-L2-002 due kuralı `L1 milestone − standart temin süresi` şeklindedir. Değer olmadan backward planning çalışmaz. | Quality (Metrology) | **Evet** – due hesabı için |
| OQ-06 | PPAP onay gecikmesi eşiği kaç gündür? | Hem `NOT-Q-01` bildirimi hem `EXC-Q-01` Task Follow-Up tetikleyicisi bu eşiğe bağlıdır. | Quality | **Evet** – bildirim/task tetikleyicisi için |
| OQ-07 | Önerilen Quick Action seti doğru mu? | Quick action'lar discovery'de sorulmadı; plan item çıktılarından türetildi. Gereksiz buton üretmemek için teyit gerekir. | Quality | Hayır |
| OQ-08 | `customer_ppap_required` ve `new_measurement_equipment_required` alanları L1 / project data'da mevcut mu? | Üç plan item'ın koşullu mantığı bu alanlara bağlıdır. Alan yoksa koşul otomatik değerlendirilemez, manuel girişe düşer (zero manual entry ihlali). | Merkezi mimari sahibi | **Evet** – koşullu mantık için |
| OQ-09 | Quality ana girdi olarak yalnızca müşteri CSR'ını seçti; R&D ve Process girdileri Critical Review ile eklendi. Bu doğru mu? | Girdi profili eksikse weekly review "Girdi bekleyenler" paneli boş çalışır. | Quality | Hayır |
| OQ-10 | Malzeme ve performans test raporları gerçekten QUA-L2-003 içinde mi kalmalı? | Ayrı takip gerekiyorsa 6. plan item gerekebilir; şu an bilinçli olarak alt detay sayıldı. | Quality | Hayır |
