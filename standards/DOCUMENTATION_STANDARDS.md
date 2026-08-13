# Dokümantasyon, PDF ve Sunum Standardı

> Sürüm: 1.0  
> Son güncelleme: 2026-08-13

## Hedef kitle dengesi

Belge, konuya yeni olan birinin ana mantığı kurmasını sağlamalı; ileri seviyedeki okuyucunun da doğru teknik ayrıntıya hızla ulaşmasına izin vermelidir. Dil sade olabilir fakat kavramlar sulandırılmaz ve okuyucu küçümsenmez.

## Katmanlı anlatım

1. **Bir dakikalık özet:** Problem, çözüm, kime yarar
2. **Hızlı başlangıç:** Kurulum ve ilk başarılı kullanım
3. **Ana kavramlar:** Sistem hangi parçalarla çalışır?
4. **Mimari:** Bileşenler, veri akışı, hata ve durum modeli
5. **Geliştirici rehberi:** Nereden genişletilir, hangi sözleşmeler korunur?
6. **Derin teknik bölüm:** Protokoller, performans, güvenlik, sınır durumları
7. **Sorun giderme ve referans:** Hatalar, yapılandırma, terimler

Okuyucu, ihtiyacına göre derinleşebilmelidir.

## Görseller

- Her görsel bir soruyu cevaplamalıdır.
- Akış için akış diyagramı, yapı için bileşen diyagramı, karşılaştırma için tablo/grafik kullanılır.
- Dekoratif kalabalık teknik açıklamanın yerine geçmez.
- Görsel ile metindeki isimler ve sürümler tutarlı olmalıdır.
- Örnekler gerçekçi, tekrar üretilebilir ve telif açısından güvenli olmalıdır.

## Zorunlu içerik

- Projenin amacı ve kapsam dışı alanlar
- Kurulum, kullanım ve örnek senaryolar
- Mimari ve veri akışı
- Güvenlik/mahremiyet yaklaşımı
- Test edilen ve edilmeyen durumlar
- Bilinen sınırlamalar
- Geliştirme ve katkı rehberi
- Sürüm ve tarih

## Proje sonu ortak inceleme kapısı

PDF ve sunum ilk taslaktan sonra kullanıcı ile birlikte incelenir:

- İlk bakışta amaç anlaşılıyor mu?
- Gereksiz veya tekrarlı bölüm var mı?
- Eksik örnek, bağlam ya da görsel var mı?
- Yeni başlayan ile uzman arasındaki seviye dengesi doğru mu?
- Kodun gerçek davranışıyla belge birebir uyumlu mu?
- Yazı boyutu, sayfa akışı, kontrast ve görsel çözünürlük yeterli mi?
- Gizli bilgi veya lisans/telif riski var mı?

Gerekirse birden fazla tur yapılır. Kod, belgeler ve sunum bu incelemeyi geçmeden proje tamamlanmış sayılmaz.
