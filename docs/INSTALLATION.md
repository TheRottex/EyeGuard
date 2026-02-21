# Kurulum ve GitHub Yayınlama Rehberi (Kaynak Kod Paylaşmadan)

Bu doküman iki şeyi anlatır:
1) EyeGuard’ı **GitHub’da kod göstermeden** yayınlama (binary-only repo / Releases)  
2) Visual Studio ile **Setup/Installer** üretme adımları (genel rehber)

> Not: GitHub’da public yayın yaparsanız herkes kurulum dosyasını indirebilir. Kaynak kodu yüklemezseniz bile, **binary tersine mühendislik** tamamen engellenemez. En güçlü koruma yöntemi: **private repo** + sadece yetkililere erişim.

---

## 1) GitHub’da Kod Göstermeden Yayınlama (Önerilen Akış)

### A) Repo’yu oluştur
1. GitHub → **New repository**
2. İsim: `EyeGuard` (veya istediğiniz)
3. **Visibility:** 
   - Gizlilik istiyorsanız: **Private**
   - Herkes indirsin istiyorsanız: **Public** (kaynak kod yüklemeden)
4. `Add a README` işaretleyin.

### B) Repo içeriği (minimum)
Bu depoya **kaynak kod** koymadan sadece dokümantasyon koyabilirsiniz:

- `README.md` (tanıtım + indirme bağlantısı)
- `docs/` (kurulum, kullanım, SSS)
- `assets/` (ekran görüntüleri)

> Installer dosyasını repo içine commit etmek yerine **Releases**’a ekleyin.

### C) Releases’a sürüm ekle
1. Repo → sağ tarafta **Releases** → **Create a new release**
2. `Tag` oluşturun: örn. `v1.0.0`
3. Release title: örn. `EyeGuard v1.0.0`
4. **Attach binaries** bölümüne:
   - `EyeGuard_Setup.exe` veya `EyeGuard.msi`
5. Release notlarına:
   - Yenilikler / düzeltmeler
   - Sistem gereksinimleri
   - (Opsiyonel) SHA256 checksum

### D) Kullanıcı için indirme yönlendirmesi
README’de “Releases sayfasından indir” dediğinizde, kullanıcı doğrudan doğru akışta ilerler.

---

## 2) Visual Studio ile Installer (Setup Project) Üretme — Genel Rehber

> Bu bölüm Visual Studio sürümüne göre ufak farklılık gösterebilir ama mantık aynı.

### A) Gerekli bileşenler (Visual Studio Installer)
- **Workloads**: “.NET desktop development” (WinForms/WPF ise)
- Kurulum projesi için genelde şu seçeneklerden biri kullanılır:
  1) **Microsoft Visual Studio Installer Projects** (VS extension)
  2) **ClickOnce / Publish**
  3) .NET `dotnet publish` + dış installer aracı (Inno Setup / WiX)

### B) Setup Project (VS Installer Projects) ile
1. Solution’a sağ tık → **Add** → **New Project**
2. `Setup Project` seçin
3. Setup projesine sağ tık → **Add → Project Output**
4. Ana uygulama projenizden:
   - **Primary Output** (EXE + gerekli dll’ler)
5. Gerekli dosyalar varsa:
   - `Content Files` olarak ekleyin (ikon, config, vb.)
6. Kısayollar:
   - `File System` → `User’s Desktop` veya `Programs Menu` → shortcut ekleyin
7. Kurulum özellikleri:
   - `ProductName`, `Manufacturer`, `Version`
   - `UpgradeCode` (sürüm yükseltme için sabit kalmalı)
8. Build alın → `Release` klasöründe `.msi` oluşur.

### C) Yayına hazır build (öneriler)
- `Release` modunda derleyin
- `.pdb` gibi debug dosyalarını yayınlamayın
- Mümkünse **code signing** (imza) kullanın (kullanıcı güveni için)

---

## 3) Güvenlik ve “Kodumu kimse görmesin” konusu

- **Repo’ya kaynak kod yüklemeyin.** Sadece doküman + görsel.
- Repo’yu **Private** tutarsanız, erişimi kontrol edersiniz.
- Public binary dağıtımda:
  - Kod görünmez ama binary üzerinden analiz mümkün olabilir.
  - En azından obfuscation, imza ve lisans/EULA ile caydırıcılık sağlayın.

