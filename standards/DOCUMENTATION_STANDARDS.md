# Dokümantasyon, PDF ve Sunum Standardı

> Sürüm: 1.1  
> Son güncelleme: 2026-08-14

## Hedef kitle dengesi

Belge, konuya yeni olan birinin ana mantığı kurmasını sağlamalı; ileri seviyedeki okuyucunun da doğru teknik ayrıntıya hızla ulaşmasına izin vermelidir. Dil sade olabilir fakat kavramlar sulandırılmaz ve okuyucu küçümsenmez.

## Katmanlı anlatım

1. **Bir dakikalık özet:** Problem, çözüm, kime yarar
2. **Hızlı başlangıç:** Kurulum ve ilk başarılı kullanım
3. **Ana kavramlar:** Sistem hangi parçalarla çalışır?
4. **Mimari:** Bileşenler, veri akışı, hata ve durum modeli
5. **Geliştirici rehberi:** Nereden genişletilir, hangi sözleşmeler korunur?
6. **Derin teknik bölüm:** Protokoller, performans, güvenlik, sınır durumları
7. **Test ve kanıt:** Ne test edildi, nasıl üretildi, hangi sonuç alındı?
8. **Sorun giderme ve referans:** Hatalar, yapılandırma, terimler

Okuyucu ihtiyacına göre derinleşebilmelidir; aynı açıklama farklı bölümlerde gereksiz tekrar edilmez.

## İki ayrı anlatım yüzeyi

- **Kullanıcı belgesi:** Kurulum, güvenli kullanım, arayüz, sık sorunlar ve güncelleme
- **Geliştirici belgesi:** Mimari, modül sınırları, state/veri şeması, testler, genişletme ve katkı akışı

Kullanıcı rehberine gereksiz iç kod ayrıntısı; geliştirici rehberine belirsiz pazarlama dili doldurulmaz.

## Görseller

- Her görsel bir soruyu cevaplamalıdır.
- Akış için akış diyagramı, yapı için bileşen diyagramı, karşılaştırma için tablo/grafik kullanılır.
- Dekoratif kalabalık teknik açıklamanın yerine geçmez.
- Görsel ile metindeki isimler ve sürümler tutarlı olmalıdır.
- Örnekler gerçekçi, tekrar üretilebilir ve telif açısından güvenli olmalıdır.
- Ekran görüntüsü geçici bir test kanıtıysa sürüm, tarih ve beklenen sonuç yazılır.

## Zorunlu içerik

- Projenin amacı ve kapsam dışı alanlar
- Kurulum, güncelleme, kaldırma ve geri dönüş
- Kullanım ve gerçekçi örnek senaryolar
- Mimari ve veri akışı
- Durum/hata modeli ve kurtarma davranışı
- Güvenlik/mahremiyet yaklaşımı
- Test edilen ve edilmeyen durumlar
- Bilinen sınırlamalar
- Geliştirme ve katkı rehberi
- Lisans ve üçüncü taraf bildirimleri
- Sürüm, tarih ve kaynak commit/tag

## Çok dilli ürünlerde belge düzeni

- Kaynak teknik belge İngilizce tutulabilir; kullanıcıya yönelik kritik rehberler desteklenen ürün dillerine çevrilir.
- Çeviriler bağımsız kopyalar halinde kontrolsüz dağılmaz; aynı sürüme bağlanır.
- Kod, UI ve belgelerde aynı terim sözlüğü kullanılır.
- Eksik çeviri gizlenmez; fallback davranışı belirtilir.

## Proje sonu ortak inceleme kapısı

PDF ve sunum ilk taslaktan sonra kullanıcı ile birlikte incelenir:

- İlk bakışta amaç anlaşılıyor mu?
- Gereksiz veya tekrarlı bölüm var mı?
- Eksik örnek, bağlam ya da görsel var mı?
- Yeni başlayan ile uzman arasındaki seviye dengesi doğru mu?
- Kodun gerçek davranışıyla belge birebir uyumlu mu?
- Test edilmemiş özellik varmış gibi anlatılıyor mu?
- Yazı boyutu, sayfa akışı, kontrast ve görsel çözünürlük yeterli mi?
- Gizli bilgi veya lisans/telif riski var mı?
- Bir geliştirici yıllar sonra yalnızca bu belgelerle projeyi ayağa kaldırabilir mi?

Gerekirse birden fazla tur yapılır. Kod, belgeler ve sunum bu incelemeyi geçmeden proje tamamlanmış sayılmaz.
