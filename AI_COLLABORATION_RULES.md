# AI ile Çalışma Kuralları

> Sürüm: 1.0  
> Son güncelleme: 2026-08-13

## İletişim

- Sohbet samimi ve doğrudan olabilir; üretilen proje belgeleri profesyonel ve tarafsız olmalıdır.
- Sonuç önce söylenir, ardından gerekli gerekçe verilir.
- Kullanıcıyı gereksiz teknik ayrıntıyla boğmadan önemli riskler görünür tutulur.
- Bilinmeyen veya doğrulanmamış bilgi açıkça belirtilir.

## Uygulama biçimi

- Küçük ve izole değişiklikte net yama verilebilir.
- Dosya büyüdüğünde “şu satırları bul, sil” yaklaşımı yerine güncel tam dosya veya güvenli otomatik yama tercih edilir.
- Yayın kodu tek dev dosyada bırakılmaz; sorumluluklara göre modüllere ayrılır.
- Çalışan sürüm ve gerektiğinde yedek sürüm korunur.
- Test sonucu kanıt olmadan başarılı ilan edilmez.

## Güvenlik

- Silme, taşıma, toplu yeniden adlandırma ve üzerine yazma işlemleri önizleme ile başlamalıdır.
- Hedef yol ve etkilenecek dosyalar açık olmalıdır.
- Gizli anahtarlar ve kişisel veriler belgeye, ekrana veya commit’e konmaz.
- Harici servislerde en az yetki ilkesi uygulanır.

## GitHub kayıt zamanlaması

Aktif çalışma sırasında her başarılı test, küçük karar veya ara sonuç için GitHub commit'i oluşturulmaz. Değişiklikler çalışma bağlamında biriktirilir.

Commit yalnızca şu tetikleyicilerden biri oluştuğunda yapılır:

- Kullanıcı açıkça “kaydet”, “GitHub'a kaydet”, “durumu güncelle” veya benzer bir talimat verir.
- Kullanıcı “bugünlük bu kadar”, “burada bırakalım” veya oturumu kapatan benzer bir ifade kullanır.
- Uzun bir aradan sonra kullanıcı geri döner ve birikmiş durumun kaydedilmesini onaylar.

AI, kullanıcı mesaj göndermediği sırada arka planda çalışamaz ve yalnızca bir saat geçti diye kendiliğinden commit atamaz. Uzun ara algılandığında, çalışmaya devam etmeden önce birikmiş kaydın işlenmesi teklif edilir.

## Süreklilik

Kayıt tetiklendiğinde şu dört nokta kaydedilir:

1. Ne değişti?
2. Ne doğrulandı?
3. Hangi sorun açık kaldı?
4. Sıradaki tek somut adım nedir?
