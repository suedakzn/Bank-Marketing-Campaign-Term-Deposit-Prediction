# Bank-Marketing-Campaign-Term-Deposit-Prediction

Bu projede, bir bankanın pazarlama kampanyalarına verilen müşteri tepkilerini analiz ederek,  
hangi müşterilerin vadeli mevduat teklifini kabul etme olasılığı olduğunu tahmin etmeye çalıştım.  
Kullanılan veri seti, bankanın telefon kampanyaları sonucunda oluşan müşteri kayıtlarını içermektedir.

## 📌 Proje Amacı

Bankanın kampanya sürecini daha verimli hale getirmek için;
- Müşteri davranışlarını analiz etmek
- Kimlerin "evet" deme olasılığının yüksek olduğunu belirlemek
- Böylece daha az kaynakla daha yüksek dönüşüm sağlamak

---

## 📁 Kullanılan Veri Seti

- **Kaynak**: UCI Bank Marketing Dataset  
- **Satır Sayısı**: 11,162  
- **Sütun Sayısı**: 17  
- **Hedef Değişken**: `deposit` (yes/no)
- Link: https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset

---
## ⚙️ Kullanılan Teknikler

### 📊 Veri Analizi (EDA - Exploratory Data Analysis)
- Veri setinin genel yapısını anlama
- Kategorik değişkenlerin dağılımlarını inceleme
- Sayısal değişkenlerin özet istatistikleri
- Korelasyon matrisi & heatmap
- Boxplot ile aykırı değer görselleştirme

### 🧹 Veri Ön İşleme
- Eksik veri kontrolü (`isnull`, `'unknown'` değerleri)
- Yüzdelik hesaplamalarla analiz
- `'unknown'` değerleri uygun şekilde temizleme veya bırakma
- Kategorik değişkenlerde one-hot encoding (dummy variable trap'e dikkat edilerek)
- Sayısal değişkenlerde outlier tespiti ve **winsorizing** uygulamaları

### 🧠 Feature Engineering
- Yeni değişken oluşturma:
  - `balance_log` (log transformation)
  - `balance_category` (düşük/orta/yüksek)
  - `campaign_efficiency` (başarıya göre oran)
  - `recent_contact` (pdays’e göre)
  - `is_retired_or_student` gibi roller bazlı kategoriler
- `pdays` gibi değişkenlerde binning (gruplama)
- Var olan değişkenleri yeniden anlamlandırma

### 🔢 Veri Dönüştürme
- Sayısal veriler için `StandardScaler` ile ölçeklendirme
- Kategorik veriler için `OneHotEncoder`/pandas `get_dummies` ile dönüşüm
- Eğitim/test ayrımı: `train_test_split` (stratify parametresi ile)

### 🤖 Modelleme (Supervised Learning)
- Logistic Regression
- Decision Tree
- K-Nearest Neighbors (KNN)
- Support Vector Machines (SVM - RBF)
- Random Forest
- XGBoost
- GridSearchCV ile XGBoost hiperparametre optimizasyonu
- Cross-validation ile model karşılaştırmaları

### 🔍 Model Performans Değerlendirmesi
- Confusion Matrix
- Accuracy, Precision, Recall, F1-score (macro average)
- Karşılaştırmalı barplot görselleştirmesi
- En iyi modelin seçimi ve yorumlanması

### ⚡ GPU Destekli Modelleme
- `tree_method="gpu_hist"` kullanılarak GPU üzerinde XGBoost eğitimi
- CPU vs GPU eğitim süresi karşılaştırması
- Eğitim süresinin zaman modülü ile ölçülmesi

### 🎯 Segmentasyon (Unsupervised Learning)
- K-Means Clustering
- Elbow yöntemi ile optimum küme sayısı belirleme
- PCA ile 2D görsel temsil
- Her küme için özet istatistik ve pazarlama önerileri üretme

### 📈 Görselleştirme
- Seaborn ve Matplotlib ile:
  - Countplot
  - Histplot
  - Boxplot
  - Heatmap
  - PCA scatterplot
  - Barplot ile model karşılaştırması

### 📝 Raporlama & Sunum
- Kaggle notebook içinde markdown ile açıklamalar
- Model yorumları, segment yorumları
- Proje özeti & geleceğe yönelik öneriler

---
## 🤖 Kullanılan Modeller

- Logistic Regression  
- Decision Tree  
- K-Nearest Neighbors (KNN)  
- Support Vector Machines (SVM – RBF)  
- Random Forest  
- XGBoost  
- XGBoost (Tuned – GridSearchCV ile optimize edildi)  
- XGBoost (GPU destekli versiyon)

---

## ✅ Neden XGBoost (Tuned) Modelini Seçtim?

Projede farklı algoritmaları karşılaştırdıktan sonra, en iyi performansı **XGBoost** modelinin verdiğini net bir şekilde gözlemledim.  
Ayrıca `GridSearchCV` ile hiperparametre optimizasyonu yaparak bu modeli daha da güçlendirdim.

### Seçim nedenlerim:

- **Doğruluk oranı en yüksek modeldi**: Accuracy %86 ile en güçlü sonucu verdi.  
- **Precision, recall ve F1-score değerleri dengeliydi**, yani sadece doğru tahmin yapmıyor, aynı zamanda tutarlı sonuçlar veriyordu.  
- **Overfitting eğilimi çok düşük**: Karar ağacı temelli olmasına rağmen regularization parametreleri sayesinde model çok daha dengeli çalıştı.  
- **GPU ile çok hızlı eğitilebiliyor**: `gpu_hist` ile eğitim süresi inanılmaz hızlandı (0.2 saniye civarı).  
- **Hiperparametre tuning sonrası performansı ciddi şekilde arttı**: `max_depth`, `learning_rate`, `subsample` gibi parametrelerle ince ayar yaptım.

> Diğer modellerin çoğu ya daha düşük doğruluk verdi ya da tek metrikte yüksek, diğerlerinde dengesizdi.  
> XGBoost ise hem teknik açıdan güçlü, hem de iş dünyasında yaygın kullanılan bir model olduğu için bu projeye en uygun seçenekti. 

---

## 📊 Segmentasyon & İş Önerileri

KMeans ile müşteri kümeleri belirlendi ve her küme için özel pazarlama stratejileri önerildi:
- Genç düşük bakiyeli müşteriler için mobil kampanyalar  
- Orta yaşlı ve uzun konuşanlar için rehber içerikler  
- VIP müşteriler için kişiselleştirilmiş yatırım danışmanlığı önerileri  
- Yaşlı gruplar için birebir iletişim stratejileri

---

## 🚀 Gelecekte Neler Yapılabilir?

- Streamlit ile canlı tahmin arayüzü oluşturulabilir  
- SHAP değerleriyle modelin açıklanabilirliği artırılabilir  
- Zaman serisi kampanya analizi yapılabilir  
- Müşteri segmentleri için öneri sistemleri geliştirilebilir  

---
## 💡 Bu Projede Neler Öğrendim?

- Gerçek bir veri setinde EDA yapmanın ne kadar önemli olduğunu,
- Outlier’larla doğru şekilde başa çıkmanın modeli nasıl etkilediğini,
- Feature engineering’in tahmin başarısını artırmada ne kadar kritik olduğunu,
- XGBoost’un gücünü ve GPU'nun hız farkını,
- Segment bazlı iş önerisi üretmenin iş zekası tarafındaki etkisini deneyimledim.
---
## 🏁 Sonuç

Bu proje sadece teknik olarak değil, iş problemini anlama ve veriye dayalı stratejiler üretme açısından da  
bana çok şey kattı. Makine öğrenmesiyle pazarlama stratejileri geliştirme fikri benim için çok değerliydi. 

---

 ## 🔗 Kaggle Notebook

 [Kaggle üzerinde projeyi görmek için buraya tıklayabilirsiniz](https://www.kaggle.com/code/suedakazan/bank-deposit-prediction-subscribe-or-not)
 

---
## 🧑‍💻 Hazırlayan

**Sueda Kazan**  
ML & Data Science Enthusiast 🌱  
LinkedIn: [linkedin.com/in/suedakzn](https://www.linkedin.com/in/suedakzn)  
GitHub: [github.com/suedakzn](https://github.com/suedakzn)

