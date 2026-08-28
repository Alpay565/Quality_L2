# 01 – FUNCTION PROFILE

**Function:** Quality
**Planning Level:** L2 only (L3 kullanılmaz – L3 yalnızca R&D'ye açıktır)
**Version:** 1.0
**Status:** Draft

---

## 1. Kapsam

Quality fonksiyonu Project Portal içinde yalnızca **L2 Function Plan** seviyesinde çalışır.
Gerekli tüm detay L2 plan item'ları içinde tutulur; alt plan seviyesi oluşturulmaz.

## 2. Project Entry Point

```text
Proje açılışında
```

Quality, L1 Project Plan açıldığı anda plana dahil olur; ayrı bir tetikleyici beklemez.

## 3. Project Active Phases

```text
Planlama  →  Validation  →  SOP
```

- **Planlama:** kalite planlama ve kontrol planı oluşturma
- **Validation:** ürün/proses doğrulama ve PPAP
- **SOP:** seri üretim kalitesine devir

## 4. Main Responsibilities

| # | Sorumluluk | Not |
|---|---|---|
| 1 | APQP / PPAP yönetimi | PPAP dosyasının derlenmesi ve müşteri onayının alınması |
| 2 | Control Plan | Proto / Pre-launch / Production kontrol planları |
| 3 | Ölçüm ekipmanı & kalibrasyon planlama | Gage/fikstür planlama ve kalibrasyon durumu |

**Kapsam dışı (bilinçli karar):**

- **PFMEA ve MSA / Gage R&R** → Process L2'sinde takip edilir. Quality katılımcıdır, plan sahibi değildir.
- **Supplier quality** (tedarikçi PPAP'ı, giriş kalite kriteri, tedarikçi audit) → Purchasing L2'sinde takip edilir.

Bu iki alan Quality L2'de plan item olarak açılmaz; `EXTERNAL_DEPENDENCY` olarak kaydedilir.

## 5. Main Inputs

| Kaynak | Girdi | Tip |
|---|---|---|
| Müşteri | CSR / müşteri kalite standartları, talep edilen PPAP seviyesi ve tarihi | Ana girdi |
| R&D | Released drawing, özel karakteristikler, design freeze | `EXTERNAL_DEPENDENCY` |
| Process | PFMEA, proses akış şeması | `EXTERNAL_DEPENDENCY` |
| Purchasing | Tedarikçi PPAP durumu | `EXTERNAL_DEPENDENCY` |

> R&D ve Process girdileri, discovery sırasında ana girdi olarak seçilmemişti; ancak start rule'lardan
> türedikleri için Critical Review kararı (öneri 2) ile resmi bağımlılık olarak kaydedilmiştir.

## 6. Main Outputs

```text
Onaylı Control Plan
Gage / ölçüm ekipmanı planı
Derlenmiş PPAP dosyası
Müşteri PSW onayı
Seri üretim kalitesine devir kaydı
```

## 7. Upstream / Downstream Functions

```text
UPSTREAM                 QUALITY L2                DOWNSTREAM
────────────────────────────────────────────────────────────────
Müşteri (CSR)      ─┐
R&D (drawing)      ─┼──►  Quality L2 Plan  ──┬──►  Müşteri (PPAP / PSW onayı)
Process (PFMEA)    ─┤                        ├──►  PM / L1 gate
Purchasing (supplier PPAP) ─┘                └──►  Seri Üretim Kalitesi
```

## 8. Completion Criteria

```text
Seri üretim kalitesine resmi devir tamamlandığında
Quality'nin proje kapsamı kapanır.
```

## 9. Main Pain Points

| # | Pain Point | Tasarıma yansıması |
|---|---|---|
| 1 | Müşteri PPAP onay dönüşü gecikiyor / interim onayda kalıyor | QUA-L2-004 ayrı plan item; `EXCEPTION_ONLY` Task Follow-Up; gecikme bildirimi; weekly review paneli |
| 2 | PPAP elemanları dağınık, eksik geç fark ediliyor | PPAP dosyası versiyon + onay kontrollü tek deliverable olarak tanımlandı |

## 10. Inherited from L1 (fonksiyon tarafından üretilmez)

```text
Project ID / Project Code / Project Name
Customer / Segment / Project Type
Project Difficulty (A0 / A1 / A2 / B / C / D)
PM / Site / Created By / Created At
```

Planned Hours ve Project Difficulty Quality tarafından belirlenmez; merkezi Workload Engine hesaplar.
