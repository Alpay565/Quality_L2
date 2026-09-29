# 13 – DESIGN REVIEW

**Function:** Quality
**Version:** 1.1
**Karar:** Seçenek **B – Tüm öneriler uygulandı**

---

## A. Strong Choices

1. **5 plan item ile yalın plan.**
   PFMEA/MSA Process'e, supplier quality Purchasing'e bırakılarak duplicate tracking baştan
   engellendi. Aynı işi iki fonksiyonun planında takip etme riski ortadan kalktı.

2. **QUA-L2-003 ve QUA-L2-004'ün ayrı satır tutulması.**
   PPAP hazırlama ile müşteri onay takibi çoğu yerde tek satırda birleştirilir. Fonksiyonun
   birincil pain point'i onay dönüşü olduğu için ayırmak doğru karardır: bekleme süresi artık
   ölçülebilir ve raporlanabilir.

3. **Due kurallarının tamamen L1 milestone'lara bağlanması.**
   Zero manual entry prensibine tam uyum; manuel tarih girişi sıfır.

4. **Alt detayların ayrı satır yapılmaması.**
   Kalibrasyon, kontrol talimatı ve test raporları ayrı plan item'a çevrilmedi; plan okunabilir
   kaldı ve bakım yükü düşük tutuldu.

---

## B. Potential Problems (tespit edilen)

| # | Problem | Durum |
|---|---|---|
| 1 | Tüm item'lar `USER_DECIDES` iken en büyük pain point (müşteri PPAP dönüşü) takipsiz kalıyordu. Bildirim aksiyon sahibi yaratmaz. | **Düzeltildi** |
| 2 | Ana girdi olarak yalnız müşteri CSR seçilmişti; oysa start rule'lar R&D drawing ve Process PFMEA'ya bağlı. Weekly review "Girdi bekleyenler" paneli veri bulamazdı. | **Düzeltildi** |
| 3 | Weekly review "Bağlı Task Follow-Up" paneli `USER_DECIDES` modunda çoğu projede boş çalışacaktı. | **Düzeltildi** (1 ile birlikte) |
| 4 | "Girdi bekleyenler" ihtiyacı vardı ama 6 kolonluk sette girdi durumu yoktu. | **Düzeltildi** (kolon eklenmeden) |
| 5 | QUA-L2-003/004 koşullu, QUA-L2-005 zorunluydu. PPAP kapsam dışıyken predecessor kopuyor, devir tetiklenemiyordu. | **Düzeltildi** |
| 6 | "Due date yaklaşınca" bildirimi 5 satırın hepsindeydi; proje sayısıyla çarpılınca bildirim körlüğü riski. | **Düzeltildi** |

---

## C. Better Alternatives – Uygulanan Değişiklikler

| # | Önceki karar | Risk | Uygulanan çözüm | Etkilenen dosya |
|---|---|---|---|---|
| 1 | Tüm plan item'lar `USER_DECIDES` | PPAP gecikmesi hiçbir yerde aksiyona dönüşmüyor | QUA-L2-004 → `EXCEPTION_ONLY` (DELAYED, BLOCKED). Diğerleri `USER_DECIDES` kaldı, kullanıcı yükü artmadı | `04_TASK_FOLLOWUP_MAPPING.json`, `03_L2_SCHEMA.json` |
| 2 | Girdi = yalnız müşteri CSR | Cross-function bekleme görünmez | R&D drawing release (`EXT-Q-01`) ve Process PFMEA/akış (`EXT-Q-02`) resmi `EXTERNAL_DEPENDENCY` olarak kaydedildi | `01_FUNCTION_PROFILE.md`, `05_WORKFLOW_DEPENDENCIES_HANDOVER.json` |
| 3 | — | Panel boş çalışır | (1) ile otomatik çözüldü | `10_WEEKLY_REVIEW_REQUIREMENTS.md` |
| 4 | Girdi durumu kolonu yok | Weekly ihtiyacı karşılanmıyor | Kolon eklenmedi; "Waiting for input" weekly review filtresi olarak tanımlandı. Kolon sayısı 6'da kaldı | `09_UI_REQUIREMENTS.md`, `10_WEEKLY_REVIEW_REQUIREMENTS.md` |
| 5 | QUA-L2-002 due = düz L1 milestone | Uzun temin süresi geç fark edilir | Due kuralı `L1 PPAP_SUBMISSION − standart temin süresi` (backward planning) | `02_L2_FUNCTION_PLAN.md`, `03_L2_SCHEMA.json` |
| 6 | QUA-L2-005 zorunlu, predecessor koşullu | Koşullu senaryoda plan çalışmaz | Start rule: `PSW onaylı VEYA (PPAP kapsam dışı VE son doğrulama tamam)` | `03_L2_SCHEMA.json`, `07_CONDITIONAL_EXCEPTION_RULES.json` |
| 7 | Due bildirimi tüm item'larda | Bildirim körlüğü | Yalnızca QUA-L2-003 ve QUA-L2-004 | `08_DOCUMENT_NOTIFICATION_REQUIREMENTS.json` |

---

## D. Decisions to Reconsider – Kalan Riskler

Aşağıdakiler tasarım kararı değil, **veri/kapsam eksikliğidir**; `12_OPEN_QUESTIONS.md` içinde takip edilir:

1. **OQ-01 – 5 plan item'ın tamamı `WORKLOAD_STANDARD_GAP`.** Standard Workload dosyası
   erişilebilir olmadığı için hiçbir eşleşme onaylanamadı. Merkezi entegrasyon öncesi
   çözülmesi gereken en büyük açık kalem.
2. **OQ-02 – PFMEA/MSA sahipliği.** Quality bunları ana sorumluluk saydı ama plan item açmadı.
   Process planında karşılığı yoksa bu iş hiçbir planda görünmez. Cross-function doğrulama şart.
3. **OQ-08 – Koşul alanlarının kaynağı.** `customer_ppap_required` ve
   `new_measurement_equipment_required` L1/project data'da tanımlı değilse üç plan item'ın
   koşullu mantığı manuel girişe düşer ve zero manual entry prensibi ihlal olur.
4. **OQ-05 / OQ-06 – Sayısal eşikler.** Gage standart temin süresi ve PPAP onay gecikme eşiği
   belirlenmeden yeni due kuralı ve yeni Task Follow-Up tetikleyicisi çalışamaz.

---

## E. Design Quality Check

```text
[x] L2 scope net
[x] L2 ile Task Follow-Up duplicate değil
[x] Her plan item gerçekten gerekli
[x] Responsible role net
[x] Input/output net
[x] Dependency net
[x] Cross-functional dependencies işaretli (EXT-Q-01 … EXT-Q-04)
[x] Conditional logic net
[x] Standard Workload mapping GAP olarak işaretli (5/5)
[x] Project Difficulty fonksiyon tarafından belirlenmiyor
[x] Workload saati fonksiyon tarafından üretilmiyor
[x] Minimum manual entry prensibi uygulandı
[x] Notification sayısı makul (3 kural)
[x] UI minimum gerekli bilgiye indirildi (6 kolon)
[x] Open questions görünür (10 kayıt)

R&D değil → L3 kontrolleri uygulanmaz.
```

---

## F. Revizyon 1.1 (2026-09-29) – Merkezi talep sonrası

**Tetikleyici:** OQ-02'nin merkezi kararla kapanması (2026-09-24): PFMEA → Process L2,
MSA / Gage R&R → Quality L2. Bölüm A.1 ve D.2'deki "PFMEA/MSA Process'te" ifadesi MSA için
geçerliliğini yitirmiştir; PFMEA için geçerlidir.

| # | Değişiklik | Karar veren |
|---|---|---|
| 1 | QUA-L2-006 MSA / Gage R&R eklendi – koşulsuz, `DESIGN_PROTO_GATE` ile başlar, predecessor yok | Koşulsuzluk: merkez · Başlangıç kuralı: fonksiyon |
| 2 | QUA-L2-007 Dimensional / Layout Inspection Report eklendi – 006 sonrası, 003'ü besler | Fonksiyon |
| 3 | QUA-L2-003'ün "ölçüm sonuçları ← QUA-L2-002" girdisi düzeltildi (002 plan üretir, sonuç değil) | Merkez tespiti (D.2), fonksiyon çözümü (#2) |
| 4 | QUA-L2-001 start = due hatası düzeltildi: start Kick-off'a alındı, satır yalnız proto CP'yi kapsar | Fonksiyon |
| 5 | SWL faz eşlemesi boş bırakıldı; merkezde yapılacak | Fonksiyon |

**Kabul edilen riskler (fonksiyon kararları):**

- **MSA ↔ gage bağı yok.** QUA-L2-006, QUA-L2-002'yi beklemez. Yeni gage gereken projede MSA
  planda başlamış görünür ama gage gelmeden fiilen yapılamaz; bu bekleme planda görünmez.
  Alternatif (fallback predecessor: `001 + (002 tamam veya pasif)`) önerildi, seçilmedi.
- **Production CP ayrı görünmez.** Pre-launch / production CP, QUA-L2-003 içinde alt detaydır
  (OQ-12).
- **Zincir süreleri tanımsız.** 006, 007 ve 003 aynı milestone'a bağlıdır (OQ-11).

