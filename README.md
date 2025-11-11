# DTC Service Web Sitesi - Deployment ve Yayınlama Rehberi

Bu rehber, DTC Service web sitenizi nasıl yayınlayacağınızı ve farklı hosting seçeneklerini adım adım açıklamaktadır.

## 📁 Proje Yapısı

```
dtcservice/
├── index.html                  # Ana sayfa
├── styles.css                  # CSS stilleri
├── script.js                   # JavaScript fonksiyonları
├── rpa.html                    # RPA otomasyonu sayfası
├── ai.html                     # AI otomasyonu sayfası
├── web3-domains.html           # Web3 domain sayfası
├── censorship-resistant.html   # Sansüre dayanıklı website sayfası
└── README.md                   # Bu dosya
```

## 🚀 Hosting Seçenekleri

### 1. Geleneksel Web Hosting (Kolay)

#### A. GitHub Pages (Ücretsiz)
```bash
# 1. GitHub hesabınızda yeni repo oluşturun
# 2. Dosyaları repo'ya yükleyin
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/KULLANICI_ADI/dtcservice.git
git push -u origin main

# 3. GitHub'da Settings > Pages > Source: "Deploy from a branch"
# 4. Branch: main, Folder: / (root) seçin
# 5. Siteniz https://KULLANICI_ADI.github.io/dtcservice adresinde yayında!
```

#### B. Netlify (Ücretsiz/Ücretli)
1. https://netlify.com'a kaydolun
2. "New site from Git" veya "Deploy manually" seçin
3. Dosyaları zip halinde yükleyin veya GitHub repo'sunu bağlayın
4. Otomatik olarak deploy edilir
5. Ücretsiz subdomain veya özel domain kullanabilirsiniz

#### C. Vercel (Ücretsiz/Ücretli)
```bash
# 1. Vercel CLI'yi kurun
npm i -g vercel

# 2. Proje klasöründe
vercel

# 3. Adımları takip edin
# 4. Siteniz otomatik olarak deploy edilir
```

#### D. Klasik Hosting (cPanel)
1. Bir hosting sağlayıcısından hosting satın alın
2. cPanel'e giriş yapın
3. File Manager'ı açın
4. public_html klasörüne tüm dosyaları yükleyin
5. Domain adresinizle siteye erişin

### 2. Web3 ve Merkezi Olmayan Hosting

#### A. IPFS ile Hosting
```bash
# 1. IPFS Desktop'ı indirin ve kurun
# https://desktop.ipfs.io/

# 2. Komut satırından (opsiyonel)
# IPFS CLI kurun
npm install -g ipfs

# IPFS'i başlatın
ipfs init
ipfs daemon

# Sitenizi IPFS'e ekleyin
ipfs add -r dtcservice/

# Çıktıdaki hash'i not edin (örn: QmXXXXXX...)
# Siteniz https://ipfs.io/ipfs/QmXXXXXX... adresinde erişilebilir
```

#### B. ENS Domain ile IPFS
```bash
# 1. ENS domain satın alın (metamask gerekli)
# https://app.ens.domains/

# 2. IPFS hash'inizi ENS'e bağlayın
# ENS yönetim panelinde:
# - Content hash alanına IPFS hash'inizi ekleyin
# - örn: ipfs://QmXXXXXX...

# 3. Web3 tarayıcılarında domain.eth ile erişilebilir
```

#### C. Fleek ile Kolay IPFS Deployment
```bash
# 1. https://fleek.co'ya kaydolun
# 2. GitHub repo'nuzu bağlayın
# 3. Build settings:
#    - Build command: (boş bırakın)
#    - Publish directory: /
# 4. Deploy edin - otomatik IPFS'e yüklenir
# 5. ENS domain bağlayabilirsiniz
```

## 🔧 Özelleştirme ve Konfigürasyon

### 1. İletişim Bilgilerini Güncelleme

`index.html`, `rpa.html`, `ai.html`, `web3-domains.html`, `censorship-resistant.html` dosyalarında:

```html
<!-- E-posta adresini güncelleyin -->
<p><i class="fas fa-envelope"></i> info@dtcservice.x</p>

<!-- Telefon numarasını güncelleyin -->
<p><i class="fas fa-phone"></i> +90 XXX XXX XX XX</p>

<!-- Sosyal medya linklerini güncelleyin -->
<a href="https://twitter.com/dtcservice" class="social-link">
<a href="https://linkedin.com/company/dtcservice" class="social-link">
```

### 2. Form Handling (İletişim Formu)

#### Netlify Forms (Kolay)
```html
<!-- index.html'deki contact form'a netlify attribute'u ekleyin -->
<form class="contact-form" id="contactForm" netlify>
```

#### Formspree (Kolay)
```html
<!-- Form action'ını güncelleyin -->
<form class="contact-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
```

#### Custom Backend (İleri Düzey)
`script.js`'deki `simulateApiCall` fonksiyonunu gerçek API endpoint'inizle değiştirin:

```javascript
async function submitForm(data) {
    const response = await fetch('YOUR_API_ENDPOINT', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(data)
    });
    return response.json();
}
```

### 3. Analytics Ekleme

#### Google Analytics
```html
<!-- Head tag'inin sonuna ekleyin -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

#### Privacy-focused alternatives
- Plausible Analytics
- Fathom Analytics
- Simple Analytics

### 4. SEO Optimizasyonu

Her sayfada meta tag'leri güncelleyin:

```html
<meta name="description" content="Sitenizin açıklaması">
<meta name="keywords" content="anahtar, kelimeler">
<meta property="og:title" content="Sayfa Başlığı">
<meta property="og:description" content="Sayfa Açıklaması">
<meta property="og:image" content="https://yoursite.com/og-image.jpg">
```

## 📱 SSL Sertifikası ve Güvenlik

### Ücretsiz SSL (Let's Encrypt)
- GitHub Pages, Netlify, Vercel otomatik SSL sağlar
- cPanel hosting için "SSL/TLS" bölümünden Let's Encrypt aktifleştirin

### Security Headers
Hosting sağlayıcınızda veya `.htaccess` dosyasında:

```apache
# .htaccess dosyası
<IfModule mod_headers.c>
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-XSS-Protection "1; mode=block"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"
</IfModule>
```

## 🌐 Domain Yönetimi

### Geleneksel Domain
1. Domain registrar'dan (.com, .net, .org) domain satın alın
2. DNS ayarlarını hosting sağlayıcınıze yönlendirin
3. A Record veya CNAME Record ekleyin

### Web3 Domain
1. ENS (.eth), Unstoppable Domains (.crypto, .nft, .x) satın alın
2. IPFS hash'ini domain'e bağlayın
3. Web3 tarayıcılarında erişilebilir hale gelir

## 🚀 Performance Optimizasyonu

### 1. Resim Optimizasyonu
```bash
# ImageOptim, TinyPNG veya Squoosh kullanın
# WebP formatına dönüştürün
# Lazy loading ekleyin
```

### 2. CDN Kullanımı
- Cloudflare (ücretsiz)
- AWS CloudFront
- Google Cloud CDN

### 3. Caching
```html
<!-- Service Worker for caching -->
<script>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
</script>
```

## 📊 Monitoring ve Analytics

### 1. Uptime Monitoring
- UptimeRobot (ücretsiz)
- Pingdom
- StatusCake

### 2. Error Tracking
- Sentry
- LogRocket
- Bugsnag

### 3. Performance Monitoring
- PageSpeed Insights
- GTmetrix
- WebPageTest

## 🔐 Backup ve Recovery

### 1. Otomatik Backup
```bash
# GitHub ile otomatik backup
git add .
git commit -m "Update: $(date)"
git push
```

### 2. Database Backup (eğer kullanıyorsanız)
```bash
# MySQL backup
mysqldump -u username -p database_name > backup.sql

# PostgreSQL backup
pg_dump database_name > backup.sql
```

## 📋 Launch Checklist

### Pre-Launch
- [ ] Tüm linkler çalışıyor
- [ ] İletişim formu test edildi
- [ ] Mobil responsive test edildi
- [ ] Farklı tarayıcılarda test edildi
- [ ] Meta tag'ler dolduruldu
- [ ] SSL sertifikası aktif
- [ ] Analytics kuruldu
- [ ] Sitemap.xml oluşturuldu

### Post-Launch
- [ ] Google Search Console'a sitemap gönderildi
- [ ] Google Analytics kontrol edildi
- [ ] Sosyal medya hesapları güncellendi
- [ ] Backup sistemleri test edildi
- [ ] Performance metrikleri ölçüldü

## 🛠️ Troubleshooting

### Yaygın Sorunlar ve Çözümleri

#### 1. CSS/JS Dosyaları Yüklenmiyor
```html
<!-- Relative path kullanın -->
<link rel="stylesheet" href="./styles.css">
<script src="./script.js"></script>
```

#### 2. Form Submit Çalışmıyor
- Browser console'da hata mesajlarını kontrol edin
- CORS ayarlarını kontrol edin
- Form action URL'ini doğrulayın

#### 3. Mobile Responsive Sorunları
```css
/* Viewport meta tag eklendiğinden emin olun */
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

#### 4. IPFS Erişim Sorunları
- Farklı gateway'leri deneyin:
  - https://ipfs.io/ipfs/YOUR_HASH
  - https://gateway.pinata.cloud/ipfs/YOUR_HASH
  - https://cloudflare-ipfs.com/ipfs/YOUR_HASH

## 📞 Destek ve Güncellemeler

### Düzenli Bakım
- İçerik güncellemeleri
- Güvenlik yamaları
- Performance optimizasyonları
- Yeni özellik eklemeleri

### İletişim
- **E-posta:** info@dtcservice.x
- **Telefon:** +90 XXX XXX XX XX
- **GitHub:** https://github.com/dtcservice

---

Bu rehber sayesinde DTC Service web sitenizi başarıyla yayınlayabilir ve yönetebilirsiniz. Herhangi bir sorun yaşarsanız, yukarıdaki troubleshooting bölümünü kontrol edin veya bizimle iletişime geçin.

**Başarılar! 🚀**