# AX Downloader — Güncel Proje Bağlamı

> Durum: Aktif prototip  
> Son güncelleme: 2026-08-13  
> Son çalışan sürüm: v0.4.0 adaptive parallel resume

## Amaç

Doğrudan HTTP/HTTPS dosya adreslerinden güvenli, kesintiye dayanıklı ve kaynak desteklediğinde paralel indirme yapabilen bir Windows/Python indirme motoru geliştirmek.

## Mevcut ortam

- Windows 10
- Python 3.12.5
- Sanal ortam: `.venv`
- Ağ istemcisi: `aiohttp`
- Kullanıcının çalışma klasörü: `Desktop\AX-Downloader`

## Mevcut prototip dosyaları

- `downloader.py`: v0.3.1 tek bağlantılı resume motoru
- `downloader_parallel.py`: v0.4.0 adaptif paralel resume motoru
- `parallel_test.py`: bağlantı sayısı hız/doğruluk kıyaslaması
- `probe.py`: HTTP Range ve kaynak analizi
- `downloader_v031_resume_backup.py` ve diğer bilinçli yedekler

Bu tek dosyalı prototipler davranış kanıtı içindir; nihai açık kaynak mimarisi değildir.

## Doğrulanan özellikler

### v0.3.1 — tek bağlantı

- Normal indirme ve tam boyut doğrulaması
- Ctrl+C sonrası kaldığı yerden devam
- CMD/süreç zorla kapatıldıktan sonra devam
- İnternet kesintisi sonrası yarım dosyayı koruma ve devam
- Windows yeniden başlatma sonrası devam
- Geçici Windows state kilidinde yeniden deneme
- Bozuk/yarım state dosyasında güvenli durma
- Range desteklemeyen kaynakta güvenli ret
- Kaynak boyutu değiştiğinde güvenli durma

### CP-005 hız kıyası

İlk koşu:

| Bağlantı | Hız |
|---:|---:|
| 1 | 3.36 MB/s |
| 4 | 2.25 MB/s |
| 8 | 3.97 MB/s |

İkinci koşu:

| Bağlantı | Hız |
|---:|---:|
| 1 | 3.78 MB/s |
| 4 | 4.59 MB/s |
| 8 | 3.95 MB/s |

Her iki koşuda 1/4/8 bağlantı çıktıları birebir aynı SHA-256 değerini verdi (`INTEGRITY_OK`). Tek bir ölçüme göre sabit bağlantı seçmek yerine adaptif motor kararı alındı.

### v0.4.0 — paralel motor

- Sekiz segmentle tam indirme, birleştirme ve geçici dosya temizliği
- Ctrl+C sonrası segment bazlı devam
- Pencere/süreç zorla kapatıldıktan sonra disk boyutlarını esas alarak devam
- Ağ kesintisinde socket timeout ile güvenli durma
- Ağ geri geldiğinde yalnızca eksik segmentten devam
- Son dosyada tam boyut doğrulaması

### CP-005G — Windows yeniden başlatma sonrası paralel resume

Test 2026-08-13 tarihinde gerçek Windows yeniden başlatmasıyla geçti.

- Program yeniden açıldığında `Kayıtlı paralel indirme bulundu.` mesajını verdi.
- Segment 3 için state/disk farkı güvenli biçimde uzlaştırıldı: `3784408 → 3817176`.
- Toplam devam noktası `43.37 MB` olarak bulundu; indirme sıfırdan başlamadı.
- Sekiz bağlantıyla eksik parçalar tamamlandı.
- Birleştirme sonunda `SIZE_OK` ve `DOWNLOAD_COMPLETE` alındı.
- Klasörde yalnızca nihai `100Mb.dat` kaldı; segment ve state dosyaları temizlendi.

## Mimari kararlar

- Segment dosyasının gerçek disk boyutu resume için birincil otoritedir.
- JSON state atomik biçimde geçici dosyaya yazılıp yeniden adlandırılır; kilit durumunda sınırlı tekrar denenir.
- Kaynak kimliği boyut, ETag ve Last-Modified ile izlenir.
- Range yanıtı ve Content-Range beklenen parçayla eşleşmeden veri kabul edilmez.
- HTTP 429 veya benzeri sunucu baskısında aktif bağlantı sayısı 8 → 4 → 1 azaltılacaktır.
- Kuyruk ve geçmiş için prototipte JSON yeterlidir; çoklu görev sürümünde SQLite değerlendirilecektir.
- v0.3.1 çalışan yedek olarak korunur.

## Bilinen sınırlamalar

- Uzak kaynaktan güvenilir hash her zaman alınamıyor; nihai doğrulama şimdilik tam boyut ve kontrollü benchmark hash eşitliğine dayanıyor.
- Range desteklemeyen kaynakta resume yoktur; güvenli tek bağlantı fallback henüz nihai tasarıma eklenmedi.
- Kuyruk, zamanlama, hız limiti, kullanıcı arayüzü ve geçmiş henüz yoktur.
- Prototip kod yayın öncesi modüllere ayrılmalıdır.
- Gerçek HTTP 429 adaptif düşüş senaryosu henüz kontrollü test edilmedi.

## Sıradaki tek somut adım

**CP-005H — Kontrollü HTTP 429 adaptif bağlantı düşürme testi.**

Beklenen:

1. Test sunucusu sekiz eşzamanlı bağlantıda kontrollü biçimde HTTP 429 üretir.
2. Motor mevcut ilerlemeyi kaybetmeden bağlantı sayısını 8 → 4 düşürür.
3. Gerekirse aynı güvenli kuralla 4 → 1 düşürür.
4. Kabul edilen segment verileri yeniden indirilmez ve aralıklar karışmaz.
5. Sonuç `SIZE_OK` ve `DOWNLOAD_COMPLETE` olur.
6. Kontrollü hash ile tek ve paralel sonuçların aynı olduğu doğrulanır.

Bundan sonra paralel kaynak değişimi, bozuk state ve birleştirme sırasında kesinti testleri yapılmalıdır.
