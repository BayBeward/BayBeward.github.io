# Dinamik Zaman Planlayıcı — Android Kurulum Rehberi

Uygulaman artık kurulabilir bir **PWA** paketi. Yani telefonda kendi simgesi olan,
tarayıcı çubuğu görünmeyen, **internetsiz de çalışan** gerçek bir uygulama gibi açılıyor.

Paket içeriği:

```
index.html                 → uygulamanın kendisi
manifest.webmanifest       → uygulama adı, simgesi, tam ekran ayarı
sw.js                      → çevrimdışı çalışma (service worker)
icons/                     → ana ekran simgeleri (192, 512, maskable)
```

---

## Yol 1 — En hızlısı: 3 dakikada telefonda (önerilen)

Android'in bir uygulamayı kurabilmesi için dosyaların bir **https adresinde** olması gerekir.
Ücretsiz ve hesapsız yolu:

1. Bilgisayarda **https://app.netlify.com/drop** adresini aç.
2. `planlayici` klasörünü (zip'ten çıkardığın klasörün **içeriğini**) sayfaya sürükle bırak.
3. Sana `https://rastgele-isim.netlify.app` gibi bir adres verir. Bu adres senin uygulaman.
4. Telefonda **Chrome** ile bu adresi aç.
5. Sağ altta çıkan **⬇️ Uygulamayı Yükle** butonuna bas.
   (Çıkmazsa: Chrome menüsü ⋮ → **Uygulamayı yükle** / **Ana ekrana ekle**)
6. Bitti. Ana ekranda kendi simgesiyle duruyor, uçak modunda bile açılıyor.

> Not: Netlify Drop linkleri kalıcıdır ama adresi kaybetmemek için kaydet.
> Kalıcı ve kendine ait bir adres istersen Yol 2'ye bak.

---

## Yol 2 — Kalıcı adres: GitHub Pages (önerilen)

Ücretsiz, süresiz ve adres asla silinmez. APK üretecekesen bu yolu seç.

1. GitHub'da yeni bir repo aç. **Adını `kullaniciadin.github.io` koy**
   (kendi kullanıcı adınla). **Public** olmalı.
2. Paketteki tüm dosyaları repoya yükle — `index.html` en üst klasörde olsun.
   Gizli `.nojekyll` dosyasını da yüklemeyi unutma (pakette hazır geliyor).
3. Repo → **Settings** → **Pages** → Source: `main` / `root` → **Save**.
4. 1-2 dakika sonra adresin hazır: `https://kullaniciadin.github.io/`
5. Telefonda Chrome ile aç → **Uygulamayı Yükle**.

Kodda değişiklik yaptığında dosyayı repoda güncellemen yeterli; uygulama kendini günceller.

### GitHub Pages'te dikkat edilecek 2 tuzak

**1. Repo adı neden `kullaniciadin.github.io` olmalı?**
APK üretirken gereken `assetlinks.json` dosyası **alan adının kökünde** durmak
zorundadır: `https://kullaniciadin.github.io/.well-known/assetlinks.json`.
Repoya `planlayici` gibi bir isim verirsen uygulaman
`kullaniciadin.github.io/planlayici/` altında kalır ve bu dosyayı köke koyamazsın.
Sadece "ana ekrana ekle" yapacaksan repo adı fark etmez, her isim çalışır.

**2. `.nojekyll` dosyası şart.**
GitHub Pages varsayılan olarak Jekyll kullanır ve Jekyll `.` veya `_` ile başlayan
klasörleri yayınlamaz. Yani `.well-known` klasörün 404 verir. Repo kökünde boş bir
`.nojekyll` dosyası olması bu davranışı kapatır.

---

## Yol 3 — Gerçek `.apk` dosyası istiyorsan

Yol 1 veya 2 ile adresin hazır olduktan sonra:

1. **https://www.pwabuilder.com** adresine git.
2. Uygulamanın https adresini yapıştır → **Start**.
3. **Android** kartında → **Generate Package**.
4. Paket seçeneklerinde **Signing key: Create new** bırak → indir.
5. İnen zip'in içinde imzalı bir **`.apk`** dosyası var (`app-release-signed.apk`).
6. Bu apk'yı telefona at (USB, WhatsApp, Drive fark etmez), dosyaya dokun.
7. Android "Bilinmeyen kaynak" uyarısı verirse → **Ayarlar** → o uygulamaya
   (Dosyalar/Chrome) **bu kaynaktan yüklemeye izin ver** → Yükle.

Bu apk, uygulamayı adresinden yükleyen bir kabuktur (Trusted Web Activity).
Yani siteyi güncellediğinde apk'yı yeniden kurmana gerek kalmaz.

> `.apk` dosyasını burada senin için derleyemedim: bu ortamda Android SDK yok ve
> Google'ın indirme sunucularına erişim kapalı. PWABuilder bu işi 2 dakikada ve
> ücretsiz yapıyor — imzalama dahil.

---

## Verilerin nerede duruyor?

Planların, seviyen ve disiplin zincirin telefonun kendi hafızasında (localStorage) tutuluyor.

- Uygulamanın içindeki **💾 Dosyaya Yedekle** ile ara ara yedek al.
- Chrome'un "site verilerini temizle" seçeneği bu verileri de siler, dikkat.
- Tarayıcıdaki veriler ile kurulu uygulamadaki veriler aynı adresi kullandığı için ortaktır;
  ama farklı bir adrese taşırsan veriler taşınmaz — yedek dosyasından **📂 Yükle** yap.

---

## Bu pakete eklenen şeyler

- Ana ekran simgesi ve uygulama adı (`manifest.webmanifest`)
- Tam ekran / tarayıcı çubuğu olmadan açılma (`display: standalone`)
- Çevrimdışı çalışma (`sw.js` — dosyalar önbelleğe alınıyor)
- Çentikli ve hareket çubuklu ekranlar için güvenli alan boşlukları
- Kurulabilir durumda görünen **Uygulamayı Yükle** butonu

Uygulamanın kendi mantığına (bloklar, pomodoro, XP, sürükle-bırak) dokunulmadı.
