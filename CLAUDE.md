# Claude Code — Proje Giriş Noktası

@START_HERE.md
@PROJECT_INDEX.md
@AI_COLLABORATION_RULES.md
@standards/AI_CONTINUITY_STANDARD.md
@standards/ENGINEERING_STANDARDS.md
@standards/SECURITY_AND_PRIVACY.md
@projects/AX_DOWNLOADER.md

## Rol

Bu depo, SenyorAKs-lab projelerinin merkezi bilgi ve AI süreklilik deposudur. Buradaki güncel proje dosyaları, doğrulanmış test sonuçları ve ortak standartlar çalışma bağlamının kaynağıdır.

## Her oturumda zorunlu başlangıç

1. Yukarıdaki içe aktarılan dosyaları tamamen oku.
2. Aktif projeyi ve sıradaki tek somut adımı kullanıcıya kısaca doğrula.
3. Test edilmemiş bir aday sürümü doğrulanmış veya çalışan sürüm gibi sunma.
4. Kod değişikliğinden önce gerçek yerel dosyaları, sürümü ve mevcut test çıktısını incele.
5. Proje kaydı ile kod çelişirse çelişkiyi görünür kıl; sessizce varsayım yapma.

## Mevcut devir özeti

- Aktif proje: AX Downloader.
- Son doğrulanan referans motor: Python v0.4.0 adaptive parallel resume.
- Kritik gerçek: CP-005K, v0.4.0'ın aynı boyutlu segment bozulmasını algılamadan `DOWNLOAD_COMPLETE` diyebildiğini kanıtladı.
- Hazırlanmış fakat kabul edilmemiş aday: v0.4.1, 1 MiB blok SHA-256 doğrulaması.
- Sıradaki tek somut adım: v0.4.1 adayını CP-005K ile doğrulamak.
- Beklenen minimum sonuçlar: `FINAL_HASH_MATCH`, `CORRUPTION_RECOVERED`, `CP005K_ENGINE_SAFE`.
- Bu sonuçlar ve eski regresyonlar geçmeden davranış sözleşmesi dondurulmaz; C#/.NET taşımasına veya UI geliştirmesine geçilmez.

Güncel ve ayrıntılı gerçekler için daima `projects/AX_DOWNLOADER.md` esas alınır; bu kısa özet kaynak dosyanın yerine geçmez.

## Depo ve kod sınırı

- Bu depo şu anda merkez bağlam/standart deposudur; güncel AX Downloader kaynak kodunun eksiksiz proje deposu değildir.
- Kullanıcının Windows çalışma klasörü `%USERPROFILE%\Desktop\AX-Downloader` olarak kaydedilmiştir.
- Gerçek kod üzerinde çalışırken önce bu yerel klasörü incele; merkez depoda olmayan dosyaları varmış gibi kabul etme.
- Yayın aşamasında AX Downloader için ayrı bir kod deposu oluşturulacak ve merkez dizine bağlanacaktır.

## Kullanıcıyla çalışma biçimi

- Kullanıcıyla iletişim Türkçe, samimi ve doğrudan olabilir; proje belgeleri profesyonel ve tarafsız kalmalıdır.
- Windows testlerinde her komutun hangi pencerede çalışacağını açıkça `1. CMD` ve `2. CMD` olarak belirt.
- Uzun dosyalarda belirsiz satır tarifleri yerine tam güncel dosya veya güvenli otomatik yama tercih et.
- Bir testin açık bulması, test mekanizmasının çalıştığını gösterebilir; ürün davranışının geçtiği anlamına gelmez.
- Sessiz veri bozulmasına sıfır tolerans uygula: bütünlük kanıtlanmadıysa tamamlanmış ilan etme.
- Silme, üzerine yazma, yayınlama ve erişim değişikliği gibi işlemlerde kapsamı kesinleştir ve geri dönüş yolunu koru.
- GitHub'a yalnızca `AI_COLLABORATION_RULES.md` içindeki kullanıcı kontrollü kayıt tetikleyicilerinden biri oluştuğunda yaz.

## Çalışma sonunda

Kayıt tetiklendiyse ilgili güncel dosyayı yerinde güncelle ve şunları açık bırak:

1. Ne değişti?
2. Ne doğrulandı?
3. Hangi sorun açık kaldı?
4. Sıradaki tek somut adım nedir?

Yeni bir “final/son/v2” bağlam kopyası oluşturma; geçmişi Git zaten korur.
