
# ⚡ Row-Level vs. Context-Level in DAX  

In **DAX (Data Analysis Expressions)**, calculations operate at different levels of granularity, including **row-level** and **context-level** calculations. Here's what they mean:  

📊 Let's say we have a **Sales** table with columns: `[Quantity]` and `[Unit Price]`.  

---

## 🟢 1. Row-Level Calculation  
A **row-level calculation** in DAX works on a **single row** of a table, typically inside a **calculated column**. The calculation is performed **independently** for each row.  

### 🔹 **Key Characteristics:**  
✅ Operates on **individual rows** of a table.  
✅ Uses **column references** directly.  
✅ ❌ Does **not** depend on external filters or aggregations.  

### ✨ **Example:**  
A **calculated column** to compute total sales **per row**:  

```DAX
Total Sales = Sales[Quantity] * Sales[Unit Price]
```

---

## 🔵 2. Context-Level Calculation  
A **context-level calculation** (also known as an **aggregate-level calculation**) works within the **filter and row context**, often inside **measures**.  

### 🔹 **Key Characteristics:**  
📌 Operates at the **aggregated level**, considering **filter and row context**.  
📌 Uses aggregation functions like `SUM()`, `AVERAGE()`, etc.  
📌 🎯 **Filter-dependent** – affected by slicers, filters, and report structure.  

### ✨ **Example:**  
A **measure** to calculate total sales dynamically:  

```DAX
Total Sales Measure = SUMX(Sales, Sales[Quantity] * Sales[Unit Price])
```

---

## 🔥 **Key Differences**  
| 🏷 **Feature**        | 🟢 Row-Level Calculation  | 🔵 Context-Level Calculation  |
|-----------------------|----------------------|---------------------------|
| 📍 **Where it's used** | **Calculated Columns** | **Measures** |
| 🎯 **Scope**          | Single row in a table | Multiple rows, affected by filters |
| ⚙️ **Example Function** | `Sales[Quantity] * Sales[Unit Price]` | `SUMX(Sales, Sales[Quantity] * Sales[Unit Price])` |
| 🔍 **Filter Dependency** | ❌ No (each row independent) | ✅ Yes (affected by slicers, filters, and visuals) |

---

## 🏆 **When to Use Each?**  
✅ Use **row-level calculations (calculated columns)** when you need to **store results** **per row** in a table.  
✅ Use **context-level calculations (measures)** when you need **dynamic aggregation** based on slicers, filters, or visual interactions.  

---

# ⚡ Row-Level and Context-Level Filtering
## 📌 **Example: Row-Level vs. Context-Level Calculation**
Let’s take an example dataset:

| 🆔 Order ID | 🛍 Product  | 📂 Category  | 💰 Sales | 🔢 Quantity |
|---------|---------|-----------|------|----------|
| 101     | Laptop  | Electronics | 1000 | 5 |
| 102     | Mouse   | Electronics | 50   | 12 |
| 103     | Phone   | Electronics | 800  | 2 |
| 104     | Chair   | Furniture   | 200  | 15 |
| 105     | Table   | Furniture   | 500  | 8 |

---

## ✅ 1. FILTER at the Row Level
Some DAX functions, like FILTER, operate this way, meaning they check each row **one by one** to see if it meets the given condition.

### 🏆 **Example: Total Sales Where Quantity > 10**

```DAX
Total Sales Large Orders = 
CALCULATE(
    SUM(Orders[Sales]), 
    FILTER(Orders, Orders[Quantity] > 10)
)
```

### 🔍 **How It Works Internally:**
- FILTER(Orders, Orders[Quantity] > 10) scans **each row** in the table.
- It **keeps only** the rows where Quantity > 10:
  - ✅ **Row 102: Mouse (Quantity = 12)**
  - ✅ **Row 104: Chair (Quantity = 15)**
- Then, CALCULATE(SUM(Orders[Sales])) adds up only these rows:
  - 💵 **50 (Mouse) + 200 (Chair) = 250**  
✔️ **Result: 250**  
---

## 🔄  2. FILTER at the Context Level
Now, let’s try the same calculation **without** using FILTER:

```DAX
Total Sales Electronics = 
CALCULATE(
    SUM(Orders[Sales]), 
    Orders[Category] = "Electronics"
)
```

### 🔍 **How It Works:**
- Instead of checking each row one by one, Power BI **modifies the entire filter context** by applying Orders[Category] = "Electronics".
- It automatically keeps only the rows **where Category = "Electronics"**:
  - ✅ **Laptop (1000)**
  - ✅ **Mouse (50)**
  - ✅ **Phone (800)**
- Then, SUM(Orders[Sales]) adds up those values:  
  ✔️ **Result: 1850**
---

## ⚖ **Key Difference Between Row-Level and Context-Level Filtering**
| 🔢 Function | ✅ Row-Level? | ⚙️ How It Works |
|----------|----------|--------------|
| **FILTER** | ✅ **Yes** | Checks each row **individually** and returns only matching rows. |
| **CALCULATE** (with direct conditions) | ❌ No | Changes the **entire filter context** for the calculation. |
| **TOPN** | ✅ **Yes** | Evaluates each row and selects the top N rows based on a condition. |
| **ALL, ALLSELECTED, ALLEXCEPT** | ❌ No | Works at the **table level** by modifying the filter context. |


### 📌 **Final Summary**
1. **Row-Level Filtering (FILTER, TOPN)**  
   - Works **row by row** and returns a subset of the original table.
   - Used inside CALCULATE, SUMX, AVERAGEX, etc.
   
2. **Context-Level Filtering (CALCULATE, ALL, ALLEXCEPT)**  
   - Changes the **entire filter context** before performing calculations.
   - More efficient for filtering **whole categories** instead of individual rows.



📖 **Supporting Reference:**  
- [📚 Microsoft Docs: Avoid using FILTER as a filter argument in DAX](https://learn.microsoft.com/en-us/dax/best-practices/dax-avoid-avoid-filter-as-filter-argument)

---


# Filtering with `FILTER` versus `CALCULATE`
The differences in Performance (Speed), Accuracy, and Efficiency Between 
 **Row-Level filtering** (e.g.,**FILTER**) versus **Context-Level filtering** (e.g.,**CALCULATE with direct conditions**) in Power BI **DAX**.

---

## 🏎️ **1. Speed (Performance) Differences**
| Method | Speed | Reason |
|--------|--------|---------|
| **CALCULATE (with direct filters)** | 🚀 **Faster** | Works on **column-level filters** and modifies the filter context efficiently. |
| **FILTER (row-level filtering)** | 🐢 **Slower** | Iterates through **each row** individually, increasing computation time. |

### ⚡ **Example: Performance Test**
#### 📌 **Scenario 1: Using CALCULATE for filtering**

```DAX
Total Sales Electronics =
CALCULATE(
    SUM(Orders[Sales]),
    Orders[Category] = "Electronics"
)
```

✅ **Faster** because Power BI directly **filters the column** and applies the condition in a single step.  

#### ⏳ **Scenario 2: Using FILTER for the same calculation**

```DAX
Total Sales Electronics Filter =
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, Orders[Category] = "Electronics")
)
```

❌ **Slower** because:
1. FILTER(Orders, Orders[Category] = "Electronics") **loops through every row**.
2. It checks **each row individually** instead of applying a direct column filter.
3. Then, SUM(Orders[Sales]) aggregates the results.

👉 **Conclusion**:  
- CALCULATE directly applies the filter at the **column level**, making it **more efficient**.  
- FILTER evaluates **row by row**, making it **slower**, especially for large datasets.

---

## 🎯 **2. Accuracy Differences**
| Method | Accuracy | Explanation |
|--------|----------|-------------|
| **CALCULATE (with direct filters)** | ✅ **Accurate** | Works well when applying filters to columns globally. |
| **FILTER** | ✅ **Accurate** | Works well for **complex row-by-row conditions**, but must be used carefully. |

### 🧐 **Example Where FILTER Is More Accurate**
If we need to filter based on a condition that depends on **multiple columns**, FILTER is **more reliable**.

#### 🔍 **Scenario: Find total sales for orders where Quantity > 10 but only for "Electronics"**

```DAX
Total Sales Large Electronics Orders =
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, Orders[Quantity] > 10 &&
    Orders[Category] = "Electronics")
)
```

✅ Here, FILTER is necessary because:
- We need to check **each row** where Quantity > 10 **AND** Category = Electronics.
- CALCULATE alone cannot filter **two column conditions row by row**.

👉 **Conclusion**:  
- **Use CALCULATE when filtering a column is enough.**  
- **Use FILTER when conditions depend on multiple row-level evaluations.**

---

## 📖 **3. Readability & Maintainability**
| Method | Readability | Maintainability |
|--------|------------|----------------|
| **CALCULATE (direct filter)** | ✅ **Easier to read** | ✅ **Easier to maintain** because it applies simple column filters. |
| **FILTER** | ❌ **More complex** | ❌ **Harder to maintain** for large datasets since it increases row evaluations. |

#### ✍️ **Example: Easier to Read Code**

```DAX
Total Sales Electronics =
CALCULATE(SUM(Orders[Sales]),
    Orders[Category] = "Electronics")
```

✅ **Easy to read**  

Now compare this with:

```DAX
Total Sales Electronics Filter =
CALCULATE(SUM(Orders[Sales]),
    FILTER(Orders, 
    Orders[Category] = "Electronics"))
```

❌ **More complex** and unnecessary for simple filters.

---


## 🎯 Conclusion & Recommendation
 ✅ **Use CALCULATE for simple column filters** (Fastest, most efficient).  
 ✅ **Use FILTER when filtering depends on multiple row-based conditions** (Accurate but slower).  
 ✅ **Avoid unnecessary use of FILTER when CALCULATE can do the job** (Improves performance).<br>
 ✅ **Use FILTER Instead of CALCULATE** only when:
- You need **row-by-row** evaluation.
- Your filter condition **depends on multiple columns**.
- You need to pass a **filtered table** into another function.
    
---


🚀 **Mastering these concepts will help you optimize DAX calculations for better performance in Power BI!**  

---
