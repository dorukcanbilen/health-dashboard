# Birim ve Referans Aralığı Notları

Bu dosya, 2023-2026 arası e-Nabız tahlil raporlarında test birimlerinin ve referans aralıklarının
neden yıldan yıla değiştiğini belgeler. Amaç: hangi değişikliklerin gerçek (lab/yöntem farkı),
hangilerinin sadece yazım farkı, hangisinin ise kaynak PDF'teki bir **etiketleme hatası**
olduğunu netleştirmek ve bu projede kullanılan verilerde hangi düzeltmenin uygulandığını
kayıt altına almak.

## 1. Düzeltilen hata: 2024 CRP birimi

**Sorun:** Doruk, İrem ve Deniz'in 2024 raporlarında birebir şu satır geçiyor:

```
CRP ( Türbidimetrik) 0.1 mg/dL 0 - 5
```

Ancak "0 - 5" aralığı mg/dL için değil, mg/L için tipik bir referanstır. mg/dL biriminde CRP
referansı genelde "0 - 0,5" civarındadır — nitekim Deniz'in doğru etiketlenmiş 26.04.2023
tahlilinde tam olarak böyle yazıyor:

```
CRP 0,0365 mg/dL 0 - 0,5
```

2025 ve 2026 raporlarında ise birim doğru şekilde "mg/L" olarak değişmiş, referans aralığı yine
aynı "0 - 5":

```
CRP ( Türbidimetrik) 2.02 mg/L 0 - 5      (Doruk, 12.08.2025)
CRP ( Türbidimetrik) 1,5 mg/L 0 - 5        (Doruk, 30.07.2026)
```

**Sonuç:** 2024'te laboratuvarın CRP yöntemi muhtemelen zaten mg/L bazında sonuç veriyordu, ama
o yılın e-Nabız PDF şablonu birim etiketini güncellemeyi unutmuş / eski "mg/dL" etiketini basmaya
devam etmiş. Bu, gerçek bir ikinci birim değişikliği değil, 2024 rapor şablonundaki bir
yazım/etiket hatasıdır.

**Uygulanan düzeltme:** Bu projede (ProcessedData/*.md ve HealthDashboard.html içindeki veri),
Doruk, İrem ve Deniz'in **14.08.2024 tarihli CRP** kaydının birimi `mg/dL` yerine `mg/L` olarak
düzeltildi. Değer (0.1 / 0.1 / 0.1) değiştirilmedi, sadece etiket düzeltildi. Deniz'in
26.04.2023 tarihli CRP kaydı (gerçekten mg/dL olan, farklı hastaneden gelen ölçüm) olduğu gibi
bırakıldı. Bu sayede CRP artık 2024-2026 arasında tek bir birimde (mg/L) tutarlı şekilde
karşılaştırılabiliyor.

Düzeltme kodda `corrections.py` dosyasındaki `CORRECTIONS` sözlüğü üzerinden uygulanıyor, böylece
kaynak PDF'ler yeniden işlenirse bu düzeltme otomatik olarak tekrar uygulanır.

## 2. Gerçek değişiklik ama hata değil: Tiroid testleri (FT3, FT4)

Doruk ve İrem'in FT3/FT4 sonuçları 2023-2024'te `pmol/L`, 2025-2026'da `ng/L` / `ng/dL` olarak
raporlanmış:

```
Serbest T3 (FT3) 4.71 pmol/L 3,1 - 6,8      (Doruk, 2023)
Serbest T3 (FT3) 5.77 pmol/L 3,1 - 6,8      (Doruk, 2024)
Serbest T3 (FT3) 4.58 ng/L (ref: 2.408-5.289)  (Doruk, 2025)
```

Sağlık kuruluşu 4 yıl boyunca hiç değişmedi (hep "Çanakkale Gelibolu 006 Nolu Aile Hekimliği
Birimi"), bu yüzden bu değişiklik bir etiket hatası değil — kliniğin 2025 civarında tiroid
testlerini gönderdiği referans laboratuvarın veya ölçüm yönteminin/analiz cihazının değişmiş
olmasından kaynaklanıyor. FT3/FT4 için farklı immünoassay yöntemleri birbirine doğrudan
çevrilemez (bilinen bir klinik kimya sorunu), bu yüzden bu testler **düzeltilmedi** — 2023-2024
ve 2025-2026 dönemleri ayrı birimler olarak veride duruyor.

TSH'de görünen "değişim" (µIU/mL → mIU/L) ise aslında aynı büyüklük — µIU/mL ve mIU/L matematiksel
olarak birebir eşdeğerdir, bu yüzden referans aralığı (0,27-4,5) hiç değişmemiş. Bir düzeltme
gerekmiyor.

## 3. Gerçek değişiklik ama hata değil: Deniz'in 2023 tahlili farklı hastanede

Deniz'in 2023 tahlili "Özel Ordu Sevgi Hastanesi"nde, 2024-2026 tahlilleri ise
"Çanakkale Gelibolu 006 Nolu Aile Hekimliği Birimi"nde yapılmış. Bu yüzden HCT, HGB, MCV, MCH,
RBC, WBC, PLT, ALT, AST, açlık glukozu gibi birçok testte 2023 referans aralığı farklı, ama
2024-2025-2026 arasında tamamen sabit (aynı laboratuvar = aynı referans seti). Bu bir hata değil,
farklı laboratuvarların kendi doğrulanmış referans aralıklarını kullanmasından kaynaklanan normal
bir durum. Düzeltme uygulanmadı.

Kreatinin'de ayrıca 2024→2025 arasında küçük bir artış var (0,32-0,59 → 0,4-0,6 mg/dL); bu aynı
laboratuvarın Deniz büyüdükçe (yaklaşık 7'den 8 yaşına) pediatrik referans aralığını güncellemiş
olmasından kaynaklanıyor olabilir. Bilgi amaçlı not edildi, düzeltme gerektirmiyor.

## 4. Hata değil, sadece yazım farkı: birebir eşdeğer birimler

Aşağıdaki birim çiftleri **matematiksel olarak birebir aynı büyüklüğü** ifade eder; sadece
raporlar arasında farklı yazılmış, gerçek bir değer dönüşümü gerekmez:

- `ng/mL` ≡ `µg/L` (Ferritin)
- `pg/mL` ≡ `ng/L` (Vitamin B12, Folat)
- `µU/mL` ≡ `mU/L` (İnsülin)
- `µIU/mL` ≡ `mIU/L` (TSH)
- `10³/uL` ≡ `10*3/uL` ≡ `10^3/uL` (Hemogram sayımları — sadece üs karakterinin PDF'te farklı
  render edilmesi, örn. "³" vs "*3")

Bu testlerde referans aralıkları zaten yıllar arasında sabit kaldığı için (örn. Ferritin
30-400 hep aynı sayılarla basıldı, sadece birim etiketi değişti), bir düzeltme uygulanmadı —
zaten doğrudan karşılaştırılabilir durumdaydılar.

## Özet tablo

| Test | Değişim türü | Düzeltme uygulandı mı? |
|---|---|---|
| CRP (2024) | Etiket hatası (mg/dL yazılmış, aslında mg/L) | ✅ Evet — mg/L olarak düzeltildi |
| FT3, FT4 | Gerçek yöntem/laboratuvar değişikliği (2025) | ❌ Hayır — orijinal birimler korundu |
| TSH | Eşdeğer birim yazımı (µIU/mL = mIU/L) | Gerek yok |
| Ferritin, Vit B12, Folat, İnsülin | Eşdeğer birim yazımı | Gerek yok |
| Deniz — genel referans kayması (2023→2024) | Farklı hastane/laboratuvar | ❌ Hayır — gerçek durum |
| Deniz — Kreatinin (2024→2025) | Muhtemelen yaşa bağlı pediatrik referans güncellemesi | ❌ Hayır — gerçek durum |
