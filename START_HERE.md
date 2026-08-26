# START HERE

> Sürüm: 1.1  
> Son güncelleme: 2026-08-26  
> Amaç: Bir insanın veya yeni bir yapay zekânın çalışmaya en az bağlam kaybıyla devam etmesi

## Okuma sırası

1. Bu dosyayı tamamen oku.
2. [CURRENT_WORK.md](CURRENT_WORK.md) içinden güncel sıralı planı ve sıradaki tek somut adımı bul.
3. [PROJECT_INDEX.md](PROJECT_INDEX.md) içinden aktif projeyi bul.
4. İlgili `projects/*.md` dosyasını tamamen oku.
5. İşin türüne göre ilgili `standards/*.md` dosyasını oku.
6. Uygulama yapmadan önce mevcut dosyaları ve son doğrulanmış test durumunu kontrol et.
7. Çalışma sonunda anlamlı değişiklikleri güncel durum dosyasına işle.

## Bilginin öncelik sırası

Çelişki olduğunda şu sıra geçerlidir:

1. Kullanıcının mevcut konuşmadaki açık talebi
2. İlgili projenin güncel durum dosyası
3. Doğrulanmış test kanıtı ve çalışan kod
4. Ortak standartlar
5. Eski karar ve değişiklik kayıtları
6. Varsayım

Varsayım, doğrulanmış bilgi gibi yazılmaz.

## Devam etmeden önce kontrol listesi

- Hangi proje üzerinde çalışılıyor?
- Son tamamlanan adım ne?
- Sıradaki tek somut adım ne?
- Hangi testler gerçekten geçti?
- Açık hata veya risk var mı?
- Kodun güncel ve yedek sürümü hangisi?
- İşlem veri silebilir, üzerine yazabilir veya dış sisteme yayın yapabilir mi?

## Güncelleme kuralı

Aşağıdaki durumlarda merkez depo güncellenir:

- Önemli gereksinim veya kapsam değişikliği
- Mimari karar
- Kritik hata ve çözümü
- Test aşamasının geçmesi veya başarısız olması
- Uzun ara öncesi devir
- Sürüm veya yayın
- PDF/sunum ortak incelemesi

Her küçük sohbet veya deneme için kayıt açılmaz. Anlamlı gelişmeler aktif çalışma boyunca biriktirilir; her testten sonra otomatik commit atılmaz. GitHub kaydı yalnızca kullanıcı açıkça “kaydet/GitHub'a kaydet” dediğinde, “bugünlük bu kadar/burada bırakalım” diyerek oturumu kapattığında veya uzun bir aradan sonra kayıt önerisini onayladığında yapılır.

AI kullanıcı yokken arka planda çalışamadığı için bir saatlik sessizlik tek başına otomatik commit oluşturmaz. Güncel durum aynı dosyada tutulur; karar ve değişiklik geçmişi gerektiğinde eklenir.

## Yeni bir AI için çalışma talimatı

- Önce oku, sonra öner.
- Test edilmemiş sonucu geçmiş gibi yazma.
- Ekran görüntüsü veya çıktı varsa kanıtı yorumla; görünmeyen ayrıntıyı uydurma.
- Kodu büyütmeden önce modül sınırlarını koru.
- Kullanıcıya mümkünse çalıştırılabilir, tam dosya veya net bir yama ver.
- Gizli bilgi, erişim anahtarı, parola, e-posta veya kişisel veri kaydetme.
- Yıkıcı işlemlerde hedefi kesinleştir ve önizleme/geri dönüş yolunu koru.
- Çalışma bittiğinde ilgili proje dosyasındaki “Son durum”, “Doğrulananlar”, “Bilinen sorunlar” ve “Sıradaki adım” bölümlerini güncelle.
