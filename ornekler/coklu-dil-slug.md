# Dile göre adres çözümleme

## Bağlam

Çok dilli bir sitede adreste dil kodu istemedik. Yolun kendisi dili söylüyor:

```
/producten/jaloezieen      Hollandaca
/produkte/jalousien       Almanca
/products/blinds        İngilizce
```

Bu, `/nl/producten` gibi öneklerden daha temiz ve her dil kendi anahtar
kelimesiyle indeksleniyor. Karşılığında iki şey gerekiyor: yol parçaları
tüm diller arasında benzersiz olmalı, ve içerik slug'ları dile göre değişmeli.

## Slug çözümleme

Ana dilin slug'ı temel `slug` kolonunda, diğerleri `slug_<kod>` kolonlarında
durur. Gelen adres hangi dile aitse ona bakılır, ama emin olmak için tüm
dillerin kolonu denenir — böylece dil değiştirdikten sonra eski adresi olan
kullanıcı da doğru kayda düşer:

```php
public function resolveRouteBinding($value, $field = null): ?Model
{
    $kolonlar = [$this->slugKolonu(app()->getLocale())];

    foreach (Locales::codes() as $kod) {
        $kolon = $this->slugKolonu($kod);
        if (! in_array($kolon, $kolonlar, true)) {
            $kolonlar[] = $kolon;
        }
    }

    return $this->where(function ($q) use ($kolonlar, $value) {
        foreach ($kolonlar as $kolon) {
            $q->orWhere($kolon, $value);
        }
    })->first();
}

private function slugKolonu(string $locale): string
{
    return $locale === Locales::primary() ? 'slug' : 'slug_' . $locale;
}
```

## Bunun ısırdığı yer

Bu tasarımın gizli bir bağımlılığı var: **her dil için kolonun var olması.**
Kolon yoksa sorgu `Unknown column` ile patlar ve **bütün detay sayfaları**
hata verir. Liste sayfaları slug çözmediği için etkilenmez — yani site
dışarıdan ayakta görünür.

Canlıda tam olarak bu oldu: bir migration uygulanmamıştı, sitenin dörtte üçü
aylarca hata verdi ve fark edilmedi.

İkinci ısırığı: kolon listesi **birincil dile** göre hesaplanır. Birincil dil
Almanca'ysa `slug_nl` gerekir; Hollandaca'ysa `slug_de`. Yerel kurulumdaki
listeyi canlıya kopyalamak yetmez — canlının birincil dilini okumak gerekir.
Bunu ilk denemede atladım ve yanlış kolonları ekledim.

## Ders

Şemaya bağımlı kod yazarken şemanın orada olduğunu varsayma. `Schema::hasColumn`
ile korumak ya da kurulum kontrol listesine yazmak, aylarca süren sessiz
arızadan iyidir.
