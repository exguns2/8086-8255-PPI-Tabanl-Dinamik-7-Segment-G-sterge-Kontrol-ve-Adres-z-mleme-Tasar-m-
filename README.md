# 8086 & 8255 PPI Tabanlı Dinamik 7-Segment Gösterge Kontrolü ve Adres Çözümleme Tasarımı

Bu proje; Intel 8086 mikroişlemcili bir sistemde **8255 PPI (Parallel Programmable Interface)** arayüz entegresi kullanılarak harici buton girişlerinin okunmasını ve 4 adet ortak anotlu 7-parçalı göstergenin (7-segment display) dinamik tarama (multiplexing) yöntemiyle sürülmesini içeren donanım-yazılım bütünleşik tasarımıdır.

---

## 🛠️ Donanım Bileşenleri ve Simülasyon Ortamı

- **Mikroişlemci:** Intel 8086
- **Arayüz Entegresi:** Intel 8255 PPI
- **Adres Çözümleme Çipi:** 74LS137 Dekoder (Kod Çözücü)
- **Mantık Kapıları:** U3 (AND) ve U5 (OR) kapıları
- **Gösterge Birimi:** 4 Adet Ortak Anotlu 7-Segment Display
- **Giriş Birimi:** Lojik 0 üreten 3 adet buton (Seçim, Arttırma, Azaltma)
- **Simülasyon Yazılımı:** Proteus

---

## 📐 Donanım Mimarisi ve Adres Çözümleme Matematiği

Projede, tasarım isterleri doğrultusunda belirlenen taban adresi yakalamak ve portları haritalandırmak için özel bir donanımsal adres çözümleme yapısı kurulmuştur.

### 1. Adres Hesaplaması
Belirlenen tasarım kriterlerine göre 8255 PPI entegresinin başlangıç adresi şu şekilde hesaplanmıştır:
- **Taban Adres:** `17E8H`

### 2. Port Haritalama (Port Addresses)
8086 sistemindeki **ardışık çift adres kuralı (even-address rule)** gereği, işlemcinin `A1` ve `A2` hatları 8255'in `A0` ve `A1` girişlerine bağlanarak port seçimi yapılmıştır. Bu doğrultuda oluşan port adresleri şu şekildedir:
- **Port A (Buton Girişleri):** `17E8H`
- **Port B (7-Segment Veri Hattı - a-h):** `17EAH`
- **Port C (Gösterge Seçim / Tarama Hattı):** `17ECH`
- **Kontrol Yazmacı (Control Register):** `17EEH`

### 3. Kod Çözücü Mantığı
`17E8H` taban adresini hatasız yakalamak amacıyla, `ADR[0..19]` adres yolundan gelen sinyaller **74LS137 dekoderi**, AND ve OR kapı kombinasyonları ile çözülerek 8255'in Entegre Seçme (`CS8255`) ucuna aktarılmıştır. Sistem sadece harici bellek/giriş-çıkış ayrımı için `$M/\overline{IO}$` terminalini de lojik olarak hesaba katmaktadır.

---

## 💻 Çalışma Mantığı ve Yazılım Algoritması

Sistem, donanım birimleri ile yazılım algoritmalarının dinamik tarama yöntemiyle senkronize çalışması esasına dayanır:

1. **Buton Kontrolü (Giriş):** Port A uçlarına (`PA0`, `PA2`, `PA3`) bağlı butonlar lojik 0 durumuna göre taranır. 
   - `GOSTERGE_secim` butonu aktif olan göstergeyi belirler.
   - `ARTTIRMA` ve `AZALTMA` butonları o göstergedeki sayısal değeri 0-9 arasında döngüsel değiştirir.
2. **Dinamik Tarama (Multiplexing):** Port B üzerinden segment verileri basılırken, Port C (`PC0`-`PC3`) üzerinden 4 adet ortak anotlu gösterge sırasıyla çok hızlı bir şekilde aktif edilir. Bu sayede insan gözü tüm göstergeleri aynı anda kararlı bir şekilde açık görür.

---

## 📊 Simülasyon ve Doğrulama Sonuçları

- **Başlangıç Durumu:** Kod ilk çalıştırıldığında tüm göstergeler `0` değerini gösterecek şekilde konfigüre edilmiştir.
- **Doğrulama:** Butonlara ardışık olarak basıldığında, 1., 2., 3. ve 4. göstergelerin sırasıyla `"1, 2, 3, 4"` değerlerine ulaştığı Proteus simülasyon ortamında dinamik olarak teyit edilmiştir.

---
*Bu proje Yıldız Teknik Üniversitesi Elektrik-Elektronik Fakültesi Elektronik ve Haberleşme Mühendisliği Bölümü EHM-3141 Mikroişlemci Sistemleri Proje Ödevi kapsamında geliştirilmiştir.*
