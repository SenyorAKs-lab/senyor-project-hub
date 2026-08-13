# ArchiveX — Güncel Proje Bağlamı

> Durum: Planlandı / beklemede  
> Son güncelleme: 2026-08-13

## Amaç

Android telefonda oluşan yeni fotoğraf ve videoları; kaynaklarına göre düzenlenmiş tek bir arşiv kökünde toplamak ve PC'ye aktarımı kolaylaştırmak.

Örnek kaynaklar: Kamera, WhatsApp, Instagram ve İndirilenler.

## Öğrenilenler

- Android 11 ve üzerindeki scoped storage kuralları klasik dosya erişimini sınırlar.
- FolderSync, MacroDroid ve MiXplorer denemelerinde özellikle `Android/data` erişimi engel oluşturdu.
- Uygulama, geniş ve kırılgan dosya izinlerine yaslanmamalıdır.
- Mevcut bilgisayar Android Studio'yu rahat çalıştırmak için uygun değil; bu nedenle geliştirme beklemede.

## Gelecek mimari yönü

- Kullanıcının seçtiği hedef klasör için Storage Access Framework
- Medya keşfi için MediaStore
- Kaynak bazlı eşleme kuralları
- Kopyalama/taşıma öncesi önizleme
- Çakışma, tekrar dosya ve kesinti yönetimi
- İşlem günlüğü ve geri alınabilirlik

## Sıradaki adım

Uygun geliştirme ortamı oluştuğunda SAF/MediaStore tabanlı izin ve veri akışı taslağı hazırlanacak.
