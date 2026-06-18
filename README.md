# Indian Budget Smartphones — EDA & Price Prediction

> 印度預算手機市場分析與價格預測  
> 資料來源：Flipkart 上架手機（₹20,000 以下）

---

## 專案簡介

印度是全球最大的智慧型手機市場之一，₹20,000（約 NT$7,500）以下的預算機型佔據主要銷售量。本專案以 **11,976 筆** Flipkart 手機資料為基礎，透過機器學習回答兩個核心問題：

- **哪些硬體規格最能影響手機售價？**
- **模型能精準預測一支手機的合理定價嗎？**

---

## 資料集

| 項目 | 說明 |
|---|---|
| 來源 | [Kaggle — Indian Smartphones Under ₹20,000](https://www.kaggle.com/datasets/demonllord/indian-smartphones-under-20000specs-prices) |
| 筆數 | 11,976 筆 |
| 欄位 | 品牌、售價、折扣率、評分、評論數、RAM/ROM、螢幕尺寸、相機規格、電池容量、處理器、保固 |

---

## 分析流程

```
1. 資料清理與特徵工程
   ├─ regex 提取數值型規格（RAM、ROM、電池、螢幕、相機像素）
   ├─ 處理器分級（Budget / Mid / High-Mid）
   └─ 性價比特徵（RAM_per_Price、Battery_per_Price、Spec_Score）

2. 探索式分析（EDA）
   ├─ 品牌市佔率分布
   ├─ 規格與價格相關性熱力圖
   └─ 各品牌定價策略 Violin Plot

3. 機器學習模型比較
   └─ Random Forest / XGBoost / LightGBM / CatBoost

4. 超參數調校（Optuna）
   └─ 貝葉斯最佳化（TPE Sampler，50 次試驗）

5. 模型可解釋性（SHAP）
   ├─ Beeswarm Summary Plot（全局特徵影響分布）
   ├─ Bar Plot（平均特徵重要性）
   └─ Waterfall Plot（單筆預測解釋）
```

---

## 主要結果

| 模型 | MAE (INR) | R² |
|---|---|---|
| Random Forest（預設） | 80.06 | 0.9382 |
| XGBoost（預設） | 79.37 | 0.9331 |
| LightGBM（預設） | 94.86 | 0.9210 |
| CatBoost（預設） | 84.26 | 0.9437 |
| **XGBoost（Optuna 調校）** | **63.21** | **0.93+** |

**SHAP 關鍵發現：**
- RAM 是預算手機定價最重要的單一特徵
- 高處理器分級對高價手機的貢獻遠大於平均值
- 折扣率（Discount_Value）對最終售價的影響排名第四

---

## 快速開始

```bash
# 1. 下載資料集（需先安裝 Kaggle CLI 並設定 API Token）
pip install kaggle
kaggle datasets download -d demonllord/indian-smartphones-under-20000specs-prices
unzip indian-smartphones-under-20000specs-prices.zip -d data/

# 2. 安裝套件
pip install -r requirements.txt

# 3. 開啟 notebook
jupyter notebook indian-smartphones-under-20000.ipynb
```

> **Kaggle API Token 設定**：至 [kaggle.com/settings](https://www.kaggle.com/settings) → API → Create New Token，將下載的 `kaggle.json` 放至 `~/.kaggle/kaggle.json`。

---

## 專案結構

```
indian-smartphones-analysis/
├── indian-smartphones-under-20000.ipynb   # 主要分析 notebook
├── requirements.txt                        # Python 套件需求
└── README.md
```

---

## 技術棧

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-3.2-green)
![SHAP](https://img.shields.io/badge/SHAP-0.52-red)
![Optuna](https://img.shields.io/badge/Optuna-4.9-purple)

`pandas` · `numpy` · `scikit-learn` · `xgboost` · `lightgbm` · `catboost` · `optuna` · `shap` · `plotly` · `seaborn`
