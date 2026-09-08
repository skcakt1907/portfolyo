# Optik e-ticaret

**Optik e-ticaret** · Laravel 13 · Bootstrap 5 · iyzico

Numaralı gözlük, güneş gözlüğü ve kontakt lens satan bir optikçi için
uçtan uca e-ticaret: katalog, sepet, üyelik, sipariş, admin paneli.

## Teknik olarak dikkat ettiğim şeyler

**Stok kilidi.** Aynı ürünü aynı anda iki kişi alırsa stok eksiye düşmesin
diye sipariş anında satır kilidi (`lockForUpdate`) kullanılıyor. Bunu
sonradan eklenen bir yama olarak değil, ödeme akışının parçası olarak
kurguladım.

**Mass-assignment koruması.** Modeller `$fillable` kullanıyor; `role`,
`total`, `status`, `order_no` gibi alanlar dışarıdan atanamıyor, sunucu
tarafında açıkça yazılıyor. Bir e-ticarette sipariş tutarının istekten
gelmesi en klasik açıklardan biri.

**Yükleme sertleştirmesi.** Görsel yüklemede uzantı istemciden değil
sunucudan türetiliyor, mime beyaz listesi var (SVG yok), yükleme klasöründe
script çalıştırma kapalı.

**Filtreleme.** Marka ve cinsiyet filtreleri, ürün özniteliklerinden
(JSON kolon) türetiliyor; marka listesi seçili kategoriye göre daralıyor.

## Ayrıca

Katalog filtresi, marka vitrini ve kayan marka şeridi; adres/telefon gibi
işletme bilgileri tek yerden yönetiliyor.
