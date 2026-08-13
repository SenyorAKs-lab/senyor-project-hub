# Güvenlik ve Mahremiyet Standardı

> Sürüm: 1.1  
> Son güncelleme: 2026-08-14

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
- Yerel IPC ve Native Messaging yalnızca izin verilen istemci/uzantı kimliğini kabul eder.

## Dosya işlemleri

- Silme ve üzerine yazma varsayılan davranış olmamalıdır.
- Dry-run, önizleme, karantina/yedek ve işlem günlüğü tercih edilir.
- Yol girişleri doğrulanmalı; proje veya izin verilen hedef kökü dışına çıkış engellenmelidir.
- Dosya adı temizleme yalnızca yasak karakterleri değil ayrılmış adları, sondaki nokta/boşluğu ve çakışmaları kapsamalıdır.
- Symlink/junction ve yol geçişi senaryoları test edilmelidir.
- Geçici dosyalar atomik tamamlanmalı; yarım sonuç nihai dosya gibi sunulmamalıdır.
- Bütünlük kanıtı olmadan geçici parçalar silinmemelidir.

## Ağ ve indirme

- HTTPS ve sertifika doğrulaması varsayılan olmalıdır.
- Yönlendirme, dosya adı, içerik uzunluğu, MIME türü ve Range yanıtları doğrulanmalıdır.
- `Content-Range` istenen başlangıç/bitiş ve toplam boyutla eşleşmeden veri kabul edilmez.
- Sunucu beklenen protokolü uygulamazsa güvenli şekilde durulmalıdır.
- Kaynağın boyut, ETag veya Last-Modified ile değiştiği anlaşılırsa eski parçalar yeni dosyayla birleştirilmemelidir.
- Boyut aynı olsa bile kayıtlı blok hash'i değişmişse bozuk blok yeniden indirilmelidir.
- Final boyut doğrulaması tek başına bütünlük kanıtı değildir.
- 429/503 tekrarları sınırlı geri çekilme ve bağlantı azaltmayla ele alınır; sonsuz döngü yapılmaz.

## Tarayıcı entegrasyonu

- Chrome Manifest V3 ve resmî Native Messaging yolu kullanılır.
- Yalnızca gerçekten kullanılan en dar izinler istenir; gelecekte lazım olabilir diye geniş izin alınmaz.
- Uzaktan kod çalıştırma, gizli sayfa takibi veya gereksiz gezinme verisi toplama yapılmaz.
- Eklenti, DRM, ödeme duvarı veya giriş kısıtını aşmaz.
- Belirsiz indirme adayında otomatik seçim yerine kullanıcı onayı istenir.
- Native Host yalnızca tanımlı uzantı origin'ini kabul eder ve gelen mesaj şemasını doğrular.

## İndirilen dosya güvenliği

- İndirilen program, script veya arşiv otomatik çalıştırılmaz/açılmaz.
- Windows'un kaynak bölgesi/Mark of the Web ve güvenlik tarama davranışı mümkün olduğunca korunur.
- Dosya hash'i veri bütünlüğünü gösterir; dosyanın zararsız olduğunu garanti etmez.
- UI kullanıcıya dosya türü, kaynak alan adı, nihai URL ve gerektiğinde risk uyarısı gösterir.

## Güncelleme güvenliği

- Güncelleme manifesti HTTPS üzerinden alınır ve şeması doğrulanır.
- Paket checksum ve mümkün olduğunda dijital imza doğrulanmadan kurulmaz.
- Kullanıcı onayı ve geri alma yolu ilk sürümlerde zorunludur.
- Başarısız güncelleme çalışan sürümü kullanılmaz hale getirmemelidir.
