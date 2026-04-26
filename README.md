# AendirDPI

Türkiye'deki ISP'lerin (özellikle Türksat) Discord ve benzeri servislere uyguladığı DPI engellemesini bypass eden Windows tool'u.

## İndir

[**Buradan en son sürümü indir →**](../../releases/latest)

## Kurulum

1. ZIP dosyasını indir
2. Bir klasöre çıkar (örn: `C:\AendirDPI\`)
3. `AendirDPI.exe`'ye sağ tıkla → **Yönetici olarak çalıştır**
4. **Start Protection** butonuna bas
5. Discord'u aç

## Özellikler

- Sistem DNS override (ISP'nin DNS hijack'ini engeller)
- Fake TLS injection (SNI inspection bypass'ı)
- TLS fragmentation
- Otomatik TTL tespiti (ağına özel optimum değer)

## Test Edilen ISP'ler

- ✅ Türksat
- ⚠️ Diğer Türkiye ISP'leri (test edilmedi, manuel TTL ayarı gerekebilir)

## Sistem Gereksinimleri

- Windows 10/11 (64-bit)
- Yönetici yetkisi

## Sorun Giderme

### Antivirüs uyarı veriyor
WinDivert sürücüsü bazı antivirüsler tarafından şüpheli olarak işaretlenir. `AendirDPI.exe` için antivirüs ayarlarından istisna ekle.

### Discord Desktop yavaş açılıyor
İlk açılış 1-2 dakika sürebilir. Hızlı erişim için Discord Web kullan: [discord.com/app](https://discord.com/app)

### TTL otomatik tespit çalışmadı
Manuel TTL'i deneyerek ağına uygun değeri bul: 3, 4, 5

## Notlar

- Bu tool eğitim ve kişisel kullanım amaçlıdır
- Güvenliğin için ZIP'i kendin indir, başka kaynaklardan inen exe'lere güvenme
