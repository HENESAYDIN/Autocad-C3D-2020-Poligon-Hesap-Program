# Birleşik AutoCAD 2020–2024 + 2025/2026 Setup.exe

Tek kurucu eski `PoligonHesap.bundle`'ı ve AutoCAD 2025/2026 için net8/net10 ayrı bundle'larını içerir. Eski `PoligonHesap.AutoCAD2020.dll` ve Core DLL byte bazında korunmuştur; legacy manifest yalnız 2020–2024 (`R23.1`–`R24.3`) serilerine açıktır.

AutoCAD 2025 için net8 ve net10 hedefli iki ayrı DLL derlenir. **2025 API referansları verilmediğinden bu iki DLL, aynı runtime'a ait 2026 AutoCAD API DLL'leriyle derlenmiştir.** Bu, .NET runtime hedefini eşler ama AutoCAD 2025 API ikili uyumluluğunu garanti etmez; 2025'te host testi şarttır. Kurucu AutoCAD 2025 Update 1.3 ve öncesinde net8 (`V.188` ve altı), Update 1.4 ve sonrasında net10 (`V.189+`) seçer. Bilinmeyen BuildVer için modern DLL kurmaz.

AutoCAD 2026 için net8 Update 1.1 ve öncesinde (`W.178` ve altı); net10 Update 1.2 ve sonrasında (`W.179+`) seçilir. 2020–2024 ve 2025/2026 aynı bilgisayardaysa uygun legacy ve modern bundle'lar birlikte kurulur. 2025 profilleri `AutoCAD`, 2026 profilleri de `AutoCAD` platformundadır; Civil 3D 2025/2026 bu özel kurucuya dahil değildir.

Yeni kurucu eski kurucu gibi yönetici yetkisi istemez; `%APPDATA%\Autodesk\ApplicationPlugins` kullanıcı klasörüne kurar ve aynı `HKCU` `PoligonHesap` kaldırma kaydını kullanır. Uninstaller legacy ve tüm modern profile paketlerini kaldırır.

## Yeniden build

Windows'ta .NET SDK 8 ve 10, Python 3, NSIS ve karşılık gelen AutoCAD 2026 managed API referans klasörleri gerekir:

```powershell
./Build-AutoCAD2020-2026.ps1 -ManagedApiPathNet8 "C:\Program Files\Autodesk\AutoCAD 2026" -ManagedApiPathNet10 "C:\Program Files\Autodesk\AutoCAD 2026"
```

Bu referans yolları hem AutoCAD 2026 profil DLL'lerinde hem AutoCAD 2025 için üretilen aynı-runtime profil DLL'lerinde kullanılır. Update/runtime API referans klasörleri ayrıyssa parametreleri ayrı verin. API dosyaları (`accoremgd.dll`, `acdbmgd.dll`, `acmgd.dll`, `AdWindows.dll`) Setup içerisine kopyalanmaz. Eski bundle'ın manifesti build sırasında R23.1–R24.3 aralığı için kontrol edilir.

Bu sandbox'ta AutoCAD 2025/2026 hedefli dört modern DLL derlenebilir ve NSIS Setup.exe üretilebilir. Gerçek AutoCAD 2025 ve 2026 hostlarında yükleme, `POLHESAP`, Türkçe CP1254, hesap, rapor ve DXF kabul testleri yapılamadı; özellikle 2025 için 2026 API referanslarıyla derleme kullanıldığı için 2025 host uyumluluğunu varsaymayın.

## Runtime kaynakları

- [Autodesk Support: AutoCAD ve .NET 10 geçişinden etkilenen ürünler](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/AutoCAD-and-Toolsets-Requirements-for-products-affected-by-the-Microsoft-NET-10-transition.html)
- [Autodesk Developer Blog: 2025/2026 ürünlerinde .NET 10 güncellemeleri](https://blog.autodesk.io/autodesk-desktop-products-2025-2026-net-10-updates/)
