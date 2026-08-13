# Değişiklik Günlüğü

Bu dosya merkez sistemdeki anlamlı değişiklikleri izler. Proje içi ayrıntılı sürüm notları ilgili projenin kendi deposunda tutulacaktır.

## 2026-08-14

- AX Downloader güncel proje kaydı baştan sona yenilendi; doğrulanmış sonuçlar, açıklar, mimari ve tam yol haritası tek kaynakta toplandı.
- CP-005H kontrollü HTTP 429 testinin 8 → 4 → 1 bağlantı düşüşü ve final bütünlük kontrolüyle geçtiği kaydedildi.
- CP-005I aynı URL/kaynak değişimi testinde `ETAG_CHANGED` ile güvenli ret ve yarım verinin korunması kaydedildi.
- CP-005J bozuk state testinde `STATE_CORRUPTED`, segment koruması ve `CP005J_PASS` kaydedildi.
- CP-005K aynı boyutlu segment bozulmasının v0.4.0 tarafından fark edilmediği; final boyut doğruyken SHA-256'nın değiştiği kritik bütünlük açığı olarak kaydedildi.
- 1 MiB blok SHA-256 kullanan v0.4.1 kodu “hazırlanmış düzeltme adayı” olarak ayrıldı; CP-005K ve regresyonlar geçmeden doğrulanmış sürüm sayılmayacağı belirtildi.
- Ürün mimarisi C# + .NET 10 LTS + WinUI 3 + SQLite + bağımsız Worker + yerel IPC olarak kesinleştirildi. Python test referansı olarak korunacak; C++ yalnızca ölçülmüş darboğaz için kullanılacak.
- İsim verilebilir kalıcı kuyruklar, zamanlama, çökme/Windows yeniden başlatma sonrası doğrulanmış bloktan devam ve sistem tepsisi gereksinimleri kesinleştirildi.
- Chrome Manifest V3 + Native Messaging, aday puanlama ve belirsiz durumda kullanıcı seçimi yaklaşımı kesinleştirildi.
- Ürün dilleri `en-US` (varsayılan/fallback), `tr-TR` ve `ru-RU` ile sınırlandırıldı.
- Açık beta öncesinde özgün ad araştırmasını zorunlu kılan N-00 proje adı kapısı mühendislik standardına eklendi; AX Downloader çalışma adı olarak işaretlendi.
- Mühendislik, güvenlik ve dokümantasyon standartları; sessiz veri bozulmasına sıfır tolerans, regresyon zorunluluğu, tarayıcı/indirme güvenliği ve proje sonu PDF/sunum incelemesiyle genişletildi.
- Ücretsiz GitHub Releases dağıtımı, yayın tarihinde maliyet doğrulama, açık kaynak lisansı seçim kapısı ve üçüncü taraf lisans taraması kayda alındı.
- Gelecek projeler için problem tanımından yayın sonrasına uzanan ortak yaşam döngüsü ve ölçülebilir, izole deneylere dayalı kontrollü Ar-Ge standardı eklendi.

## 2026-08-13

- AX Downloader CP-005G testi geçti: Windows yeniden başlatmasından sonra paralel indirme 43.37 MB'den devam ederek doğrulandı.
- Merkez proje ve AI süreklilik yapısı kuruldu.
- Mühendislik, dokümantasyon, güvenlik ve AI devir standartları eklendi.
- Dört proje için güncel bağlam dosyaları oluşturuldu.
- Tekrar kullanılabilir proje, durum, karar, test, devir ve belge inceleme şablonları eklendi.
