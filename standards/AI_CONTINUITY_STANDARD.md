# AI Çalışma Sürekliliği Standardı

> Sürüm: 1.0  
> Son güncelleme: 2026-08-13

## Amaç

ChatGPT, Codex, Gemini veya başka bir AI ile çalışmaya geçildiğinde; önceki kararlar, doğrulanmış sonuçlar ve sıradaki adım kaybolmadan devam edebilmek.

## Bilgi mimarisi

- `START_HERE.md`: Evrensel giriş ve okuma sırası
- `PROJECT_INDEX.md`: Bütün projelerin kısa durumu
- `projects/*.md`: Proje başına güncel kaynak
- `standards/*.md`: Ortak ve kalıcı kurallar
- `templates/*.md`: Yeni proje ve kayıt şablonları
- `CHANGELOG.md`: Merkez sistemin tarihsel değişimleri

Tek, dev bir bağlam dosyası oluşturulmaz. Güncel bilgi proje dosyasına, kalıcı ortak kural standarda, tarihsel karar karar günlüğüne gider.

## Kayıt tetikleyicileri

- Kapsam veya gereksinim değişti
- Mimari karar alındı
- Kritik hata bulundu/çözüldü
- Test aşaması sonuçlandı
- Çalışmaya uzun ara verilecek
- Sürüm/yayın yapıldı
- PDF/sunum incelemesi tamamlandı

Küçük, geri alınabilir denemeler ve günlük sohbet tekrarları kaydedilmez.

## Devir paketinin asgari içeriği

- Amaç ve kapsam
- Çalışan sürüm/dosya
- Mimari özet
- Alınan kararlar ve gerekçeleri
- Geçen testler ve kanıtları
- Başarısız testler/açık sorunlar
- Bilinen sınırlamalar
- Sıradaki tek somut adım
- Çalıştırma ve doğrulama komutları
- Güvenlik veya veri kaybı uyarıları

## Güncelleme disiplini

- Güncel durum dosyası yerinde güncellenir; “final_v2_son_gercek.md” benzeri kopyalar açılmaz.
- Git geçmişi eski sürümü korur.
- Kararların geçmişi silinmez; geçersiz kaldıysa yeni kararla bağlantı kurulur.
- Kanıt ile yorum ayrılır.
- Dosyanın son güncelleme tarihi yazılır.
- Sırlar, kişisel veriler ve geçici erişim bilgileri kaydedilmez.
