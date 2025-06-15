# 🛍️ Ürün Fiyat Tahmin Modeli

Bu proje, **Kaggle - academy2025** yarışmasından alınan bir veri seti kullanılarak ürün fiyatlarını tahmin etmeyi amaçlamaktadır. Geliştirilen makine öğrenimi modelleri, ürün, tarih ve pazar bilgileri gibi çeşitli özniteliklere dayanarak fiyat tahmininde bulunur.

## 📁 İçerik

* [Veri Seti](#veri-seti)
* [Kurulum](#kurulum)
* [Veri Keşfi ve Ön İşleme](#veri-keşfi-ve-ön-işleme)
* [Özellik Mühendisliği](#özellik-mühendisliği)
* [Modelleme ve Değerlendirme](#modelleme-ve-değerlendirme)
* [Sonuçlar](#sonu00e7lar)

---

## 📊 Veri Seti

**Kaynak:** Kaggle - academy2025 yarışması
**Dosyalar:**

* `train.csv`: Eğitim veri seti (227520 satır, 8 sütun)
* `testFeatures.csv`: Test veri seti (45504 satır, 8 sütun)

**Sütunlar:**

* `tarih`: Ürün fiyatının kaydedildiği tarih
* `ürün`: Ürünün adı
* `ürün besin değeri`: Ürünün besin içeriği
* `ürün kategorisi`: Kategorisi
* `ürün fiyatı`: Hedef değişken
* `ürün üretim yeri`: Üretim yeri
* `market`: Satıldığı market
* `şehir`: Satış yapılan şehir
* `id`: (sadece test setinde)

Veri setlerinde eksik değer bulunmamaktadır.

---

## ⚙️ Kurulum

Aşağıdaki kütüphanelerin yüklü olması gerekmektedir:

```bash
pip install pandas numpy seaborn matplotlib scikit-learn xgboost
```

---

## 🔍 Veri Keşfi ve Ön İşleme

* `tarih` sütunu `date` olarak yeniden adlandırıldı ve `year`, `month`, `day`, `week`, `season` gibi zaman tabanlı değişkenler çıkarıldı.
* Aykırı değerler Z-skoru yöntemine göre temizlendi (`ürün fiyatı`, `ürün besin değeri`).
* Kategorik değişenlerin hedef değişkenle ilişkisi görselleştirildi (boxplotlar).

---

## 💠 Özellik Mühendisliği

* **Mean Encoding:** `ürün`, `market`, `şehir`, `ürün kategorisi`, `üretim yeri` sütunları hedefe göre ortalama kodlandı.
* **Yeni Özellikler:**

  * `hafta_sonu` (gün bazlı)
  * `tatil_sezonu` (ay bazlı)
* **One-Hot Encoding:** Ortalama kodlanmış sütunların ayrıştırılması.
* **Korelasyon Matrisi:** Sayısal öznitelikler arasında ilişki ısı haritası ile analiz edildi.
* **RFE (Recursive Feature Elimination):** En iyi 10 özellik belirlendi.

---

## 🤖 Modelleme ve Değerlendirme

Farklı modeller RMSE, MAE ve R² metrikleriyle değerlendirildi:

### 📈 Linear Regression

* **RMSE:** 5.40
* **MAE:** 3.62
* **R²:** 0.8591

### 🌲 Random Forest Regressor

* **RMSE:** 1.39
* **MAE:** 0.77
* **R²:** 0.9906

### 🚀 XGBoost Regressor

* **İlk Model:**

  * RMSE: 1.10, MAE: 0.65, R²: 0.9942
* **Erken Durdurma ve Regularization:**

  * RMSE: 1.46, MAE: 0.89, R²: 0.9897
* **RandomizedSearchCV ile Optimizasyon:**

  * En iyi parametreler:

    ```python
    {'subsample': 1.0, 'reg_lambda': 1, 'reg_alpha': 0.5,
     'min_child_weight': 3, 'max_depth': 6, 'colsample_bytree': 0.9}
    ```
  * RMSE: 1.16, MAE: 0.68, R²: 0.9935
* **Final RFE Modeli (10 özellik ile):**

  * RMSE: 1.35, MAE: 0.76, R²: 0.9912
* **Final Optimize Model (Regularization + EarlyStopping):**

  * RMSE: 1.58, MAE: 0.95, R²: 0.9879

---

## 📌 Sonuçlar

* **XGBoost Regressor**, doğrusal regresyon ve rastgele orman modellerine kıyasla en iyi performansı göstermiştir.
* `ürün_encoded` ve `year`, fiyat tahmininde en etkili özniteliklerdir.
* **Hiperparametre optimizasyonu**, model başarımını anlamlı ölçüdé iyileştirmiştir.
* Karmaşıklık azaltma bazı durumlarda performansı düşürse de aşırı öğrenmeyi azaltmıştır.

---

## 🔮 Gelecek Çalışmalar

* GridSearchCV ile daha detaylı hiperparametre aramaları
* Alternatif regresyon modelleri (CatBoost, LightGBM)
* Yeni zaman serisi tabanlı özelliklerin eklenmesi
* Veri artırımı ve model ansamblları
