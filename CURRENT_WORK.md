# Güncel Sıralı Çalışma Planı

> Tarih: 2026-08-17
> Durum: CP-008 kabul edildi; çalışma güvenli noktada durduruldu
> Aktif ürün projesi: AX Downloader

## Son kabul edilen nokta

- Python v0.4.1 davranış referansı Windows üzerinde kabul edildi.
- CP-005K ve QG-004 ile QG-011 arasındaki Python kapıları geçti.
- C#/.NET 10 çekirdeğinin QG-CS001 ile QG-CS008 arasındaki kapıları geçti.
- Son komut `run_csharp_session_conformance.cmd` idi.
- .NET SDK 10.0.400 ile dokuz proje derlendi.
- CP-001 ile CP-007 regresyonları yeşil kaldı ve 22 CP-008 işaretleyicisinin
  tamamı görüldü.

## Değişmez çalışma kuralı

- Adımlar sırayla uygulanır.
- Kabul kapısı tanımlanmadan üretim davranışı eklenmez.
- Test edilmemiş aday “tamamlandı” diye yazılmaz.
- Sessiz veri bozulmasına sıfır tolerans uygulanır.
- C# taşıması eski kabul edilmiş kapıları her yeni aşamada tekrar geçirir.
- Yıkıcı veya harici yazma işlemi kullanıcı onayı olmadan yapılmaz.

## 1. QG-CS009 sözleşmesini dondur — sıradaki tek adım

- [ ] HTTP 200/no-Range kaynağın tek bağlantılı indirme state modelini belirle.
- [ ] Tam veya blok bazlı doğrulanmış ilerleme sınırını açıkça yaz.
- [ ] Süreç kapanması, ağ kesintisi ve Windows yeniden başlatması sonrası
  davranışı kararlaştır.
- [ ] `ETag`, `Last-Modified`, boyut ve final URL değişiminin ret kurallarını
  tanımla.
- [ ] Eksik/aşırı/kesilmiş HTTP gövdesi için yanlış final üretmeme kanıtını
  tanımla.
- [ ] Atomik final yükseltmesi ve deterministik geçici veri temizliğini tanımla.
- [ ] Yerel bağımsız test sunucusu ve required-marker listesini yaz.
- [ ] CP-001 ile CP-008 arasını zorunlu regresyon olarak bağla.

Kabul ölçütü: üretim kodu değişmeden önce test komutu, hata enjeksiyonları,
gerekli işaretleyiciler ve başarısızlık koşulları `QUALITY_GATES.md` içinde
yoruma yer bırakmayacak şekilde bulunur.

## 2. CP-009 tek bağlantılı yolu uygula ve doğrula

- [ ] En küçük bağımlılıksız C# uygulamasını ekle.
- [ ] Yerel fixture/self-test ve kaynak preflight'larını geçir.
- [ ] Windows `.NET 10` tam regresyon paketini çalıştır.
- [ ] QG-CS009 ve önceki bütün kapılar geçmeden aşamayı kabul etme.

## 3. İndirme yaşam döngüsünü dondur

- [ ] Pause: doğrulanmış/recoverable işi koru.
- [ ] Stop: yeniden başlatılabilir işi ve kullanıcı niyetini açıklaştır.
- [ ] Cancel: ilişkili geçici veriyi güvenli ve hedefli biçimde temizle.
- [ ] Güvenli çıkış, süreç kapanması ve Windows restart sınırlarını ayır.
- [ ] Her geçiş için state machine ve hata enjeksiyonu kapısı oluştur.

## 4. Ürün altyapısına geç

Sıra: SQLite kuyruk → bağımsız Worker/IPC → sistem tepsisi → WinUI 3 →
TR/EN/RU yerelleştirme → Chrome Native Messaging → paketleme/güncelleme.
Her katman kendi kabul kapısı geçmeden sonraki katmana geçmez.

## Sıradaki tek somut adım

`projects/AX_DOWNLOADER.md` ve çalışma paketindeki güncel handoff dosyalarını
okuyup yalnızca QG-CS009 no-Range/tek-bağlantı sözleşmesini tasarla.
