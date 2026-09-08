# Aykut — Portfolyo

Laravel ile müşteri projeleri geliştiriyorum: kurumsal siteler, e-ticaret,
çok dilli tanıtım siteleri ve bir CRM/faturalama sistemi. Aşağıdakilerin
tamamı **canlıda, gerçek müşterilerde çalışıyor.**

Bu depo kaynak kodu içermez — müşteri projelerinin kodu onların. Burada ne
yaptığımı, hangi sorunları nasıl çözdüğümü ve seçilmiş birkaç teknik örneği
bulacaksınız.

---

## Canlı projeler

| Proje | Ne | Yığın | Adres |
|---|---|---|---|
| [İş Ortağım](projeler/is-ortagim.md) | CRM + faturalama + bayi yönetimi | Laravel 12 · MariaDB | isortagim.dnkreatif.com |
| [MC Gordijnen](projeler/mc-gordijnen.md) | Hollanda perde firması, çok dilli | Laravel 13 | mcgordijnen.com |
| [Marmaris Travel Center](projeler/marmaris-travel.md) | Tur/transfer rezervasyon | Laravel 12 | travelcentermarmaris.com |
| [AEGEA Reserve](projeler/aegea-reserve.md) | Gurme ürün e-ticareti | Laravel 13 · iyzico | aegeareserve.com |
| [FGG Holding](projeler/fgg-holding.md) | Kurumsal site, 4 dil + RTL | PHP | fggholding.com |
| [Marmaris Health Center](projeler/marmaris-health.md) | Sağlık turizmi | PHP | marmarishealthcenter.com.tr |

## Teslime hazır

| Proje | Ne | Yığın | Durum |
|---|---|---|---|
| [Limon Optik](projeler/limon-optik.md) | Optik e-ticaret, sepet + ödeme | Laravel 13 · iyzico | tamamlandı, yayın bekliyor |

## Kendi projelerim

Müşteri işi değil — kendi ihtiyacım için yazdıklarım.

| Proje | Ne | Yığın | Durum |
|---|---|---|---|
| [SiteWatch](projeler/sitewatch.md) | Site ve SSL izleme paneli | Laravel 11 · Livewire | çalışıyor, henüz yayında değil |
| [Gece Oto Kurtarma](projeler/gece-oto-kurtarma.md) | Tek kişilik indie oyun | Unity · C# | geliştirme sürüyor |

---

## Teknik örnekler

Gerçek projelerden çıkarılmış, kendi başına anlaşılır örnekler:

- [Dile göre adres çözümleme](ornekler/coklu-dil-slug.md) — `/producten/gordijnen` ve `/produkte/gordijnen` aynı kayda nasıl düşer
- [Mükerrer bildirim önleme](ornekler/mukerrer-bildirim.md) — aynı mailin iki kez gitmesini engelleyen "önce yeri kap" deseni
- [Kapatılan dilin adreslerini kurtarma](ornekler/eski-adres-yonlendirme.md) — 96 adresi elle liste yazmadan yönlendirmek
- [Eski veriyle yaşamak](ornekler/varchar-tarih.md) — metin olarak saklanmış tarihleri kırmadan sorgulamak

---

## Nasıl çalışıyorum

**Ölçerim, tahmin etmem.** Bir hatanın sebebini bulduğumu söylemeden önce
yerelde yeniden üretirim. "Muhtemelen şudur" ile "kanıtladım" arasındaki farkı
önemsiyorum.

**Canlıya dokunmadan önce yedek ve geri dönüş planı.** Her teslim paketinde
ölç → uygula → doğrula → geri al adımları ayrı dosyalar hâlinde olur.

**Kodun yanına neden yazarım.** Bir tuzağa düşülmüşse, bir sonraki kişi aynı
tuzağa düşmesin diye sebebini de yazarım — "ne yaptığını" kod zaten söylüyor.
