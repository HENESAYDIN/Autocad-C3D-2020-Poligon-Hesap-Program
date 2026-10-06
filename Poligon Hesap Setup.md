# Birleşik Poligon Hesap Setup teslim notu

`dist/PoligonHesap_Setup.exe` tek dosya olarak üretildi. İçinde önceki kurulumdaki 2020–2024 legacy bundle ve AutoCAD 2025/2026 için ayrı net8/net10 bundle'ları bulunur (toplam 5 bundle). Orijinal `PoligonHesap.AutoCAD2020.dll` ve legacy `PoligonHesap.Core.dll` byte bazında korundu. Legacy manifest yalnız AutoCAD 2020–2024 (`R23.1`–`R24.3`) aralığına sınırlandı; legacy DLL 2025/2026 hostunda yüklenmez.

Kurucu, AutoCAD 2025 BuildVer `V.188` ve altında net8'i; `V.189` ve üstünde net10'u seçer. AutoCAD 2026 için `W.178` ve altında net8; `W.179` ve üstünde net10 seçilir. Bilinmeyen BuildVer yapıları fail-closed davranır. AutoCAD 2025 için oluşturulan iki DLL, AutoCAD 2025 managed API referansları olmadığı için aynı runtime'a ait AutoCAD 2026 API referans klasörleriyle derlendi. Bu yalnızca derleme doğrulamasıdır; 2025 API ikili uyumluluğu/çalışma garantisi değildir. AutoCAD 2025 hostunda kabul testi gerekir.

Kurucu eski kurucu gibi yönetici izni istemez; `%APPDATA%\\Autodesk\\ApplicationPlugins` altına ve aynı `HKCU` `PoligonHesap` uninstall kaydına kurulur. 2020–2024 ve 2025/2026 aynı makinede ise algılanan sürümlere karşılık gelen paketleri birlikte kurar. Civil 3D 2025/2026 bu özel Setup kapsamına dahil değildir.

Doğrulamalar: 73 çekirdek assertion, 15 tam-matris runtime/profile assertion, .NET 8 ve .NET 10 CP1254 round-trip, 2025/2026 BuildVer runtime seçim testleri, bundle staging ve NSIS Setup üretimi geçti. Setup payload'i 7-Zip ile açılarak tüm beş bundle'ın manifest/DLL varlığı doğrulandı; orijinal iki legacy DLL'in SHA-256 değerleri kaynak bundle dosyalarıyla eşleşti.

Dört AutoCAD 2025/2026 plugin DLL'i sağlanan 2026 API referanslarıyla derlendi. Gerçek Windows AutoCAD hostu bulunmadığı için 2020–2024/2025/2026 içinde yükleme, `POLHESAP`, Türkçe Rasat, hesap, rapor ve DXF kabul testleri yapılamadı; dijital imza eklenmedi. Son kullanıcıya yaymadan önce gerçek AutoCAD 2025/2026 hostlarında test edilmelidir.

Yeniden üretim: [`Build-AutoCAD2020-2026.ps1`](Build-AutoCAD2020-2026.ps1) ve [`installer/combined2020-2026/README.md`](installer/combined2020-2026/README.md).
