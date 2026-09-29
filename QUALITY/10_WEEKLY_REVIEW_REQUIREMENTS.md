# 10 – WEEKLY REVIEW REQUIREMENTS

**Function:** Quality
**Version:** 1.1

---

## 1. Weekly Review'da Görülmek İstenenler

| # | Panel | İçerik | Kaynak |
|---|---|---|---|
| 1 | Open + Overdue plan items | Bu hafta açık olan ve süresi geçmiş Quality plan item'ları | L2 plan + L1 milestone due |
| 2 | PPAP onay bekleyenler | Müşteriye sunulmuş, PSW onayı bekleyen dosyalar + bekleme süresi (gün) | QUA-L2-004 |
| 3 | Girdi bekleyenler (Waiting for input) | Başka fonksiyondan girdi beklediği için başlayamayan item'lar + bekleyen kaynak fonksiyon | `EXT-Q-01` … `EXT-Q-05` |
| 4 | Bağlı Task Follow-Up aksiyonları | Quality plan item'larına bağlanmış açık Follow-Up Task'lar | Mevcut Task Follow-Up katmanı |

## 2. Panel Detayları

### Panel 2 – PPAP onay bekleyenler
Fonksiyonun birincil pain point'i olduğu için ayrı panel olarak tanımlanmıştır.
Gösterilecek minimum bilgi:

```text
Project
Sunum tarihi
Bekleme süresi (gün)
Durum: Submitted / Interim / Rejected / Approved
Eşik aşıldı mı? (NOT-Q-01 ile aynı eşik – OQ-06)
```

### Panel 3 – Girdi bekleyenler
`09_UI_REQUIREMENTS.md` kararı gereği bu ihtiyaç L2 tablosuna kolon eklenerek değil,
weekly review filtresi olarak karşılanır. Gösterilecek minimum bilgi:

```text
Plan Item
Beklenen girdi
Kaynak fonksiyon (R&D / Process / Purchasing / Customer)
Ne zamandır bekliyor
```

### Panel 4 – Bağlı Task Follow-Up
QUA-L2-004 `EXCEPTION_ONLY` moduna alındığı için (Critical Review önerisi 1),
gecikme/blokaj durumlarında bu panel otomatik dolar.
Diğer item'lar `USER_DECIDES` olduğundan panel yalnızca gerçekten aksiyon gerektiren
kayıtları gösterir — boş çalışma riski ortadan kalkmıştır.

## 3. Weekly Review'da İstenmeyenler

```text
Workload / capacity paneli   – workload fonksiyon tarafından üretilmez
MUST3                        – bu fonksiyon için talep edilmedi
Project risks                – PM / L1 kapsamında
Decisions required           – bu fonksiyon için talep edilmedi
```
