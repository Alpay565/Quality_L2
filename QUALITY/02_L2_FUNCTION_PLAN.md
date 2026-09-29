# 02 – L2 FUNCTION PLAN

**Function:** Quality
**Plan Level:** L2
**Version:** 1.1
**Status:** Draft

> **Planned Hours kolonu bu tabloda bulunmaz.**
> Saat/workload merkezi Standard Workload Engine tarafından, L1'den gelen Project Difficulty
> ile `06_STANDARD_WORKLOAD_MAPPING.json` eşleşmesi birleştirilerek hesaplanır.

---

| Seq | Plan Item | Responsible Role | Start Rule | Input | Output | Due Rule | Predecessor | Successor | Conditional | Task Follow-Up Mode | Standard Workload Ref |
|---|---|---|---|---|---|---|---|---|---|---|---|
| QUA-L2-001 | Control Plan (Proto) | Quality Engineer | L1 milestone (Kick-off) ile başlar | Müşteri CSR & kalite standartları; Process PFMEA / proses akış şeması *(external)* | Onaylı proto Control Plan (versiyon kontrollü) | L1 Design/Proto gate | — (L1 milestone) | QUA-L2-003 | No | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-002 | Gage / Ölçüm Ekipmanı Planlama | Quality Engineer (Metrology) | R&D drawing release geldiğinde *(external)* | Released drawing, özel karakteristikler | Gage planı + kalibre, kullanıma hazır ölçüm ekipmanı | L1 ölçüm/PPAP milestone **− standart temin süresi** (backward) | R&D drawing release *(external)* | QUA-L2-003 | Yes – `New Measurement Equipment Required = Yes` | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-003 | PPAP Dosyası Hazırlama | Quality Engineer | QUA-L2-001 onaylı **ve** (QUA-L2-002 tamam **veya** pasif) **ve** QUA-L2-006 + QUA-L2-007 tamam | Onaylı proto Control Plan; MSA raporu *(006)*; boyutsal ölçüm raporu *(007)*; malzeme & performans test raporları; tedarikçi PPAP durumu *(external)* | Derlenmiş PPAP dosyası + pre-launch / production Control Plan (versiyon + onay kontrollü) | L1 PPAP submission milestone | QUA-L2-001, QUA-L2-002, QUA-L2-006, QUA-L2-007 | QUA-L2-004 | Yes – `Customer PPAP Required = Yes` | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-004 | PPAP Müşteri Sunumu & Onay Takibi | Quality Engineer (Approver: Quality Manager) | QUA-L2-003 tamamlanıp dosya müşteriye sunulduğunda | Derlenmiş PPAP dosyası | Müşteri PSW onayı (veya interim / red kararı) | L1 PPAP approval milestone | QUA-L2-003 | QUA-L2-005 | Yes – `Customer PPAP Required = Yes` | **EXCEPTION_ONLY** | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-005 | Seri Üretim Kalitesine Devir | Quality Engineer → Series Quality | PSW onayı alındığında **veya** PPAP kapsam dışıysa son doğrulama tamamlandığında | PSW onayı / son doğrulama kaydı; production control plan *(003)* | İmzalı seri kalite devir kaydı | L1 SOP milestone | QUA-L2-004 *(koşullu bypass)* | — (fonksiyon kapanır) | No | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-006 | MSA / Gage R&R | Quality Engineer (Metrology) | L1 milestone (Design/Proto gate) ile başlar | Proto Control Plan (ölçülecek karakteristikler); kullanılacak ölçüm ekipmanı (yeni veya mevcut gage) | Onaylı MSA / Gage R&R raporu (versiyon + onay kontrollü) | L1 PPAP submission milestone *(offset: OQ-11)* | — (L1 milestone) | QUA-L2-007, QUA-L2-003 | No | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |
| QUA-L2-007 | Dimensional / Layout Inspection Report | Quality Engineer (Metrology) | QUA-L2-006 tamam **ve** PPAP numuneleri geldiğinde *(external)* | Onaylı MSA raporu; released drawing & özel karakteristikler *(external)*; PPAP numune parçaları *(external)* | Boyutsal ölçüm raporu (versiyon + onay kontrollü) | L1 PPAP submission milestone *(offset: OQ-11)* | QUA-L2-006 | QUA-L2-003 | No | USER_DECIDES | *WORKLOAD_STANDARD_GAP* |

---

## Akış

```text
L1 Kick-off          ──► QUA-L2-001 Control Plan (Proto) ──────────┐
R&D drawing release  ──► QUA-L2-002 Gage Planlama (koşullu) ───────┤
                                                                   ├──► QUA-L2-003 PPAP Dosyası
L1 Design/Proto gate ──► QUA-L2-006 MSA ──► QUA-L2-007 Dimensional ┘            │
                                              ▲                                 ▼
                             PPAP numuneleri ─┘               QUA-L2-004 PPAP Onay Takibi
                             (Process, external)                (koşullu, EXCEPTION_ONLY)
                                                                                │
                                                  PPAP kapsam dışı ise bypass   │
                                                                                ▼
                                                              QUA-L2-005 Seri Kaliteye Devir
                                                                                │
                                                                                ▼
                                                                         L1 SOP milestone
```

## Notlar

- **Tüm due tarihleri L1 milestone'lardan türetilir.** Tek istisna QUA-L2-002'dir: uzun temin
  süresi nedeniyle ilgili L1 milestone'dan standart temin süresi geriye alınarak hesaplanır
  (backward planning). Manuel tarih girişi hiçbir satırda beklenmez.
- **Ayrı satır açılmayan alt detaylar:** kalibrasyon takibi → QUA-L2-002 içinde;
  kontrol/ölçüm talimatı yazımı → QUA-L2-001 içinde; pre-launch / production Control Plan
  revizyonları ve malzeme & performans test raporları → QUA-L2-003 içinde (PPAP elemanı).
- **Revizyon 1.1 (2026-09-29):** QUA-L2-006 (MSA / Gage R&R) merkezi karar gereği
  (OQ-02 kapanışı, 2026-09-24) ve QUA-L2-007 (boyutsal ölçüm) 003'ün girdi eksiği nedeniyle
  eklendi. Yeni ID'ler listenin sonuna eklendi; mevcut ID'ler değişmedi.
- **Plan item ≠ Follow-Up Task.** Bu tablodaki satırlar plan aktiviteleridir; Task Follow-Up
  kayıtları yalnızca `04_TASK_FOLLOWUP_MAPPING.json` içindeki kurallara göre oluşur.
