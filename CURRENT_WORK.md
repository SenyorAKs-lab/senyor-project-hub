# Güncel Sıralı Çalışma Planı

> Tarih: 2026-08-18
> Durum: **AX Downloader C# Core CP-021 Windows kabul edildi; çalışma güvenli noktada durduruldu**
> Aktif ürün projesi: AX Downloader
> Sıradaki checkpoint: **CP-022 Runtime Progress & Telemetry Contract**

## Son kabul edilen nokta

- Python v0.4.1 davranış referansı Windows üzerinde kabul edildi.
- C#/.NET 10 çekirdeğinde CP-001→CP-021 kabul edildi.
- CP-021 son koşusunda CP-001→CP-020 regresyon zinciri yeşil kaldı.
- CP-021 kabul işaretleyicileri:
  - `CSHARP_CP021_BUILD_OK`
  - `CSHARP_CP021_CONFORMANCE_OK`

CP-021 doğruladı:
- aktif named queue Pause sırasında state persistence;
- tail job'ın beklemesi;
- Pause sonrası global execution lease release;
- başka named queue'nun çalışabilmesi;
- başka queue aktifken paused queue Resume'un bloklanması;
- Range pause/resume wire doğrulaması;
- final hash doğruluğu.

## Güncel kabul zinciri

CP-009 no-Range fallback → CP-010 automatic dispatch → CP-011 controlled Stop →
CP-012 destructive Cancel → CP-013 lifecycle state machine → CP-014 runtime
controller → CP-015 durable persistence/restart recovery → CP-016 persistent
sequential queue → CP-017 sequential worker → CP-018 scheduled queue start →
CP-019 named queues/isolation → CP-020 named queue runtime/global lease →
**CP-021 named queue Pause/Resume runtime**.

## Değişmez çalışma kuralı

- Adımlar sırayla uygulanır.
- Bir seferde yalnızca bir küçük checkpoint ele alınır.
- Yeni checkpoint önce bütün eski kabul edilmiş regresyonları çalıştırır.
- Test edilmemiş aday “tamamlandı” diye yazılmaz.
- `BUILD_OK` + `CONFORMANCE_OK` olmadan PASS verilmez.
- Windows/.NET 10 sonucu kabul otoritesidir.
- İlk hatada tekrar çalıştırılmaz; önce kök neden analiz edilir.
- Test bypass edilmez; assertion kaldırılmaz, failing branch skip edilmez,
  unsafe null suppression kullanılmaz.
- Candidate paketleri eski klasörün üstüne çıkarılmaz; her zaman yeni temiz
  klasör kullanılır.
- Normal Chat'te büyük/riski yüksek repo çapı refactor yapılmaz.

## Sıradaki tek somut adım — CP-022

**Runtime Progress & Telemetry Contract**

Amaç: UI veya IPC katmanına geçmeden önce core'un güvenilir, throttle edilmiş,
job/queue kimliği taşıyan progress snapshot/event sözleşmesini dondurmak.

Minimum veri:
- Queue ID
- Job ID
- current state
- trusted/downloaded bytes
- total bytes (biliniyorsa)
- percent
- speed
- ETA (anlamlıysa)
- Parallel/Single mode
- event ordering için timestamp/sequence

Zorunlu kabul davranışları:
- [ ] Parallel aggregate progress doğru.
- [ ] Single mode progress doğru.
- [ ] Normal transferde percent monotonic.
- [ ] Pause/Stop sonrasında progress quiescent noktada donar.
- [ ] Resume sahte progress sıçraması yapmaz.
- [ ] Başarılı completion exact %100 üretir.
- [ ] Failed/Canceled false %100 üretmez.
- [ ] no-Range restart eski partial byte'ları fake network resume gibi raporlamaz.
- [ ] Event frekansı UI'yi boğmayacak şekilde throttle edilir.
- [ ] Doğru Queue ID + Job ID tagging zorunludur.
- [ ] Cross-job/cross-queue telemetry leakage yoktur.
- [ ] Terminal snapshot deterministiktir.

## CP-022'ye karıştırılmayacak katmanlar

- WinUI ekranları
- SQLite migration
- bağımsız Worker process
- IPC
- Chrome Native Messaging
- update sistemi
- UI localization
- signing/release packaging

## Güncel day-end handoff

`projects/AX_DOWNLOADER_HANDOFF_2026-08-18_CP021.md`

## Devam cümlesi

`AX Downloader devam — CP-021 accepted, CP-022 Runtime Progress & Telemetry Contract'tan başla.`
