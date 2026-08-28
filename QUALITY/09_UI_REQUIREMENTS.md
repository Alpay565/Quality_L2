# 09 – UI REQUIREMENTS

**Function:** Quality
**Version:** 1.0

> Quality bağımsız bir uygulama tasarlamaz. Global Project Portal UI korunur.
> Aşağıdakiler yalnızca Quality L2 ekranının minimum ihtiyacıdır.

---

## 1. L2 Ekran Görünümü

```text
Table (global portal standardı)
```

Timeline / Gantt, milestone view veya proje bazlı gruplu görünüm **talep edilmemiştir.**
Ek görünüm geliştirme yükü oluşturmamak için tek görünümde kalınmıştır.

## 2. Ana Kolonlar (6)

| # | Kolon | Kaynak |
|---|---|---|
| 1 | Project | L1'den inherit |
| 2 | Plan Item | L2 plan |
| 3 | Responsible | L2 plan |
| 4 | Due | L1 milestone'dan hesaplanır (manuel giriş yok) |
| 5 | Status | Portal standart plan item durumu |
| 6 | Task | Bağlı Follow-Up Task göstergesi |

## 3. Detail Drawer (kolon değil)

Aşağıdakiler tabloyu şişirmemek için detay alanında tutulur:

```text
Input listesi ve girdi durumu (hazır / bekliyor + kaynak fonksiyon)
Output / deliverable
Predecessor – Successor
Conditional işareti ve koşul ifadesi
Standard Workload referansı / GAP işareti
Bağlı dokümanlar (Control Plan, PPAP dosyası)
Exception kaydı (EXC-Q-01 / EXC-Q-02)
```

> **Not (Critical Review problem 4):** "Girdi bekleyenler" ihtiyacı kolon olarak değil,
> weekly review filtresi olarak karşılanır. Böylece kolon sayısı 6'da kalır.
> Bkz. `10_WEEKLY_REVIEW_REQUIREMENTS.md`.

## 4. Quick Actions

Minimum set önerilir — gereksiz buton üretilmez:

```text
Open detail
Upload document
Link / create Follow-Up Task
Mark output ready
```

> Bu set discovery sırasında ayrıca sorulmadı; plan item çıktılarından türetilmiştir.
> Teyit `12_OPEN_QUESTIONS.md` içinde **OQ-07** olarak açık bırakılmıştır.

## 5. Fonksiyonun UI'da talep etmediği alanlar

```text
Planned Hours kolonu          – workload merkezi Engine'de hesaplanır
Project Difficulty seçici     – L1'de PM tarafından belirlenir
Yeni task status ekranı       – mevcut Task Follow-Up korunur
Fonksiyona özel ayrı dashboard
```
