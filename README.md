# VetCare365 🐾

## 📌 Proje Tanımı
**VetCare365**, Microsoft Dynamics 365 Finance and Operations (D365 F&O) geliştirme ortamında geliştirilmiş kapsamlı bir veteriner klinik yönetim modülüdür.  
Projenin amacı, veteriner kliniklerinde gerçekleştirilen tüm operasyonları **tek bir modül üzerinden takip edilebilir** hale getirmektir.  

Bu kapsamda:  
- 🐾 **Hayvan kayıtları** tutulur (isim, sahip bilgisi, tür, ırk, cinsiyet).  
- 🧬 **Tür ve ırk yönetimi** yapılır (örn. Kedi → Van Kedisi, Köpek → Golden Retriever).  
- 👤 **Müşteri–hayvan ilişkisi** kurulur.  
- 🩺 **Muayene ve ziyaret kayıtları** saklanır (başlangıç/bitiş zamanı, müşteri, hayvan).  

VetCare365; veri tutarlılığı, kullanıcı dostu formlar, raporlama desteği ve rol bazlı güvenlik ile kliniklerin daha verimli çalışmasını sağlar.  

---

## 🎯 Amaçlar
- Klinik verilerinin düzenli saklanması  
- Lookup & Display Method ile hızlı veri seçimi  
- Validasyon metotlarıyla hatasız veri girişi  
- Raporlama ile geçmiş muayene takibi  
- Güvenlik rolleriyle yetkilendirme  

---

## 🗂️ Veri Modeli
### 📑 Tablolar
- **VetSpecies** → Hayvan türleri  
- **VetBreed** → Hayvan ırkları  
- **VetPet** → Evcil hayvan kayıtları  
- **VetVisitTransactions** → Muayene ve ziyaret kayıtları  

### 🔗 İlişkiler
- `VetBreed.VetSpeciesId → VetSpecies.VetSpeciesId`  
- `VetPet.VetSpeciesId → VetSpecies.VetSpeciesId`  
- `VetPet.VetBreedId → VetBreed.VetBreedId`  
- `VetVisitTransactions.VetSpeciesId → VetSpecies.VetSpeciesId`  
- `VetVisitTransactions.VetBreedId → VetBreed.VetBreedId`  

---

## 💻 Formlar
- **SpeciesForm** → Hayvan Türleri  
- **BreedForm** → Hayvan Irkları  
- **PetForm** → Evcil Hayvanlar  
- **VisitForm** → Muayene Ziyaretleri  

Formlarda:  
- Lookup tanımları ile kolay seçim  
- Display Method ile ID yerine isim gösterimi  
- Zorunlu alan kontrolleri (`validateField`, `validateWrite`)  

---

## ⚙️ Uygulanan X++ Metotları
- `validateDelete()` → İlişkili kaydı olan hayvanların silinmesini engeller  
- `initValue()` → Yeni kayıt açıldığında tarih ataması  
- `modifiedField()` → Tür değişince ırk alanını temizler  
- `write()` → Kayıt kaydedilmeden loglama & bilgilendirme  

---

## 📊 Raporlama (SSRS)
- **VisitHistoryReport**  
  - Alanlar: Ziyaret Tarihi, Müşteri Adı, Evcil Hayvan Adı  
  - Menüye eklenen Report Item üzerinden erişilir.  

---

## 🔐 Güvenlik & Roller
- **VetCare365UserRole** tanımlandı.  
- Bu rol → PetForm, VisitForm, BreedForm, VisitHistoryReport erişimine sahip.  
- Yetkisiz kullanıcılar erişemez.  

---

## 🚀 Kurulum & Kullanım

### 1️⃣ Ön Gereksinimler
- Microsoft D365 F&O Sanal Makinesi (Bunun nedeni lisanslı bir ürün olmasıdır.)
- Visual Studio (Dynamics 365 Extension yüklü)  
- SQL Server & SSRS  
- GitHub erişimi  

---

### 2️⃣ Sanal Makine Üzerinde Hazırlık
1. Microsoft’un sağladığı eğitim laboratuvarına giriş yapın:  
   👉 [Customize in Visual Studio exercise](https://learn.microsoft.com/tr-tr/training/modules/customize-visual-studio-finance-operations/7-exercise)  
2. Sanal makinede oturum açın.  
3. Tarayıcı üzerinden GitHub’a girin ve bu repository içindeki **VeterinerKlinigi** klasörüne gidin.  
4. Buradan şu dosyaları seçip indirin:  
   - `VeterinerDemo-Gulbahar.axmodel`  
   - `VeterinerKlinigi.zip`  


---

### 3️⃣ Model Import İşlemi
İndirilen proje dosyasını modele alarak yapılan işlemleri görmek için komut istemi üzerinde verilenleri sırası ile çalıştırmanız gereklidir:

Not: mkdir işleminden sonra oluşturulan Export dosyası içine indirilern axmodel dosyası atılmalıdır.
```powershell
mkdir C:\Export 
cd "C:\AOSService\PackagesLocalDirectory\bin"

.\ModelUtil.exe -import -metadatastorepath="C:\AOSService\PackagesLocalDirectory" -file="C:\Export\VeterinerDemo-Gulbahar.axmodel"
