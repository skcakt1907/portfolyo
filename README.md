# Aykut — Portfolyo

Laravel ile müşteri projeleri geliştiriyorum: kurumsal siteler, e-ticaret,
çok dilli tanıtım siteleri ve bir CRM/faturalama sistemi. Aşağıdakilerin
tamamı **canlıda, gerçek kullanıcılarda çalışıyor.**

Bu depo kaynak kodu içermez ve müşteri adı vermez — projelerin kodu ve
markası onların. Burada ne yaptığımı, hangi sorunları nasıl çözdüğümü ve
seçilmiş birkaç teknik örneği bulacaksınız.

Referans veya canlı adres gerekiyorsa görüşmede paylaşabilirim.

---

## Canlı projeler

| Proje | Ne | Yığın |
|---|---|---|
| [CRM + faturalama](projeler/crm-faturalama.md) | Müşteri, fatura, gider, bayi ve görev yönetimi · 200+ tablo | Laravel 12 · MariaDB |
| [Perde firması](projeler/perde-firmasi.md) | Hollanda pazarına çok dilli katalog sitesi | Laravel 13 |
| [Tur ve transfer](projeler/tur-rezervasyon.md) | Rezervasyon akışı, fiyat kademeleri, çok dilli | Laravel 12 |
| [Gurme e-ticaret](projeler/gurme-eticaret.md) | Katalog, sepet, iyzico ödeme, iki dil | Laravel 13 |
| [Kurumsal site](projeler/kurumsal-cokdilli.md) | 4 dil + Arapça/Farsça için RTL yerleşim | PHP |
| [Sağlık turizmi](projeler/saglik-turizmi.md) | Tanıtım sitesi ve talep formu | PHP |
| [Oto kurtarma / yol yardımı](projeler/oto-kurtarma.md) | 7/24 çekici firması, mobil öncelikli | PHP |
| [Parke ve zemin kaplama](projeler/parke-zemin.md) | Hizmet tanıtımı ve teklif formu | PHP |

## Geliştirme sürüyor

| Proje | Ne | Yığın |
|---|---|---|
| [Tekne turu rezervasyon](projeler/tekne-rezervasyon.md) | Çok satıcılı rezervasyon, WhatsApp entegrasyonu | Laravel 13 · **Filament** |
| [Oto galeri ve yedek parça](projeler/oto-galeri.md) | Galeri + parça satışı + kiralama, tek panelde | Laravel 13 |
| [Optik e-ticaret](projeler/optik-eticaret.md) | Sepet, ödeme, üyelik, admin paneli | Laravel 13 · iyzico |

## Ürün geliştirme

| Proje | Ne | Yığın |
|---|---|---|
| [Sektörel tema serisi](projeler/tema-serisi.md) | 24 sektör için 52 satılabilir tema, ana temadan türetilen üretim akışı | PHP |

## Kendi projem

Müşteri işi değil — kendi ihtiyacım için yazdım.

| Proje | Ne | Yığın | Durum |
|---|---|---|---|
| [SiteWatch](projeler/sitewatch.md) | Site ve SSL izleme paneli | Laravel 11 · Livewire | çalışıyor, henüz yayında değil |

---

## Teknik örnekler

Gerçek projelerden çıkarılmış, kendi başına anlaşılır örnekler:

- [Dile göre adres çözümleme](ornekler/coklu-dil-slug.md) — `/producten/jaloezieen` ve `/produkte/jalousien` aynı kayda nasıl düşer
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
