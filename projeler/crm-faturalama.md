# CRM sistemi

**CRM + faturalama + bayi yönetim sistemi** · Laravel 12 · MariaDB · canlı

Müşteri takibi, teklif/proforma, faturalama, gider yönetimi, görev ve destek
modülleri, bayi ağı ve otomatik hatırlatma e-postaları. 200+ tablo, günlük
aktif kullanımda.

## Sorumluluğum

Sistemi devraldım ve üzerinde çalışıyorum: yeni modüller, hata teşhisi,
canlı veri düzeltmeleri.

## Çözdüğüm somut sorunlar

**Raporlar sıfır dönüyordu.** Tarih kolonları `varchar` olarak saklanmış ve
içinde üç farklı biçim vardı: `2026-07-03`, tam tarih-saat, ve Unix zaman
damgası. Metin karşılaştırması yapan sorgular kayıt kaybediyordu. 40 sorguyu
`DATE()` ile sarmalayan bir düzeltme yazdım, tekrar eden desen için de bir
sorgu makrosu ekledim.

**Aynı hatırlatma maili iki kez gidiyordu.** Şikâyet vardı ama loglarda iz
yoktu. Canlı veriye baktığımda bildirim kayıtlarının %100'ünün çift yazıldığını,
çiftlerin 0–2 saniye arayla düştüğünü gördüm. Sebep: mail önce gönderiliyor,
kayıt sonra tutuluyordu; iki tetik aynı anda çalışınca ikisi de "gönderilmemiş"
görüyordu. Sırayı ters çevirdim — önce kayıt yeri kapılıyor, sonra mail gidiyor.

**İşlemler sahteydi.** Kod, bakiyeyle ödeme akışını `DB::beginTransaction()`
içine almıştı — doğru refleks. Ama tablolar MyISAM'di ve MyISAM işlem
desteklemiyor; `rollBack()` sessizce hiçbir şey yapmıyordu. Ölçtüm:

```
    işlem öncesi     : 50
    işlem içinde     : 999999
    rollBack sonrası : 999999      ← geri alınmadı
```

Pratik riski: bakiye düşüldükten sonra ikinci adım patlarsa müşterinin parası
gider, faturası ödenmemiş kalır. Para akışındaki 8 tabloyu InnoDB'ye taşıdım —
önce ayrı bir kopyada denedim, satır/indeks/karakter seti korunduğunu doğruladım,
canlıda ölç → yedekle → dönüştür → doğrula adımlarıyla uyguladım. 2546 satır,
sıfır kayıp.

**Ödeme tarihi hiç yazılmıyordu.** Kod, olmayan bir kolona yazmaya çalışıyordu;
kontrol her seferinde başarısız dönüyor, hiçbir şey yazılmıyor ve **hata da
vermiyordu**. Ödenmiş 1688 faturanın 34'ünde tahsilat tarihi boştu, o ödemeler
hiçbir günlük rapora düşmemişti. Aynı hatanın üç ayrı dosyada tekrarlandığını
buldum.

**Görevlere çoklu atama.** Tek kişilik atama alanını çoklu hale getirdim.
Eski raporlar tek alana baktığı için "birincil sorumlu" kavramını koruyarak
ara tablo ekledim — hiçbir mevcut ekran bozulmadı, 201 kayıt kayıpsız taşındı.

## Öğrendiğim

Sessiz başarısızlık en pahalısı. Bu projede bulduğum ciddi hataların
neredeyse tamamı hata vermeyen, log düşmeyen, sadece yanlış sonuç üreten
şeylerdi. Artık "çalışıyor gibi görünüyor"a güvenmiyorum; ölçüyorum.
