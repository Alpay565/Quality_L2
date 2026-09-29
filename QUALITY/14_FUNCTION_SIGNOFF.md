# 14 – FUNCTION SIGN-OFF

```text
Function:      Quality
Reviewed By:   <fonksiyon sahibi – imza bekliyor>
Date:          <imza tarihi>
Version:       1.1
Status:        Draft
```

---

## Sign-Off Checklist

```text
[x] Function scope approved
[x] L2 plan approved
[x] Inputs/outputs approved
[x] Dependencies approved
[ ] Cross-functional handovers reviewed      → Process / Purchasing / R&D ile ortak doğrulama bekliyor (OQ-03, OQ-04, OQ-13)
[x] Task Follow-Up mapping approved
[ ] Standard Workload mapping reviewed       → 7/7 GAP, faz eşlemesi boş, merkezi eşleştirme bekliyor (OQ-01)
[x] Conditional/exception logic reviewed
[x] Document/notification needs reviewed
[x] UI requirements reviewed
[x] Critical Review discussed                → Seçenek B, tüm öneriler uygulandı
[x] Open questions accepted                  → 13 kayıt (1'i kapandı), 4'ü blocking

R&D only:
n/a – Quality L2-only fonksiyondur, L3 oluşturulmamıştır.
```

## Status Açıklaması

Paket **Draft** durumundadır. `Reviewed` durumuna geçiş için:

1. **OQ-01** – Standard Workload eşleştirmesinin merkezi olarak tamamlanması
2. **OQ-05, OQ-06** – Gage temin süresi ve PPAP gecikme eşiği değerlerinin girilmesi
3. **OQ-08** – Koşul alanlarının L1 / project data'da tanımlanması

`Approved` durumuna geçiş, merkezi entegrasyondaki Contract Validation ve
Cross-Function Conflict Analysis adımlarından sonra verilir.

## Paket İçeriği

```text
/QUALITY/
├── 01_FUNCTION_PROFILE.md
├── 02_L2_FUNCTION_PLAN.md
├── 03_L2_SCHEMA.json
├── 04_TASK_FOLLOWUP_MAPPING.json
├── 05_WORKFLOW_DEPENDENCIES_HANDOVER.json
├── 06_STANDARD_WORKLOAD_MAPPING.json
├── 07_CONDITIONAL_EXCEPTION_RULES.json
├── 08_DOCUMENT_NOTIFICATION_REQUIREMENTS.json
├── 09_UI_REQUIREMENTS.md
├── 10_WEEKLY_REVIEW_REQUIREMENTS.md
├── 11_BUSINESS_RULES.md
├── 12_OPEN_QUESTIONS.md
├── 13_DESIGN_REVIEW.md
└── 14_FUNCTION_SIGNOFF.md

L3 klasörü oluşturulmamıştır (L3 yalnızca R&D fonksiyonuna açıktır).
```
