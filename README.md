# 🧠 CIFAR-10 Veri Seti ile CNN Tabanlı Görüntü Sınıflandırma ve Web Uygulaması

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![Gradio](https://img.shields.io/badge/Gradio-Web_App-F97316?style=flat)

## 📌 Proje Özeti
Bu proje, **CIFAR-10** açık kaynaklı görüntü veri seti kullanılarak sıfırdan eğitilen bir **Evrişimli Sinir Ağı (CNN)** modeli ile 10 farklı nesne sınıfını (`Uçak, Araba, Kuş, Kedi, Geyik, Köpek, Kurbağa, At, Gemi, Kamyon`) sınıflandırmayı ve eğitilen modeli **Gradio** aracılığıyla etkileşimli bir web uygulamasına dönüştürmeyi amaçlamaktadır.

---

## 🛠️ Model Mimarisi ve Hiperparametreler

| Parametre / Bileşen | Değer / Açıklama |
| :--- | :--- |
| **Görev (Task)** | Çok Sınıflı Görüntü Sınıflandırma (10 Sınıf) |
| **Veri Seti** | CIFAR-10 (40.000 Train, 10.000 Validation, 10.000 Test) |
| **Girdi Boyutu** | $32 \times 32 \times 3$ (RGB Renkli Görüntü) |
| **Mimari Yapı** | 3 Bloklu Özel CNN (`Conv2d + ReLU + MaxPool2d` $\rightarrow$ `Flatten` $\rightarrow$ `Linear + Dropout`) |
| **Aktivasyon / Çıkış** | Gizli Katmanlar: `ReLU` \| Çıkış Katmanı: `Softmax` |
| **Kayıp Fonksiyonu** | Cross-Entropy Loss |
| **Optimizer** | Adam (`lr = 0.001`) |
| **Batch Size / Epoch** | `64` / `15` (Dropout: `0.3`) |
| **Eğitim Ortamı** | Google Colab (GPU / CUDA) |

---

## 📊 Test Seti Performans Metrikleri

Model 15 epoch boyunca eğitilmiş ve hiç görmediği 10.000 görselden oluşan CIFAR-10 test seti üzerinde aşağıdaki sonuçları elde etmiştir:

* **Toplam Eğitim Süresi:** `222.02 saniye`
* **Test Accuracy:** `%75.54`
* **Macro Precision:** `0.7544`
* **Macro Recall:** `0.7554`
* **Macro F1-Score:** `0.7542`
* **Ortalama Tahmin Süresi (Inference Time):** `0.2242 ms / görüntü`

---

## 📱 Gerçek Dünya Görüntüleri ile Test ve Değerlendirme

Eğitilen modelin gerçek dünya koşullarındaki genelleştirme yeteneğini ölçmek amacıyla, telefon kamerasıyla çekilen 5 farklı gerçek ortam fotoğrafı Gradio web arayüzü üzerinden modele verilmiştir:

| Test No | Gerçek Sınıf | Model Tahmini | Güven Oranı ve Dağılım | Sonuç |
| :---: | :---: | :---: | :--- | :---: |
| **1** | Köpek | **Köpek** | **%58 Köpek**, %40 Kedi, %2 Geyik | ✅ Başarılı |
| **2** | Araba | **Gemi** | **%95 Gemi**, %5 Kamyon, %0 Uçak | ❌ Başarısız |
| **3** | Kedi | **At** | **%57 At**, %29 Uçak, %12 Kedi | ❌ Başarısız |
| **4** | Kuş | **Kuş** | **%66 Kuş**, %34 Uçak, %0 Geyik | ✅ Başarılı |
| **5** | Kamyon | **Kamyon** | **%100 Kamyon**, %0 Araba, %0 Gemi | ✅ Başarılı |

### 💡 Başarılı ve Başarısız Durumların Teknik Analizi
* **Başarılı Tahminler (Kamyon %100, Kuş %66, Köpek %58):** Kamyon görseli doğrudan ön cepheden ve kadrajı dolduracak şekilde çekildiği için model büyük araç hatlarını %100 güvenle sınıflandırmıştır. Kuş ve köpek görsellerinde ana nesneler belirgin olduğu için doğru sınıfa atanmış, ancak morfolojik benzerlikler nedeniyle kuşta %34 uçağa, köpekte %40 kediye olasılık payı verilmiştir.
* **Başarısız Tahminler (Araba $\rightarrow$ Gemi, Kedi $\rightarrow$ At):**
  * *Bakış Açısı ve Arka Plan Gürültüsü:* Araba fotoğrafı balkondan yüksek (kuşbakışı) açıyla ve yoğun bina/bitki arka planıyla çekilmiştir. Eğitim verisindeki arabalar yer hizasından çekildiği için model yüksek açılı gri tavan yapısını gemi silüetiyle karıştırmıştır.
  * *Duruş (Poz) Farklılığı ve Çözünürlük Kaybı:* Kedi koltuk üzerinde kıvrılmış pozisyonda çekildiği ve görsel $32 \times 32$ piksele küçültüldüğünde ince yüz detayları kaybolduğu için model kıvrık gövde formunu "At" sınıfı olarak yorumlamıştır.

---

## 🚀 Uygulama (Deployment) ve Çalıştırma
Model **Gradio** web arayüzü ile canlıya alınmıştır. Depodaki `.ipynb` dosyasını Google Colab üzerinde çalıştırarak canlı arayüz üzerinden yeni görüntülerle test gerçekleştirebilirsiniz.
