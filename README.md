
# 📊Anomaly Detection in Time-Series Data

This project focuses on identifying anomalies in numerical data using basic statistical methods and visualizations. It's a part of a hands-on assignment where the aim is to detect outliers and unusual patterns that deviate significantly from the norm.

---

## 🎯 Objective

To implement and compare various statistical techniques for detecting anomalies in a dataset, including:

- ✅ Z-Score Method  
- ✅ Interquartile Range (IQR)  
- ✅ Rolling Mean & Standard Deviation

These techniques help in identifying data points that are significantly different and may indicate rare, faulty, or interesting events.

---

## 📁 Dataset

The dataset consists of numeric time-series values. The column of interest used in analysis is:

- `value`: Continuous numerical data (e.g., temperature, usage, etc.)

---

## 🧪 Techniques Implemented

### 1. Z-Score Method
- Calculates how many standard deviations a point is from the mean.
- Data points with a Z-score > 3 (or < -3) are considered anomalies.

```python
from scipy.stats import zscore
df['zscore'] = zscore(df['value'])
anomalies = df[df['zscore'].abs() > 3]
```

---

### 2. IQR Method (Interquartile Range)
- Based on quartiles:  
  Q1 = 25th percentile  
  Q3 = 75th percentile  
  IQR = Q3 - Q1  
- Values outside [Q1 - 1.5×IQR, Q3 + 1.5×IQR] are flagged.

```python
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

anomalies = df[(df['value'] < lower_bound) | (df['value'] > upper_bound)]
```

---

### 3. Rolling Mean & Standard Deviation
- A rolling window is used to calculate local mean and std deviation.
- If a point deviates more than 2×std from the rolling mean, it's an anomaly.

```python
rolling_mean = df['value'].rolling(window=10).mean()
rolling_std = df['value'].rolling(window=10).std()

df['anomaly'] = ((df['value'] - rolling_mean).abs() > 2 * rolling_std)
```

---

## 📊 Visualization

Anomalies were visualized using **matplotlib** and **seaborn** for better interpretability.

```python
plt.figure(figsize=(12, 5))
plt.plot(df['value'], label='Original Data')
plt.scatter(df[df['anomaly']].index, df[df['anomaly']]['value'], color='red', label='Anomalies')
plt.legend()
plt.title('Anomaly Detection with Rolling Statistics')
plt.show()
```

---

## 📚 Libraries Used

- `pandas` – Data manipulation  
- `numpy` – Numerical calculations  
- `scipy` – Z-score calculation  
- `matplotlib`, `seaborn` – Data visualization

---

## 📈 Results

- All three techniques successfully identified anomalies.  
- Each method has its pros & cons depending on the data distribution and trend.  
- Rolling statistics are useful when data has time-based fluctuations.

---

## 🔮 Future Enhancements

In future versions, the following can be added:
- Isolation Forest or Local Outlier Factor  
- Autoencoders / LSTM for deep learning-based anomaly detection  
- Deployment via Gradio or Streamlit for real-time detection

---

## 👩‍💻 Author

**Faryal Nimra**  
📧 [faryalnimra190@gmail.com](mailto:faryalnimra190@gmail.com)  
🔗 [LinkedIn](https://linkedin.com/in/faryal-nimra-4a49a32b6)  
🌐 [Portfolio](https://portfolio-five-beige-ixn8l41et7.vercel.app/)

---

## 💡 Note

This project is created for educational purposes and helps in understanding foundational statistical techniques for anomaly detection. It can serve as a building block for more advanced solutions in real-world monitoring systems.
