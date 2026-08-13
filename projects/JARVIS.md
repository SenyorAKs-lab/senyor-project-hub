# Jarvis — Güncel Proje Bağlamı

> Durum: Fikir / beklemede  
> Son güncelleme: 2026-08-13

## Amaç

Android üzerinde çalışan, Türkçe ses ve metinle kullanılabilen, koyu arayüzlü kişisel bir yardımcı geliştirmek.

## Hedef yetenekler

- Sesli ve yazılı komut
- Arama başlatma
- SMS hazırlama/gönderme akışı
- Google araması
- Hotword veya hızlı tetikleme
- Mümkün olduğunca hafif ve çevrimdışı çalışabilen yapı

## Ana riskler

- Mikrofon, arama ve mesaj izinleri hassastır.
- Arka plan/hotword kısıtları Android sürümüne göre değişir.
- Yanlış komutun arama veya mesaj gibi etkili eylem üretmesi engellenmelidir.
- Mevcut geliştirme ortamı Android projesi için sınırlıdır.

## Tasarım ilkesi

Yüksek etkili eylemler açık kullanıcı onayı olmadan tamamlanmamalıdır. Kimlik bilgileri ve kişisel konuşma verileri merkez depoya yazılmamalıdır.

## Sıradaki adım

Uygun geliştirme ortamı oluştuğunda izin matrisi, çevrimdışı/çevrimiçi yetenek ayrımı ve güvenli onay akışı için fizibilite hazırlanacak.
