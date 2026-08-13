# Güvenlik ve Mahremiyet Standardı

> Sürüm: 1.0  
> Son güncelleme: 2026-08-13

## Depoya girmemesi gerekenler

- Parola, erişim anahtarı, token, çerez ve oturum bilgisi
- Kişisel e-posta, telefon, adres veya kimlik bilgisi
- Özel dosya yollarında gereksiz kullanıcı adı
- Telif hakkı ihlali yaratabilecek içerik veya test verisi
- Gerçek kullanıcı verisi

Örneklerde sahte ve açıkça örnek olduğu belli değerler kullanılır.

## Erişim

- Harici uygulamalara yalnızca gereken depo ve gereken izin verilir.
- Başlangıçta özel depo tercih edilir.
- Açık kaynak yayından önce geçmiş ve dosyalar sır sızıntısı açısından taranır.
- Koruma kuralları, bağımlılık taraması ve iki aşamalı doğrulama yayın öncesi kontrol edilir.

## Dosya işlemleri

- Silme ve üzerine yazma varsayılan davranış olmamalıdır.
- Dry-run, önizleme, karantina/yedek ve işlem günlüğü tercih edilir.
- Yol girişleri doğrulanmalı; proje kökü dışına çıkış engellenmelidir.
- Geçici dosyalar atomik olarak tamamlanmalı, yarım sonuç nihai dosya gibi sunulmamalıdır.

## Ağ ve indirme

- HTTPS ve sertifika doğrulaması varsayılan olmalıdır.
- Yönlendirme, dosya adı, içerik uzunluğu ve Range yanıtları doğrulanmalıdır.
- Sunucu beklenen protokolü uygulamazsa güvenli şekilde durulmalıdır.
- Kaynağın değiştiği anlaşılırsa eski parçalar yeni dosyayla birleştirilmemelidir.
