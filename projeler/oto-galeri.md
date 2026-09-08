# Oto galeri ve yedek parça

**Araç galerisi + parça satışı + kiralama** · Laravel 13 · geliştirme sürüyor

Tek bir işletmenin üç farklı işini aynı panelde toplayan sistem: ikinci el
araç galerisi, yedek parça satışı ve araç kiralama.

## Modüller

**Galeri** — araç kayıtları, çoklu fotoğraf, satılan araç arşivi ve araç
karşılaştırma ekranı.

**Yedek parça** — kategorili parça kataloğu, sipariş ve sipariş kalemleri.

**Kiralama** — araç kiralama kayıtları.

**Aracını sat** — ziyaretçinin kendi aracı için teklif talebi göndermesi.

## Neden tek sistem

Üçü ayrı yazılabilirdi ama araç kaydı üçünde de ortak: galeride satılık,
kiralamada müsait, "aracını sat"ta gelen teklifin konusu. Aynı veriyi üç ayrı
yerde tutmak, üç ayrı yerde bozulması demekti.

Ortak olanı paylaştırıp modüllere özgü alanları ayırdım.

## Not

Rakip bir sitede beğenilen üç özellik (satılan araçlar arşivi, araç
karşılaştırma, aracını sat) istendi ve eklendi. Böyle isteklerde
"aynısını yapalım" yerine **o özelliğin neden işe yaradığını** anlamaya
çalışıyorum — araç karşılaştırma işe yarıyor çünkü ikinci el alıcısı
kararsız ve iki ilan arasında sekme değiştirip duruyor.
