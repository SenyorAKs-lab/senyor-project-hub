# Proje Dizini

> Son güncelleme: 2026-08-14

| Proje | Durum | Son doğrulanan aşama | Kritik açık / sıradaki ana adım |
|---|---|---|---|
| [AX Downloader](projects/AX_DOWNLOADER.md) | Aktif prototip / bütünlük sertleştirme | CP-005H 429 adaptasyonu, CP-005I kaynak değişimi ve CP-005J bozuk state güvenli reddi geçti; CP-005K aynı boyutlu bozulma açığını doğruladı | v0.4.1 blok bütünlüğü adayını CP-005K ile doğrula ve bütün regresyonları yeniden çalıştır |
| [ArchiveX](projects/ARCHIVEX.md) | Planlandı / beklemede | Gereksinim ve Android depolama kısıtları belirlendi | SAF/MediaStore tabanlı mimari tasarım |
| [Local Metadata Manager](projects/LOCAL_METADATA_MANAGER.md) | Planlama | Güvenli toplu metadata yenileme kapsamı belirlendi | Dry-run ve karantina odaklı teknik tasarım |
| [Jarvis](projects/JARVIS.md) | Fikir / beklemede | Android kişisel asistan hedefleri belirlendi | İzin, güvenlik ve çevrimdışı yetenek fizibilitesi |

## Durum tanımları

- **Fikir:** Henüz gereksinimler netleşiyor.
- **Planlama:** Kapsam ve mimari tasarlanıyor.
- **Aktif prototip:** Çalışan kod ve deneysel testler var; yayın mimarisi değil.
- **Bütünlük sertleştirme:** Sessiz veri bozulması, kesinti ve protokol sınırları sistematik olarak test ediliyor.
- **Yayın adayı:** Kod, test, güvenlik ve belgeler gözden geçiriliyor.
- **Yayınlandı:** Sürüm etiketli ve kamuya açık kullanıma hazır.
- **Beklemede:** Bilinçli olarak durduruldu; yeniden başlama koşulu kayıtlı.
