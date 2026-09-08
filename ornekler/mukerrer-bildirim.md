# Mükerrer bildirim önleme — "önce yeri kap"

## Sorun

Müşteri "aynı hatırlatma maili iki kez geliyor" dedi. Loglarda mükerrer
kayıt yoktu, yani ilk bakışta şikâyet doğrulanamıyordu.

Canlı veriye baktığımda bildirim kayıtlarının **%100'ü çift** yazılmıştı ve
çiftler 0–2 saniye arayla düşüyordu:

```
362 | 1gun | 2026-08-14 06:30:24
363 | 1gun | 2026-08-14 06:30:25   ← 1 saniye sonra
```

## Sebep

Kod şu sırayı izliyordu:

```php
if ($zatenGonderildiMi($fatura)) {
    return;                       // 1. kontrol et
}

Mail::to($alici)->send(...);      // 2. gönder
$log->kaydet($fatura);            // 3. kaydet
```

Görev iki ayrı yoldan tetiklenebiliyordu. İkisi aynı anda çalıştığında ikisi
de 1. adımda "gönderilmemiş" görüyor, ikisi de mail atıyordu.

Kardeş tabloda benzersizlik kısıtı vardı — ama o yalnızca ikinci **kaydı**
engelliyordu, mail çoktan gitmiş oluyordu. Logların temiz görünmesinin sebebi
buydu: kısıt kanıtı siliyor, sorunu silmiyordu.

## Çözüm

Sırayı ters çevirmek. Kayıt satırı, göndermeden **önce** yazılır ve bu yazma
işlemi atomik bir "yer kapma" olarak kullanılır:

```php
// Benzersizlik kısıtı (fatura_id, tip, tarih) üzerinde tanımlı.
// insertOrIgnore ikinci çalışmada 0 döner -> mail GÖNDERİLMEDEN atlanır.
$kapildi = DB::table('bildirim_log')->insertOrIgnore([
    'fatura_id' => $fatura->id,
    'tip'       => $tip,
    'tarih'     => today(),
    'durum'     => 'gonderiliyor',
]);

if (! $kapildi) {
    return;                        // başka bir çalışma üstlendi
}

try {
    $ok = Mail::to($alici)->send(...);
    $log->guncelle(['durum' => $ok ? 'ok' : 'fail']);
} catch (\Throwable $e) {
    $log->guncelle(['durum' => 'fail', 'hata' => $e->getMessage()]);
}
```

Kritik nokta: **kontrol ile eylem arasındaki boşluğu veritabanına kapattırmak.**
Uygulama içinde "önce bak sonra yap" yazdığın her yerde bu boşluk vardır.

## Doğrulama

Aynı komutu arka arkaya iki kez çalıştırdım:

```
1. çalışma : mail gitti,  log 1 satır
2. çalışma : "bugün zaten gönderilmiş, atlandı",  log 1 satır
```

## Not

Bu desen yalnızca benzersizlik kısıtı gerçekten varsa çalışır. Kardeş bir
tabloda kısıt eksikti; onu da ekledim — ama önce mevcut mükerrer satırları
temizlemek gerekti, yoksa `ALTER TABLE` reddeder.
