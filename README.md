<div align="center">

# 🕵️‍♂️ WinPersistHunter

![C++](https://img.shields.io/badge/C++-17%2B-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![DFIR](https://img.shields.io/badge/Tool-DFIR-8B0000?style=for-the-badge)

**Windows Kalıcılık (Persistence) Noktaları İçin Gelişmiş Statik Analiz ve Veri Besleme Aracı**

</div>

---

## 📌 Genel Bakış

**WinPersistHunter**, Windows işletim sistemlerindeki kalıcılık (persistence) mekanizmalarını hızlıca taramak için C++ ile geliştirilmiş bir Blue Team / DFIR aracıdır. Gelişmiş APT grupları ve zararlı yazılımlar tarafından kullanılan kritik Registry anahtarlarını, sistem alt bileşenlerini ve başlangıç yollarını derinlemesine tarar, gürültüyü filtreler ve adli analize hazır hale getirir.

> 🎯 **MITRE ATT&CK Hedef Matrisi:** Bu araç, Windows sistemlerindeki **T1547** (Boot or Logon Autostart Execution), **T1037** (Boot or Logon Initialization Scripts) ve **T1053** (Scheduled Task/Job) tekniklerini proaktif olarak tespit etmek amacıyla tasarlanmıştır.

## 🛠️ Teknik Özellikler

| Özellik | Analizdeki Rolü ve Açıklaması |
| :--- | :--- |
| ⚡ **Zero-Dependency** | Sadece yerel Windows API'leri kullanır. Harici kütüphane içermez; canlı sistem bütünlüğünü korur. |
| 🧹 **Noise Reduction** | Silinmiş (Dead-link), geçerli imzaya sahip ve meşru Safe-OS dosyalarını eler, analisti yormaz. |
| 🔒 **Data Loss Prevention** | Dizin altında eski analiz verisi (`p.txt`) varsa üzerine yazılmasını önlemek için interaktif uyarı mekanizması barındırır. |
| 🤖 **Oto-Entegrasyon** | Logları optimize eder ve analiz için otomatik olarak **PathsParser** aracına besler. |
| 🛡️ **UAC Desteği** | Dahili manifest yapısı sayesinde kısıtlı erişim hatalarını önlemek için otomatik olarak Admin yetkisiyle çalışır. |

## ⚙️ Kurulum ve Derleme

Projeyi derlemek için sisteminizde **MinGW (GCC/g++)** kurulu olmalıdır.

```cmd
git clone https://github.com/KULLANICI_ADIN/WinPersistHunter.git
cd WinPersistHunter
build.bat
```

> 💡 **Not:** Derleme tamamlandığında dizinde otomatik olarak yönetici kalkanına (UAC Admin) sahip `Parser.exe` dosyanız oluşacaktır.

## 💻 Operasyon Akışı

Aracı (`Parser.exe`) çalıştırdığınızda şu adımlar sırasıyla gerçekleşir:

1. **Veri Güvenliği Kontrolü:** Çalışma dizininde `p.txt` kontrol edilir, varsa üzerine yazılmaması için analist onayına sunulur.
2. **Derin Tarama:** Registry (Run, Winlogon, Services, COM, LSA, GPO vb.) ve harici tüm başlangıç yolları (7 ana modül) taranır.
3. **Filtreleme & Ayıklama:** Kullanıcı tercihine göre güvenilir sistem dosyaları ve sahte girdiler gürültüyü azaltmak için filtrelenir.
4. **Dışa Aktarım:** Şüpheli ve doğrulanması gereken veriler bulunduğunuz dizine `p.txt` formatında kaydedilir.
5. **Analiz Entegrasyonu:** Onay verilmesi durumunda `PathsParser.exe` yerel olarak kontrol edilir; yoksa otomatik indirilir ve analiz başlatılır.

## 🖥️ Terminal Önizlemesi

```text
[+] WinPersistHunter v1.0 başlatılıyor...
[*] Yetki Kontrolü: NT AUTHORITY\SYSTEM (Administrator) OK.

[!] UYARI: Klasörde zaten 'p.txt' dosyası bulunuyor!
[!] İşleme devam ederseniz eski analiz verileri otomatik olarak silinecektir.
[?] Eski verinin silinmesini onaylıyor musunuz? (y/n): y

[*] Modül 1 taranıyor... %APPDATA%\...\Startup [1 şüpheli LNK bulundu]
[*] Modül 2 taranıyor... HKLM\...\Run [Temiz]
[*] Modül 7 taranıyor... .NET Startup Hooks [Temiz]
[+] Tarama tamamlandı. Yeni sonuçlar 'p.txt' dosyasına yazıldı.

[?] PathsParser ile kombine edilsin mi? (y/n): y
[+] 'pathsparser.exe' klasörde zaten mevcut. İndirme adımı atlanıyor.
[*] PathsParser otomatik olarak başlatılıyor...
```

---

<div align="center">
  <i>Siber güvenlik uzmanları ve adli bilişim analistleri için geliştirilmiştir.</i>
</div>
