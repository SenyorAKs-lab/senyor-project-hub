# AX Downloader — Güncel Proje Bağlamı

> Durum: Aktif geliştirme / gerçek WinUI + Worker + IPC + SQLite
> Son güncelleme: 2026-08-26
> Python davranış referansı: v0.4.1 — Windows kabulü tamamlandı
> C#/.NET aşaması: **CP-042 R1 Windows kabul edildi**
> Sıradaki tek somut adım: **CP-043 Safe Empty Queue Deletion**

## Kısa durum

AX Downloader; doğrudan HTTP/HTTPS kaynaklarından güvenli indirme, kriptografik
blok doğrulaması, Range/no-Range otomatik yönlendirme, kalıcı job/queue
kayıtları, bağımsız Worker, yerel IPC ve gerçek WinUI arayüzü olan bir Windows
indirme yöneticisi olarak geliştiriliyor.

Python v0.4.1 referans motorunun CP-005K ve QG-004→QG-011 kalite kapıları
Windows üzerinde kabul edildi ve C# taşımasının davranış oracle'ı olarak
korunuyor.

C#/.NET 10 zincirinde CP-001→CP-042 Windows üzerinde kabul edildi. Son otorite
2026-08-26 tarihli CP-042 R1 `full` koşusudur.

## Güncel kabul kanıtı — CP-042 R1

- Kapı: Safe Queue Reorder (one-step Up/Down)
- Komut: `run_csharp_tests.cmd full`
- Sonuç: CP-001→CP-042 tam regresyonu, gerçek WinUI x64 build'i, Worker IPC
  smoke'u ve restart-order persistence doğrulaması geçti.
- Nihai kapılar:
  - `CSHARP_CP042_BUILD_OK`
  - `CSHARP_CP042_CONFORMANCE_OK`
  - `AX_FULL_CP001_CP042_OK`
  - `AX_TESTS_OK`
- Ürün davranış kanıtı:
  - UI reorder başlatma ve receipt doğrulaması geçti;
  - kuyruk ID/metadata değişmedi;
  - job ve schedule bağları değişmedi;
  - Worker restart sonrasında yeni sıra korundu;
  - normal komut istemi geri döndü.
- İlk Candidate koşusunda görülen `CS0136`, R1'de yalnız gölgelenen döngü
  yereli `index` → `columnIndex` değiştirilerek düzeltildi; ürün semantiği
  ve assertion'lar değiştirilmedi.

## Kabul edilen C# checkpoint zinciri

| Checkpoint | Davranış | Sonuç |
|---|---|---|
| CP-001→008 | C# temel parity ve güvenlik kapıları | Geçti |
| CP-009→021 | Dispatch, lifecycle, persistence ve named queue runtime | Geçti |
| CP-022→029 | Telemetry, SQLite, recovery, Worker ve IPC | Geçti |
| CP-030→036 | Gerçek WinUI shell, kontroller ve yeni indirme akışı | Geçti |
| CP-037 | Gerçek URL → gerçek Worker indirmesi | Geçti |
| CP-038 | UI handoff + canlı takip | Geçti |
| CP-039 | Dedicated queue management view | Geçti |
| CP-040 | Safe named queue creation | Geçti |
| CP-041 | Safe named queue rename | Geçti (R2) |
| CP-042 | Safe queue reorder | Geçti (R1) |

## Güncel runtime modeli

### Download mode

- Range-capable → segmented/parallel engine
- no-Range → single full-body engine

### Job kontrolleri

- Start
- Pause
- Resume
- Stop
- Cancel

### Queue/runtime

- persistent ordering
- sequential worker
- named queues
- per-queue schedule
- global single active queue lease
- queue create / rename / one-step reorder
- restart-persistent catalog state

### UI / process boundary

- gerçek WinUI 3 shell
- bağımsız .NET Worker
- kimliği doğrulanmış yerel named-pipe IPC
- canlı telemetry, yeni indirme handoff'u ve kuyruk yönetimi
- UI; Core, Worker, Application veya SQLite'a doğrudan erişmez

### Persistence/recovery

- SQLite durable job ve queue kayıtları
- schedule persistence
- restart recovery
- atomic writes + lock retry
- corruption rejection/preservation

## Değişmez davranış sınırları

1. Dosya boyutu tek başına başarı kanıtı değildir.
2. Resume yalnız doğrulanmış/recoverable sınıra güvenir.
3. Unverified tail yeni transferden önce temizlenir.
4. Range protokol ihlalleri unsafe resume'a dönüşmez.
5. Source identity değişimi güvenli ret üretir.
6. Yanlış final dosya üretilemez.
7. Pause/Stop destructive değildir; recoverable veriyi korur.
8. Cancel yalnız quiescent transfer sonrasında ilgili artifact'ları siler.
9. Completed/Canceled terminaldir; Failed yanlışlıkla Completed sayılmaz.
10. Queue katalog mutasyonları job, schedule, storage ve artifact bağlarını
    değiştiremez.

## Kabul edilmiş paket

- `AX_Downloader_CSharp_CP042_Accepted.zip`
- SHA-256: `52fdeeb4452d23aff1d93ca535b364eb62107c7a7efab302142046714fb3dfe6`
- ZIP bütünlük testi geçti; 559 girişli tek kök; build/cache artığı yok.

## Sıradaki tek somut adım — CP-043

**Safe Empty Queue Deletion**

İlk adım ürün kodu değil, silme uygunluk ve receipt sözleşmesini dondurmaktır.
En az şu güvenlik sınırları kabul kapısına yazılacaktır:

- yalnız boş ve çalışmayan kuyruk silinebilir;
- non-empty, active ve unknown queue mutation olmadan reddedilir;
- kalan queue/job/schedule/storage bağları korunur;
- hiçbir download/partial artifact silinmez;
- UI yalnız doğrulanmış IPC receipt sonrasında başarı gösterir;
- restart sonrasında silme kalıcıdır;
- varsayılan/son kuyruk politikası uygulamadan önce açıkça belirlenir;
- ilk ürün kabulü paylaşılan katalog mutasyonu nedeniyle `full` çalışır.

Toplu silme, dolu/aktif kuyruk silme, dosya temizliği, Settings ve final görsel
polish CP-043 kapsamına alınmaz.

## Çalışma disiplini

- Tek checkpoint / tek küçük davranış.
- Windows/.NET 10 + gerçek WinUI/Worker sonucu kabul otoritesidir.
- `BUILD_OK` + `CONFORMANCE_OK` olmadan PASS yazılmaz.
- İlk hatada kör tekrar yok; önce kök neden.
- Test bypass, assertion kaldırma ve unsafe null suppression yok.
- Her candidate yeni temiz klasöre çıkarılır.

## Güncel devam cümlesi

`AX Downloader devam — CP-042 R1 Windows accepted; CP-043 Safe Empty Queue Deletion sözleşmesinden başla.`
