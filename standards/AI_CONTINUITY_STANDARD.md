# AI Çalışma Sürekliliği Standardı

> Sürüm: 1.1  
> Son güncelleme: 2026-08-14

## Amaç

ChatGPT, Codex, Claude Code, Gemini veya başka bir AI ile çalışmaya geçildiğinde; önceki kararlar, doğrulanmış sonuçlar ve sıradaki adım kaybolmadan devam edebilmek.

## Bilgi mimarisi

- `START_HERE.md`: Evrensel giriş ve okuma sırası
- `PROJECT_INDEX.md`: Bütün projelerin kısa durumu
- `projects/*.md`: Proje başına güncel kaynak
- `standards/*.md`: Ortak ve kalıcı kurallar
- `templates/*.md`: Yeni proje ve kayıt şablonları
- `CHANGELOG.md`: Merkez sistemin tarihsel değişimleri

Tek, dev bir bağlam dosyası oluşturulmaz. Güncel bilgi proje dosyasına, kalıcı ortak kural standarda, tarihsel karar karar günlüğüne gider.

## Platform giriş dosyaları

- `AGENTS.md`: Genel kodlama ajanları için ince giriş noktası
- `CLAUDE.md`: Claude Code'un otomatik proje belleği ve kaynak içe aktarma noktası
- `GEMINI.md`: Gemini ve benzeri sistemler için ince giriş noktası

Bu dosyalar güncel proje gerçeklerini çoğaltmaz; evrensel girişe, standartlara ve aktif proje kaynağına yönlendirir. Platform adaptörü ile kaynak dosya çelişirse proje kaynağı esas alınır ve adaptör düzeltilir.

## Kayıt içeriğini oluşturan gelişmeler

Aşağıdaki gelişmeler bir sonraki kayıt için biriktirilir:

- Kapsam veya gereksinim değişti
- Mimari karar alındı
- Kritik hata bulundu/çözüldü
- Test aşaması sonuçlandı
- Sürüm/yayın yapıldı
- PDF/sunum incelemesi tamamlandı

Bunların her biri tek başına anında GitHub commit'i oluşturmaz. Küçük, geri alınabilir denemeler ve günlük sohbet tekrarları kayda alınmaz.

## GitHub commit tetikleyicileri

Birikmiş durum yalnızca şu hallerde depoya işlenir:

1. Kullanıcı açıkça “kaydet”, “GitHub'a kaydet” veya eşdeğer bir talimat verir.
2. Kullanıcı “bugünlük bu kadar”, “burada bırakalım” veya oturumu kapatan eşdeğer bir ifade kullanır.
3. Kullanıcı uzun bir aradan sonra geri döner ve kayıt önerisini onaylar.

AI, kullanıcı yokken arka planda sayaç çalıştırıp commit atamaz. Bu nedenle bir saatlik sessizlik otomatik işlem değil, kullanıcı geri döndüğünde kayıt önerisi için işarettir.

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
