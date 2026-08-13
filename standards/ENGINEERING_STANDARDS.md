# Mühendislik Standardı

> Sürüm: 1.1  
> Son güncelleme: 2026-08-14

## Hedef

Kod, yıllar sonra projeyi ilk kez açan bir geliştiricinin nereden başlayacağını anlayabileceği kadar düzenli; uzman bir geliştiricinin de gereksiz açıklamalar içinde kaybolmayacağı kadar kesin olmalıdır.

## Tasarım ilkeleri

- Her modülün tek ve açık bir sorumluluğu olmalıdır.
- Ağ, depolama, iş mantığı, Worker, kullanıcı arayüzü ve dış entegrasyon sınırları ayrılmalıdır.
- İş kuralları kullanıcı arayüzüne veya rastgele yardımcı fonksiyonlara gömülmemelidir.
- UI kapanması uzun süren işi durdurmamalıysa iş bağımsız süreç/Worker içinde yürütülmelidir.
- Bağımlılıklar mümkün olduğunca dışarıdan verilmeli ve test edilebilir olmalıdır.
- Genel bir soyutlama yalnızca gerçek tekrar veya değişim ihtiyacı varsa eklenmelidir.
- Ölçülmemiş performans sorunu için daha karmaşık dil veya yerel kod eklenmez.

## Prototipten ürüne geçiş

- Prototip hızlı davranış kanıtı içindir; doğrudan üretim mimarisi sayılmaz.
- Taşımadan önce davranış sözleşmesi, hata kodları ve kabul testleri dondurulur.
- Yeni dildeki uygulama eski prototipin kontrollü testlerini birebir geçmelidir.
- Eski prototip, taşıma tamamlanana kadar test oracle/referans olarak korunur.
- UI, çekirdek davranış kanıtlanmadan geliştirme kararlarının merkezi yapılmaz.

## Her proje için yaşam döngüsü

1. Gerçek problem, hedef kullanıcı, kapsam dışı alanlar ve başarı ölçütü yazılır.
2. N-00 isim kapısı için çalışma adı seçilir ve çakışma araştırması planlanır.
3. Risk, mahremiyet, veri kaybı ve platform kısıtları erken değerlendirilir.
4. En hızlı güvenli teknolojiyle davranış prototipi ve test oracle'ı hazırlanır.
5. Kritik hata senaryoları kontrollü sunucu/veri ve hata enjeksiyonuyla kanıtlanır.
6. Davranış sözleşmesi dondurulur; kalıcı dil ve mimariye geçilir.
7. Çekirdek, kalıcı veri ve arka plan işleyişi UI'dan önce doğrulanır.
8. UI, yerelleştirme, erişilebilirlik ve dış entegrasyonlar eklenir.
9. Güvenlik, regresyon, paketleme, güncelleme ve geri dönüş testleri tamamlanır.
10. Kod, kullanıcı/geliştirici belgeleri ve PDF/sunum ortak incelemeden geçer.
11. Yayın sonrası sorunlar issue → test → düzeltme → regresyon → sürüm akışıyla yönetilir.

Teknoloji seçimi her projede yeniden değerlendirilir; AX Downloader için alınan C# kararı bütün gelecek projelere körlemesine uygulanmaz.

## Adlandırma ve biçim

- Açık kaynak kodundaki tanımlayıcılar İngilizce olmalıdır.
- İsimler niyeti anlatmalı; belirsiz kısaltmalardan kaçınılmalıdır.
- Projenin seçtiği otomatik biçimlendirici ve linter tek kaynak olmalıdır.
- Sabitler, hata kodları ve durumlar merkezi ve tutarlı tanımlanmalıdır.
- Kullanıcıya gösterilen metinler iş mantığına gömülmez; yerelleştirme kaynağından alınır.

## N-00 — proje adı kapısı

Her kamuya açık proje geliştirme başında çalışma adı alabilir; açık beta öncesinde nihai ad kapısından geçer:

1. Ayırt edici, uydurma veya güçlü biçimde özgün adaylar üret.
2. İnternet araması, GitHub, uygulama mağazaları, paket kayıtları, alan adı ve sosyal hesapları tara.
3. Uygun olduğunda TÜRKPATENT, WIPO ve EUIPO gibi marka veri tabanlarını kontrol et.
4. Aynı veya karıştırılabilir isim riski varsa adayı reddet.
5. Seçim gerekçesi ve arama tarihi proje kaydına yazılır.
6. Beta ve v1.0 öncesinde tarama tekrar edilir.

Bu süreç mutlak hukuki garanti değildir; çakışma riskini erken ve belgeli biçimde azaltan mühendislik kapısıdır.

## Yerelleştirme

- Varsayılan/fallback dil açıkça belirlenir.
- Kaynak kod, state ve loglarda dil bağımsız sabit hata kodları kullanılır.
- Kullanıcı metinleri ayrı kaynak dosyalarında tutulur.
- Çeviriler yalnızca kelime eşitliğiyle değil bağlam, taşma, çoğul ve erişilebilirlikle test edilir.
- Desteklenecek diller proje başında sınırlandırılır; gelecekteki varsayımsal diller için gereksiz kapsam oluşturulmaz.

## Yorum ve dokümantasyon

- Yorum “kod ne yapıyor?” yerine “neden böyle yapılıyor?” sorusunu cevaplar.
- Karmaşık protokol, uyumluluk veya güvenlik kararı ilgili karar kaydına bağlanır.
- Her halka açık API'nin girdi, çıktı, hata ve yan etki sözleşmesi açıklanır.
- Durum makineleri ve veri migrasyonları tablo veya diyagramla belgelenir.

## Hata yönetimi

- Hatalar sessizce yutulmaz.
- Kullanıcı mesajı eyleme dönük; teknik günlük teşhis edilebilir olmalıdır.
- Tekrar deneme sınırlı, gecikmeli, iptal edilebilir ve gözlemlenebilir olmalıdır.
- Yarım işlem dosyaları atomik yazma, geçici dosya ve güvenli devam mekanizmasıyla korunmalıdır.
- Bütünlük doğrulanmadıysa işlem tamamlanmış ilan edilmez.
- Belirsizlikte yanlış otomasyon yerine açık kullanıcı müdahalesi durumu tercih edilir.

## Test katmanları

- Birim: Saf iş kuralları ve sınır durumları
- Entegrasyon: Dosya sistemi, ağ, veritabanı, IPC ve süreç sınırları
- Uçtan uca: Gerçek kullanıcı akışı
- Dayanıklılık: Ctrl+C, süreç kapanması, ağ kesintisi, yeniden başlatma, bozuk state
- Hata enjeksiyonu: 429/503, timeout, kısa/fazla yanıt, disk dolması, byte bozulması
- Güvenlik: Yol geçişi, istenmeyen üzerine yazma, sır sızıntısı, zararlı girdi
- Regresyon: Yeni düzeltmenin daha önce geçen davranışları bozmaması

Test adı senaryoyu ve beklenen sonucu anlatmalıdır. “Geçti” kaydı tarih, ortam ve kanıtla tutulur. Testin bir açık bulması da başarılı bir test çalışmasıdır; ürün özelliğinin geçtiği anlamına gelmez.

## Düzeltme akışı

Her ciddi hata için sıra:

1. Tekrar üret
2. Kök nedeni belirle
3. Başarısız test oluştur
4. En küçük doğru düzeltmeyi yap
5. Yeni testi geçir
6. Tüm ilgili regresyonları çalıştır
7. Hedef işletim sisteminde gerçek senaryoyu doğrula

“Çalıştı gibi” veya rastgele yeniden deneme eklemek çözüm sayılmaz.

## Tamamlanma tanımı

Bir özellik ancak şu koşullarda tamamdır:

- Kabul kriterleri ölçülebilir ve geçmiştir.
- Hata ve kesinti yolu test edilmiştir.
- Veri kaybı/bütünlük etkisi değerlendirilmiştir.
- Log ve kullanıcı mesajları anlamlıdır.
- Belge ve çeviriler davranışla uyumludur.
- Önceki regresyonlar geçmiştir.
- Bilinen sınırlamalar açıkça yazılmıştır.

Hiçbir proje için dürüst olmayan “sıfır hata” garantisi verilmez. Veri işleyen projelerde sessiz veri bozulmasına sıfır tolerans uygulanır.

## Sürüm ve yayın

- Anlamlı sürümlerde SemVer tercih edilir.
- Her yayın: değişiklik notu, kurulum, yükseltme, bilinen sınırlamalar, checksum ve geri dönüş bilgisi içerir.
- Prototip kodu modülerleştirme ve yayın kontrolü yapılmadan üretim hazır sayılmaz.
- Kaynak kod ile yayımlanan paket aynı commit/tag ile izlenebilir olmalıdır.

## Kontrollü yenilik

- Kanıtlanmış çözüm varsayılan başlangıç noktasıdır, değişmez dogma değildir.
- Yeni fikir önce problem, hipotez, ölçüm, risk ve durdurma koşuluyla RFC/karar kaydına alınır.
- Deney, kararlı daldan ve gerçek kullanıcı verisinden izole edilir.
- Ana ürüne kabul için mevcut çözümden ölçülebilir fayda ve tüm regresyonların geçmesi gerekir.
- Sonuç olumsuzsa deney de değerlidir; neyin neden çalışmadığı belgelenir.
