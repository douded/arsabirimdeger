🧱 Genel Ekran Yerleşimi

Arayüz iki ana karttan oluşur:

Sol Kart → Veri Girişi ve Hesaplama

Sağ Kart → Kaydedilenler Listesi

📱 Mobilde bu iki kart alt alta görünür.

💻 Masaüstünde kartlar yan yana gösterilir.

1️⃣ Veri Girişi ve Hesaplama Alanı

Bu alan, tek bir bağımsız bölüm / daire / bağımsız bölüm için veri girip hesaplama yaptığın kısımdır.

🔹 Düzenleme Modu Göstergesi

Üstte “Düzenleme Modu Aktif” yazan turuncu bir banner (Düzenleme Modu Aktif) bulunur.

Bu alan sadece kayıt düzenlerken görünür.

Yeni kayıt oluştururken gizlidir.

🔹 Form Alanları
1. Kayıt Adı / Blok / Daire No
Kayıt Adı / Blok / Daire No
Örn: A Blok Daire 5


Tamamen sana yardımcı olacak bir tanımlama alanıdır.

Listede bu ad görünür.

Boş bırakırsan “İsimsiz Kayıt” olarak kaydedilir.

2. Arsa Alanı (m²)
Arsa Alanı (m²)


Parselin toplam yüzölçümü.

Örn: 1500 (m²)

3. Arsa Pay / Arsa Payda
Arsa Pay
Arsa Payda


Tapudaki arsa payını ifade eder:

Örn: Arsa payı 30/1500 ise:

Arsa Pay: 30

Arsa Payda: 1500

Bu değerler, arsa payı alanı hesabında kullanılır.

4. Daire Alanı (m²)
Daire Alanı (m²)


Bağımsız bölümün yüzölçümü.

Örn: 120 (m²)

5. Yapı Birim Maliyeti (TL/m²)
Yapı Birim Maliyeti (TL/m²)


İlgili yapı sınıfına göre birim m² maliyeti.

Örn: 18000 (TL/m²)

6. Toplam Değer (TL)
Toplam Değer (TL)


Söz konusu dairenin piyasa toplam değeri.

Örn: 6.500.000 (TL)

7. Şerefiye Oranı / Müteahhit Karı (%)
Şerefiye Oranı  Müteahhit Karı (%)


Toplam değer içerisindeki şerefiye + müteahhit karı oranı.

Örn: 15 (%15)

🔘 Butonlar
HESAPLA & KAYDET
HESAPLA & KAYDET


Bu butona bastığında:

Formdaki tüm değerler okunur.

Aşağıdaki formüllere göre hesaplama yapılır.

Sonuçlar ekrandaki sonuç kutusunda gösterilir.

Kayıt, tarayıcı localStorage’ına emlakDB anahtarı altında eklenir (veya güncellenir).

Sağ taraftaki “Kaydedilenler” listesi otomatik güncellenir.

Eğer düzenleme modundaysan (önceden kaydedilmiş bir kaydı açtıysan), bu buton mevcut kaydı günceller.
Değilsen, yeni bir kayıt oluşturur.

Temizle / Yeni Kayıt
Temizle / Yeni Kayıt


Formdaki tüm alanları sıfırlar.

Sonuç kutusunu gizler.

Düzenleme modunu kapatır.

Buton tekrar “HESAPLA & KAYDET” (mavi) haline döner.

Yeni bir kayıt girmek için bu butonu kullanarak “sıfır ekran”a dönersin.

2️⃣ Hesaplama Mantığı (Formüller)

Form verilerini şu obje ile topluca okur:

{
  arsaAlani,
  arsaPay,
  arsaPayda,
  daireAlani,
  yapiMaliyeti,
  toplamDeger,
  serefiyeOrani
}


Ardından şu sıralama ile hesaplanır:

1. Arsa Payı Alanı
Arsa Payı Alanı = Arsa Alanı × Arsa Pay / Arsa Payda


Kod karşılığı:

arsaPayiAlani = (arsaAlani * arsaPay) / (arsaPayda || 1);

2. Toplam Yapı Maliyeti
Toplam Yapı Maliyeti = Daire Alanı × Yapı Birim Maliyeti


Kod:

toplamYapiMaliyeti = daireAlani * yapiMaliyeti;

3. Şerefiye Değeri
Şerefiye = Toplam Değer × (Şerefiye Oranı / 100)


Kod:

serefiyeDegeri = toplamDeger * (serefiyeOrani / 100);

4. Arsa Değeri
Arsa Değeri = Toplam Değer − (Toplam Yapı Maliyeti + Şerefiye)


Kod:

arsaDegeri = toplamDeger - (toplamYapiMaliyeti + serefiyeDegeri);

5. Birim Değer (TL/m²)
Birim Değer = Arsa Değeri / Arsa Payı Alanı


Kod:

let birimDeger = 0;
if (arsaPayiAlani > 0) {
    birimDeger = arsaDegeri / arsaPayiAlani;
}


Bunların hepsi hesapla(veri) fonksiyonunda yapılır ve şu şekilde döner:

return {
  arsaPayiAlani,
  toplamYapiMaliyeti,
  serefiyeDegeri,
  arsaDegeri,
  birimDeger
};

3️⃣ Sonuçların Gösterilmesi

Sonuçlar, formun altındaki gri sonuç kutusunda gösterilir:

Arsa Payı Alanı → X m²

Toplam Yapı Maliyeti → X TL

Şerefiye Değeri → X TL

Arsa Değeri → X TL

Birim Değer → X TL/m² (vurgulu ve mavi)

Tüm değerler formatTL() fonksiyonu ile Türkçe sayı formatında (1.234,56) gösterilir.

4️⃣ Kaydedilenler Listesi

Sağ taraftaki kartta, daha önce kaydettiğin tüm kayıtlar listelenir.

Tablo Sütunları

Tanım → Kayıt Adı / Blok / Daire No (veya “İsimsiz Kayıt”)

Birim Değer → Hesaplanan TL/m² değeri

İşlem → Düzenle / Sil butonları

Veriler şu key altında saklanır:

localStorage["emlakDB"]


Bu emlakDB değeri bir dizi (Array) formunda kayıtları tutar.

Sıralama

Liste güncellendiğinde:

kayitlar.slice().reverse() kullanılır,

Yani en son eklenen kayıt en üstte görünür.

5️⃣ Kayıt Düzenleme

Listede herhangi bir satırdaki “Düzenle” butonuna bastığında:

İlgili kayıt, id alanına göre bulunur.

Form alanları, kayıt içindeki inputs verileriyle doldurulur.

Gizli recordId alanı set edilir.

Üstte turuncu “Düzenleme Modu Aktif” banner’ı görünür.

Mavi “HESAPLA & KAYDET” butonu:

Turuncu renkli,

“GÜNCELLE” yazan bir butona dönüşür.

Aynı anda hesaplama fonksiyonu çalıştırılarak sonuç kutusu anında gösterilir.

Ekran yukarıya kaydırılır (kullanıcı formu ve sonucu birlikte görsün diye).

Daha sonra:

Değerlerde değişiklik yapıp GÜNCELLE butonuna basarsan:

İlgili kayıt id üzerinden güncellenir.

localStorage tekrar yazılır.

Liste yenilenir.

İşlem sonrası form temizle() ile sıfırlanır.

6️⃣ Kayıt Silme

Sil butonuna bastığında:

Kullanıcıdan confirm(...) ile onay alınır.

İlgili kayıt, localStorage içindeki emlakDB dizisinden filtrelenir.

Liste güncellenir.

Eğer o an formda düzenlenen kayıt silindiyse, form da temizlenir.

7️⃣ Veri Saklama ve Sınırlar

Tüm veriler yalnızca bu tarayıcı ve bu cihaz üzerinde saklanır.

Farklı bir cihazda veya farklı bir tarayıcıda açarsan:

localStorage["emlakDB"] boş olduğundan kayıt görünmez.

Tarayıcı geçmişini / site verilerini silersen kayıtlar da kaybolur.

Not: Bu bilerek böyle tasarlanmıştır; hiçbir veri sunucuya gönderilmez.

📱 Mobil Kullanım

Tüm input’lar font-size: 16px ile ayarlandığı için:

iPhone gibi cihazlarda, input’a tıklayınca otomatik zoom sorunu azaltılmıştır.

Tasarım mobile uyumludur:

Küçük ekranlarda kartlar alt alta gelir.

Tablo yatay taşma durumunda kaydırılabilir (overflow-x: auto).

Öneri:

Uygulamayı tarayıcıda açıp:

“Ana Ekrana Ekle” (Add to Home Screen) yaparak,

Kısa yol üzerinden adeta bir mobil app gibi kullanabilirsin.

⚠️ Uyarı

Bu araç:

Bir hesaplama ve kayıt tutma aracıdır.

Hukuki, mali, vergisel veya resmi değerleme beyanı olarak tek başına kullanılmamalıdır.

Resmi raporlar ve mevzuat çerçevesinde, SPK, BDDK, ilgili yönetmelikler, Emsal araştırmaları ve teknik raporlar her zaman ayrıca dikkate alınmalıdır.

Uygulamanın kullanımı sonucu oluşabilecek hata ve kararlardan kullanıcı sorumludur.

# 🧮 Arsa Payı ve Değer Hesaplayıcı (Pro)

Bu uygulama, **arsa payı alanı**, **arsa değeri**, **yapı maliyeti**, **şerefiye değeri** ve **birim değer (TL/m²)** gibi kalemleri hızlı ve tutarlı şekilde hesaplayıp saklaman için tasarlanmış bir **mini emlak değerleme aracı**dır.

Tamamen **tarayıcı üzerinde** çalışır:

- ✅ Veri, kullanıcının kendi tarayıcısında `localStorage` ile saklanır  
- ✅ Sunucu tarafı yok, backend yok  
- ✅ İnternetsizken bile, aynı cihaz ve tarayıcıda çalışmaya devam eder  
- ✅ Mobil ve masaüstü uyumlu (responsive tasarım)

---

## 🔗 Nasıl Açılır?

GitHub Pages ile yayına alındığında, örneğin şu şekilde çalışır:

```text
https://douded.github.io/arsabirimdeger/
