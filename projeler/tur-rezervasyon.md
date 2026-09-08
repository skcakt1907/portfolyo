# Tur/transfer sitesi

**Tur ve transfer rezervasyon sitesi** · Laravel 12 · canlı ·


Çok dilli içerik, fiyat kademeleri ve rezervasyon akışı. Tur listeleri,
detay sayfaları, transfer talep formu.

## Not

Bu projede veritabanı MariaDB/MyISAM üzerinde çalışıyordu ve bir aşamada
yedeksiz veri kaybı yaşandı. Sonrasında düzenli yedek ve geri dönüş
prosedürü oturttum; artık her canlı müdahale öncesi yedek alınıyor ve
her teslim paketinde geri alma adımı ayrı dosya olarak bulunuyor.

Kötü giden bir şeyi portfolyoya yazmak tuhaf gelebilir ama bu olay
çalışma biçimimi kalıcı olarak değiştirdi — bugün yaptığım her canlı
değişiklik ölç → yedekle → uygula → doğrula sırasıyla ilerliyor.
