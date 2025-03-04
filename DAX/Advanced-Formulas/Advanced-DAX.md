
# 📘 Advanced DAX Concepts in Power BI  

This repository contains in-depth explanations of key **DAX (Data Analysis Expressions) concepts** in Power BI, focusing on **time intelligence, filtering, and row/context-level calculations**. Each section includes examples, best practices, and performance insights.  

## 📂 Contents  

### ⏳ **Time Intelligence in Power BI**  
Time Intelligence functions in DAX enable users to perform **time-based calculations** efficiently. These functions help analyze trends, compare periods, and calculate cumulative values over time—essential for financial, sales, and operational reporting.  

📖 **Topics Covered:**  
- ✅ Prerequisites for Time Intelligence Functions  
- ✅ Steps to Create a Calendar Table  
- ✅ Best Practices  
- ✅ Key Features of Time Intelligence  

🔗 **[Click here](./TimeIntelligence.md) to explore Time Intelligence functions.**  

---

### 🎯 **Filtering in Power BI (DAX)**  
Power BI offers multiple ways to filter data using DAX. This section covers various **filtering functions**, including their use cases and performance comparisons.  

📖 **Topics Covered:**  
- 🎯 **CALCULATE** (Column-Level Filtering)  
- 🔎 **FILTER** (Row-Level Filtering)  
- 📊 **TOPN** (Top N Filtering)  
- ❌ **ALL** (Removes Filters)  
- 🚀 **ALLEXCEPT**  
- 🎛 **ALLSELECTED**  
- 🛠 **KEEPFILTERS**  
- 🔄 **REMOVEFILTERS**  
- 🔗 **CROSSFILTER**  

🔗 **[Check here](Filtering-DAXs.md) for Filtering in Power BI.**  

---

### ⚡ **Row-Level & Context-Level Filtering**  
DAX calculations work at different granularities, such as **row-level** and **context-level** filtering. This section explains their differences and when to use each.  

📖 **Topics Covered:**  
- ✅ **Row-Level Calculation**  
- ✅ **Context-Level Calculation**  
- ✅ **Key Differences**  
- ✅ **Row-Level and Context-Level Filtering**  
- ✅ **Filtering with `FILTER` vs `CALCULATE`**  

🔗 **[Check here](./Filtering-row%26context-Level.md) to understand Row & Context-Level Filtering.**  

---

### 🔝 **TopN in Power BI (DAX)**  
The `TOPN` function is useful for ranking and selecting the **top N rows** based on specific criteria. This section covers its behavior, common issues, and how to enhance its functionality.  

📖 **Topics Covered:**  
- 📌 **The Problem: TOPN Ignores Filters**  
- ✅ **Solution: Use `TOPN` Inside `CALCULATE` with `ALLSELECTED`**  
- 🏆 **How to Create a Dynamic TOPN Table in Power BI**  
- 🎛 **Bonus: Make the "Top N" Value User-Controlled**  

🔗 **[Check here](./TopN.md) for TopN function details.**  

---

## 📌 **Contributing**  
If you’d like to contribute, feel free to submit pull requests or open issues. Let’s enhance this knowledge base together! 🚀  

## 📜 **License**  
This repository is open-source and available under the MIT License.  

---

Happy Learning! 🎓✨  

