# AendirDPI v3.1

Türkiye'deki DPI (Deep Packet Inspection) tabanlı internet engellemelerini bypass eden Windows uygulaması. Discord, Twitter, Instagram ve diğer engellenmiş servislere VPN kullanmadan erişim sağlar.

## Windows SmartScreen Uyarısı

İlk açışta "Windows kişisel bilgisayarınızı korudu" uyarısı gelebilir. Bu uygulama henüz Microsoft veritabanında yeterince yer almadığı için normal bir Windows davranışıdır.

**Çalıştırmak için:**
1. **Ek bilgi** linkine tıkla
2. **Yine de çalıştır** butonuna bas

Tool yeterince indirildikçe veya kalıcı code signing certificate alındığında bu uyarı kaybolacaktır.

## Bu Sürümde Neler Var

### 🚇 Selective WARP Tunnel
Discord Desktop gibi sadece DPI bypass ile çözülemeyen uygulamalar için Cloudflare WARP üzerinden **seçici tünel**. Sadece engellenmiş sitelerin trafiği tünelden gider, oyunlar ve diğer uygulamalar normal hızda çalışmaya devam eder.

- Discord Desktop **~30 saniyede** açılır
- Steam, online oyunlar ve diğer trafik etkilenmez
- Ping yükselmez, indirme hızı düşmez

### 🎨 Modern Kullanıcı Arayüzü
- **System Tray desteği** — Pencere kapatıldığında sistem tepsisinde çalışmaya devam eder
- **Akıllı kapatma davranışı** — İlk kapatmada sorulur, tercih hatırlanır
- **Tray menüsünden kontrol** — Pencereyi açmadan Start/Stop yapabilirsin
- **Uygulama ikonu** — Pencere, taskbar ve sistem tepsisinde özel logo

### 🛡️ Çift Katman Koruma
1. **DPI Bypass Layer** — System DNS override + Fake TLS injection + TCP fragmentation (web siteleri için)
2. **Tunnel Layer** — Cloudflare WARP üzerinden selective routing (uygulamalar için)

İki katman birlikte çalışır ve birbirine müdahale etmez.

### 🔧 Özelleştirilebilir Domain Listesi
`resources/blacklist-tr.txt` dosyasını düzenleyerek istediğin domain'leri korumaya alabilirsin. Eklediğin her domain için:
- DPI bypass uygulanır (web siteleri için)
- Tunnel routing aktif olur (ISP içeriği göremez)

## Kurulum

1. Aşağıdaki **Assets** kısmından `AendirDPI-v3.1-win64.zip` dosyasını indir
2. Bir klasöre çıkar (örnek: `C:\AendirDPI\`)
3. `AendirDPI.exe` dosyasına **sağ tık → Yönetici olarak çalıştır**
4. **Start Protection** butonuna bas
5. Discord, Twitter veya başka engellenmiş bir siteyi/uygulamayı aç

İlk başlatmada Cloudflare WARP'a anonim hesap oluşturulur. Sonraki çalıştırmalarda bu hesap otomatik kullanılır.

## Sistem Gereksinimleri

- Windows 10/11 (64-bit)
- Yönetici yetkisi (paket yakalama için zorunlu)

## Test Edilmiş Ortam

| Özellik | Durum |
|---------|-------|
| Türksat ISP | ✅ Sorunsuz çalışıyor |
| Discord Web | ✅ Anında açılır |
| Discord Desktop | ✅ ~30 saniyede açılır |
| Steam (oyun trafiği) | ✅ Etkilenmez |
| Online oyun ping'leri | ✅ Etkilenmez |
| YouTube, Google, vb. | ✅ Etkilenmez |

Diğer Türkiye ISP'leri (TTNet, Superonline, Vodafone) ve farklı sosyal medya platformları test edilmedi. Test eden kullanıcılardan geri bildirim bekliyoruz.

## Domain Listesi Düzenleme

İstediğin sitenin trafiğini ISP'den gizlemek için:

1. AendirDPI klasöründe `resources/blacklist-tr.txt` dosyasını aç (Notepad ile)
2. Her satıra bir domain ekle:
`discord.com
discord.gg
facebook.com
twitter.com
instagram.com`
3. Kaydet
4. AendirDPI'yi Stop → Start yap
5. Eklediğin domain'lerin trafiği artık tünelden gider

**Önemli:** Eklediğin her domain için trafik Cloudflare WARP üzerinden gider. Çok fazla domain eklemek (1000+) routing tablosunu şişirebilir.

## Bilinen Durumlar

### Antivirüs Uyarısı
Bazı antivirüs yazılımları (Kaspersky, Avast, Windows Defender) WinDivert ve WireGuard sürücülerini "RiskTool" veya "PUA" olarak işaretleyebilir. Bu **false positive** uyarısıdır — bu sürücüler birçok yasal araçta (Wireshark, GoodbyeDPI gibi) kullanılır.

**Çözüm:** Antivirüs ayarlarından AendirDPI klasörü için istisna ekleyin.

### Discord Desktop İlk Açılış
İlk Discord Desktop açılışında 30-60 saniye sürebilir (Cloudflare WARP tünel handshake'i). Sonraki açılışlar daha hızlıdır.

### Otomatik TTL Tespiti
İlk başlatmada **Re-detect optimal TTL** butonuna basarak ağına özel optimum TTL değerini bulabilirsin. Türksat için varsayılan TTL=4 çoğu durumda yeterlidir.

## Sorun Giderme

**Discord Desktop açılmıyor:**
1. Stop Protection → Start Protection (tüneli yeniden kur)
2. Discord'u tamamen kapat (Task Manager'dan tüm Discord process'lerini sonlandır)
3. AendirDPI'yi yeniden başlat
4. Discord'u tekrar aç

**Steam yavaş veya ping yüksek:**
- Steam IP'leri tünelden geçmemeli. Eğer geçiyorsa bu bir bug — Issues sekmesinden bildirin
- Geçici çözüm: Tray menüden Stop Protection

**Antivirüs dosyaları siliyor:**
- Antivirüsü geçici olarak duraklat (15 dakika)
- AendirDPI klasörü için kalıcı istisna ekle
- AendirDPI'yi yeniden çalıştır

**Eklediğim domain çalışmıyor:**
- Stop → Start yap, DNS cache yenilensin
- Domain'in spelling'ini kontrol et (boşluk, yanlış karakter olmamalı)
- 1-2 dakika bekle, DomainResolver yeni IP'leri çözmesi zaman alır

## Nasıl Çalışır

Tool çalıştığında iki katman aktif olur:

**Layer 1 — DPI Bypass:**
- System DNS Cloudflare 1.1.1.1'e override edilir
- Blacklist'teki domain'lere giden TLS ClientHello'ları yakalanır
- Sahte TLS paketi (decoy SNI) ISP'ye gönderilir, ISP "zararsız trafik" sanır
- Gerçek paket TCP fragmentation ile parçalanır, ISP DPI'ı şaşırır

**Layer 2 — Selective WARP Tunnel:**
- Cloudflare WARP'a anonim WireGuard hesabı oluşturulur
- Blacklist domain'leri DNS resolve edilir, IP'ler alınır
- Bu IP'ler için Windows routing tablosuna kural eklenir → tünel adapter'a yönlendir
- Sadece bu IP'lere giden trafik şifreli WireGuard tünelinden geçer
- ISP sadece şifreli UDP paketleri görür, içeriği bilemez

Diğer tüm trafik (Steam, YouTube, oyunlar) doğrudan ISP'den geçer, hız ve ping etkilenmez.

## Teknik Detaylar

- **DPI Bypass:** WinDivert ile paket yakalama, Fake TLS ClientHello injection, TCP fragmentation
- **Tunnel:** wireguard-nt (Microsoft) + Cloudflare WARP API
- **DNS:** System-wide override → Cloudflare 1.1.1.1
- **Routing:** Win32 IP Helper API ile selective IPv4/IPv6 routing
- **Stack:** C++23, ImGui, MSVC

## Sürüm Geçmişi

### v3.1 (İlk public release)
- DPI bypass layer (System DNS + Fake TLS + TCP Fragment)
- Selective WARP Tunnel ile Discord Desktop ~30 saniye açılış
- System tray entegrasyonu
- Akıllı close button (Hide/Quit dialog)
- Otomatik TTL tespiti
- Cloudflare WARP üzerinden anonim selective routing
- Özelleştirilebilir domain listesi

## Sorumluluk Reddi

Bu uygulama eğitim ve kişisel kullanım amaçlıdır. Kullanıcı kendi yerel yasalarından sorumludur. Tool'un kullanımından doğacak hiçbir durumdan geliştirici sorumlu tutulamaz.

## Geri Bildirim

Sorun yaşıyorsanız, başka bir ISP'de test ettiyseniz veya öneri varsa **Issues** sekmesinden bildirin.

<div align="center">

[![Website](https://img.shields.io/badge/Website-aendirr.com-0e75b6?style=for-the-badge&logo=google-chrome&logoColor=white)](https://aendirr.com)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/aendirr)
[![Discord](https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/aendirr)
[![Telegram](https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/aendirr)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:iletisimaendirr@gmail.com)

</div>
