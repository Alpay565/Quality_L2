# 11 – BUSINESS RULES

**Function:** Quality
**Version:** 1.0

---

## BR-Q-01 – Planlama seviyesi
Quality yalnızca **L2 Function Plan** seviyesinde çalışır. L3 alt plan oluşturulmaz
(L3 yalnızca R&D'ye açıktır). Tüm detay L2 plan item'ları ve detail drawer alanlarında tutulur.

## BR-Q-02 – Project Difficulty ve workload
Project Difficulty L1 Project Plan'da PM tarafından belirlenir ve Quality L2 planına
**inherit** edilir. Quality:
- difficulty belirlemez,
- Planned Hours tahmin etmez,
- A0–D saat tablosu üretmez,
- kendi workload katsayısını oluşturmaz.

Planned Workload = `L1 Project Difficulty` + `Standard Workload Mapping` → merkezi Workload Engine.

## BR-Q-03 – Manuel tarih girişi
Hiçbir plan item için manuel start/due tarihi istenmez.
- Tüm due tarihleri L1 milestone'lardan türetilir.
- Tek istisna QUA-L2-002'dir: `L1 PPAP_SUBMISSION − standart temin süresi` (backward planning).

## BR-Q-04 – Task Follow-Up sınırı
Quality yeni bir task status sistemi, kart yapısı, global filtre veya lifecycle tanımlamaz.
Yalnızca hangi durumda Task Follow-Up'a aksiyon taşınacağını belirler:
- QUA-L2-004 → `EXCEPTION_ONLY` (DELAYED, BLOCKED)
- Diğer tüm item'lar → `USER_DECIDES`

## BR-Q-05 – Plan item ile Follow-Up Task ayrımı
L2 plan satırı otomatik olarak Follow-Up Task'a dönüşmez. Normal ilerleyen bir plan item için
ayrı takip kaydı açılmaz; yalnızca gecikme/blokaj/istisna durumunda (BR-Q-04 kuralına göre) açılır.

## BR-Q-06 – Kapsam sınırı: PFMEA / MSA
PFMEA ve MSA / Gage R&R **Process L2**'sinde takip edilir. Quality katılımcıdır, plan sahibi değildir.
Quality L2'de bu başlıklar için plan item açılmaz; `EXT-Q-02` olarak kaydedilir.

## BR-Q-07 – Kapsam sınırı: Supplier quality
Tedarikçi PPAP'ı, giriş kalite kriterleri ve tedarikçi audit **Purchasing L2**'sinde takip edilir.
Quality bu çıktıyı QUA-L2-003 için girdi olarak tüketir; `EXT-Q-03` olarak kaydedilir.

## BR-Q-08 – Koşullu zincir bütünlüğü
`Customer PPAP Required = No` olduğunda QUA-L2-003 ve QUA-L2-004 pasif hale gelir.
Bu durumda QUA-L2-005 zinciri kopmaz; başlangıcı **son doğrulama tamamlandığında** tetiklenir.

## BR-Q-09 – Fonksiyon kapanışı
Quality'nin proje kapsamı, seri üretim kalitesine **imzalı devir kaydı** oluştuğunda kapanır.
PPAP onayı tek başına fonksiyon kapanışı sayılmaz.

## BR-Q-10 – Bildirim prensibi
Bildirim varsayılan davranış değil istisnadır. Otomatik bildirim yalnızca:
- QUA-L2-004 PPAP onay gecikmesi (Portal + Email),
- QUA-L2-003 ve QUA-L2-004 due date yaklaşması (Portal)
için tanımlıdır. Diğer plan item'lar için otomatik bildirim yoktur.

## BR-Q-11 – Doküman onay ve versiyon
Onay + versiyon kontrolü gerektirenler: **Control Plan**, **PPAP dosyası / PSW**,
**PSW onay kaydı**. Gage planı ve seri devir kaydı yalnızca kayıt altında tutulur.
Drive/klasör mimarisi merkezi sistem tarafından yönetilir.

## BR-Q-12 – Standard Workload GAP
Standard Workload eşleşmesi bulunamayan plan item için Quality saat tahmini üretmez;
`WORKLOAD_STANDARD_GAP` işareti konur ve merkezi entegrasyon öncesi gözden geçirilir.
Bu çalışmada 5 plan item'ın tamamı GAP durumundadır.

## BR-Q-13 – Zero manual entry
Aşağıdaki alanlar Quality kullanıcısından tekrar istenmez; L1'den gelir:
`Project ID, Project Code, Project Name, Customer, Segment, Project Type,
Project Difficulty, PM, Created By, Created At, Site`
