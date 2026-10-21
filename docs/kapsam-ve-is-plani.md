# Bul-Getir: Proje Başlığı, Kapsam ve İş Planı

## 1. Proje künyesi

| Alan | Bilgi |
| --- | --- |
| Proje adı | Bul-Getir: Kampüs Kayıp Eşya İlan Uygulaması |
| Ders | Mobile Programming, 2026–2027 Güz |
| Ekip | 1 kişi (tüm roller proje yürütücüsünde) |
| Proje yürütücüsü | Azizhan Kaya, 1306220029 |
| Platform | Android ve iOS (Flutter) |
| Veri saklama | SQLite (cihazda) + Firebase (uzak sunucu) |
| GitHub | [github.com/AzizhanKaya/bul-getir](https://github.com/AzizhanKaya/bul-getir) (public) |

## 2. Amaç ve çözülen problem

Amaç, kampüste kaybolan bir eşyanın sahibine en kısa sürede ve tek bir yerden ulaşmasını sağlamaktır.

Bugün kayıp eşya duyuruları WhatsApp grupları, Instagram hesapları, panolar ve güvenlik birimi arasında dağınık durumda. Bulan kişi nereye haber vereceğini, kaybeden kişi nereye bakacağını bilmiyor. Duyurular kısa sürede mesaj akışında kayboluyor ve aranamıyor.

Bul-Getir bu duyuruları tek bir uygulamada toplar. İlanlar kategoriye, konuma ve tarihe göre aranabilir. Benzer bir ilan eklendiğinde ilgili kullanıcıya bildirim gider.

## 3. Hedef kitle ve kullanım senaryoları

Hedef kitle: üniversite öğrencileri, akademik ve idari personel, kampüs güvenlik birimi.

1. **Kaybeden:** Ayşe kütüphanede kulaklığını unutur. Uygulamada "Kayıp" ilanı açar, fotoğraf ve konum ekler.
2. **Bulan:** Mehmet yemekhanede bir öğrenci kartı bulur. Fotoğrafını çeker, yerini haritada işaretler ve "Bulundu" ilanı girer.
3. **Eşleşme:** Ayşe'ye benzer ilan bildirimi gelir. Uygulama içinden Mehmet'e yazar, eşyayı teslim alır ve ilan "Teslim edildi" olarak kapanır.

## 4. Kapsam

**Temel özellikler (mutlaka yapılacak):**

| Özellik | Ne yapar | Kullanılan mobil yetenek |
| --- | --- | --- |
| Giriş / kayıt | Okul e-postasıyla hesap açma | Firebase Authentication |
| İlan oluşturma | Kayıp veya Bulundu ilanı: başlık, kategori, açıklama, tarih | Form, doğrulama |
| Fotoğraf ekleme | Kameradan veya galeriden fotoğraf | Kamera, galeri izni |
| Konum | İlanın yerini haritada işaretleme | GPS, Google Maps |
| Liste ve arama | Kategori, tür ve tarihe göre filtreleme | Liste, arama |
| Çevrimdışı kullanım | İlanlar cihazda saklanır, internet gelince eşitlenir | SQLite |
| Bildirim | Eşleşen ilan ve yeni mesaj bildirimi | Firebase Cloud Messaging |
| Uygulama içi mesaj | İlan sahibiyle iletişim | Firestore (gerçek zamanlı) |

**Ek özellikler (zaman kalırsa):**

- Akıllı eşleştirme: kategori, konum ve tarihe göre benzer ilanları önerme
- QR etiket: eşyaya yapıştırılan QR okutulunca sahibine bildirim
- Karanlık tema

## 5. Kapsam dışı

- Ödüllü ilan veya ödeme sistemi
- Web veya masaüstü sürümü
- Okulun öğrenci bilgi sistemiyle entegrasyon
- Yapay zekâ ile görüntü benzerliği (ileri sürüm fikri)

## 6. Teknoloji ve mimari

| Katman | Teknoloji |
| --- | --- |
| Uygulama | Flutter (Dart) |
| Yerel veritabanı | SQLite (`sqflite` paketi) |
| Uzak sunucu | Firebase: Authentication, Cloud Firestore, Cloud Storage, Cloud Messaging |
| Harita ve konum | `google_maps_flutter`, `geolocator` |
| Kamera | `image_picker`, QR için `mobile_scanner` |

**Çalışma mantığı:**

```
Arayüz (ekranlar)
      ↓
Repository katmanı ── internet varken eşitleme ──> Firebase
      ↓
SQLite (çevrimdışı önbellek, taslaklar, favoriler)
```

Uygulama veriyi önce SQLite'a yazar, böylece internetsiz de çalışır. Bağlantı gelince Repository katmanı bekleyen kayıtları Firebase'e gönderir ve yeni ilanları SQLite'a çeker. Bu yapı dersin "SQLite ve uzak sunucu" şartının ikisini birden karşılar.

## 7. Veri modeli

| Tablo | Temel alanlar | SQLite | Firestore |
| --- | --- | --- | --- |
| users | id, ad, e-posta, foto_url | Oturum açan kullanıcı | Tüm kullanıcılar |
| items | id, tür (kayıp/bulundu), başlık, kategori, açıklama, foto_url, enlem, boylam, tarih, durum, kullanıcı_id, sync_status | Önbellek + taslaklar | Ana kayıt |
| categories | id, ad, ikon | Sabit liste | Sabit liste |
| messages | id, ilan_id, gönderen_id, alıcı_id, metin, zaman | Son mesajlar | Ana kayıt |
| favorites | kullanıcı_id, ilan_id | Yalnızca cihazda | Yok |

Fotoğraflar Firebase Storage'da tutulur, ilan kaydında yalnızca bağlantısı saklanır.

## 8. Haftalık iş planı

| Hafta | Aşama | Yapılacaklar | Teslim / Kilometre taşı |
| --- | --- | --- | --- |
| 1–6 | Planlama | Konu seçimi, kapsam, iş planı, organizasyon şeması, GitHub reposu, ekran taslakları | **6. hafta: 1. sunum** |
| 7–8 | Altyapı ve ön prototip | Flutter projesi, Firebase kurulumu, SQLite şeması, giriş ekranı, ilan oluşturma | **Vize haftası: ön prototip** |
| 9–10 | Temel özellikler | Kamera ile fotoğraf, harita ve konum, liste ve arama, SQLite–Firebase eşitleme | İlerleme raporu |
| 11–12 | Bildirim ve mesajlaşma | Bildirimler, uygulama içi mesaj; zaman kalırsa ek özellikler | İlerleme raporu |
| 13–14 | Test ve teslim | Cihaz testleri, hata düzeltme, final raporu, demo videosu, LinkedIn tanıtım paylaşımı | **Final sunumu + video + rapor** |

Her hafta ilerleme raporu yazılır ve GitHub'a düzenli commit atılır. Derste sunum sırası yaklaşık üç haftada bir gelir. Vize haftasının kesin tarihi ders takvimine göre güncellenecek.

## 9. Riskler ve önlemler

| Risk | Etkisi | Önlem |
| --- | --- | --- |
| Zaman yetmemesi | Final teslimi gecikir | Temel/ek özellik ayrımı, haftalık hedef kontrolü |
| SQLite–Firebase eşitleme hataları | Veri kaybı veya çift kayıt | Sunucudaki kayıt esas alınır, `sync_status` ile izlenir |
| Sahte veya uygunsuz ilan | Güven kaybı | Okul e-postası zorunlu, ilan şikâyet butonu |
| Kişisel veri paylaşımı | Gizlilik sorunu | Telefon numarası gizli, iletişim yalnızca uygulama içinden |
| Firebase ücretsiz kota sınırı | Hizmet durur | Fotoğraflar yüklenmeden önce sıkıştırılır |

## 10. Tanıtım planı (halkla ilişkiler)

Proje, tamamlandığında LinkedIn'de tek bir paylaşımla tanıtılır. Paylaşım 13–14. haftalarda, final sürümü hazır olduğunda yapılır ve bağlantısı ilerleme raporuna ve final raporuna eklenir.

Paylaşımda yer alacaklar:

- Projenin amacı ve çözdüğü problem
- Uygulamadan ekran görüntüleri veya kısa demo videosu
- Kullanılan teknolojiler (Flutter, SQLite, Firebase)
- GitHub reposunun bağlantısı
