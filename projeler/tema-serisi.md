# Sektörel tema serisi

**52 tema · 24 sektör** · düz PHP · satılabilir ürün

Küçük işletmelerin hızlıca kurumsal siteye kavuşması için hazırlanan,
sektöre göre içerik ve görsel dili değişen tema serisi.

## Kapsanan sektörler

Kurumsal (14 sürüm), restoran, otel, inşaat, avukatlık, mimarlık, kuaför,
belediye, tur, spor salonu, spa, optik, okul, nakliyat, muhasebe, medikal,
kreş, hafriyat, fotoğrafçılık, dernek, beach club, apart ve diğerleri.

## Nasıl üretiliyor

Tek tek elle yazmak yerine bir **ana tema** ve ondan türeyen bir üretim
akışı kurdum:

- Ortak yapı, bileşenler ve yönetim paneli ana temada
- Sektöre özgü olan: içerik bloklarının sırası, renk paleti, tipografi ve
  demo içerik
- Yeni bir sektör eklemek, sıfırdan tema yazmak yerine tanım dosyası yazmak

Böylece bir düzeltme ana temada yapılıp hepsine yansıyabiliyor; 52 ayrı
projeyi ayrı ayrı bakmak gerekmiyor.

## Neden düz PHP

Bu temalar paylaşımlı hostinge, çoğu zaman teknik bilgisi olmayan bir
kullanıcı tarafından kurulacak. Composer, migration ve terminal gerektiren
bir kurulum burada engel olur. Dosyaları at, veritabanı bilgisini gir, çalışsın
— hedef buydu.

Doğru aracı seçmek her zaman en güçlü aracı seçmek değil.

## Öğrendiğim

Seri üretimde asıl zorluk kod değil, **tekrarı nerede keseceğine karar
vermek.** Fazla ortaklaştırırsan her sektör birbirine benziyor ve tema
satın alma sebebi kalmıyor; az ortaklaştırırsan 52 ayrı proje bakımı
çıkıyor. Ortak olan yapı, farklı olan görünüm ve içerik oldu.
