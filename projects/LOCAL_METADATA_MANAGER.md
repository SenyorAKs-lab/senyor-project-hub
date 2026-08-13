# Local Metadata Manager — Güncel Proje Bağlamı

> Durum: Planlama  
> Son güncelleme: 2026-08-13

## Amaç

Jellyfin'den bağımsız çalışan yerel bir Windows aracıyla film klasörlerindeki metadata ve görselleri güvenli biçimde yeniden oluşturmak.

## Hedef akış

1. Kullanıcı film kök klasörünü seçer.
2. Her film klasörü taranır.
3. Video dosyaları, özellikle MP4, korunur.
4. Eski NFO, poster, backdrop, logo ve diğer metadata dosyaları aday olarak gösterilir.
5. Başlık eşleşmesi doğrulanır.
6. Metadata ve görseller sıfırdan oluşturulur.

## Güvenlik gereksinimleri

- Varsayılan ilk çalışma `dry-run` olmalıdır.
- Kör silme yerine karantina veya geri alınabilir yedek kullanılmalıdır.
- Etkilenecek dosyalar işlemden önce listelenmelidir.
- Her işlem denetim günlüğüne yazılmalıdır.
- Eşleşmeyen veya belirsiz başlıklar insan onayına bırakılmalıdır.
- Video dosyaları için koruma kuralı açık ve test edilmiş olmalıdır.

## Sıradaki adım

Dosya sınıflandırma kuralları, dry-run raporu, karantina yapısı ve geri alma sözleşmesini içeren teknik tasarım hazırlanacak.
