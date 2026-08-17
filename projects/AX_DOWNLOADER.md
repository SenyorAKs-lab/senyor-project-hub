# AX Downloader — Güncel Proje Bağlamı

> Durum: Aktif geliştirme / C# çekirdek + runtime altyapısı
> Son güncelleme: 2026-08-18
> Python davranış referansı: v0.4.1 — Windows kabulü tamamlandı
> C#/.NET aşaması: **CP-021 Windows kabul edildi**
> Sıradaki tek somut adım: **CP-022 Runtime Progress & Telemetry Contract**

## Kısa durum

AX Downloader; doğrudan HTTP/HTTPS kaynaklarından güvenli indirme, doğrulanmış
resume, Range/no-Range otomatik yönlendirme, kalıcı job/queue kayıtları,
sequential worker, named queues, schedule ve runtime kontrol katmanları olan bir
Windows indirme yöneticisi olarak geliştiriliyor.

Python v0.4.1 referans motorunun CP-005K ve QG-004→QG-011 kalite kapıları
Windows üzerinde kabul edildi ve C# taşımasının davranış oracle'ı olarak
korunuyor.

C#/.NET 10 çekirdeğinde CP-001→CP-021 zinciri Windows üzerinde kabul edildi.
Son otorite CP-021 testidir ve aynı koşuda CP-001→CP-020 regresyonları yeşil
kaldı.

## Güncel kabul kanıtı — CP-021

- Tarih: 2026-08-18
- Kapı: Named Queue Pause / Resume Runtime
- Kabul işaretleyicileri:
  - `CSHARP_CP021_BUILD_OK`
  - `CSHARP_CP021_CONFORMANCE_OK`
- Doğrulanan davranışlar:
  - aktif named queue Pause sırasında job state persist edilir;
  - tail job otomatik başlamaz;
  - Pause global execution lease'i bırakır;
  - başka named queue çalışabilir;
  - başka queue lease sahibiyken paused queue Resume bloklanır;
  - lease serbest kalınca Resume güvenli şekilde devam eder;
  - Range pause/resume wire seviyesinde doğrulanır;
  - final hash eşleşir.

## Kabul edilen C# checkpoint zinciri

| Checkpoint | Davranış | Sonuç |
|---|---|---|
| CP-001→008 | C# temel parity ve güvenlik kapıları | Geçti |
| CP-009 | no-Range single-session fallback | Geçti |
| CP-010 | Automatic Range/no-Range dispatch | Geçti |
| CP-011 | Controlled Stop + Safe Resume | Geçti |
| CP-012 | Destructive Cancel Cleanup | Geçti |
| CP-013 | Job Lifecycle State Machine | Geçti |
| CP-014 | Runtime Job Controller | Geçti |
| CP-015 | Durable Job Persistence + Restart Recovery | Geçti |
| CP-016 | Persistent Sequential Queue Foundation | Geçti |
| CP-017 | Sequential Queue Worker | Geçti |
| CP-018 | Persistent Scheduled Queue Start | Geçti (R2) |
| CP-019 | Persistent Named Queues + Isolation | Geçti |
| CP-020 | Named Queue Runtime Control + Global Execution Lease | Geçti |
| CP-021 | Named Queue Pause/Resume Runtime | Geçti |

CP-018'in ilk adayı yalnızca yeni test harness'ındaki nullable derleme hataları
nedeniyle durdu. R2 explicit `?? throw` null sözleşmeleriyle gerçek anlamda
düzeltildi; assertion veya davranış testi atlanmadı ve R2 tam kabul testinden
geçti.

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
- Pause lease'i bırakır
- başka queue aktifken paused queue Resume bloklanır

### Persistence/recovery
- durable job records
- queue persistence
- named queue registry
- schedule persistence
- restart recovery
- atomic writes + lock retry
- corruption rejection/preservation

## Değişmez davranış sınırları

1. Dosya boyutu tek başına başarı kanıtı değildir.
2. Resume yalnızca doğrulanmış/recoverable sınıra güvenir.
3. Unverified tail yeni transferden önce temizlenir.
4. Range protokol ihlalleri unsafe resume'a dönüşmez.
5. Source identity değişimi güvenli ret üretir.
6. Yanlış final dosya üretilemez.
7. Pause/Stop destructive değildir; recoverable veriyi korur.
8. Cancel yalnızca quiescent transfer sonrasında ilgili artifact'ları siler.
9. Completed/Canceled terminaldir; Failed yanlışlıkla Completed sayılmaz.
10. Named queue runtime'da aynı anda yalnızca bir queue global lease sahibi olur.

## Kalıcı mimari yönü

- Core/runtime: C# + .NET 10
- UI: WinUI 3 / Windows App SDK
- Gelecek kalıcı backend: SQLite
- Uzun süren işler: bağımsız .NET Worker
- UI/Worker: kimliği doğrulanmış yerel IPC
- Tarayıcı: Chrome Manifest V3 + Native Messaging
- Python: fixture, test serverı, hata enjeksiyonu ve davranış referansı
- Diller: en-US fallback + tr-TR + ru-RU

## Sıradaki tek somut adım — CP-022

**Runtime Progress & Telemetry Contract**

Tek ve UI-safe bir telemetry sözleşmesi dondurulacak. En az:
- Queue ID
- Job ID
- state
- trusted/downloaded bytes
- total bytes
- percent
- speed
- ETA
- Parallel/Single mode
- ordering için timestamp/sequence

Kabul kapısı şunları kanıtlamalı:
- Parallel aggregate progress doğru;
- Single progress doğru;
- normal transferde percent monotonic;
- Pause/Stop sonrasında progress donar;
- Resume sahte sıçrama yapmaz;
- yalnız başarılı final exact %100 üretir;
- Failed/Canceled false %100 üretmez;
- no-Range restart semantiği fake resume gibi raporlanmaz;
- eventler UI'yi boğmayacak şekilde throttle edilir;
- Queue ID/Job ID izolasyonu korunur;
- cross-queue telemetry leakage olmaz;
- terminal snapshot deterministiktir.

CP-022'ye WinUI, SQLite migration, Worker process, IPC, browser extension,
update sistemi, localization UI veya signing karıştırılmaz.

## Çalışma disiplini

- Tek checkpoint / tek küçük davranış.
- Her yeni checkpoint önce bütün kabul edilmiş regresyonları çalıştırır.
- Windows/.NET 10 test sonucu otoritedir.
- `BUILD_OK` + `CONFORMANCE_OK` olmadan PASS yazılmaz.
- İlk hatada tekrar çalıştırılmaz; önce kök neden analiz edilir.
- Test bypass edilmez, assertion kaldırılmaz, unsafe null suppression yapılmaz.
- Her candidate yeni temiz klasöre çıkarılır.
- Normal Chat'te büyük repo refactor'larından kaçınılır.

## Güncel handoff

Ayrıntılı day-end kayıt:
`projects/AX_DOWNLOADER_HANDOFF_2026-08-18_CP021.md`

Devam cümlesi:

`AX Downloader devam — CP-021 accepted, CP-022 Runtime Progress & Telemetry Contract'tan başla.`
