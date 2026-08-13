# AX Downloader — Güncel Proje Bağlamı

> Durum: Aktif prototip / bütünlük sertleştirme  
> Son güncelleme: 2026-08-14  
> Windows üzerinde son doğrulanan motor: v0.4.0 adaptive parallel resume  
> Hazırlanan fakat henüz kabul edilmeyen aday: v0.4.1 block-verified parallel resume  
> Sıradaki tek somut adım: CP-005K düzeltme doğrulaması

## Çok önemli ad ve sürüm notu

`AX Downloader` yalnızca çalışma adıdır. Açık beta veya genel yayın öncesinde benzersiz isim araştırması yapılacak ve çakışma riski taşımayan nihai ad seçilecektir. İnternet, GitHub, uygulama mağazaları, alan adı/sosyal hesaplar ve uygun marka veri tabanlarında araştırma tamamlanmadan bu ad kamuya açık marka olarak kullanılmaz.

v0.4.1 kod taslağının bulunması onun doğrulandığı anlamına gelmez. Kabul için CP-005K ve önceki regresyonların Windows ortamında yeniden geçmesi gerekir.

## Ürün amacı

Doğrudan HTTP/HTTPS kaynaklarından her tür dosyayı güvenli ve kesintiye dayanıklı biçimde indirebilen; kaynak izin verdiğinde adaptif paralel indirme kullanan; kalıcı kuyruk, zamanlama, arka plan Worker'ı, sistem tepsisi ve Chrome entegrasyonu sunan açık kaynak bir Windows indirme yöneticisi geliştirmek.

Ürün yalnızca medya indiricisi değildir. Genel amaçlı indirme çekirdeği temel üründür; video/akış analizi bunun üzerinde ayrı bir medya katmanıdır.

## Kapsam dışı ve dürüst sınırlar

- DRM, ödeme duvarı, üyelik veya erişim koruması aşılmaz.
- Yetkisiz veya telif ihlali oluşturan indirmeler teşvik edilmez.
- “Her sitede yüzde yüz yakalama” sözü verilmez.
- Doğrudan HTTP/HTTPS ve normal HTML5 kaynakları ilk hedef; HLS/DASH sonraki ayrı medya motorudur.
- Belirsiz adaylarda sessiz otomatik karar yerine kullanıcı seçimi istenir.
- Bütünlük kontrolü zararlı yazılım taraması değildir; indirilen çalıştırılabilirler otomatik başlatılmaz.

## Mevcut geliştirme ortamı ve prototipler

- Windows 10
- Python 3.12.5
- Sanal ortam: `.venv`
- Ağ istemcisi: `aiohttp`
- `downloader.py`: v0.3.1 tek bağlantılı resume referansı
- `downloader_parallel.py`: v0.4.0 Windows üzerinde doğrulanan paralel referans
- v0.4.1 aday kodu: 1 MiB blok SHA-256 kayıtlarıyla bütünlük onarımı
- `parallel_test.py`: bağlantı sayısı hız/doğruluk kıyası
- `probe.py`: HTTP Range ve kaynak analizi
- CP-005H/I/J/K kontrollü yerel test sunucuları

Tek dosyalı Python kodları ürün mimarisi değildir. Bunların amacı davranışı hızlıca kanıtlamak ve C# taşımasına test oracle/sözleşme sağlamaktır.

## Kanıtlanan davranışlar

### v0.3.1 — tek bağlantılı resume

- Normal indirme ve tam boyut doğrulaması
- Ctrl+C sonrası kaldığı yerden devam
- CMD/süreç zorla kapatıldıktan sonra devam
- İnternet kesintisi sonrası yarım dosyayı koruma ve devam
- Windows yeniden başlatma sonrası devam
- Geçici Windows state kilidinde sınırlı yeniden deneme
- Bozuk/yarım state dosyasında güvenli durma
- Range desteklemeyen kaynakta güvenli ret
- Kaynak boyutu değiştiğinde güvenli durma

Önceki dayanıklılık serisinde kontrolsüz kapanma, internet kesintisi ve Windows yeniden başlatma senaryoları ayrı ayrı geçti. Resume noktası sıfıra dönmedi ve nihai sonuç `SIZE_OK` + `DOWNLOAD_COMPLETE` verdi.

### CP-005 — paralel hız ve eşitlik kıyası

İki bağımsız koşuda 1, 4 ve 8 bağlantı denendi. Hız sıralaması koşudan koşuya değişti; tüm çıktılar birebir aynı SHA-256 değerini verdi (`INTEGRITY_OK`). Bu sonuç sabit bağlantı sayısı yerine adaptif motor kararına yol açtı.

| Koşu | 1 bağlantı | 4 bağlantı | 8 bağlantı |
|---|---:|---:|---:|
| 1 | 3.36 MB/s | 2.25 MB/s | 3.97 MB/s |
| 2 | 3.78 MB/s | 4.59 MB/s | 3.95 MB/s |

### v0.4.0 — paralel motor

- Sekiz segmentle tam indirme, birleştirme ve başarı sonrası geçici dosya temizliği
- Ctrl+C ve süreç kapanması sonrası segment bazlı devam
- Segment state/disk farkını gerçek dosya boyutuyla uzlaştırma
- Ağ kesintisinde socket timeout ile güvenli durma
- Ağ geri geldiğinde eksik segmentlerden devam
- Son dosyada tam boyut doğrulaması

### CP-005G — Windows yeniden başlatma sonrası paralel resume

- Gerçek Windows yeniden başlatması yapıldı.
- Motor kayıtlı paralel indirmeyi buldu.
- Segment 3 state/disk farkı `3784408 → 3817176` olarak uzlaştırıldı.
- Toplam devam noktası 43.37 MB bulundu; sıfırdan başlamadı.
- Sekiz segment tamamlandı; `SIZE_OK` ve `DOWNLOAD_COMPLETE` alındı.
- Başarıdan sonra segment/state dosyaları temizlendi.

### CP-005H — kontrollü HTTP 429 ve adaptif bağlantı düşürme

- Yerel sunucu 8 bağlantıda HTTP 429 üretti.
- Motor kabul edilmiş ilerlemeyi koruyup 8 → 4 bağlantıya düştü.
- Tekrar 429 sonrası 4 → 1 bağlantıya düştü.
- Bir bağlantıda indirme tamamlandı.
- Nihai dosya kontrollü kaynakla aynı boyut ve SHA-256 değerini verdi (`INTEGRITY_OK`).
- Sunucudaki istemci bağlantıyı kapattı kayıtları, motorun 429 sonrası diğer istekleri bilinçli iptal etmesiydi; veri kaybı değildi.

### CP-005I — aynı URL'de kaynak değişimi

- Kaynak A ile yarım paralel indirme oluşturuldu.
- Aynı URL ve aynı boyut korunurken kaynak B'ye geçildi; ETag ve içerik değişti.
- Motor `ETAG_CHANGED` ile resume'u reddetti ve birleştirmeye geçmedi.
- Mevcut state ve segmentler reddetme sırasında değiştirilmedi (`PARTIAL_STATE_PRESERVED`).

### CP-005J — bozuk state dosyası

- Geçerli state ve segmentler oluşturulup state kontrollü biçimde bozuldu.
- Motor `STATE_CORRUPTED` verdi; indirme/birleştirme başlamadı.
- Segmentler korundu ve nihai dosya yaratılmadı.
- Kontrol aracı sonucu `CP005J_PASS` olarak doğruladı.

### CP-005K — aynı boyutlu segment bozulması

Bu test başarı değil, kritik bir açığın kanıtıdır:

- Yarım indirmedeki bir segmentin tek byte'ı değiştirildi.
- Segment boyutu ve state geçerli bırakıldı; segment SHA-256 değeri değişti.
- v0.4.0 motor boyut aynı olduğu için bozulmayı fark etmeden birleştirdi.
- Nihai boyut doğru kaldı (`FINAL_SIZE_MATCH`) fakat kaynakla SHA-256 eşleşmedi (`FINAL_HASH_MISMATCH`).
- Kontrol sonucu `SAME_SIZE_CORRUPTION_NOT_DETECTED` ve `CP005K_GAP_CONFIRMED` oldu.

Bu nedenle v0.4.0 yayınlanabilir veya veri bütünlüğü güvenli sayılamaz.

## v0.4.1 düzeltme adayı

Aday tasarım şu mekanizmaları ekler:

- Her segment için 1 MiB'lık doğrulanmış blok SHA-256 listesi
- State şeması/sürüm doğrulaması
- Resume açılışında diskteki blokları kayıtlı hash'lerle karşılaştırma
- İlk uyuşmayan bloktan sonrasını güvenli biçimde kırpma
- Yalnızca bozuk veya doğrulanmamış blokları yeniden indirme
- Birleştirme sırasında her bloğu tekrar doğrulama
- Birleştirme sonrasında `BLOCK_INTEGRITY_OK` ve final SHA-256 üretme

Henüz kabul kriteri karşılanmış değildir. CP-005K testinde `FINAL_HASH_MATCH`, `CORRUPTION_RECOVERED` ve `CP005K_ENGINE_SAFE` görülmeli; ardından eski kesinti ve 429 testleri yeniden çalıştırılmalıdır.

## Kalıcı ürün mimarisi kararı

Python prototipi tamamlandıktan sonra UI yazılmadan önce çekirdek C#'a taşınacaktır.

- Dil/runtime: C# + .NET 10 LTS
- Masaüstü UI: WinUI 3 / Windows App SDK
- Kalıcı veri: SQLite
- İndirme süreci: UI'dan bağımsız .NET Worker
- UI/Worker iletişimi: kimliği doğrulanmış yerel IPC
- Tarayıcı köprüsü: Chrome Manifest V3 + Native Messaging Host
- Python: referans davranış, hata enjeksiyonu ve test araçları
- C++: yalnızca profilleme gerçek bir darboğaz gösterirse dar, izole bir modülde

Gerekçe: Python hızlı prototipleme ve kontrollü test için verimli oldu; final Windows ürünü için C#/.NET daha doğal dağıtım, Worker/IPC, SQLite, sistem entegrasyonu ve WinUI 3 uyumu sağlar. Python uygulaması tamamen bitirildikten sonra port yapılmayacak; çekirdek davranış sözleşmesi dondurulunca, UI'dan önce geçilecektir.

Resmî referanslar:

- .NET destek politikası: https://dotnet.microsoft.com/en-us/platform/support/policy
- WinUI 3: https://learn.microsoft.com/windows/apps/winui/winui3/
- Chrome Native Messaging: https://developer.chrome.com/docs/extensions/develop/concepts/native-messaging
- Chrome minimum izin politikası: https://developer.chrome.com/docs/webstore/program-policies/permissions

## Hedef bileşen sınırları

```text
Chrome Extension ── Native Messaging ──┐
                                       │
WinUI 3 UI ───────── Yerel IPC ────────┼── .NET Worker / Download Engine
System Tray ───────── Yerel IPC ───────┘             │
                                                     ├── SQLite queue/history
                                                     ├── .axpart / segment files
                                                     └── block hashes / recovery state
```

- `Downloader.Core`: durum modeli, kurallar ve sözleşmeler
- `Downloader.Infrastructure`: HTTP, disk, hashing ve SQLite
- `Downloader.Worker`: indirme/queue yürütme ve kurtarma
- `Downloader.Cli`: UI olmadan test edilebilir komut satırı kabuğu
- `Downloader.WinUI`: yalnızca görünüm ve kullanıcı etkileşimi
- `Downloader.NativeHost`: Chrome ile masaüstü arasındaki dar güvenli köprü
- `Downloader.Tests`: birim, entegrasyon, hata enjeksiyonu ve uçtan uca testler

## Kalıcı kuyruk gereksinimleri

- Birden fazla isimli kuyruk: örneğin Gece İndirilecekler, Programlar, Filmler
- Sürükle-bırak sıralama, öncelik, manuel veya planlı başlangıç
- Kuyruk başına hız ve eşzamanlı indirme sınırı
- Öğe durumları: Waiting, Scheduled, Analyzing, Downloading, Paused, Verifying, Completed, Failed, NeedsUserAction
- Tamamlanan öğeler yeniden başlatmada tekrar indirilmez.
- Yarım öğe son yazılan byte'tan değil son doğrulanan bloktan devam eder.
- Yarım öğe doğrulandıktan sonra sıradaki öğeye geçilir.
- SQLite işlemleri atomik olmalı; açılışta veritabanı ile disk uzlaştırılmalıdır.
- Kuyruk tamamlanınca varsayılan işlem hiçbir şeydir; uyku/kapatma yalnızca açık kullanıcı tercihiyle etkinleşir.

## Arka plan ve sistem tepsisi davranışı

- WinUI penceresini kapatmak Worker'ı durdurmaz; pencere gizlenir.
- Sistem tepsisinde aktif indirme sayısı, hız ve kuyruk durumu görünür.
- Tepsiden uygulamayı aç, tümünü duraklat ve güvenli çık işlemleri yapılır.
- Güvenli çık state'i diske yazar ve Worker'ı kontrollü kapatır.
- Elektrik/süreç/Windows kesintisinde bir sonraki açılışta kurtarma yapılır.
- Kullanıcı isterse Worker Windows oturum açılışında yeniden başlar.

## Dil kararı

Yalnızca üç arayüz dili planlanacaktır:

- `en-US`: varsayılan ve fallback
- `tr-TR`
- `ru-RU`

Metinler koda gömülmez; `.resw` kaynaklarında tutulur. Motor dil bağımsız hata kodları üretir, UI bunları seçili dile çevirir. Başka dil paketleri mevcut kapsamda yoktur.

## Chrome eklentisi gereksinimleri

- Manifest V3 ve resmî Native Messaging kullanılacak.
- Sağ tıkla “uygulamayla indir”, hemen başlat veya isimli kuyruğa ekle seçenekleri olacak.
- Eklenti indirme motoru olmayacak; masaüstü Worker'ına doğrulanmış aday bilgisi gönderir.
- Aday puanlama; gerçek kullanıcı tıklaması, `Content-Type`, `Content-Disposition`, boyut, uzantı, istek türü, kaynak alan adı ve HTML media kaynağını birlikte kullanır.
- Poster, logo, font, script, küçük görsel ve reklam/pop-up kaynakları elenir.
- Belirsizlik varsa format/kalite/boyut/kaynak gösterilir ve kullanıcı seçer.
- En az izin istenir; gizli izleme, uzaktan çalıştırılan kod veya gereksiz veri toplama yapılmaz.

## Mühendislik kabul standardı

Bir özelliğin bir defa çalışması yeterli değildir:

1. Sorun tekrar üretilebilir hale getirilir.
2. Kök neden belirlenir.
3. Önce başarısız olan otomatik/kontrollü test hazırlanır.
4. En küçük doğru düzeltme uygulanır.
5. Yeni test geçirilir.
6. Önceki regresyonlar yeniden çalıştırılır.
7. Gerçek Windows senaryosu doğrulanır.

“Sıfır hata” garantisi verilmez; fakat sessiz veri bozulmasına sıfır tolerans uygulanır. Bütünlük kanıtlanmadıysa `DOWNLOAD_COMPLETE` yazılmaz.

## UI'ya kadar ayrıntılı yol haritası

### Faz 1 — Python referans motorunu sertleştirme

1. CP-005K açığını v0.4.1 blok doğrulamasıyla kapat.
2. CP-005H/I/J ve kapanma/ağ/yeniden başlatma regresyonlarını yeniden çalıştır.
3. Range isteğine `200`, hatalı/eksik `Content-Range`, değişen toplam boyut senaryolarını test et.
4. DNS, TLS, timeout, connection reset, HTTP 429/503 ve geri çekilme davranışını test et.
5. Disk dolması, yazma izni, salt-okunur klasör, state kilidi ve birleştirme kesintisini test et.
6. Eski state sürümü, kayıp state, orphan segment ve disk/state uyuşmazlığını test et.
7. Geçici dosyaların yalnızca kanıtlanmış başarıdan sonra temizlendiğini doğrula.

### Faz 2 — Davranış sözleşmesini dondurma

1. URL/probe girdi ve çıktıları
2. Resume kabul/red kuralları
3. State şeması ve migrasyonları
4. Segment/blok bütünlük kuralları
5. Dosya adı, çakışma ve hedef klasör kuralları
6. Pause/resume/stop/cancel/güvenli çık anlamları
7. Dil bağımsız hata kodları
8. Range desteklenmeyen kaynakta güvenli tek bağlantı davranışı

### Faz 3 — C#/.NET Core + CLI + Tests

1. Çözüm ve modül sınırlarını kur.
2. Davranış sözleşmesini C# çekirdeğine taşı.
3. Python kontrollü sunucularını C# kabul testlerinde kullan.
4. Bütün checkpoint'leri C# CLI üzerinde geçir.
5. Ölçüm olmadan C++ ekleme.

### Faz 4 — SQLite kalıcı kuyruk

1. Queue/item şeması ve migrasyonlar
2. İsim, sıralama, durum ve zamanlama
3. Çökme sonrası atomik kurtarma
4. Disk/veritabanı uzlaştırması
5. Binlerce öğeyle performans ve sorgu testleri

### Faz 5 — Bağımsız Worker + IPC + tepsi

1. UI'dan bağımsız indirme süreci
2. Yetkili yerel IPC
3. Pencere kapanırken devam
4. Güvenli çık ve yeniden açılış kurtarması
5. Sistem tepsisi ve isteğe bağlı otomatik başlangıç

### Faz 6 — WinUI 3

1. Ana ekran, aktif indirmeler, kuyruklar, geçmiş, zamanlayıcı ve ayarlar
2. Tam ekran ve yan yana orta boy responsive düzen
3. TR/EN/RU kaynakları ve İngilizce fallback
4. Uzun Rusça metin, DPI, klavye ve erişilebilirlik testleri
5. Dosya bazında başlat/duraklat/devam/iptal; iptal geçici verileri güvenle temizler

## UI sonrası yol haritası

1. Chrome Native Messaging ve temel eklenti
2. Doğrudan dosya adaylarını yakalama ve reklam/gereksiz kaynak filtreleme
3. HLS/DASH medya katmanı
4. Güvenlik, düşük kaynak, uzun çalışma ve büyük kuyruk regresyonları
5. Paketleme, SemVer, changelog, checksum ve güncelleme kanalı
6. Proje deposu, lisans, katkı rehberi, güvenlik politikası ve issue/PR şablonları
7. README, kullanıcı rehberi, geliştirici mimarisi ve sorun giderme
8. PDF/sunum ilk taslağı ve kullanıcıyla ortak inceleme kapısı

## Yayın ve güncelleme yaklaşımı

- Kod projenin ayrı GitHub deposunda tutulacak; merkez hub bağlam ve standart bağlantısını taşıyacak.
- GitHub Releases üzerinde sürümlü kurulum paketi, checksum, changelog ve bilinen sınırlamalar yayımlanacak.
- Kararlı ve beta kanalları ayrılacak.
- İlk güvenli updater sürümü kullanıcı onayı olmadan kurulum yapmayacak.
- Başarısız güncellemede mevcut çalışan sürüm ve geri alma yolu korunacak.
- Katkılar issue → tasarım/karar → başarısız test → PR → inceleme → regresyon sırasından geçecek.

## Lisans, dağıtım maliyeti ve imzalama kararı

- Kaynak kodu ve kurulum paketini GitHub üzerinden yayımlamak için ücretli mağaza zorunlu değildir; GitHub Releases temel dağıtım kanalı olabilir.
- Microsoft Store yayını ve ticari kod imzalama kamuya açık ilk sürüm için zorunlu varsayılmaz; güven, SmartScreen deneyimi ve erişim faydası değerlendirilerek ayrıca kararlaştırılır.
- Mağaza, sertifika ve imzalama ücretleri zamanla ve ülkeye göre değişebileceği için yayın tarihinde resmî kaynaklardan yeniden doğrulanır; bugünkü fiyat belgeye sabitlenmez.
- Açık kaynak lisansı henüz seçilmemiştir. Açık beta öncesinde katkı, yeniden kullanım ve türev ürün hedefleri karşılaştırılarak lisans kararı kayda alınacaktır.
- Üçüncü taraf bağımlılık lisansları ve bildirimleri otomatik tarama ve insan incelemesiyle doğrulanmadan paket yayımlanmaz.

## Topluluk ve deneysel Ar-Ge yaklaşımı

- Kararlı çekirdek kanıtlanmış protokol ve güvenlik yöntemleri üzerinde kalır.
- Yeni veya literatürde örneği az olan fikirler önce bir RFC/karar kaydında problem, hipotez, ölçüt, risk ve geri dönüş planıyla tanımlanır.
- Deneyler ayrı dal veya laboratuvar modülünde yapılır; sonuçlar ölçülmeden ana motora alınmaz.
- Başarılı deney için yalnızca “çalıştı” çıktısı yetmez; mevcut çözümle doğruluk, hız, kaynak kullanımı, bakım ve güvenlik kıyası gerekir.
- Topluluk issue ve tartışmalarından gelen öneriler aynı kanıt kapısından geçer; popülerlik teknik kanıtın yerine kullanılmaz.
- Yeni teknoloji geliştirme hedefi teşvik edilir fakat kullanıcı verisi ve kararlı sürüm üzerinde kontrolsüz deney yapılmaz.

## Tahmini süre

Bu bir taahhüt değil, mevcut kapsam ve kullanıcının iş temposuna göre planlama aralığıdır:

- UI çalışmalarına başlama: yaklaşık 4–6 hafta
- Kalıcı kuyruk ve arka plan Worker'ı olan kullanılabilir beta: 8–12 hafta
- Chrome eklentili, paketlenmiş ve sağlam v1.0: 12–18 hafta
- Kesintili/mesai sonrası gerçekçi toplam: yaklaşık 3–5 ay

## Bilinen açıklar ve riskler

- CP-005K sessiz aynı-boyutlu segment bozulması v0.4.0'da açıktır.
- v0.4.1 henüz CP-005K ve tam regresyon kabulünden geçmemiştir.
- Uzak kaynak güvenilir final hash sağlamayabilir; yerel blok zinciri bozulmayı yakalar fakat kaynağın baştan kötü içerik sunmasını kanıtlayamaz.
- Range desteklemeyen kaynaklarda resume ve paralellik sınırlıdır.
- Worker/SQLite/IPC/WinUI/Chrome bileşenleri henüz uygulanmamıştır.
- Son ürün adı henüz seçilmemiştir.

## Sıradaki tek somut adım ve geçiş koşulu

**CP-005K düzeltme doğrulaması:** v0.4.1 adayını aynı boyutlu tek-byte segment bozulması senaryosunda çalıştır.

Beklenen minimum kanıt:

1. Motor bozuk bloğu resume öncesi veya birleştirmede algılar.
2. Bozuk/doğrulanmamış bölümü güvenli sınırdan yeniden indirir.
3. Sağlam segment/blokları gereksiz indirmez.
4. Nihai boyut kaynakla eşleşir.
5. Nihai SHA-256 kontrollü kaynakla eşleşir.
6. Test aracı `CORRUPTION_RECOVERED` ve `CP005K_ENGINE_SAFE` verir.
7. Ardından kapanma, ağ kesintisi, Windows restart, 429, kaynak değişimi ve bozuk state regresyonları geçer.

Bu koşullar sağlanmadan davranış sözleşmesi dondurulmaz ve C# taşımasına geçilmez.
