# Mühendislik Standardı

> Sürüm: 1.0  
> Son güncelleme: 2026-08-13

## Hedef

Kod, yıllar sonra projeyi ilk kez açan bir geliştiricinin nereden başlayacağını anlayabileceği kadar düzenli; uzman bir geliştiricinin de gereksiz açıklamalar içinde kaybolmayacağı kadar kesin olmalıdır.

## Tasarım ilkeleri

- Her modülün tek ve açık bir sorumluluğu olmalıdır.
- Ağ, depolama, iş mantığı, kullanıcı arayüzü ve yapılandırma sınırları ayrılmalıdır.
- İş kuralları kullanıcı arayüzüne veya rastgele yardımcı fonksiyonlara gömülmemelidir.
- Bağımlılıklar mümkün olduğunca dışarıdan verilmeli ve test edilebilir olmalıdır.
- Genel bir soyutlama, yalnızca gerçek tekrar veya değişim ihtiyacı varsa eklenmelidir.

## Adlandırma ve biçim

- Açık kaynak kodundaki tanımlayıcılar İngilizce olmalıdır.
- İsimler niyeti anlatmalı; belirsiz kısaltmalardan kaçınılmalıdır.
- Projenin seçtiği otomatik biçimlendirici ve linter tek kaynak olmalıdır.
- Sabitler, hata kodları ve durumlar merkezi ve tutarlı tanımlanmalıdır.

## Yorum ve dokümantasyon

- Yorum “kod ne yapıyor?” yerine “neden böyle yapılıyor?” sorusunu cevaplar.
- Karmaşık protokol, uyumluluk veya güvenlik kararı ilgili karar kaydına bağlanır.
- Her halka açık API'nin girdi, çıktı, hata ve yan etki sözleşmesi açıklanır.

## Hata yönetimi

- Hatalar sessizce yutulmaz.
- Kullanıcıya gösterilen mesaj eyleme dönük; teknik günlük teşhis edilebilir olmalıdır.
- Tekrar deneme sınırlı, gecikmeli ve gözlemlenebilir olmalıdır.
- Yarım işlem dosyaları atomik yazma, geçici dosya ve güvenli devam mekanizmasıyla korunmalıdır.

## Test katmanları

- Birim: Saf iş kuralları ve sınır durumları
- Entegrasyon: Dosya sistemi, ağ, veritabanı ve süreç sınırları
- Uçtan uca: Gerçek kullanıcı akışı
- Dayanıklılık: Ctrl+C, süreç kapanması, ağ kesintisi, yeniden başlatma, bozuk state
- Güvenlik: Yol geçişi, istenmeyen üzerine yazma, sır sızıntısı, zararlı girdi

Test adı senaryoyu ve beklenen sonucu anlatmalıdır. “Geçti” kaydı tarih ve kanıtla tutulur.

## Sürüm ve yayın

- Anlamlı sürümlerde SemVer tercih edilir.
- Her yayın: değişiklik notu, kurulum, yükseltme, bilinen sınırlamalar ve geri dönüş bilgisi içerir.
- Prototip kodu, modülerleştirme ve yayın kontrolü yapılmadan üretim hazır sayılmaz.
