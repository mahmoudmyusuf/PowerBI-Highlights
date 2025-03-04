# ✨ **Filtering in Power BI (DAX)**

Power BI provides multiple ways to filter data using DAX. Some of the key functions include **CALCULATE**, **FILTER**, and **TOPN**, along with other filtering functions. Below is a detailed explanation of each with examples and differences.


The total summary can be calculated using the following DAX expression:

```DAX
Total Sales = SUM(Orders[Sales])
```
Once the **Total Sales** measure is defined, various **Filtering functions** can be applied to perform advanced analysis.

---

## 🔍 **1. CALCULATE (Column-Level Filtering)**
The **CALCULATE** function is one of the most powerful DAX functions. It **modifies** the context of a calculation by applying specific filters.

✅ **Best for:** Simple filters applied to entire columns.  
🚀 **Performance:** Fast  

### **Syntax:**
```DAX
CALCULATE(<expression>, <filter1>, <filter2>, ...)
```

### **Example: Sales for Office Supplies**
```DAX
Total Sales Office = 
CALCULATE(
    [Total Sales],
    Orders[Category] = "Office Supplies"
)
```

### **Example: Apply Two Filters**
```DAX
Total Sales Calculate = 
CALCULATE(
    [Total Sales],
    Orders[Category] = "Office Supplies",
    Orders[Quantity] > 5
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

### **Example: Apply Two Filters**
```DAX
Total Sales Filter = 
CALCULATE(
    [Total Sales],
    FILTER(Orders, [Category] = "Office Supplies" &&
    Orders[Quantity] > 5)
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
Top 3 Sales = TOPN(3, Orders, Orders[Sales], DESC)
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
    [Total Sales],
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
     [Total Sales],
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
    [Total Sales],
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
    [Total Sales],
    KEEPFILTERS(Orders[Category] = "Office Supplies"),
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
     [Total Sales],
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
Total Return CrossFilter = 
CALCULATE(
     [Total Return],
    CROSSFILTER(Returns[Order ID], Orders[Order ID], BOTH)
)
```
- **Changes relationship** between `Customers` and `Orders` to **bidirectional**.
- Useful for **custom filtering logic**.

---

![Animated GIF](Filtering.gif)

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

