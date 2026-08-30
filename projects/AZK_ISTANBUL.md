# AZK İstanbul

> Son güncelleme: 2026-08-31
> Durum: **Aktif geliştirme — v11 Team & Assignment Foundation Windows accepted**

## Amaç

AZK İstanbul organizasyon operasyonlarını tek sistemde toplamak: sipariş planlama, kurulum/toplama takibi, ürün kataloğu, firma/mekân kayıtları, saha ekibi atama, işlem geçmişi ve rol bazlı kullanım.

## Güncel teknik taban

- Work/Sites kaynaklı orijinal v9 tasarım baseline'ı korunuyor.
- Next.js 16 + React 19 + TypeScript + Vinext/Vite + Tailwind/shadcn.
- Yerel geliştirme Cloudflare D1 ile kalıcı veri kullanıyor.
- Ana geliştirme akışı Windows üzerinde `AZK-DEV.bat` ile yürütülüyor.

## Kabul edilen zincir

### v10 — Persistent Core

- Siparişler, sipariş ürünleri, ürünler, kategoriler, firmalar, mekânlar ve işlem geçmişi D1'e taşındı.
- F5 / uygulama yeniden başlatma sonrası verinin kalması sağlandı.
- Sipariş listesi ve detay ekranı aynı kalıcı veri kaynağına bağlandı.
- Sabit tarih prototipi kaldırıldı; gerçek cihaz tarihi kullanılmaya başlandı.
- Kategori filtreleri dinamik hale getirildi.

### v11 — Team & Assignment Foundation — WINDOWS ACCEPTED

Kullanıcı 2026-08-31 tarihinde yerel Windows ortamında sistemin tam anlamıyla çalıştığını doğruladı.

Kapsam:

- 8 kişilik kalıcı ekip modeli: 1 yönetici, 1 ofis, 6 saha.
- D1 `staff_users` ve `order_assignments` tabloları.
- Yeni siparişte saha ekibi atama.
- Sipariş kartı ve detay ekranında görevli ekip görünümü.
- Siparişler ekranında saha çalışanına göre filtre.
- Ayarlar üzerinden ekip isimlerini düzenleme.
- Rol bazlı UI görünürlüğü.
- Yerel geliştirme için kullanıcı değiştirme/test simülasyonu.
- Saha hesabı ana listelerde yalnız kendisine atanmış siparişleri görür.
- Atanmamış sipariş URL'sinde erişim uyarısı gösterilir.
- Saha rolünde ürün ekle/sil/adet değiştirme kontrolleri kapalıdır; not ve operasyon tamamlama akışları açıktır.
- `/api/state` üzerinden tüm siparişleri topluca silip yeniden yazan eski yaklaşım kaldırıldı.
- Yeni siparişler kayıt bazlı `POST /api/orders`, detay değişiklikleri kayıt bazlı `PATCH /api/orders/[id]` ile kaydedilir.
- Katalog veya ekip ayarı kaydı artık sipariş snapshot'ını ezmez.

## Bilinen sınır

v11'deki kullanıcı değiştirici gerçek authentication değildir. API seviyesinde gerçek session/authorization henüz yoktur.

## Sıradaki ana milestone

### v12 — Authentication & Server Authorization

Hedefler:

- Gerçek kullanıcı adı/şifre girişi.
- Güvenli oturum yönetimi.
- Yönetici / Ofis / Saha rollerinin sunucu tarafında doğrulanması.
- Saha çalışanının yalnız atanmış siparişlere API seviyesinde erişebilmesi.
- Yetkisiz mutasyonların yalnız UI'da gizlenmekle kalmayıp server tarafında reddedilmesi.
- Oturum açma/kapatma ve temel kullanıcı yönetimi.

## Güvenli çalışma kuralı

- Accepted baseline'ın üstüne kontrollü küçük sürümler uygulanır.
- Eski klasörün üstüne kontrolsüz tam ZIP çıkarılmaz.
- Büyük sürümler benzersiz isimli checkpoint olarak saklanır.
- Yerel D1 verisi ve `.wrangler` bilinçsiz reset paketlerinde silinmez.
- Candidate sürüm kullanıcı Windows testi geçmeden accepted sayılmaz.

## Devam cümlesi

`AZK İstanbul devam — v11 Team & Assignment Foundation Windows accepted; v12 Authentication & Server Authorization ile başla.`
