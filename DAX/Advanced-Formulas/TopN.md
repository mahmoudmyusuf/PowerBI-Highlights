# TopN
The **TOPN** function returns the top N rows of a table based on a specific column.

✅ **Best for:** Selecting the top **N** rows based on a condition.  
🚀 **Performance:** Optimized for ranking.

### **Syntax:**
```DAX
TOPN(N, <table>, <orderBy_expression>, <order[DESC/ASC]>)
```

### **Example: Top 5 Sales Orders**
```DAX
Top 5 Sales All Columns = TOPN(5, Orders, Orders[Sales], DESC)
```
### **Example: Top 5 Sales Orders with selected Columns**
**🛠️ Create a New Table Using `SUMMARIZE` + `TOPN`**  
```DAX
Top 5 Orders Table = 
VAR TopOrders = 
    TOPN(5, 
        Orders, 
        Orders[Sales], DESC
    )

RETURN
    SUMMARIZE(
        TopOrders, 
        Orders[Order ID], 
        Orders[Customer Name], 
        Orders[Sales]
    )
```

## **🔹 Making `TOPN` Tables Responsive to Filters and Visuals in Power BI**  

By default, `TOPN` ignores existing filters and returns a static result. However, you can make it **dynamic** so it updates based on filters and visuals in your Power BI dashboard.  

### **1️⃣ The Problem: `TOPN` Ignores Filters**  
⚠️ If you use `TOPN` inside a measure without considering the existing filter context, it **won't react to slicers or visuals**.  

#### **📌 Example: Static `TOPN` Calculation**  
```DAX
Top 5 Sales = TOPN(5, Orders, Orders[Sales], DESC)
```
🔴 **Issue:**  
- ❌ This table will always show the same 5 orders, ignoring filters!  

✅ **Solution Needed:**  
- 🔄 Modify it to respect filters dynamically.  

---

### **2️⃣ Solution: Use `TOPN` Inside `CALCULATE` with `ALLSELECTED`**  
To make `TOPN` update dynamically, we **combine it with `ALLSELECTED`** so it respects the active filters.  

#### **✅ Example: Dynamic `TOPN` Measure**  
```DAX
Top 5 Sales Dynamic = 
CALCULATE(
    SUM(Orders[Sales]), 
    TOPN(5, ALLSELECTED(Orders), Orders[Sales], DESC)
)
```
💡 **Why This Works?**  
✔️ `ALLSELECTED(Orders)`: **Keeps only the filters applied in visuals (not all filters are removed).**  
✔️ `TOPN(5, ...)`: **Finds the top 5 based on the filtered data, not the full dataset.**  
✔️ `CALCULATE`: **Modifies the filter context to apply `TOPN` dynamically.**  

---

### **3️⃣ 🚀 Bonus: Make the "Top N" Value User-Controlled**  
You can allow users to **choose how many top results they want** using a slicer.  

#### **🛠️ Step 1: Create a "Top N" Parameter**  
1️⃣ Go to **Modeling → New Parameter**.  
2️⃣ Name it **TopN_Value**.  
3️⃣ Set the **Range** (e.g., 1 to 10).  
4️⃣ Click **OK**.  

#### **🛠️ Step 2: Use the Parameter in `TOPN`**  
```DAX
Top N Dynamic = 
VAR SelectedN = SELECTEDVALUE(TopN_Value[TopN_Value], 5) -- Default to 5 if no selection
RETURN
    CALCULATE(
        SUM(Orders[Sales]), 
        TOPN(SelectedN, ALLSELECTED(Orders), Orders[Sales], DESC)
    )
```
✅ **What This Does?**  
✔️ `SELECTEDVALUE(TopN_Value[TopN_Value], 5)`: Picks the **Top N value** selected by the user.  
✔️ Users can **change the slicer to adjust the "Top N" dynamically** in real time.  

---

### **5️⃣ 🎯 Conclusion**  

| **🛠️ Solution** | **⚙️ How It Works** | **📈 Best Use Case** |
|-------------|----------------|-----------------|
| **Basic `TOPN` (Static)** | Always returns the same Top N | ❌ Doesn't change with filters |
| **`TOPN` + `SUMMARIZE` (Table)** | Always returns the same Top N | ❌ Doesn't change with filters |
| **`TOPN` + `ALLSELECTED`** | Updates based on visual filters | ✅ Works dynamically in reports |
| **User-Controlled `Top N`** | Lets users choose the number of top results | 🚀 Best for interactive dashboards |

---
