# Kapatılan dilin adreslerini kurtarma

## Sorun

Müşteri iki dili kaldırmak istedi. O dillerdeki **96 adres** Google'da
kayıtlıydı; hepsini 404 bırakmak sitenin aramadaki yerini düşürürdü.

## Neden statik liste yazmadım

En kolay yol 96 satırlık bir yönlendirme tablosu yazmaktı. Ama:

- Yeni ürün eklendiğinde listede olmaz
- Bir slug değişirse liste bayatlar
- Diller geri açılırsa listeyi elle temizlemek gerekir

## Çözüm

Eşleştirmeyi kayıttan yaptım. Kapatılan dillerin slug kolonları veritabanında
**duruyor** — sadece aktif diller listesinden çıkarıldılar. Kayıt onlara
bakılarak bulunur, sonra ana dildeki adrese kalıcı (301) yönlendirilir:

```php
/** Yol sözlüğünde tanımlı ama artık aktif olmayan diller */
public static function kapatilanDiller(): array
{
    return array_values(array_diff(
        array_keys(Yollar::SAYFALAR['catalog']),
        Locales::codes()
    ));
}
```

Rotalar bu listeden üretilir. **Diller geri açılırsa liste boşalır ve hiçbir
rota kaydedilmez** — ayrıca bir şey yapmaya gerek kalmaz.

## İki tuzak

**1. Yönlendirme kendi kendine dönüyordu.** `/produkte` → `/produkte`.

Sebep: dil algılama ara katmanı adresteki yol kelimesinden dili "Almanca"
çıkarıyor, sonra adres üretici benim ürettiğim Hollandaca adresi tekrar
Almancaya çeviriyordu. Hedefi üretmeden önce dili ana dile sabitlemek gerekti.

**2. Parametreler yer değiştiriyordu.** Detay sayfalarında yol öneki düşüyordu.

Laravel, tip belirtilmemiş controller parametrelerini **isme göre değil sıraya
göre** geçiriyor. Rota parametreleri `(slug, sayfa)` sırasında geliyordu, metot
ise `($sayfa, $slug)` bekliyordu. Rotaları açık çağrıya çevirerek çözdüm:

```php
Route::get($ep($sayfa) . '/{slug}',
    fn (string $slug) => $ey()->detay($sayfa, $slug));
```

İkisi de canlıya çıksaydı "yönlendirme var ama yanlış yere gidiyor" derdik —
fark etmesi zor bir hata.

## Silinen içerik

Kaldırılan hizmetin sayfaları silinmemiş, pasife alınmıştı. Yönlendirme kaydı
buluyor ama hedef sayfa yine hata veriyordu. Pasif kayıtta doğrudan ilgili
listeye gönderdim — ve **301 değil 302** kullandım, çünkü içerik panelden geri
açılabilir. 301 deseydik arama motoru adresi kalıcı olarak düşürürdü.

## Sonuç

193 adresin tamamı ya çalışıyor ya doğru yere yönleniyor.
