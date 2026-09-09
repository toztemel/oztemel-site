# Teknik denetim — TODO

Bu liste, statik sitenin (index.html + styles.css) teknik denetiminden çıkan
bulguları içerir. Her madde bağımsız: istediğiniz sırayla, istediğiniz
session'da tek tek uygulayabilirsiniz. Bir maddeyi bitirince kutucuğu
işaretleyin.

Dosyalar: `index.html`, `styles.css`, kök dizin (`robots.txt`, `sitemap.xml`
henüz yok).

---

## Başlık hiyerarşisi

- [ ] **1. Hizmet ve aşama başlıkları h3 olmalı**
  Şu an `Services` / `How we work` bölüm başlıkları (`.band-name`) h2; içlerindeki
  `.service h2` ("Architecture and design" vb.) ve `.stage h2` ("Discovery" vb.)
  öğe başlıkları da h2 — yani üst bölümle aynı seviyede görünüyorlar, alt öğe
  gibi değil. Ekran okuyucu başlık listesinde hiyerarşi kayboluyor.
  Düzeltme: `.service h2` ve `.stage h2` etiketlerini `h3` yapın. CSS class'ları
  aynı kalabilir (seçiciler `class="service"` üzerinden çalışıyor, sadece
  `<h2>` → `<h3>` etiket değişimi; stil dosyasında `.service h2`/`.stage h2`
  seçicilerini de `.service h3`/`.stage h3` olarak güncellemeyi unutmayın).

- [ ] **2. Experience bölümündeki alan başlıkları heading değil**
  "Banking and payments" vb. `<b>` ile işaretli, heading değil — Services/How we
  work'teki eşdeğer öğeler heading kullanırken burada tutarsız.
  Düzeltme: `<b>` yerine `<h3>` yapın (madde 1 ile tutarlı), ya da bilinçli
  olarak `<dl><dt>/<dd>` kullanın.

## Semantik HTML

- [ ] **3. `<section>` elemanlarının erişilebilir adı yok**
  Her `.band` bir `<section>`, içindeki başlığa `aria-labelledby` ile
  bağlanmamış — tarayıcılar başlıksız section'ları landmark olarak
  yayınlamayabilir.
  Düzeltme: Her `.band-name` başlığına bir `id` verin, section'a
  `aria-labelledby="o-id"` ekleyin (Services, How we work, Experience, About,
  Contact — 5 section).

## JSON-LD yapısal veri

- [ ] **4. Yapısal veri hiç yok**
  Sayfada `<script type="application/ld+json">` yok — arama motorları
  işletmeyi/kişiyi/konumu anlayamıyor.
  Düzeltme: `ProfessionalService` + `founder: Person` şemasını `<head>`'e
  ekleyin:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "ProfessionalService",
    "name": "Öztemel",
    "description": "Software architecture consultancy in Amsterdam.",
    "url": "https://oztemel.nl/",
    "email": "hello@oztemel.nl",
    "address": {
      "@type": "PostalAddress",
      "addressLocality": "Amsterdam",
      "addressCountry": "NL"
    },
    "identifier": { "@type": "PropertyValue", "name": "KvK", "value": "42151764" },
    "founder": {
      "@type": "Person",
      "name": "…",
      "jobTitle": "Software Architect",
      "sameAs": ["https://www.linkedin.com/in/…"]
    }
  }
  ```
  Not: LinkedIn URL'i ve founder adı doldurulmalı (bkz. index.html'deki
  `href="#"` LinkedIn linki ve About bölümündeki boş paragraf).

## Meta etiketler

- [ ] **5. og:image yok**
  Open Graph etiketleri (og:type/title/description/url) var ama og:image yok
  — sosyal paylaşımda kart görselsiz görünüyor.
  Düzeltme: 1200×630 bir görsel ekleyip `og:image` ile referans verin; isteğe
  bağlı `twitter:card`/`twitter:image` de eklenebilir.

- [ ] **6. theme-color yok**
  Düzeltme: `<meta name="theme-color" content="#ffffff">` ekleyin (isteğe
  bağlı `prefers-color-scheme: dark` varyantı).

- [ ] **7. icon-192.png / icon-512.png kullanılmıyor**
  Dosyalar diskte var ama hiçbir yerden referans verilmiyor (manifest.json
  yok). Sayfa ağırlığına eklenmiyorlar (istenmiyorlar) ama repoda amaçsız
  duruyorlar.
  Düzeltme: Kullanım niyeti yoksa silin; PWA simgesi isteniyorsa
  `manifest.json` ekleyip `<link rel="manifest">` ile bağlayın.

## robots.txt ve sitemap.xml

- [ ] **8. İkisi de yok**
  Düzeltme: Kök dizine basit bir `robots.txt`:
  ```
  User-agent: *
  Allow: /
  Sitemap: https://oztemel.nl/sitemap.xml
  ```
  ve tek URL'lik bir `sitemap.xml` ekleyin.

## Erişilebilirlik

- [ ] **9. LinkedIn ikon linkinin dokunma hedefi çok küçük**
  `.icon-link` sadece 20×20px SVG'yi sarıyor, padding yok — WCAG 2.5.8 (AA)
  minimum 24×24px altında, mobilde yanlış dokunma riski.
  Düzeltme: `.icon-link`'e `padding: 0.5rem` gibi bir değer ekleyip görsel
  hizalamayı negatif margin ile koruyun; en az ~36-44px hedef alan.

  ✅ Kontrol edildi, sorun yok (uygulama gerekmiyor):
  - Renk kontrastı (`--muted` #666666 beyaz zeminde ~5.74:1, AA geçiyor)
  - Odak görünürlüğü (`a:focus-visible` belirgin outline veriyor)
  - Bağlantı metinleri ve alt nitelikleri (SVG'ler `role="img"`+`aria-label`
    ile doğru etiketli, LinkedIn linki `aria-label="LinkedIn"` taşıyor)

## Mobil

- [ ] **10. Taşma — gerçek cihaz/DevTools ile doğrulanmadı**
  `clamp()` minimum değerleri 320px genişlikte taşma yaratmayacak şekilde
  hesaplandı ama otomatik testte tarayıcı penceresi daraltılamadı. Bir kez
  gerçek mobil cihaz veya DevTools responsive modda görsel kontrol yapın.

  ✅ Kontrol edildi, sorun yok: viewport meta doğru tanımlı.

## Performans

  ✅ Kontrol edildi, sorun yok — uygulama gerekmiyor:
  - Font yükleme stratejisi (`preconnect` + `display=swap` kullanılmış)
  - Toplam sayfa ağırlığı küçük (~35KB + font), gereksiz istek/analytics yok

## E-posta bağlantısı

  ✅ Kontrol edildi, sorun yok: `mailto:hello@oztemel.nl` doğru biçimde.

---

## Kapsam dışı ama önemli

- [ ] **11. `files.zip` ve `mark.svg` repoda untracked duruyor**
  `files.zip` sitenin eski kaynak dosyalarının bir arşivi (index.html eski
  sürümü + ikonlar), `mark.svg` kullanılmayan bir logo varyantı. İkisi de
  commit edilirse GitHub Pages kökten servis ettiği için herkese açık hale
  gelir — dağıtım kirliliği, siteye değer katmıyor.
  Düzeltme: Commit etmeyin/silin; isterseniz `.gitignore`'a ekleyin.

---

## Bekleyen (önceki oturumdan, bu denetimden bağımsız)

Bunlar TODO değil ama tamamlanmamış içerik olarak `index.html` içinde işaretli:
- `#manifesto` alt bölümünün içeriği (`<!-- Manifesto — to be written -->`)
- LinkedIn profil URL'i (`href="#"`, Contact bölümünde)

~~About bölümündeki kişisel paragraf~~ — tamamlandı: bölüm başlığı
"Tayfun Öztemel" oldu, paragraf eklendi (bilinçli olarak birinci tekil, sitenin
geri kalanındaki şirket sesinden ayrı).
