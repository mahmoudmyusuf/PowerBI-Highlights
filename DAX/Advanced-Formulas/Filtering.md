# ✨ **Filtering in Power BI (DAX)**

Power BI provides multiple ways to filter data using DAX. Some of the key functions include **CALCULATE**, **FILTER**, and **TOPN**, along with other filtering functions. Below is a detailed explanation of each with examples and differences.

---

## 🔍 **1. CALCULATE (Column-Level Filtering)**
The **CALCULATE** function is one of the most powerful DAX functions. It **modifies** the context of a calculation by applying specific filters.

✅ **Best for:** Simple filters applied to entire columns.  
🚀 **Performance:** Fast  

### **Syntax:**
```DAX
CALCULATE(<expression>, <filter1>, <filter2>, ...)
```

### **Example: Sales for Electronics Category**
```DAX
Total Sales Electronics =
CALCULATE(
    SUM(Orders[Sales]),
    Orders[Category] = "Electronics"
)
```

### **Key Features:**
✅ Used to modify the existing filter context.  
✅ Can be combined with other filters like `FILTER`, `ALL`, `ALLSELECTED`, etc.  
✅ **Filters the entire table** where `Category = "Electronics"`.  
✅ **Fast** because it applies column-level filtering.

---

## 🌱 **2. FILTER (Row-Level Filtering)**
The **FILTER** function returns a table that meets a given condition. It is often used inside functions like `CALCULATE` or `SUMX` when you need row-level filtering.

✅ **Best for:** Complex conditions that require row-by-row evaluation.  
🐢 **Performance:** Slower (iterates over rows)  

### **Syntax:**
```DAX
FILTER(<table>, <condition>)
```

### **Example: Sales for Orders Where Quantity > 10**
```DAX
Total Sales Large Orders =
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, Orders[Quantity] > 10)
)
```

### **Key Features:**
✅ Works at the row level, returning a subset of the original table.  
✅ Used inside functions like `CALCULATE`, `SUMX`, `AVERAGEX`, etc.  
✅ **Scans each row individually** to check if `Quantity > 10`.  
✅ **Slower** than `CALCULATE` because it loops through rows.

---

## 🏆 **3. TOPN (Top N Filtering)**
The **TOPN** function returns the top N rows of a table based on a specific column.

✅ **Best for:** Selecting the top **N** rows based on a condition.  
🚀 **Performance:** Optimized for ranking.

### **Syntax:**
```DAX
TOPN(N, <table>, <orderBy_expression>, <order[DESC/ASC]>)
```

### **Example: Top 3 Sales Orders**
```DAX
Top 3 Sales =
TOPN(3, Orders, Orders[Sales], DESC)
```

### **Key Features:**
✅ Returns a table, not a single value.  
✅ Often used inside `SUMX`, `CALCULATE`, or visual-level filters.  
✅ Returns the **top 3 rows** based on `Sales`.  
✅ Useful for **ranking and leaderboard reports**.

---

## 🔒 **4. ALL (Removes Filters)**
✅ **Best for:** Ignoring filters applied by visuals.  
🚀 **Performance:** Fast (modifies filter context directly).

### **Example: Calculate Total Sales Without Filters**
```DAX
Total Sales All =
CALCULATE(
    SUM(Orders[Sales]),
    ALL(Orders)
)
```
- Ignores **all filters** applied to the `Orders` table.
- Useful for **percentages and total comparisons**.

---

## 🔄 **5. ALLEXCEPT (Removes Filters Except for Specific Columns)**
✅ **Best for:** Keeping only certain filters while removing others.  
🚀 **Performance:** Similar to `ALL`.

### **Example: Keep Only "Category" Filter**
```DAX
Total Sales by Category =
CALCULATE(
    SUM(Orders[Sales]),
    ALLEXCEPT(Orders, Orders[Category])
)
```
- **Removes all filters** except for `Category`.
- Useful for keeping hierarchy filters in reports.

---

## 🔎 **6. ALLSELECTED (Keeps Filters Applied by the Visual)**
✅ **Best for:** Keeping only **user-applied filters** in visuals.  
🚀 **Performance:** Similar to `ALL`, but **preserves user selections**.

### **Example: Sales Within User-Selected Range**
```DAX
Total Sales Selected =
CALCULATE(
    SUM(Orders[Sales]),
    ALLSELECTED(Orders)
)
```
- Keeps only the filters **applied in the report visual**.
- Useful for interactive dashboards.

---

## ⚙️ **7. KEEPFILTERS (Combining Multiple Filters)**
✅ **Best for:** Adding filters **without overriding existing ones**.  
🚀 **Performance:** Can be slower if used inside `FILTER`.

### **Example: Apply Two Filters Without Overriding**
```DAX
Total Sales Multi-Filter =
CALCULATE(
    SUM(Orders[Sales]),
    KEEPFILTERS(Orders[Category] = "Electronics"),
    Orders[Quantity] > 5
)
```
- Keeps the existing filter on `Category` and **adds** the `Quantity` filter.
- Useful for **combining multiple filters**.

---

## 🚫 **8. REMOVEFILTERS (Removes Specific Filters)**
✅ **Best for:** Removing filters from **specific columns**.  
🚀 **Performance:** Fast.

### **Example: Ignore Only Category Filter**
```DAX
Total Sales No Category Filter =
CALCULATE(
    SUM(Orders[Sales]),
    REMOVEFILTERS(Orders[Category])
)
```
- Removes only the `Category` filter.
- Useful for **dynamic calculations**.

---

## ♻️ **9. CROSSFILTER (Controls Relationships Between Tables)**
✅ **Best for:** Changing filter direction in **relationships**.  
🚀 **Performance:** Depends on data model complexity.

### **Example: Change Filter Direction in a Relationship**
```DAX
Total Sales CrossFilter =
CALCULATE(
    SUM(Orders[Sales]),
    CROSSFILTER(Customers[CustomerID], Orders[CustomerID], BOTH)
)
```
- **Changes relationship** between `Customers` and `Orders` to **bidirectional**.
- Useful for **custom filtering logic**.

---


### **🔒 Final Summary: Choosing the Best Filter**
Here’s the combined table with all the relevant data:  

| Function       | Works At     | Returns     | Context Modification       | Best For                 | Performance | Use Case |
|--------------|------------|------------|----------------------|----------------------|-------------|----------|
| **CALCULATE** | Column     | A single value | Yes                  | Simple filters       | 🚀 Fast    | Modify filter context and apply multiple conditions. |
| **FILTER**    | Row        | A table       | No                   | Complex conditions   | 🐢 Slower  | Return a filtered subset of a table for row-wise calculations. |
| **TOPN**      | Row        | A table       | No                   | Top N values         | 🚀 Optimized | Return the top N rows based on a metric. |
| **ALL**       | Table      | A table       | Yes (Removes filters) | Remove all filters   | 🚀 Fast    | Remove all filters from a column or table. |
| **ALLEXCEPT** | Table      | A table       | Yes (Keeps specific filters) | Keep only some filters | 🚀 Fast    | Remove all filters except the specified columns. |
| **ALLSELECTED** | Table   | A table       | Yes (Keeps report-level filters) | Keep user-applied filters | 🚀 Fast    | Keep only report-level filters. |
| **KEEPFILTERS** | Column   | A table       | No (Preserves filters) | Combining multiple filters | 🐢 Slower  | Add filters while keeping existing ones. |
| **REMOVEFILTERS** | Table | A table       | Yes                  | Removing specific filters | 🚀 Fast    | Remove specific filters while keeping others. |
| **CROSSFILTER** | Relationship | - | ⚡ Depends | Controlling relationships | ⚡ Depends | Control the direction of relationships between tables. |


### **Key Takeaways**
✔ **Use `CALCULATE` for fast column-based filtering.**  
✔ **Use `FILTER` for row-by-row evaluations, but only when needed.**  
✔ **Use `ALL`, `REMOVEFILTERS`, and `ALLEXCEPT` to modify filter contexts.**  
✔ **Use `TOPN` for ranking and leaderboard reports.**  

---
## ⚡ Differences between (`FILTER`) and (`CALCULATE` with direct conditions)

The differences in Performance (Speed), Accuracy, and Efficiency Between **Row-Level and Context-Level Filtering in DAX**  

 **Row-Level filtering (`FILTER`)** versus **Context-Level filtering (`CALCULATE` with direct conditions)** in Power BI.

---

### 🚀 **1. Speed (Performance) Differences**
| Method | Speed | Reason |
|--------|--------|---------|
| **CALCULATE (with direct filters)** | 🚀 **Faster** | Works on **column-level filters** and modifies the filter context efficiently. |
| **FILTER (row-level filtering)** | 🐢 **Slower** | Iterates through **each row** individually, increasing computation time. |

#### ⚡ **Example: Performance Test**
##### 📌 **Scenario 1: Using `CALCULATE` for filtering**
```DAX
Total Sales Electronics =
CALCULATE(
    SUM(Orders[Sales]),
    Orders[Category] = "Electronics"
)
```
✅ **Faster** because Power BI directly **filters the column** and applies the condition in a single step.  

##### ⏳ **Scenario 2: Using `FILTER` for the same calculation**
```DAX
Total Sales Electronics Filter =
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, Orders[Category] = "Electronics")
)
```
❌ **Slower** because:
1. `FILTER(Orders, Orders[Category] = "Electronics")` **loops through every row**.
2. It checks **each row individually** instead of applying a direct column filter.
3. Then, `SUM(Orders[Sales])` aggregates the results.

👉 **Conclusion**:  
- `CALCULATE` directly applies the filter at the **column level**, making it **more efficient**.  
- `FILTER` evaluates **row by row**, making it **slower**, especially for large datasets.

---

### 🎯 **2. Accuracy Differences**
| Method | Accuracy | Explanation |
|--------|----------|-------------|
| **CALCULATE (with direct filters)** | ✅ **Accurate** | Works well when applying filters to columns globally. |
| **FILTER** | ✅ **Accurate** | Works well for **complex row-by-row conditions**, but must be used carefully. |

#### 🧐 **Example Where `FILTER` Is More Accurate**
If we need to filter based on a condition that depends on **multiple columns**, `FILTER` is **more reliable**.

##### 🔍 **Scenario: Find total sales for orders where `Quantity > 10` but only for "Electronics"**
```DAX
Total Sales Large Electronics Orders =
CALCULATE(
    SUM(Orders[Sales]),
    FILTER(Orders, Orders[Quantity] > 10 && Orders[Category] = "Electronics")
)
```
✅ Here, `FILTER` is necessary because:
- We need to check **each row** where `Quantity > 10` **AND** `Category = Electronics`.
- `CALCULATE` alone cannot filter **two column conditions row by row**.

👉 **Conclusion**:  
- **Use `CALCULATE` when filtering a column is enough.**  
- **Use `FILTER` when conditions depend on multiple row-level evaluations.**

---

### 📖 **3. Other Factors: Readability & Maintainability**
| Method | Readability | Maintainability |
|--------|------------|----------------|
| **CALCULATE (direct filter)** | ✅ **Easier to read** | ✅ **Easier to maintain** because it applies simple column filters. |
| **FILTER** | ❌ **More complex** | ❌ **Harder to maintain** for large datasets since it increases row evaluations. |

#### ✍️ **Example: Easier to Read Code**
```DAX
Total Sales Electronics =
CALCULATE(SUM(Orders[Sales]), Orders[Category] = "Electronics")
```
✅ **Easy to read**  

Now compare this with:
```DAX
Total Sales Electronics Filter =
CALCULATE(SUM(Orders[Sales]), FILTER(Orders, Orders[Category] = "Electronics"))
```
❌ **More complex** and unnecessary for simple filters.

---

### 🔄 **4. When to Use `FILTER` Instead of `CALCULATE`?**
Use **`FILTER`** only when:
1. You need **row-by-row** evaluation (e.g., `Quantity > 10 && Sales > 1000`).
2. Your filter condition **depends on multiple columns**.
3. You need to pass a **filtered table** into another function.

---

### 🏆 **Final Recommendation:**
| Situation | Best Approach |
|-----------|--------------|
| Filtering based on a single column | `CALCULATE( SUM(Orders[Sales]), Orders[Category] = "Electronics")` |
| Filtering with multiple **row-based** conditions | `FILTER(Orders, Orders[Quantity] > 10 && Orders[Category] = "Electronics")` |

---

### ✅ **Conclusion**
- ✅ **Use `CALCULATE` for simple column filters** (Fastest, most efficient).  
- ✅ **Use `FILTER` when filtering depends on multiple row-based conditions** (Accurate but slower).  
- ✅ **Avoid unnecessary use of `FILTER` when `CALCULATE` can do the job** (Improves performance).

---



## **🔹 Making `TOPN` Tables Responsive to Filters and Visuals in Power BI**  

By default, `TOPN` ignores existing filters and returns a static result. However, you can make it **dynamic** so it updates based on filters and visuals in your Power BI dashboard.  

### **1️⃣ The Problem: `TOPN` Ignores Filters**  
⚠️ If you use `TOPN` inside a measure without considering the existing filter context, it **won't react to slicers or visuals**.  

#### **📌 Example: Static `TOPN` Calculation**  
```DAX
Top 5 Sales Static = 
TOPN(5, Orders, Orders[Sales], DESC)
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

### **3️⃣ How to Create a Dynamic `TOPN` Table in Power BI**  
📊 You can create a **dynamic table** that changes based on slicers and visuals.  

#### **🛠️ Step 1: Create a New Table Using `SUMMARIZE` + `TOPN`**  
```DAX
Top 5 Orders Table = 
VAR TopOrders = 
    TOPN(5, 
        ALLSELECTED(Orders), 
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
✅ **Why This Works?**  
✔️ `ALLSELECTED(Orders)`: Ensures the table respects the filters from visuals.  
✔️ `SUMMARIZE`: Selects only the needed columns.  
✔️ `TOPN(5, ...)`: Dynamically selects the top 5 based on **filtered data**.  

---

### **4️⃣ 🚀 Bonus: Make the "Top N" Value User-Controlled**  
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
| **`TOPN` + `ALLSELECTED`** | Updates based on visual filters | ✅ Works dynamically in reports |
| **`TOPN` + `SUMMARIZE` (Table)** | Creates a filtered table of Top N | ✅ Best for detailed analysis |
| **User-Controlled `Top N`** | Lets users choose the number of top results | 🚀 Best for interactive dashboards |

---



## 📊 **What Does "Works at the Row Level" Mean in DAX?**  

When we say a function **"works at the row level"**, it means that the function **evaluates each row individually** before returning a result. Some DAX functions, like `FILTER`, operate this way, meaning they check each row **one by one** to see if it meets the given condition.

### 📌 **Example: Row-Level vs. Context-Level Calculation**
Let’s take an example dataset:

| 🆔 Order ID | 🛍 Product  | 📂 Category  | 💰 Sales | 🔢 Quantity |
|---------|---------|-----------|------|----------|
| 101     | Laptop  | Electronics | 1000 | 5 |
| 102     | Mouse   | Electronics | 50   | 12 |
| 103     | Phone   | Electronics | 800  | 2 |
| 104     | Chair   | Furniture   | 200  | 15 |
| 105     | Table   | Furniture   | 500  | 8 |

---

### ✅ **1. `FILTER` Works at the Row Level**
The `FILTER` function checks **each row** one by one to see if it meets the condition.

#### 🏆 **Example: Total Sales Where Quantity > 10**
```DAX
Total Sales Large Orders = 
CALCULATE(
    SUM(Orders[Sales]), 
    FILTER(Orders, Orders[Quantity] > 10)
)
```
#### 🔍 **How It Works Internally:**
- `FILTER(Orders, Orders[Quantity] > 10)` scans **each row** in the table.
- It **keeps only** the rows where `Quantity > 10`:
  - ✅ **Row 102: Mouse (Quantity = 12)**
  - ✅ **Row 104: Chair (Quantity = 15)**
- Then, `CALCULATE(SUM(Orders[Sales]))` adds up only these rows:
  - 💵 **50 (Mouse) + 200 (Chair) = 250**  
✔️ **Result: 250**  


### 🔄 **2. `CALCULATE` Without `FILTER` (Context-Level)**
Now, let’s try the same calculation **without** using `FILTER`:

```DAX
Total Sales Electronics = 
CALCULATE(
    SUM(Orders[Sales]), 
    Orders[Category] = "Electronics"
)
```
#### 🔍 **How It Works:**
- Instead of checking each row one by one, Power BI **modifies the entire filter context** by applying `Orders[Category] = "Electronics"`.
- It automatically keeps only the rows **where Category = "Electronics"**:
  - ✅ **Laptop (1000)**
  - ✅ **Mouse (50)**
  - ✅ **Phone (800)**
- Then, `SUM(Orders[Sales])` adds up those values:  
  ✔️ **Result: 1850**


### ⚖ **Key Difference Between Row-Level and Context-Level Filtering**
| 🔢 Function | ✅ Row-Level? | ⚙️ How It Works |
|----------|----------|--------------|
| **FILTER** | ✅ **Yes** | Checks each row **individually** and returns only matching rows. |
| **CALCULATE** (with direct conditions) | ❌ No | Changes the **entire filter context** for the calculation. |
| **TOPN** | ✅ **Yes** | Evaluates each row and selects the top N rows based on a condition. |
| **ALL, ALLSELECTED, ALLEXCEPT** | ❌ No | Works at the **table level** by modifying the filter context. |


### 📌 **Final Summary**
1. **Row-Level Filtering (`FILTER`, `TOPN`)**  
   - Works **row by row** and returns a subset of the original table.
   - Used inside `CALCULATE`, `SUMX`, `AVERAGEX`, etc.
   
2. **Context-Level Filtering (`CALCULATE`, `ALL`, `ALLEXCEPT`)**  
   - Changes the **entire filter context** before performing calculations.
   - More efficient for filtering **whole categories** instead of individual rows.


### ⚡ **Performance and Efficiency Considerations**
### 🏎️ **1. Speed (Performance) Differences**
| Method | ⚡ Speed | 📌 Reason |
|--------|--------|---------|
| **CALCULATE (with direct filters)** | 🚀 **Faster** | Works on **column-level filters** and modifies the filter context efficiently. |
| **FILTER (row-level filtering)** | 🐢 **Slower** | Iterates through **each row** individually, increasing computation time. |

#### 🏆 **Example: Performance Test**
#### ✅ **Using CALCULATE for filtering**
```DAX
Total Sales Electronics = 
CALCULATE(
    SUM(Orders[Sales]), 
    Orders[Category] = "Electronics"
)
```
✅ **Faster** because Power BI directly **filters the column** and applies the condition in a single step.  

#### ❌ **Using FILTER for the same calculation**
```DAX
Total Sales Electronics Filter = 
CALCULATE(
    SUM(Orders[Sales]), 
    FILTER(Orders, Orders[Category] = "Electronics")
)
```
❌ **Slower** because:
1. FILTER scans **each row** instead of applying a direct column filter.
2. It checks **each row individually** before aggregation.

👉 **Conclusion**:  
- ✔️ **CALCULATE directly applies the filter at the column level**, making it **more efficient**.  
- ✔️ **FILTER evaluates row by row**, making it **slower**, especially for large datasets.


### 🎯 **Conclusion**
- ✅ **Use CALCULATE for simple column filters** (Fastest, most efficient).  
- ✅ **Use FILTER when filtering depends on multiple row-based conditions** (Accurate but slower).  
- ✅ **Avoid unnecessary use of FILTER when CALCULATE can do the job** (Improves performance).  

📖 **Supporting Reference:**  
- [📚 Microsoft Docs: Avoid using FILTER as a filter argument in DAX](https://learn.microsoft.com/en-us/dax/best-practices/dax-avoid-avoid-filter-as-filter-argument)

---
