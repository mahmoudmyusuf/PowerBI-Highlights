# 📘 Advanced Power BI DAX Techniques

This repository contains detailed explanations of advanced **DAX filtering and time intelligence** concepts in Power BI. Below are the key topics covered:

## 📅 **Time Intelligence in Power BI** ([TimeIntelligence.md](TimeIntelligence.md))
Time Intelligence functions in DAX enable time-based calculations such as period comparisons, cumulative totals, and trends over time. These functions are essential for financial, sales, and operational reporting.

### 📌 **Contents:**
✅ Prerequisites for Time Intelligence Functions  
✅ Steps to Create a Calendar Table  
✅ Best Practices  
✅ Key Features of Time Intelligence  

---

## 🔍 **Filtering in Power BI (DAX)** ([Filtering in Power BI (DAX).md](Filtering-row&context-Level.md))
DAX provides multiple filtering functions to refine data analysis. This section covers different filtering approaches, their use cases, and best practices.

### 📌 **Contents:**
✅ **CALCULATE** - Column-Level Filtering  
✅ **FILTER** - Row-Level Filtering  
✅ **TOPN** - Top N Filtering  
✅ **ALL** - Removes Filters  
✅ **ALLEXCEPT** - Removes Filters Except Certain Columns  
✅ **ALLSELECTED** - Respects Applied Filters  
✅ **KEEPFILTERS** - Preserves Existing Filters  
✅ **REMOVEFILTERS** - Removes Specific Filters  
✅ **CROSSFILTER** - Controls Relationships Between Tables  

---

## ⚖ **Row-Level vs. Context-Level Filtering** ([Filtering-row&context-Level.md](Filtering-row&context-Level.md))
DAX calculations operate at different levels, impacting how results are computed. This section clarifies the difference between **row-level and context-level** calculations and how to apply them correctly.

### 📌 **Contents:**
✅ Row-Level Calculation  
✅ Context-Level Calculation  
✅ Key Differences  
✅ Row-Level and Context-Level Filtering  
✅ Key Difference Between Row-Level and Context-Level Filtering  
✅ Filtering with `FILTER` vs. `CALCULATE`  

---

## 🔝 **Top N Filtering in Power BI** ([TopN.md](TopN.md))
The `TOPN` function is used to extract the top N rows from a table based on a given column. This section explains how to use `TOPN` effectively, including handling its limitations.

### 📌 **Contents:**
✅ The Problem: `TOPN` Ignores Filters  
✅ Solution: Use `TOPN` Inside `CALCULATE` with `ALLSELECTED`  
✅ How to Create a Dynamic `TOPN` Table in Power BI  
✅ Bonus: Make the "Top N" Value User-Controlled  

---

🚀 **Stay tuned for more insights and advanced DAX techniques!** 🎯
