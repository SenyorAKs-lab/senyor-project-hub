# Değişiklik Günlüğü

Bu dosya merkez sistemdeki anlamlı değişiklikleri izler. Proje içi ayrıntılı sürüm notları ilgili projenin kendi deposunda tutulacaktır.

## 2026-08-26

- AX Downloader C# CP-042 R1 Safe Queue Reorder, Windows `full` koşusunda
  CP-001→CP-042 zinciri, gerçek WinUI x64 build, Worker IPC smoke ve
  restart-order persistence ile kabul edildi.
- Nihai `CSHARP_CP042_BUILD_OK`, `CSHARP_CP042_CONFORMANCE_OK`,
  `AX_FULL_CP001_CP042_OK` ve `AX_TESTS_OK` işaretleyicileriyle normal
  komut istemi dönüşü kaydedildi.
- İlk Candidate koşusundaki `CS0136` yerel isim çakışmasının R1'de yalnız
  `index` → `columnIndex` düzeltmesiyle kapatıldığı; ürün semantiği ve test
  assertion'larının değiştirilmediği kaydedildi.
- `AX_Downloader_CSharp_CP042_Accepted.zip` kabul paketi
  `52fdeeb4452d23aff1d93ca535b364eb62107c7a7efab302142046714fb3dfe6` SHA-256 değeriyle sabitlendi.
- Güncel çalışma planı ve proje bağlamı CP-042 Accepted tabanına taşındı.
- Sıradaki tek ürün adımı CP-043 Safe Empty Queue Deletion olarak belirlendi;
  ilk işlem silme uygunluk ve receipt sözleşmesini dondurmaktır.

## 2026-08-17

- AX Downloader Python v0.4.1 davranış referansının CP-005K ve QG-004–QG-011
  Windows kabul zincirini tamamladığı kaydedildi.
- C#/.NET 10 taşımasında QG-CS001–QG-CS008 kapılarının Windows üzerinde geçtiği
  ve son CP-008 koşusunda .NET SDK 10.0.400 ile dokuz projenin derlendiği
  kaydedildi.
- CP-008 uçtan uca oturumunda önceki CP regresyonlarının yeşil kaldığı, 22 yeni
  session/wire/server/build/final işaretleyicisinin tamamının görüldüğü ve komut
  isteminin hata işaretleyicisi olmadan geri döndüğü kaydedildi.
- Aktif proje bağlamı ve güncel çalışma planı eski CP-005K aday durumundan
  CP-008 kabul noktasına taşındı.
- Sıradaki tek adım, HTTP 200/no-Range kaynaklar için C# tek bağlantılı yolun
  QG-CS009 kabul sözleşmesini üretim kodundan önce dondurmak olarak belirlendi.

## 2026-08-14

- Güncel sıralı çalışma planı eklendi. Modelden bağımsız AI çalışma sistemi için yalnızca ölçülebilir parçaların baseline → minimal çekirdek → ince adaptörler ve kalite kapıları → A/B doğrulama sırasıyla uygulanmasına karar verildi; uygulama masaüstü oturumuna ertelendi.
- Claude Code'un otomatik okuyacağı kök `CLAUDE.md` giriş dosyası eklendi; mevcut proje kaynağı ve ortak standartlar `@path` içe aktarımlarıyla bağlandı.
- AI süreklilik standardına platform giriş dosyaları ve kaynak tekrarını önleyen adaptör kuralı eklendi.
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
