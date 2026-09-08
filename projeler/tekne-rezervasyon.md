# Tekne turu rezervasyon platformu

**Çok satıcılı rezervasyon sistemi** · Laravel 13 · **Filament** · geliştirme sürüyor

Tekne sahiplerinin araçlarını listelediği, ziyaretçilerin tarih seçip talep
gönderdiği bir platform. Yönetim paneli Filament ile kuruldu.

## Kapsam

- Tekne kayıtları: fotoğraflar, özellikler, kapasite, ekstralar
- Sezona göre değişen fiyat kademeleri
- Müsaitlik takvimi ve kapalı dönemler
- Rezervasyon talepleri, durum geçmişi ve işlem kaydı
- Satıcı başına komisyon ayarları
- KVKK onay kayıtları
- Sayfa/SSS gibi içeriklerin panelden yönetimi

## Teknik olarak dikkat ettiğim şeyler

**Ödemesiz, talep bazlı akış.** Platform tahsilat yapmıyor; ziyaretçi talep
gönderiyor, satıcıyla anlaşma dışarıda kuruluyor. Bu bilinçli bir karar —
ödeme almak sorumluluk, mutabakat ve iade süreci demek. Müşterinin ihtiyacı
o değildi.

**WhatsApp entegrasyonu ve imza doğrulama.** Talepler WhatsApp üzerinden de
akıyor. Gelen webhook'lara **imza doğrulaması** ekledim — bu olmadan uç nokta
herkese açık ve sahte mesaj enjekte edilebilir durumda olurdu. Dışarıdan
veri kabul eden her uçta ilk sorduğum şey bu: gerçekten göndermesi gereken
taraftan mı geliyor?

**Müsaitlik çakışması.** Aynı tarihe iki talep gelebilir; kapalı dönemler ve
mevcut rezervasyonlar ayrı ayrı kontrol ediliyor.

## Değişen kapsamla çalışmak

Proje "özel tekne kiralama" olarak başladı, sonra **kişi başı grup turuna**
döndü. Bu, fiyatlandırmanın ve kapasite mantığının baştan kurulması demekti —
tekne başına fiyat ile kişi başına fiyat farklı iki model.

Bir ara "tek firma" modeline geçilmesi denendi ve aynı gün geri alındı.
Geri alabilmemizin sebebi değişikliğin küçük commit'ler hâlinde ilerlemesiydi.

Müşteri fikrini değiştirdiğinde tartışmak yerine **geri dönüşü ucuz tutmak**
daha işe yarıyor.
