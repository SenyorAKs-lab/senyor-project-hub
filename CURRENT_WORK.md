# Güncel Sıralı Çalışma Planı

> Tarih: 2026-08-14  
> Durum: Planlandı — uygulama henüz başlamadı  
> Çalışma zamanı: Kullanıcı masaüstüne geçtiğinde  
> Aktif ürün projesi: AX Downloader

## Değişmez çalışma kuralı

- Adımlar sırayla uygulanır.
- Her aşamanın kabul ölçütü doğrulanmadan sonraki aşamaya geçilmez.
- Test edilmemiş aday sonuç “tamamlandı” olarak yazılmaz.
- Sessiz veri bozulmasına sıfır tolerans uygulanır.
- Yıkıcı veya harici yazma işlemi kullanıcı onayı olmadan yapılmaz.

## 1. Masaüstü ve Claude Code bağlantısını doğrula

- [ ] Claude Code'u kur ve çalıştır.
- [ ] `senyor-project-hub` deposunu klonla veya güncel halini çek.
- [ ] AX Downloader yerel klasörünü bağla:

  ```bat
  claude --add-dir "%USERPROFILE%\Desktop\AX-Downloader"
  ```

- [ ] Claude'un `CLAUDE.md`, içe aktarılan merkez kaynakları ve yerel AX Downloader dosyalarını okuduğunu doğrula.
- [ ] Aktif proje, doğrulanan son sürüm, kritik CP-005K açığı ve sıradaki adım doğru özetlenmeden kod değişikliğine izin verme.

**Kabul ölçütleri**

- Claude kaynakları doğru okur.
- v0.4.0 doğrulanmış referans, v0.4.1 doğrulanmamış aday olarak ayrılır.
- Sıradaki ürün adımı CP-005K doğrulaması olarak belirtilir.

## 2. AI çalışma sistemi için baseline oluştur

- [ ] Mevcut sistemi değiştirmeden başlangıç halini dondur.
- [ ] Yazılım geliştirme ve proje yönetimini temsil eden 12–20 gerçek görev seç.
- [ ] Ölçütleri tanımla: başarı/kabul testi, süre, yeniden iş, regresyon, talimat ihlali, insan müdahalesi ve token/API/CI maliyeti.
- [ ] Mevcut yöntemle baseline sonuçlarını kaydet.

**Kabul ölçütleri**

- Aynı görev seti yeni sistemle tekrar çalıştırılabilir.
- Başlangıç değerleri ölçülmeden “verim arttı” denmez.

## 3. Minimal model-nötr çekirdeği tasarla

- [ ] Kök `AGENTS.md` sözleşmesini kısa ve model-nötr tut.
- [ ] Yalnızca değişmez proje ve çalışma kurallarını içer.
- [ ] Sohbet dökümü, ayrıntılı proje geçmişi veya tekrarlı platform metni ekleme.
- [ ] Hedef olarak 120–200 satırı aşmayan bir çekirdek tasarla.

## 4. Katmanlı proje hafızasını standardize et

- [ ] `PROJECT_CONTEXT.md`
- [ ] `ARCHITECTURE.md`
- [ ] `QUALITY_GATES.md`
- [ ] `HANDOFF.md`
- [ ] `docs/adr/ADR-xxxx.md`
- [ ] `verified`, `pending`, `blocked`, `next` ve `evidence` alanlarını ortak şemaya bağla.
- [ ] Güncel bilgi ile tarihsel kararı birbirinden ayır.

## 5. İnce model adaptörlerini bağla

- [ ] `CLAUDE.md`
- [ ] `GEMINI.md`
- [ ] `.github/copilot-instructions.md`
- [ ] Gerekli olduğunda Cursor, Windsurf, Aider ve Continue adaptörleri
- [ ] Adaptörler ortak kuralları kopyalamaz; kanonik çekirdeğe yönlendirir.
- [ ] Platforma özel kısmı 5–20 satırlık ince katman olarak tut.

## 6. Yol kapsamlı kuralları ayır

- [ ] `core`
- [ ] `frontend/UI`
- [ ] `tests`
- [ ] `docs`
- [ ] `security/release`
- [ ] Her kuralın yalnız ilgili alan çalışılırken yüklenmesini sağla.

## 7. Beş görev sözleşmesini oluştur

- [ ] `implement`
- [ ] `diagnose`
- [ ] `review`
- [ ] `release`
- [ ] `handoff`

Her sözleşme `goal`, `inputs/constraints`, `acceptance criteria`, `output contract` ve `evidence` alanlarını içerir.

## 8. Makineyle doğrulanan kalite kapılarını kur

- [ ] Test, lint, typecheck, build, security, integrity ve CI kapıları
- [ ] Doğrulama komutları ve beklenen işaretler
- [ ] Kanıt raporu
- [ ] Harici veya yıkıcı işlemler için insan onayı
- [ ] En az yetki, secrets yasağı, dış içeriği güvenilmez kabul etme, allowlist, sandbox ve branch protection kuralları

## 9. Adaptör drift ve senkronizasyon kontrolünü kur

- [ ] Kanonik dosyaya bağlantı kontrolü
- [ ] Eski veya çelişkili kural taraması
- [ ] Eksik adaptör ve dosya kontrolü
- [ ] CI içinde otomatik doğrulama

## 10. A/B ölçümü yap ve kabul kararı ver

- [ ] Aynı görev setini yeni sistemle çalıştır.
- [ ] Tek seferde yalnızca bir değişkeni değiştir.
- [ ] Sonuçları baseline ile karşılaştır.
- [ ] Yalnız ölçülen fayda sağlayan parçaları kalıcılaştır.
- [ ] Fayda sağlamayan veya bürokrasi üreten parçaları sadeleştir ya da çıkar.

**Kabul ölçütleri**

- Daha düşük hata/yeniden iş, daha güvenli AI devri veya ölçülebilir süre avantajı görülür.
- Yalnızca “daha güzel cevap” alınması başarı sayılmaz.

## 11. AX Downloader teknik geliştirmesine geri dön

- [ ] v0.4.1 blok bütünlüğü adayını CP-005K ile doğrula.
- [ ] `FINAL_HASH_MATCH`
- [ ] `CORRUPTION_RECOVERED`
- [ ] `CP005K_ENGINE_SAFE`
- [ ] Ardından kapanma, ağ kesintisi, Windows yeniden başlatma, HTTP 429, kaynak değişimi ve bozuk state regresyonlarını çalıştır.
- [ ] Tümü geçmeden davranış sözleşmesini dondurma; C#/.NET veya UI aşamasına geçme.

## Bu kayıt ne değildir

- Yeni AI sistemi uygulanmış değildir.
- v0.4.1 doğrulanmış değildir.
- AX Downloader yol haritasındaki aşamalar geçilmiş değildir.

## Sıradaki tek somut adım

Masaüstünde önce **1. Masaüstü ve Claude Code bağlantısını doğrula** bölümünden başla.
