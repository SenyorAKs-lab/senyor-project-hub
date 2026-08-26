# Güncel Sıralı Çalışma Planı

> Tarih: 2026-08-26
> Durum: **AX Downloader C# CP-042 R1 Windows kabul edildi; çalışma güvenli noktada**
> Aktif ürün projesi: AX Downloader
> Son Windows-kabul edilmiş taban: **CP-042 Safe Queue Reorder**
> Sıradaki checkpoint: **CP-043 Safe Empty Queue Deletion**

## Son kabul edilen nokta

- Python v0.4.1 davranış referansı Windows üzerinde kabul edilmiş durumda.
- C#/.NET 10 ürün zincirinde CP-001→CP-042 Windows üzerinde kabul edildi.
- CP-042 R1 ilk ürün kabulü, paylaşılan Core kuyruk kataloğu değiştiği için
  `run_csharp_tests.cmd full` ile yapıldı.
- Tam koşu CP-001→CP-042 regresyon zincirini, gerçek WinUI x64 build'i, Worker
  IPC smoke'unu ve yeniden başlatma sonrası sıra kalıcılığını tamamladı.
- Komut istemi hata işaretleyicisi olmadan normal döndü.

## CP-042 kabul kanıtı

Gözlenen nihai işaretleyiciler:

- `CSHARP_CP042_UI_QUEUE_REORDER_LAUNCH_OK`
- `CSHARP_CP042_UI_QUEUE_REORDER_RECEIPT_OK`
- `CSHARP_CP042_QUEUE_ID_METADATA_UNCHANGED`
- `CSHARP_CP042_QUEUE_JOBS_SCHEDULE_UNCHANGED`
- `CSHARP_CP042_QUEUE_RESTART_ORDER_PERSISTENCE_OK`
- `CSHARP_CP042_BUILD_OK`
- `CSHARP_CP042_CONFORMANCE_OK`
- `AX_FULL_CP001_CP042_OK`
- `AX_TESTS_OK`

R1 düzeltme geçmişi:

- İlk Windows koşusu `MainWindow.xaml.cs(2272,18)` konumunda `CS0136` ile
  durdu; `BuildQueueManagementCard(int index)` içindeki döngü aynı kapsamda
  yeniden `index` tanımlıyordu.
- R1 yalnız döngü yerelini `columnIndex` olarak yeniden adlandırdı ve
  `CP042_WINUI_CARD_LOCAL_SCOPE_SAFE` regresyon korumasını ekledi.
- Core, IPC, Worker, SQLite, kuyruk sıralama semantiği ve kabul assertion'ları
  gevşetilmedi.

## Kabul edilmiş paket kaydı

- Dosya: `AX_Downloader_CSharp_CP042_Accepted.zip`
- SHA-256: `52fdeeb4452d23aff1d93ca535b364eb62107c7a7efab302142046714fb3dfe6`
- Paket kökü: `AX_Downloader_CSharp_CP042_Accepted/`
- Arşiv doğrulaması: başarılı; 559 giriş; `bin/`, `obj/`,
  `__pycache__/` ve bytecode artığı yok.
- Aday paket ve CP-041 Accepted yedeği korunur; üzerlerine yazılmaz.

## Güncel ürün zinciri

- CP-001→CP-021: çekirdek indirme, resume, lifecycle ve named queue runtime
- CP-022→CP-029: telemetry, SQLite, bootstrap/recovery, Worker ve IPC
- CP-030→CP-036: gerçek WinUI shell, kontroller ve yeni indirme akışı
- CP-037: gerçek URL'den gerçek indirmeye uçtan uca Worker akışı
- CP-038: UI handoff ve canlı takip
- CP-039: kuyruk yönetim görünümü
- CP-040: güvenli kuyruk oluşturma
- CP-041: güvenli kuyruk yeniden adlandırma
- CP-042: güvenli tek-adım kuyruk sıralama — **WINDOWS ACCEPTED (R1)**

## Değişmez çalışma kuralı

- Bir seferde yalnızca bir küçük checkpoint ele alınır.
- Yeni checkpoint önce runner politikasının gerektirdiği eski kabul zincirini
  çalıştırır.
- Test edilmemiş aday “tamamlandı” diye yazılmaz.
- `BUILD_OK` + `CONFORMANCE_OK` olmadan PASS verilmez.
- Windows/.NET 10 ve gerçek WinUI/Worker koşusu kabul otoritesidir.
- İlk hatada kör tekrar yapılmaz; kök neden analiz edilir.
- Test bypass edilmez; assertion kaldırılmaz, failing branch skip edilmez,
  unsafe null suppression kullanılmaz.
- Candidate paketleri eski klasörün üstüne çıkarılmaz.
- GitHub kaydı yalnız kullanıcının açık isteğiyle yapılır.

## Sıradaki tek somut adım — CP-043

**Safe Empty Queue Deletion**

Önce sözleşme ve test kanıtı dondurulacak, ardından en küçük ürün değişikliği
uygulanacak.

Zorunlu sınırlar:

- Yalnız gerçekten boş ve çalışmayan bir kuyruk silinebilir.
- İş içeren, aktif veya bilinmeyen kuyruk mutasyon yapmadan reddedilir.
- Kalan kuyrukların ID, storage key, ad, sıra, schedule ve job bağları korunur.
- İndirme/partial artifact'larına dokunulmaz.
- Receipt, beklenen tek kuyruk kaldırma sonucunu kanıtlamadan UI başarı göstermez.
- Worker yeniden başlatıldığında silme kalıcıdır.
- Varsayılan/son kuyruk politikası koddan önce açıkça karara bağlanır.
- Paylaşılan katalog mutasyonu nedeniyle ilk Windows ürün kabulü `full`
  koşusuyla yapılır.

CP-043'e toplu silme, dolu/aktif kuyruk silme, dosya temizliği, Settings veya
final görsel polish karıştırılmaz.

## Devam cümlesi

`AX Downloader devam — CP-042 R1 Windows accepted; CP-043 Safe Empty Queue Deletion sözleşmesinden başla.`
