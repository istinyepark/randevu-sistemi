# 🚀 Randevu Sistemi Kurulum Rehberi

## 📋 İçindekiler
1. [Sorun Neydi?](#sorun-neydi)
2. [Çözüm: Google Apps Script](#çözüm-google-apps-script)
3. [Adım Adım Kurulum](#adım-adım-kurulum)
4. [Test Etme](#test-etme)
5. [Sorun Giderme](#sorun-giderme)

---

## ❌ Sorun Neydi?

Eski sistemde şu hata alıyordunuz:
```
API keys are not supported by this API
Expected OAuth2 access token
```

**Neden?**
- ✅ API Key ile Google Calendar'ı **okuyabilirsiniz**
- ❌ API Key ile Google Calendar'a **yazamazsınız** (randevu oluşturamazsınız)

Google Calendar'a yazma işlemi için OAuth 2.0 veya Service Account gerekiyor.

---

## ✅ Çözüm: Google Apps Script

Google Apps Script kullanarak **backend kurmadan**, tamamen **ücretsiz** ve **kolay** bir şekilde sorunu çözdük!

### Avantajlar:
- ✅ **Tamamen ücretsiz**
- ✅ **Backend kurulumu yok**
- ✅ **Google Calendar'a direkt yazma yetkisi var**
- ✅ **CORS sorunu yok** (JSONP kullanıyoruz)
- ✅ **10-15 dakikada kurulur**
- ✅ **Güvenli** (Google'ın sunucusunda çalışır)

### Nasıl Çalışıyor?

**Eski Sistem (Hatalı):**
```
Müşteri Sayfası → Direkt Calendar API → ❌ Yetki Hatası
```

**Yeni Sistem (Çalışıyor!):**
```
Müşteri Sayfası → Apps Script (JSONP) → Google Calendar → ✅ Başarılı
```

---

## 🛠️ Adım Adım Kurulum

### 1️⃣ Google Apps Script Kurulumu (10 dakika)

#### a) Apps Script Projesi Oluştur

1. **Google Drive'a git**: https://drive.google.com
2. **Yeni → Diğer → Google Apps Script** tıkla
   - Eğer "Google Apps Script" görünmüyorsa: **Yeni → Diğer → Diğer uygulamalara bağlan** ile ekle
3. Proje adını **"Randevu Backend"** yap

#### b) Kodu Yapıştır

1. Açılan editörde varsayılan kodu **tamamen sil**
2. Bu klasördeki **`apps-script-backend.js`** dosyasının **tüm içeriğini** kopyala
3. Apps Script editörüne **yapıştır**

#### c) Ayarları Yap

Kodun en üstünde bulunan `CONFIG` bölümünü düzenle:

```javascript
const CONFIG = {
  CALENDAR_ID: 'primary', // veya 'sizin@gmail.com'
  TIMEZONE: 'Europe/Istanbul'
};
```

**CALENDAR_ID Seçenekleri:**
- `'primary'` → Kendi varsayılan takvimin
- `'ornek@gmail.com'` → Başka bir takvim

#### d) Deploy Et (Yayınla)

1. **💾 Kaydet** (Ctrl+S veya Cmd+S)
2. Sağ üstte **Deploy → New Deployment** tıkla
3. **⚙️ Ayarlar** (Dişli simgesi) → **Web App** seç
4. Ayarlar:
   - **Description**: `v1` veya `İlk sürüm`
   - **Execute as**: `Me`
   - **Who has access**: `Anyone` ✅ **ÇOK ÖNEMLİ!**
5. **Deploy** tıkla
6. **Authorize access** → Google hesabınızla giriş yap
7. Açılan sayfada **"Advanced" → "Go to Randevu Backend (unsafe)"** tıkla
8. **Allow** tıkla

#### e) URL'i Kopyala

Deploy tamamlandıktan sonra size bir **Web App URL** verilecek:

```
https://script.google.com/macros/s/AKfycby...../exec
```

**Bu URL'i kopyalayın! İleride kullanacaksınız.** 📋

---

### 2️⃣ Admin Panelini Ayarla (2 dakika)

1. **`admin.html`** dosyasını tarayıcıda aç
2. **⚙️ Ayarlar** sekmesine git
3. **Apps Script Web App URL** alanına kopyaladığın URL'i yapıştır
4. **💾 Kaydet** tıkla

---

### 3️⃣ Çalışanları ve Vardiyaları Ayarla (5 dakika)

1. Admin panelinde **👥 Çalışanlar** sekmesine git
2. Çalışan ekle/düzenle
3. **📅 Vardiyalar** sekmesinde haftalık vardiya planlaması yap
   - 🌅 Sabah (10:00-18:00)
   - 🌆 Akşam (14:00-22:00)
   - 🌞 Full (10:00-22:00)
   - ❌ İzinli

---

### 4️⃣ Müşteri Sayfasını Paylaş (1 dakika)

1. Admin panelinde **⚙️ Ayarlar** sekmesinde
2. **"🔗 Müşteri Randevu Linki"** bölümünden müşteri linkini kopyala
3. Bu linki müşterilerinize gönderebilirsiniz

**Önemli:** Dosyaları web'e yüklemeden kullanmak için:
- `admin.html` ve `index.html` **aynı klasörde** olmalı

---

## 🧪 Test Etme

### Test 1: Apps Script Çalışıyor mu?

1. Tarayıcıda yeni sekme aç
2. Apps Script URL'ini yapıştır ve sonuna `?action=test&callback=test` ekle:
   ```
   https://script.google.com/macros/s/AKfycby...../exec?action=test&callback=test
   ```
3. Görmek istediğin:
   ```javascript
   test({"status":"ok","message":"Apps Script çalışıyor!"})
   ```

### Test 2: Randevu Oluşturma

1. **`index.html`** sayfasını aç
2. Apps Script URL'ini gir ve **💾 Kaydet** tıkla
3. Bir tarih, çalışan ve saat seç
4. Bilgilerini gir ve **Randevuyu Onayla** tıkla
5. Başarılı mesajı görmek istediğin:
   ```
   ✅ Randevunuz başarıyla oluşturuldu!
   ```

### Test 3: Google Calendar'da Kontrol

1. https://calendar.google.com adresine git
2. Oluşturduğun randevuyu göreceksin!

### Test 4: Admin Panelinde Randevuları Görme

1. **`admin.html`** sayfasını aç
2. **📋 Randevular** sekmesine git
3. Oluşturduğun randevuları görmelisin

---

## 🔧 Sorun Giderme

### Hata: "Randevular yüklenirken hata oluştu"

**Sebep:** Apps Script URL eksik veya yanlış

**Çözüm:**
1. Admin panelinde **⚙️ Ayarlar** → Apps Script URL kontrol et
2. URL'in sonunda `/exec` olmalı
3. **💾 Kaydet** ve sayfayı yenile (Ctrl+F5 veya Cmd+Shift+R)
4. Tarayıcı önbelleğini temizle

---

### Hata: "Apps Script yüklenemedi"

**Sebep:** Deploy ayarları yanlış

**Çözüm:**
1. Apps Script'te **Deploy → Manage Deployments** git
2. **"Who has access"** ayarının **"Anyone"** olduğundan emin ol
3. Gerekirse **yeniden deploy et**
4. Yeni URL'i admin panelinde güncelle

---

### Hata: "Authorization required"

**Sebep:** Apps Script yetkilendirilmemiş

**Çözüm:**
1. Apps Script editöründe **testCreateAppointment** fonksiyonunu seç
2. **Run** tıkla
3. Açılan pencerede yetki ver
4. Tekrar deploy et

---

### Hata: "Calendar bulunamadı"

**Sebep:** CALENDAR_ID yanlış

**Çözüm:**
1. Apps Script kodunda `CALENDAR_ID` değerini kontrol et
2. `'primary'` veya kendi gmail adresin olmalı
3. **💾 Kaydet** ve **Deploy → New Deployment** ile yeniden deploy et

---

### Randevular Görünmüyor

**Sebep:** Tarayıcı önbelleği eski dosyaları gösteriyor

**Çözüm:**
1. **Ctrl+F5** (Windows) veya **Cmd+Shift+R** (Mac) ile sayfayı yenile
2. Tarayıcı önbelleğini temizle (Ctrl+Shift+Del)
3. Tarayıcı konsolunu aç (F12) ve hataları kontrol et

---

## 📁 Dosya Yapısı

```
randevu_app/
├── apps-script-backend.js      ← Google Apps Script'e yapıştırılacak
├── index.html                  ← Müşteri randevu sayfası
├── admin.html                  ← Admin paneli
└── KURULUM_REHBERI.md          ← Bu dosya
```

---

## 🌐 Web'e Yükleme (Opsiyonel)

Dosyaları bilgisayarınızdan çalıştırmak yerine web'e yüklemek isterseniz:

### Seçenek 1: Netlify (Önerilen - Ücretsiz)
1. https://netlify.com adresine git
2. **Add new site → Deploy manually**
3. `index.html` ve `admin.html` dosyalarını sürükle-bırak
4. Otomatik URL alacaksın (örn: `https://randevu-kulahcioglu.netlify.app`)
5. Admin panelini aç ve Apps Script URL'ini kaydet
6. Müşteri linkini paylaş

### Seçenek 2: GitHub Pages (Ücretsiz)
1. GitHub hesabı oluştur
2. Yeni repo oluştur (public)
3. Dosyaları yükle
4. Settings → Pages → **Source: main branch** → Save
5. Verilen URL ile sistemi kullan

### Seçenek 3: Vercel (Ücretsiz)
1. https://vercel.com adresine git
2. Import Git Repository
3. Deploy

---

## 🔒 Güvenlik Notları

- Apps Script **"Anyone"** erişimine açık olmalı ama bu güvenlidir çünkü:
  - Sadece randevu oluşturma/okuma yapabilir
  - Silme/düzenleme fonksiyonları yok
  - Google'ın kendi sunucusunda çalışır
  - Rate limiting var (kötüye kullanıma karşı)

- Daha fazla güvenlik isterseniz:
  - Apps Script'e API key sistemi ekleyebilirsiniz
  - IP kısıtlaması yapabilirsiniz
  - Günlük randevu limiti koyabilirsiniz (kod içinde zaten var)

---

## 📞 Destek

Sorun yaşarsanız:

1. **KURULUM_REHBERI.md** dosyasındaki "Sorun Giderme" bölümüne bakın
2. Apps Script'te **View → Executions** ile hata kayıtlarına bakın
3. Tarayıcı konsolunu açın (F12) ve hataları kontrol edin
4. Apps Script URL'in doğru kaydedildiğinden emin olun

---

## ✅ Sonuç

Artık sisteminiz:
- ✅ Tamamen çalışıyor
- ✅ Google Calendar'a yazabiliyor
- ✅ CORS sorunu yok (JSONP kullanıyor)
- ✅ Ücretsiz
- ✅ Backend sunucusu gerektirmiyor
- ✅ Basit ve temiz kod yapısı

**Keyifli kullanımlar! 🎉**
