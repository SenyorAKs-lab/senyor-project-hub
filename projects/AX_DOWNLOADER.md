# AX Downloader — Güncel Proje Bağlamı

> Durum: Aktif geliştirme / C# çekirdek taşıması
> Son güncelleme: 2026-08-17
> Python davranış referansı: v0.4.1 — Windows kabulü tamamlandı
> C#/.NET aşaması: CP-008 kabul edildi
> Sıradaki tek somut adım: QG-CS009 sözleşmesini dondur

## Kısa durum

AX Downloader, doğrudan HTTP/HTTPS kaynaklarından dosya indiren ve kesinti
sonrasında yalnızca kriptografik olarak doğrulanmış bloklardan devam eden bir
Windows indirme yöneticisi olarak geliştiriliyor.

Python v0.4.1 referans motoru; aynı boyutlu segment bozulması, süreç kapanması,
ağ kesintisi, gerçek Windows yeniden başlatması, bozuk state, kaynak kimliği
değişimi, Range protokol ihlalleri ve merge kesintisi kapılarını gerçek
`aiohttp` yolu üzerinde geçti. Bu davranış `BEHAVIOR_CONTRACT_v0.4.1.md` ile
donduruldu ve C# taşımasının test oracle'ı oldu.

C#/.NET 10 çekirdeğinde CP-001 ile CP-008 arasındaki bütün kalite kapıları
Windows üzerinde kabul edildi. CP-008, yalnızca URL ve hedef çalışma klasörü
alan tek C# oturum giriş noktasının probe, state, kaynak doğrulama, adaptif
paralel transfer, doğrulanmış finalizasyon ve temizliği güvenli biçimde
birleştirdiğini kanıtladı.

Bu sonuç çekirdeğin önemli bir kilometre taşıdır; ürünün veya kullanıcı
arayüzünün tamamlandığı anlamına gelmez.

## Son Windows kabul kanıtı — QG-CS008

- Tarih: 2026-08-17
- Komut: `run_csharp_session_conformance.cmd`
- Ortam: Windows, .NET SDK 10.0.400, hedef `net10.0`
- Derleme: dokuz projenin tamamı başarılı
- Regresyon: CP-001 ile CP-007 arasındaki bütün kapılar yeşil kaldı
- CP-008: 22 zorunlu session, wire, server, build ve final işaretleyicisinin
  tamamı görüldü
- Süreç: komut istemi hata işaretleyicisi veya traceback olmadan geri döndü

Kritik son işaretleyiciler:

- `CSHARP_SESSION_FINAL_SIZE_MATCH`
- `CSHARP_SESSION_FINAL_HASH_MATCH`
- `CSHARP_SESSION_FINAL_PROMOTION_CONFORMANT`
- `CSHARP_SESSION_STATE_CLEANED`
- `CSHARP_SESSION_SEGMENTS_CLEANED`
- `CSHARP_SESSION_MERGE_TEMP_CLEANED`
- `PYTHON_CSHARP_END_TO_END_SESSION_SAFE`
- `PYTHON_END_TO_END_SESSION_SERVER_SAFE`
- `CSHARP_CP008_BUILD_OK`
- `CSHARP_CP008_CONFORMANCE_OK`

## Kabul edilen kalite zinciri

### Python v0.4.1 referans motoru

| Kapı | Kanıtlanan davranış | Windows sonucu |
|---|---|---|
| CP-005K | Aynı boyutlu segment bozulmasını algılama ve onarma | Geçti |
| QG-004 | Temiz indirme, final hash ve geçici veri temizliği | Geçti |
| QG-005 | Zorla süreç kapatma sonrası doğrulanmış resume | Geçti |
| QG-006 | Kontrollü ağ kesintisi ve resume | Geçti |
| QG-007 | Gerçek Windows yeniden başlatması sonrası resume | Geçti |
| QG-008 | Bozuk state'i değiştirmeden güvenli ret | Geçti |
| QG-009 | `ETag` / `Last-Modified` değişiminde güvenli ret | Geçti |
| QG-010 | Probe ve segment Range ihlallerinde güvenli ret | Geçti |
| QG-011 | Merge kesintisi sonrası temiz yeniden oluşturma | Geçti |

### C#/.NET 10 çekirdeği

| Kapı | Kanıtlanan davranış | Windows sonucu |
|---|---|---|
| QG-CS001 | Dosya adı, Range, segment/blok ve SHA-256 temel kuralları | Geçti |
| QG-CS002 | Sürüm-3 state, atomik kayıt ve Windows kilit güvenliği | Geçti |
| QG-CS003 | HTTP probe/redirect ve tam Range protokol doğrulaması | Geçti |
| QG-CS004 | `.axpart`, 1 MiB doğrulanmış blok ve bozulma onarımı | Geçti |
| QG-CS005 | Sınırlı paralellik, fail-fast iptal ve güvenli resume | Geçti |
| QG-CS006 | Merge doğrulaması ve atomik final yükseltmesi | Geçti |
| QG-CS007 | HTTP 429 için 8 → 4 → 1 adaptif geri çekilme | Geçti |
| QG-CS008 | Uçtan uca paralel indirme oturumu | Geçti |

## Değişmez bütünlük kuralları

1. Dosya boyutunun eşleşmesi tek başına başarı kanıtı değildir.
2. Resume ve merge yalnızca art arda doğrulanmış 1 MiB SHA-256 bloklarına
   güvenir.
3. Doğrulanmamış kuyruk diskten kırpılmadan yeni veri istenmez.
4. Merge, state'teki her blok hash'ini fiziksel segmentten tekrar doğrular.
5. Kaynak `ETag` veya `Last-Modified` değişirse mevcut state/segmentler
   değiştirilmeden resume reddedilir.
6. Final dosya yalnızca tam boyut, tam doğrulanmış kapsam ve kabul testinde tam
   hash eşleşmesi sonrasında atomik olarak oluşturulur.
7. Reddedilen veya kesilen hiçbir yol yanlış `DOWNLOAD_COMPLETE` üretmez.
8. Başarılı finalizasyondan sonra state, segment ve `.axmerge` artıkları
   deterministik biçimde temizlenir.

## Kalıcı mimari kararı

- Çekirdek/runtime: C# + .NET 10 LTS
- Masaüstü arayüz: WinUI 3 / Windows App SDK
- Kalıcı kuyruk ve geçmiş: SQLite
- Uzun süren işler: UI'dan bağımsız .NET Worker
- UI/Worker bağlantısı: kimliği doğrulanmış yerel IPC
- Tarayıcı bağlantısı: Chrome Manifest V3 + Native Messaging
- Python: davranış referansı, fixture, hata enjeksiyonu ve test sunucuları
- C++: yalnızca profilleme gerçek bir darboğaz gösterirse dar bir modülde

İlk kullanıcı arayüzü dilleri yalnızca `en-US` (fallback), `tr-TR` ve `ru-RU`
olacaktır.

## Tamamlanmayan ürün katmanları

- Range desteklemeyen kaynak için uçtan uca tek bağlantılı C# yolu
- Pause / Stop / Cancel ve güvenli çıkış anlamları
- SQLite kuyruk, planlama ve geçmiş
- Bağımsız Worker, IPC ve sistem tepsisi
- WinUI 3 arayüzü ve yerelleştirme
- Chrome Native Messaging ve aday seçimi
- Paketleme, imzalama, güncelleme, geri dönüş ve yayın belgeleri

Bu katmanlar test edilmiş gibi gösterilmez.

## Bilinen sınırlar ve riskler

- Mevcut CP-008 oturumu paralel-capable kaynak gerektirir; HTTP 200/no-Range
  kaynağın tam indirme yolu henüz C# çekirdeğinde kabul edilmedi.
- Pause/Stop/Cancel davranışı henüz ürün sözleşmesi değildir. Tasarım sırasında
  “pause/stop recoverable veriyi korur, cancel ilişkili geçici veriyi temizler”
  sınırı açıkça test edilmelidir.
- `AX Downloader` çalışma adıdır. Açık beta öncesinde internet, GitHub, mağaza,
  alan adı ve uygun marka veri tabanlarıyla N-00 isim kapısından geçmelidir.
- DRM, ödeme duvarı veya erişim koruması aşılmaz; her sitede yüzde yüz yakalama
  vaadi verilmez.

## Sıradaki tek somut adım

QG-CS009 kalite kapısını, üretim koduna dokunmadan önce açıkça tanımla:

- probe HTTP 200 döndüğünde ve byte Range desteklenmediğinde güvenli tek
  bağlantılı indirme;
- süreç/ağ/Windows kesintisinde hangi verinin doğrulanmış sayılacağı;
- yeniden başlatma ve kaynak kimliği değişimi kuralları;
- yanlış final üretmeme ve atomik final yükseltmesi;
- başarı/ret/kesinti sonrası state ve geçici dosya temizliği;
- CP-001 ile CP-008 arasındaki bütün kapıların zorunlu regresyon olarak kalması.

Kapı sözleşmesi dondurulmadan CP-009 üretim kodu yazılmaz. CP-009 kabul edilmeden
Pause/Stop/Cancel veya ürün kabuğuna geçilmez.

## Yeni bir AI için başlama sırası

1. Bu dosyayı ve kök `CURRENT_WORK.md` dosyasını oku.
2. Çalışma paketindeki `AGENTS.md`, `HANDOFF.md`,
   `BEHAVIOR_CONTRACT_v0.4.1.md` ve `QUALITY_GATES.md` dosyalarını tamamen oku.
3. CP-008'i kabul edilmiş say; durum keşfi için gereksiz yere yeniden çalıştırma.
4. İlk işi QG-CS009 sözleşmesi ve ölçülebilir başarısızlık kanıtı yapmak olarak
   sınırla.
5. Ekran çıktısı veya gerçek test olmadan yeni bir aşamayı geçmiş yazma.
