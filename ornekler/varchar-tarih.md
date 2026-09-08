# Eski veriyle yaşamak — metin olarak saklanmış tarihler

## Durum

Devraldığım bir sistemde tarih kolonlarının 56'sı `varchar`. İçlerinde üç
farklı biçim yan yana duruyor:

```
2026-07-03              tarih
2026-07-03 14:22:10     tarih + saat
1751500800              Unix zaman damgası
```

Bunun sonucu: `whereBetween` gibi karşılaştırmalar **metin** karşılaştırması
yapıyor ve kayıt kaybettiriyor. Aylık raporlar sıfır dönüyordu.

Bir tabloda 682 üyenin 647'si (%95) Unix damgası olarak saklanmıştı — 2022
yılı raporda "0 üye" gösteriyordu, gerçekte 313 üye vardı.

## Neden hemen kolon tipini değiştirmedim

Cazip olan `ALTER TABLE ... MODIFY date` demek. Ama:

- Dönüşüm sırasında ayrıştırılamayan değer sessizce `0000-00-00` olur
- Bu kolonlara yazan onlarca yer var, hepsi aynı anda uyumlu olmalı
- Canlı bir muhasebe sisteminde geri dönüşü zor

Önce **okumayı** düzeltip sistemi doğru çalışır hâle getirmek, veriyi sonra
temizlemek daha güvenli.

## Yaptığım

Karşılaştırmaları veritabanına yaptırdım — `DATE()` her üç biçimi de doğru
yorumluyor:

```php
// önce (metin karşılaştırması, kayıt kaybettiriyordu)
->whereBetween('faturalar.odenen_tarih', [$bas, $son])

// sonra
->whereRaw('DATE(faturalar.odenen_tarih) BETWEEN ? AND ?', [$bas, $son])
```

40 çağrı yeri vardı. Tekrar eden deseni bir sorgu makrosuna çevirdim ki yeni
kod yazarken aynı tuzağa düşülmesin:

```php
Builder::macro('whereTarihBetween', function (string $kolon, $bas, $son) {
    $g = fn ($d) => $d instanceof DateTimeInterface
        ? $d->format('Y-m-d')
        : Carbon::parse($d)->toDateString();

    return $this->whereRaw("DATE($kolon) BETWEEN ? AND ?", [$g($bas), $g($son)]);
});
```

## Bedeli

`DATE(kolon)` indeksi kullanılamaz hâle getirir. Bu tablolarda satır sayısı
düşük olduğu için kabul edilebilir bir takas — ama kalıcı çözüm değil,
ödünç alınmış zaman.

## Ders

Eski sistemlerde "doğru olan" ile "şu an yapılabilir olan" farklı şeyler.
Önce yanlış sonucu durdur, veriyi ayrı bir işte temizle. İkisini aynı anda
yapmaya çalışmak, ikisini de yarım bırakmanın en hızlı yolu.
