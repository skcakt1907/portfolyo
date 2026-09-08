# MC Gordijnen

**Hollandalı perde/güneşlik firmasının tanıtım sitesi** · Laravel 13 · canlı
· mcgordijnen.com

Ürün kataloğu, proje galerisi, hizmetler, rehber yazıları ve iletişim.
Başlangıçta dört dilli (Hollandaca, Almanca, İngilizce, Türkçe).

## Dikkat çeken tasarım kararı

Adreste dil kodu yok — **yolun kendisi dili söylüyor**:

```
    /producten/gordijnen      Hollandaca
    /produkte/gordijnen       Almanca
    /products/curtains        İngilizce
```

Bu, `/nl/producten` gibi öneklerden daha temiz görünüyor ve SEO'da her dil
kendi anahtar kelimesiyle indeksleniyor. Karşılığında yol parçalarının tüm
diller arasında benzersiz olması gerekiyor; bunu bir sözlük sınıfı ve bir
test ile garanti altına aldım.

## Çözdüğüm somut sorunlar

**Sitenin dörtte üçü açılmıyordu, kimse fark etmemişti.** Ürün, kategori,
galeri ve hizmet detay sayfalarının tamamı hata veriyordu — sitemap'teki 193
adresin 144'ü. Liste sayfaları çalıştığı için site dışarıdan ayakta
görünüyordu; ziyaretçi bir ürüne tıklayınca hata sayfasıyla karşılaşıyordu.

Sebep: detay sayfaları kaydı bulurken her dil için ayrı bir slug kolonuna
bakıyor, o kolonlar canlıda hiç oluşturulmamıştı. Teşhisi tahminle değil
yeniden üreterek yaptım — yerelde kolonları silince tablo birebir aynı oldu
(liste 200, detay hata), geri ekleyince hepsi düzeldi.

**Dil kapatınca site tamamen düştü.** Müşteri Almanca ve Türkçe'yi kaldırmak
istedi. Kaldırdıktan sonra her sayfa hata vermeye başladı: ayarlardaki
"birincil dil" hâlâ Almanca'ydı ve rota adları birincil dile göre veriliyordu.
Birincil dil aktif diller arasında kalmayınca hiçbir sayfa adını alamadı.
Ayarı düzelttim ve kalıcı koruma ekledim — ayardaki dil kapatılmışsa sistem
ilk aktif dile düşüyor, aynı hata bir daha siteyi düşüremez.

**Kalkan dillerin 96 adresi.** Google o adresleri indekslemişti; hepsini 404
bırakmak sitenin aramadaki yerini düşürürdü. Elle 96 satırlık yönlendirme
listesi yazmak yerine kayıttan eşleştiren bir çözüm yazdım — yeni ürün
eklendiğinde veya slug değiştiğinde kendini günceller, diller geri açılırsa
kendiliğinden devre dışı kalır.

Sonuç: 193 adresin tamamı ya çalışıyor ya doğru yere yönleniyor.

## Öğrendiğim

Bir sitenin "ayakta" olması içinin çalıştığı anlamına gelmiyor. Ana sayfayı
kontrol eden bir izleme aracı bu arızayı aylarca kaçırırdı — nitekim kaçırdı.
