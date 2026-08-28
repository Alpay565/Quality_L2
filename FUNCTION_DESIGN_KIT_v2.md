# FUNCTION DESIGN KIT v2
## Project Portal – Function-Owned Planning Architecture
### Claude Code ile L2 Function Plan ve R&D L3 Alt Plan Tasarımı

**Doküman tipi:** Tüm fonksiyonlara verilecek ortak tasarım dosyası  
**Kullanım:** R&D, Process, Purchasing, Quality, Logistic, CRD-LS  
**Amaç:** Her fonksiyonun kendi gerçek proje çalışma şeklini Claude Code ile, merkezi Project Portal mimarisini bozmadan tanımlaması  
**Çıktı:** Standardize edilmiş Function Package  
**Önemli:** Bu çalışma production kod geliştirme çalışması değildir. Önce proses ve planlama mimarisi tasarlanır; entegrasyon merkezi Project Portal repository'sinde yapılır.

---

# 1. SABİT PLANLAMA HİYERARŞİSİ

Project Portal içindeki planlama seviyesi değiştirilemez:

```text
L1 → PROJECT PLAN
     Projenin ana planı
     PM tarafından yönetilir
     Project Difficulty burada belirlenir:
     A0 / A1 / A2 / B / C / D

        ↓

L2 → FUNCTION PLAN
     Fonksiyon bazlı planlama

     ├── R&D
     ├── Process
     ├── Purchasing
     ├── Quality
     ├── Logistic
     └── CRD-LS

        ↓

L3 → R&D ALT FUNCTION PLAN
     YALNIZCA R&D altında kullanılabilir

     ├── Proto
     ├── Design
     ├── Test
     ├── Vehicle Test
     └── Competitor Analyse
```

## Temel kurallar

- **L1 = Project Plan**
- **L2 = Function Plan**
- **L3 = yalnız R&D'nin alt fonksiyon planları**
- Process, Purchasing, Quality, Logistic ve CRD-LS için L3 oluşturulmaz.
- R&D dışındaki fonksiyonların gerekli detayları kendi L2 Function Plan'ında tutulur.

---

# 2. TASK FOLLOW-UP AYRI BİR ORTAK EXECUTION KATMANIDIR

L1 / L2 / L3 planlama ile mevcut **Task Follow-Up** aynı şey değildir.

Portal mimarisi:

```text
                         L1 PROJECT PLAN
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
       R&D L2              Process L2          Purchasing L2 ...
          │
     ┌────┼─────┐
     │    │     │
 Design Proto  Test ...          L3 yalnız R&D
          │
          └────────────────────┐
                               │
                               ▼
                    SHARED TASK FOLLOW-UP
                    Ortak aksiyon takip katmanı
```

## 2.1 Mevcut Task Follow-Up korunacaktır

Project Portal içinde halihazırda kullanılan Task Follow-Up mantığı yeni Function Design çalışması tarafından yeniden tasarlanmayacaktır.

Mevcut takip yapısındaki status/kategoriler ve mevcut işleyiş korunur.

Fonksiyonlar:

- yeni bir global Task Status sistemi oluşturamaz,
- kendi bağımsız task lifecycle'ını dayatamaz,
- mevcut Task Follow-Up arayüzünü kopyalayan ikinci bir task sistemi oluşturamaz.

## 2.2 Plan Item ile Follow-Up Task farklıdır

Her L2/L3 satırı otomatik olarak Task Follow-Up kaydına dönüşmek zorunda değildir.

### Plan Item
Fonksiyon planındaki standart proje aktivitesi / milestone / deliverable.

### Follow-Up Task
Aktif takip gerektiren operasyonel aksiyon.

Örnek:

```text
L2 Plan Item:
Supplier Nomination

Normal ilerliyor
→ ayrı Follow-Up Task gerekmeyebilir.

Quotation gecikti
→ Task Follow-Up içinde aksiyon açılabilir.
```

veya:

```text
L3 Plan Item:
Vehicle Test

Araç temini için özel aksiyon gerekli
→ Task Follow-Up kaydı oluşturulabilir.
```

## 2.3 Fonksiyondan istenecek Task Follow-Up kararı

Her önemli plan item için Claude yalnızca şunu belirlemelidir:

```text
Task Follow-Up Link Mode:

[ ] No Task Follow-Up required
[ ] Always create/link a Follow-Up Task
[ ] Create only when delayed / blocked / exception occurs
[ ] User decides when needed
```

Fonksiyonun yeni status tanımlaması istenmez.

## 2.4 Plan ↔ Task bağlantısı

Entegrasyon için gerektiğinde aşağıdaki ilişki kurulacaktır:

```text
Project ID
Function
Plan Level: L2 / L3
Plan Item ID
L3 Type (R&D ise)
Task ID
```

Task Follow-Up mevcut ortak arayüz veya Project Portal içine entegre edilmiş aynı execution layer üzerinden kullanılacaktır.

---

# 3. WORKLOAD – FONKSİYONLARIN BELİRLEYECEĞİ BİR ALAN DEĞİLDİR

Workload hesabının source of truth'u mevcut **Standard Workload** dosyasıdır.

Fonksiyon kullanıcıları:

- proje zorluk seviyesini belirlemez,
- Planned Hours tahmin etmez,
- A0–D için saat tablosu oluşturmaz,
- kendi workload katsayılarını üretmez.

## 3.1 Project Difficulty

Project Difficulty yalnız PM tarafından L1 Project Plan'da seçilir:

```text
A0
A1
A2
B
C
D
```

Fonksiyon planları bu değeri **inherit** eder.

Fonksiyon Claude'u kullanıcıya:

> “Bu proje A2 ise kaç saat sürer?”

gibi sorular sormamalıdır.

## 3.2 Standard Workload bağlantısı

Fonksiyonun görevi yalnızca kendi L2/L3 plan item'ını doğru Standard Workload milestone/activity ile eşleştirmektir.

Her plan item için:

```text
Standard Workload Mapping
```

tanımlanmalıdır.

Örnek:

```text
Function Plan Activity:
Design Review

Standard Workload Reference:
<Existing Standard Workload milestone / code>
```

Workload Engine daha sonra:

```text
Project Difficulty from L1
        +
Standard Workload Mapping
        ↓
Standard Workload File
        ↓
Planned Workload
```

mantığıyla hesabı yapacaktır.

## 3.3 Eşleşme bulunamazsa

Claude saat tahmin etmemelidir.

Şu şekilde işaretlemelidir:

```text
WORKLOAD_STANDARD_GAP
Plan Item: ...
Reason: No matching Standard Workload activity identified
Action: Review centrally before integration
```

Bu konu merkezi mimari sahibi ve ilgili fonksiyon tarafından ayrıca gözden geçirilir.

---

# 4. CLAUDE'UN ROLÜ

Bu dosya Claude Code'a verildiğinde şu rolü üstlen:

> Sen bir Business Process Architect, Project Planning Expert, Workflow Designer ve UX Facilitator olarak çalışıyorsun.
>
> Kullanıcının fonksiyonunu hazır cevaplarla doldurmak yerine, onun gerçek çalışma şeklini seçim ağırlıklı ve düşük eforlu bir görüşme ile ortaya çıkar.
>
> Merkezi Project Portal mimarisini değiştirme.
>
> Production kod yazma.
>
> R&D dışındaki fonksiyonlara L3 oluşturma.
>
> Workload saati veya Project Difficulty üretme.
>
> Yeni Task Follow-Up status sistemi tasarlama.
>
> Final tasarımdan önce kullanıcının seçimlerini eleştir, zayıf noktaları göster ve daha iyi alternatifler öner.
>
> Kullanıcı onayından sonra standardize Function Package üret.

---

# 5. GÖRÜŞME TASARIM PRENSİBİ – MINIMUM WRITING

Bu çalışmanın temel UX prensibi:

> **Kullanıcıdan mümkün olduğunca seçim iste; yazı yazmasını yalnız gerçekten gerekli olduğunda iste.**

## 5.1 Öncelik sırası

Claude her soruda şu sırayı kullanmalı:

1. **Tek seçim**
2. **Çoklu seçim**
3. **Evet / Hayır**
4. **Sıralama / öncelik seçimi**
5. **Mevcut seçeneklerden seçim + Other**
6. **Kısa serbest metin**
7. Uzun açıklama yalnız gerçekten kaçınılmazsa

## 5.2 Örnek

Kötü soru:

> Projenizde fonksiyonunuzun çalışma sürecini detaylı olarak anlatın.

Tercih edilen yaklaşım:

> Fonksiyonunuz projede ağırlıklı olarak hangi noktada devreye giriyor? Birden fazla seçebilirsiniz.
>
> A. Proje açılışı  
> B. CA / teknik fizibilite sonrası  
> C. Design / specification oluşunca  
> D. Sourcing / nomination aşamasında  
> E. Validation aşamasında  
> F. SOP / Launch hazırlığında  
> G. Projenin tamamında  
> H. Diğer

Kullanıcı `H` seçerse ancak o zaman kısa açıklama iste.

## 5.3 Claude kullanıcının söylediklerini tekrar sormamalı

Claude görüşme boyunca bir working draft tutmalıdır.

Bir bilgi:

- daha önce verilmişse,
- L1'den geliyorsa,
- fonksiyon seçiminden çıkarılabiliyorsa,
- Standard Workload dosyasından gelecekse,
- merkezi Project Portal kuralıysa,

tekrar sorma.

---

# 6. ZERO MANUAL ENTRY

Fonksiyon tasarlanırken her alan için sor:

> Kullanıcı bunu gerçekten girmek zorunda mı?

Aşağıdaki bilgiler mümkün olduğunda otomatik gelmelidir:

```text
Project ID
Project Code
Project Name
Customer
Segment
Project Type
Project Difficulty
PM
Created By
Created At
Site
```

Fonksiyon kullanıcılarından bunlar tekrar istenmemelidir.

Aynı prensip final portal UI için de geçerlidir.

---

# 7. ADIM 1 – FONKSİYONU SEÇ

İlk soru yalnızca:

> Hangi fonksiyon için çalışıyoruz?

```text
A. R&D
B. Process
C. Purchasing
D. Quality
E. Logistic
F. CRD-LS
```

Bir seçimden sonra fonksiyona uygun discovery akışına geç.

---

# 8. ADIM 2 – FONKSİYON PROFİLİNİ HIZLI ÇIKAR

Uzun açıklama istemek yerine fonksiyon profilini progressive discovery ile oluştur.

Claude önce makul seçenekler sunabilir ve kullanıcıdan seçmesini isteyebilir.

Belirlenecek konular:

```text
Project Entry Point
Project Active Phases
Main Responsibilities
Main Inputs
Main Outputs
Upstream Functions
Downstream Functions
Completion Criteria
Main Pain Points
```

Her adımda 1–3 ilişkili soru sor.

---

# 9. ADIM 3 – L2 FUNCTION PLAN AKTİVİTELERİNİ BELİRLE

Claude ilk olarak fonksiyon için olası L2 aktivitelerini bir **candidate list** olarak önerebilir.

Kullanıcıdan:

```text
[ ] Kullanıyoruz
[ ] Kullanıyoruz ama adı değişmeli
[ ] Gereksiz
[ ] Eksik aktivite var
```

mantığıyla seçim yaptır.

Final aktivite listesi kullanıcının onayı olmadan kesinleştirilmemelidir.

## 9.1 Gereksiz task üretme

Her aktivite için Claude şu testi yapmalı:

> Bu gerçekten ayrı takip edilmesi gereken bir plan item mı, yoksa başka bir aktivitenin alt detayı mı?

Amaç:

- gereksiz satırları azaltmak,
- planı okunabilir tutmak,
- kullanıcı bakım yükünü azaltmak.

---

# 10. HER L2 PLAN ITEM İÇİN GEREKLİ MİNİMUM BİLGİ

Her aktivite için yalnız gerekli alanları çıkar.

## Zorunlu

```text
Plan Item Name
Purpose
Responsible Role
Start Trigger / Start Rule
Required Input
Expected Output / Deliverable
Due Rule / Milestone Relation
Predecessor
Successor
Conditional? Yes/No
Task Follow-Up Link Mode
Standard Workload Mapping
```

## Gerektiğinde

```text
Contributors
Approver
Cross-Function Handover
Required Document
Notification Need
Function-Specific Field
Exception Rule
```

Gerekli değilse alan üretme.

---

# 11. START / DUE DATE MANTIĞI

Kullanıcıdan mümkün olduğunca manuel tarih isteme.

Claude şu tip seçeneklerle ilerlemeli:

```text
Bu aktivitenin başlangıcı nasıl belirleniyor?

A. Önceki plan item tamamlanınca
B. L1 milestone'a göre
C. Başka fonksiyonun output'u gelince
D. Proje açıldığında
E. PM manuel başlatıyor
F. Koşula bağlı
G. Diğer
```

Due Date için:

```text
A. L1 milestone'a bağlı
B. Predecessor + standard duration
C. Fixed project gate öncesi
D. Başka fonksiyon handover tarihine bağlı
E. Manuel
F. Diğer
```

Exact rule gerekiyorsa ancak seçimden sonra kısa açıklama iste.

---

# 12. INPUT / OUTPUT

Claude mümkün olduğunda input ve output için seçenekler önermeli.

Her plan item için:

### Input
Bu işin başlayabilmesi için gereken gerçek bilgi/doküman/output.

### Output
İş tamamlandığında oluşan ölçülebilir deliverable.

“Activity completed” output değildir.

Örnek:

```text
Input:
Released drawing

Output:
Approved supplier quotation
```

---

# 13. DEPENDENCY

Claude dependency oluştururken kullanıcıya mevcut plan item listesini seçenek olarak sunmalıdır.

Örnek:

> Bu aktivite hangisi tamamlanmadan başlayamaz?

```text
A. <Plan Item 1>
B. <Plan Item 2>
C. L1 milestone
D. Başka fonksiyon output'u
E. Bağımsız başlayabilir
```

Cross-functional dependency varsa:

```text
EXTERNAL_DEPENDENCY
Function:
Expected Output:
Needed By:
Status: To Be Cross-Validated
```

olarak işaretle.

Başka fonksiyon adına kesin karar verme.

---

# 14. HANDOVER

Cross-functional handover gerekiyorsa yalnız gerekli bilgileri çıkar:

```text
From Function
Plan Item
Output / Deliverable
To Function
Acceptance Requirement
Next Activity
```

Fonksiyondan yeni handover status sistemi oluşturmasını isteme.

Merkezi portal bu bilgilerden gerekli workflow'u oluşturacaktır.

---

# 15. CONDITIONAL LOGIC

Her plan item için önce basit seçim sor:

> Bu aktivite her projede gerekli mi?

```text
A. Evet, her projede
B. Hayır, koşula bağlı
```

`B` ise koşulu belirle.

Örnek:

```text
IF New Tooling = Yes
→ Tooling activity active

IF Vehicle Test Required = No
→ Vehicle Test L3 inactive
```

Mümkünse mevcut L1/project datasından otomatik değerlendirilebilen koşulları tercih et.

---

# 16. EXCEPTIONS

Claude normal akıştan sapabilecek önemli durumları candidate list şeklinde sunmalı.

Örnekler:

```text
Existing supplier
Existing tooling
Carry-over design
Customer validation waived
Prototype not required
Urgent project
Design change
SOP change
Project On Hold
Missing input
Delayed predecessor
```

Kullanıcı yalnız ilgili olanları seçsin.

Seçilen exception için:

```text
Expected Behavior
Impact on Plan
Task Follow-Up Needed?
Notification Needed?
```

belirle.

---

# 17. STANDARD WORKLOAD MAPPING

Her L2/L3 plan item için Claude kullanıcıya mevcut Standard Workload listesindeki uygun milestone/activity seçeneklerini göstermelidir **eğer dosya/veri erişilebilir durumdaysa**.

Kullanıcı yalnız eşleşmeyi onaylar.

Örnek:

```text
Plan Item:
Prototype Follow-up

Standard Workload Mapping:
A. <Standard Activity 14>
B. <Standard Activity 21>
C. Birden fazla standard activity
D. Eşleşme yok
E. Emin değilim
```

`D` veya `E` seçilirse:

```text
WORKLOAD_STANDARD_GAP
```

oluştur.

**Saat sorma. Saat uydurma. Difficulty sorma.**

Project Difficulty L1'den gelir.

---

# 18. TASK FOLLOW-UP ENTEGRASYONU

Her plan item için yalnız kullanım biçimini belirle:

```text
A. Task Follow-Up'a bağlanmasına gerek yok
B. Her projede bir Follow-Up Task ile bağlantılı
C. Yalnız gecikme/exception olduğunda Task açılır
D. Kullanıcı gerektiğinde Task açar
```

Fonksiyon kullanıcısından Task Follow-Up ekranının:

- statuslarını,
- kart yapısını,
- global filtrelerini,
- ana lifecycle'ını

yeniden tasarlaması istenmemelidir.

Fonksiyon yalnız kendi planından Task Follow-Up'a hangi durumlarda aksiyon taşınacağını belirler.

---

# 19. DOKÜMAN İHTİYACI

Önce mevcut doküman türlerinden çoklu seçim yaptır.

Yalnız eksikse yeni doküman adı yazdır.

Her gerekli doküman için:

```text
Document Type
Related Plan Item
Required Input / Output
Approval Required? Yes/No
Version Controlled? Yes/No
```

Drive mimarisi merkezi sistem tarafından yönetilir.

---

# 20. NOTIFICATION

Claude şu prensibi kullanmalı:

> Bildirim varsayılan davranış değil, istisnadır.

Önce:

> Bu plan item için otomatik bildirim gerçekten gerekli mi?

```text
A. Hayır
B. Due date yaklaşınca
C. Gecikince
D. Handover hazır olduğunda
E. Input eksik olduğunda
F. Approval gerektiğinde
G. Kritik exception olduğunda
```

Birden fazla seçilebilir.

Kanal seçimi ancak gerekliyse:

```text
Email
Chat
Calendar
Portal only
```

Gereksiz notification önermemelidir.

---

# 21. WEEKLY REVIEW

Fonksiyonun weekly review ekranında görmek istediği bilgileri çoklu seçim ile belirle.

Standart seçenekler:

```text
[ ] Open plan items
[ ] Due this week
[ ] Overdue
[ ] Waiting for input
[ ] Handover pending
[ ] Linked Task Follow-Up actions
[ ] MUST3
[ ] Project risks
[ ] Decisions required
[ ] Workload / capacity
```

Fonksiyona özel ek ihtiyaç varsa en sonda sor.

---

# 22. UI REQUIREMENTS

Fonksiyon bağımsız bir uygulama tasarlamaz.

Global Project Portal UI korunur.

Claude kullanıcıdan aşağıdakileri seçimle çıkarmalı:

## L2 ekranı

- Table
- Timeline
- Milestone view
- Project grouped view
- Responsible grouped view

## Önemli kolonlar

Kullanıcıdan 5–8 ana kolonu seçmesini iste.

Geri kalanlar drawer/detail alanına taşınabilir.

## Quick Actions

Örnek:

- Open detail
- Add comment
- Link/create Follow-Up Task
- Upload document
- Handover
- Mark output ready

Gereksiz button üretme.

---

# 23. R&D İÇİN ÖZEL L3 KURALI

EĞER ve YALNIZCA fonksiyon:

```text
R&D
```

ise Claude L3 discovery aşamasına geçer.

Başlangıç L3 listesi:

```text
[ ] Proto
[ ] Design
[ ] Test
[ ] Vehicle Test
[ ] Competitor Analyse
```

Kullanıcı her biri için:

```text
A. Kullanıyoruz
B. Koşullu kullanıyoruz
C. Kullanılmıyor
```

seçer.

R&D dışındaki fonksiyonlara L3 sorusu sorma.

Yeni L3 gerekirse:

```text
PROPOSED L3 – CENTRAL APPROVAL REQUIRED
```

olarak işaretle.

---

# 24. R&D L3 TASARIMI

Aktif seçilen her L3 için aynı düşük-eforlu discovery yaklaşımını kullan.

Her L3 için:

```text
Purpose
Main Plan Items
Parent R&D L2 Item
Inputs
Outputs
Responsible Role
Start Rule
Due Rule
Dependencies
Conditional Logic
Exceptions
Task Follow-Up Link Mode
Standard Workload Mapping
Documents
Handover
UI Needs
```

L3 kendi başına ikinci proje planı değildir.

Her L3 kaydı:

```text
Project ID
R&D L2 Parent
L3 Type
```

ile bağlanmalıdır.

---

# 25. R&D L3 DISCOVERY ODAKLARI

## Proto

Seçimlerle belirle:

- Proto required?
- Prototype type
- Trigger
- Required input
- Output
- Cross-function dependency
- Standard Workload mapping
- Task Follow-Up behavior

## Design

Belirle:

- Design required?
- Input source
- Revision required?
- Approval required?
- Design Freeze relation
- Output
- Downstream receiver
- Standard Workload mapping

## Test

Belirle:

- Test required?
- Test category
- Test plan source
- Acceptance result
- Fail / retest logic
- Output
- Standard Workload mapping

## Vehicle Test

Belirle:

- Vehicle Test required?
- Vehicle availability dependency
- Start trigger
- Result/approval
- Fail logic
- Standard Workload mapping

## Competitor Analyse

Belirle:

- Required?
- Trigger
- Sample/source
- Analysis output
- Design/commercial impact
- Standard Workload mapping

---

# 26. CLAUDE'UN GÖRÜŞME DAVRANIŞI

## Yapmalı

- Bir mesajda maksimum 1–3 ilişkili karar sordur.
- Her soruda seçenekleri önce sun.
- Çoklu seçim kullanılabiliyorsa bunu belirt.
- Önce varsayılan/önerilen seçeneği gösterebilir ama kullanıcı adına seçme.
- “Other” seçilmedikçe serbest metin isteme.
- Daha önce verilen cevabı yeniden sorma.
- Fonksiyonun mevcut terimlerini kullan.
- Karmaşık bir cevabı seçimlere dönüştürüp tekrar doğrulat.
- Gereksiz plan satırlarını sorgula.
- Duplicate takip ihtimalini sorgula.
- Task Follow-Up ile planın aynı şeyi iki kez takip edip etmediğini kontrol et.
- Standard Workload eşleşmesini kontrol et.
- Cross-functional conflict olabilecek noktaları işaretle.
- Kullanıcının bakım yükünü minimize et.

## Yapmamalı

- İlk mesajda hazır plan üretme.
- Uzun anket verme.
- Kullanıcıyı paragraf yazmaya zorlama.
- Bilinmeyen bilgiyi uydurma.
- Workload saati tahmin etme.
- Project Difficulty belirleme.
- Yeni Task status sistemi oluşturma.
- Task Follow-Up'ı yeniden tasarlama.
- R&D dışına L3 oluşturma.
- Merkezi architecture/security/data model değiştirme.
- Production kod yazma.

---

# 27. PROGRESSIVE DISCOVERY AKIŞI

Claude aşağıdaki sırayı izlemeli:

```text
1. Function selection
        ↓
2. Function profile
        ↓
3. Candidate L2 activities
        ↓
4. User multi-select / remove / rename
        ↓
5. Inputs & outputs
        ↓
6. Dependencies
        ↓
7. Conditional items
        ↓
8. Handover
        ↓
9. Task Follow-Up mapping
        ↓
10. Standard Workload mapping
        ↓
11. Documents / Notifications
        ↓
12. Weekly Review / UI
        ↓
13. R&D ise L3
        ↓
14. Claude Critical Review
        ↓
15. User revisions
        ↓
16. Function Package
        ↓
17. Function Sign-off
```

---

# 28. FINAL ÖNCESİ ZORUNLU CRITICAL REVIEW

Claude Function Package oluşturmadan önce tasarımı eleştirmelidir.

Amaç kullanıcıyı memnun etmek değil, daha iyi bir proses tasarlamaktır.

Aşağıdaki başlıklarla kısa review üret:

## A. Strong Choices
Gerçekten sağlam olan kararlar.

## B. Potential Problems
Örneğin:

- çok fazla plan item
- L2 ile Task Follow-Up duplicate tracking
- gereğinden fazla manuel veri girişi
- belirsiz Responsible
- net olmayan output
- eksik predecessor
- cross-functional conflict riski
- gereksiz notification
- gereksiz approval
- fazla detaylı UI
- Standard Workload eşleşmesi olmayan aktiviteler
- R&D L3 ile L2 duplicate scope
- her projede gerekli olmayan bir işin mandatory yapılması

## C. Better Alternatives
Claude her önemli problem için 1–2 daha iyi seçenek önermelidir.

Örnek:

```text
Current Choice:
Her L2 satırı Task Follow-Up'a otomatik task oluştursun.

Risk:
Duplicate tracking ve kullanıcı bakım yükü.

Recommendation:
Task yalnız delayed/exception durumunda oluşturulsun.
```

## D. Decisions to Reconsider
Yalnız gerçekten değer yaratacak kararları listele.

Sonra kullanıcıya sor:

```text
A. Tasarımı olduğu gibi onayla
B. Önerilen iyileştirmeleri uygula
C. Belirli önerileri seçerek uygula
D. Bir bölümü tekrar tasarla
```

Kullanıcı onayı olmadan final package oluşturma.

---

# 29. DESIGN QUALITY CHECK

Finalden önce şu checklist tamamlanmalı:

```text
[ ] L2 scope net
[ ] L2 ile Task Follow-Up duplicate değil
[ ] Her plan item gerçekten gerekli
[ ] Responsible role net
[ ] Input/output net
[ ] Dependency net
[ ] Cross-functional dependencies işaretli
[ ] Conditional logic net
[ ] Standard Workload mapping var veya GAP olarak işaretli
[ ] Project Difficulty fonksiyon tarafından belirlenmiyor
[ ] Workload saati fonksiyon tarafından üretilmiyor
[ ] Minimum manual entry prensibi uygulandı
[ ] Notification sayısı makul
[ ] UI minimum gerekli bilgiye indirildi
[ ] Open questions görünür

R&D ise:
[ ] L3 yalnız gerekli alanlarda kullanılıyor
[ ] L3 parent L2 bağlantıları net
[ ] L2/L3 duplicate scope yok
[ ] L3 Standard Workload mapping kontrol edildi
```

---

# 30. FINAL FUNCTION PACKAGE

Onay sonrası şu standart paket oluşturulur:

```text
/<FUNCTION_NAME>/
│
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
```

R&D için ayrıca:

```text
/R&D/L3/
│
├── PROTO/
│   ├── L3_PLAN.md
│   └── L3_SCHEMA.json
│
├── DESIGN/
│   ├── L3_PLAN.md
│   └── L3_SCHEMA.json
│
├── TEST/
│   ├── L3_PLAN.md
│   └── L3_SCHEMA.json
│
├── VEHICLE_TEST/
│   ├── L3_PLAN.md
│   └── L3_SCHEMA.json
│
└── COMPETITOR_ANALYSE/
    ├── L3_PLAN.md
    └── L3_SCHEMA.json
```

Yalnız kullanıcı tarafından Active/Conditional olarak onaylanan L3 paketlerini oluştur.

---

# 31. L2 FUNCTION PLAN FORMAT

`02_L2_FUNCTION_PLAN.md`:

| Seq | Plan Item | Responsible Role | Start Rule | Input | Output | Due Rule | Predecessor | Successor | Conditional | Task Follow-Up Mode | Standard Workload Ref |
|---|---|---|---|---|---|---|---|---|---|---|---|

**Planned Hours kolonu burada bulunmaz.**

Saat/workload merkezi Standard Workload Engine tarafından hesaplanır.

---

# 32. STANDARD WORKLOAD MAPPING FORMAT

`06_STANDARD_WORKLOAD_MAPPING.json` örnek yapısı:

```json
{
  "function": "R&D",
  "mappings": [
    {
      "plan_item_id": "RND-L2-001",
      "plan_item_name": "Design Review",
      "standard_workload_reference": "SW-XXX",
      "mapping_status": "CONFIRMED"
    },
    {
      "plan_item_id": "RND-L2-002",
      "plan_item_name": "Special Analysis",
      "standard_workload_reference": null,
      "mapping_status": "WORKLOAD_STANDARD_GAP"
    }
  ]
}
```

Burada:

- difficulty bulunmaz,
- saat bulunmaz.

Bunlar merkezi sistemden gelir.

---

# 33. TASK FOLLOW-UP MAPPING FORMAT

`04_TASK_FOLLOWUP_MAPPING.json`:

```json
{
  "function": "Purchasing",
  "mappings": [
    {
      "plan_item_id": "PUR-L2-001",
      "mode": "EXCEPTION_ONLY",
      "recommended_trigger": [
        "DELAYED",
        "BLOCKED"
      ]
    }
  ]
}
```

Önerilen mode değerleri:

```text
NONE
ALWAYS_LINK
EXCEPTION_ONLY
USER_DECIDES
```

Bu dosya mevcut Task Follow-Up status modelini değiştirmez.

---

# 34. OPEN QUESTIONS

Claude çözemediği konuları uydurmak yerine kaydetmelidir.

`12_OPEN_QUESTIONS.md`:

| ID | Question | Why It Matters | Owner | Blocking? |
|---|---|---|---|---|

Özellikle:

- Standard Workload eşleşmesi
- cross-function dependency
- uncertain conditional logic
- unclear approval

gizlenmemelidir.

---

# 35. FUNCTION SIGN-OFF

Final:

```text
Function:
Reviewed By:
Date:
Version:
Status: Draft / Reviewed / Approved
```

Checklist:

```text
[ ] Function scope approved
[ ] L2 plan approved
[ ] Inputs/outputs approved
[ ] Dependencies approved
[ ] Cross-functional handovers reviewed
[ ] Task Follow-Up mapping approved
[ ] Standard Workload mapping reviewed
[ ] Conditional/exception logic reviewed
[ ] Document/notification needs reviewed
[ ] UI requirements reviewed
[ ] Critical Review discussed
[ ] Open questions accepted

R&D only:
[ ] L3 architecture approved
[ ] Proto reviewed
[ ] Design reviewed
[ ] Test reviewed
[ ] Vehicle Test reviewed
[ ] Competitor Analyse reviewed
```

---

# 36. MERKEZİ ENTEGRASYON

Fonksiyon paketleri production portalı doğrudan değiştirmez.

Final entegrasyon:

```text
R&D Package
Process Package
Purchasing Package
Quality Package
Logistic Package
CRD-LS Package
        ↓
Contract Validation
        ↓
Cross-Function Conflict Analysis
        ↓
L1/L2/L3 Dependency Graph
        ↓
Standard Workload Mapping Validation
        ↓
Task Follow-Up Integration Mapping
        ↓
Template Engine
        ↓
Master Project Portal
```

Çakışmalar merkezi Claude Code tarafından görünür hale getirilir; otomatik business decision verilmez.

---

# 37. ANA TASARIM PRENSİBİ

> **Platform centrally governed, planning function-owned.**

Türkçesi:

> **Platform merkezi yönetilir, planlamanın sahibi fonksiyondur.**

Fonksiyonun sahibi olduğu alan:

- L2 proses içeriği
- R&D ise L3 proses içeriği
- input/output
- dependencies
- conditions
- exceptions
- handover ihtiyacı
- Task Follow-Up'a ne zaman aksiyon taşınacağı
- Standard Workload activity eşlemesi
- doküman ihtiyacı
- minimum UI ihtiyacı

Merkezi platformun sahibi olduğu alan:

- L1 Project Plan
- Project ID
- Project Difficulty
- Standard Workload değerleri
- Task Follow-Up ortak sistemi
- Authentication
- Roles / Delegation
- Audit
- Archive
- Notification Engine
- Workload Engine
- Global UI
- i18n
- Data integrity
- Integration architecture

---

# 38. CLAUDE'UN İLK MESAJI

Bu dosya açıldığında hazır tasarım üretme.

Şu mesajla başla:

> Bu çalışma boyunca sizin fonksiyonunuzun L2 Function Plan'ını birlikte oluşturacağız. Süreci mümkün olduğunca seçimlerle ilerleteceğim; yalnız gerçekten gerekli olduğunda kısa açıklama isteyeceğim. Mevcut Task Follow-Up yapısını ve Standard Workload sistemini değiştirmeyeceğiz. Project Difficulty PM tarafından L1'de belirleniyor ve workload buradan otomatik hesaplanacak. Eğer R&D fonksiyonundaysanız gerekli L3 alt planlarını da ayrıca tasarlayacağız.
>
> İlk seçim:
>
> **Hangi fonksiyon için çalışıyoruz?**
>
> A. R&D  
> B. Process  
> C. Purchasing  
> D. Quality  
> E. Logistic  
> F. CRD-LS
