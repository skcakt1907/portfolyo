# SiteWatch

**Site ve sertifika izleme paneli** · Laravel 11 · Livewire · Tailwind

> Kendi yazdığım iç araç — müşteri projesi değil. Çalışır durumda, gerçek
> sitelerle test edildi, **henüz bir sunucuda yayında değil.**

Yönettiğim müşteri sitelerinin ayakta olup olmadığını, SSL sertifikalarının
ne zaman biteceğini ve alan adı sürelerini izleyen iç araç.

## Neden yazdım

İki olay üst üste geldi: bir müşteri sitesinin detay sayfaları aylarca hata
verdi ve kimse fark etmedi; başka bir sitede SSL sertifikası bitti ve durumu
müşteri bize bildirdi. İkisi de bizim önce görmemiz gereken şeylerdi.

## Ne yapıyor

- Siteleri düzenli aralıkla kontrol eder, durum kodu ve yanıt süresi kaydeder
- SSL sertifikasının kalan gününü ve sağlayıcısını okur
- Çöken siteye hem yöneticiye hem müşteriye e-posta atar
- Panelde filtreler: çöken siteler, SSL süresi dolanlar, süresi dolmuşlar

Uyarı maili **iki ard arda başarısız kontrolden sonra** gidiyor — tek
seferlik bir kesintide gece yarısı kimseyi uyandırmasın diye.

## Bildiğim eksiği

Panel her site için yalnızca ana sayfayı kontrol ediyor. Yukarıda anlattığım
"detay sayfaları çökük ama ana sayfa çalışıyor" durumunu **bu hâliyle
yakalayamaz**. Site başına ek kontrol adresleri eklenmesi sıradaki iş.

Bir aracın ne yapamadığını bilmek, ne yaptığını bilmek kadar önemli.
