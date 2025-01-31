
# Handling Multiple Headers in Power Query


This guide provides a step-by-step solution to transform multiple header rows into a single, usable header row in Power Query. This process is essential for cleaning and preparing data for analysis when dealing with complex datasets that have multiple header rows.

<img src="/img/Blank-Rows & Columns & Mutliple Headers.jpg" alt="Empty Columns Image" width="800" style="float: right; margin-left: 15px; margin-bottom: 15px;" />
<img src="/img/Blank-Rows & Columns & Mutliple Headers post2.jpg" alt="Project Image" width="300" style="float: right; margin-left: 15px; margin-bottom: 15px;" />

---

## **Overview**

When working with datasets in Power Query, you may encounter tables with **multiple header rows**. These headers can make data analysis challenging because Power Query expects a single header row. This documentation explains how to:

1. **Transpose** the table to convert multiple header rows into columns.
2. **Fill down** to handle null values in the header rows.
3. **Merge** the header columns into a single column using a delimiter.
4. **Transpose back** to create a single header row.
5. **Promote headers** and optionally **unpivot** the data for analysis.

---

## **Steps to Fix Multiple Headers**

### **1. Transpose the Table**
Transposing the table converts rows into columns and vice versa. This step makes the multiple header rows easier to manipulate.

```m
TransposedTable = Table.Transpose(tablename),
```

### **2. Fill Down to Handle Null Values**
Filling down ensures that null values in the header rows are replaced with the last non-null value above them.

```m
FillDown = Table.FillDown(TransposedTable, Table.ColumnNames(TransposedTable)),
```

### **3. Merge Columns into a Single Header**
Combine the multiple header columns into a single column using a delimiter (e.g., `,`).

```m
MergedColumns = Table.CombineColumns(FillDown,{"Column1", "Column2", "Column3"},
    Combiner.CombineTextByDelimiter(",", QuoteStyle.None),"Merged"),
```

### **4. Transpose Back to Create a Single Header Row**
Transpose the table again to convert the merged column into a single header row.

```m
TransposedBack = Table.Transpose(MergedColumns),
```

### **5. Promote Headers**
Promote the first row as the header row for the table.

```m
PromoteHeaders = Table.PromoteHeaders(TransposedBack, [PromoteAllScalars=true]),
```

### **6. Optional: Unpivot the Data**
If needed, unpivot the data to convert columns into rows for easier analysis.

```m
FilledDown = Table.FillDown(PromoteHeaders,{",,Category Name"}), // To Fix This Column
    UnpivotedOtherColumns = Table.UnpivotOtherColumns(FilledDown, {",,Category Name", ",,Shipping Mode - country"}, "Attribute", "Value"),
    SplitByUniqueDelimiter = Table.SplitColumn(UnpivotedOtherColumns, "Attribute", 
        Splitter.SplitTextByDelimiter(",", QuoteStyle.Csv), {"SalesRep", "ShipCompany", "ShipStatus"})
```

---

## **Full Power Query (M) Code**

```m
let
    // Load the Excel file
    Source = Excel.Workbook(File.Contents("C:\Users\Mahmud\Downloads\assignment-3 -Clean Data 2.xlsx"), null, true),
    
    // Select the "before" sheet
    before_Sheet = Source{[Item="before", Kind="Sheet"]}[Data],
    #"Removed Columns" = Table.RemoveColumns(before_Sheet,{"Column1", "Column2", "Column3", "Column4"}),
    tablename = Table.SelectRows(#"Removed Columns", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    
    // Step 1: Transpose the table to make multiple headers rows as columns
    TransposedTable = Table.Transpose(tablename),
    
    // Step 2: Fill up/down to handle null values in the header rows
    FillDown = Table.FillDown(TransposedTable, Table.ColumnNames(TransposedTable)),

    // Step 3: Merge columns into one column using a unique delimiter
    MergedColumns = Table.CombineColumns(FillDown,{"Column1", "Column2", "Column3"},
      Combiner.CombineTextByDelimiter(",", QuoteStyle.None),"Merged"),
    
    // Step 4: Transpose back to make the merged column a single row
    TransposedBack = Table.Transpose(MergedColumns),
    
    // Step 5: Promote the first row as headers
    PromoteHeaders = Table.PromoteHeaders(TransposedBack, [PromoteAllScalars=true]),

    // Step 6:  Unpivot the data and Spli by dilimiter (used in Step 3: Merge). 
    FilledDown = Table.FillDown(PromoteHeaders,{",,Category Name"}), // To Fix This Column
    UnpivotedOtherColumns = Table.UnpivotOtherColumns(FilledDown, {",,Category Name", ",,Shipping Mode - country"},
      "Attribute", "Value"),
    SplitByUniqueDelimiter = Table.SplitColumn(UnpivotedOtherColumns, "Attribute", 
        Splitter.SplitTextByDelimiter(",", QuoteStyle.Csv), {"SalesRep", "ShipCompany", "ShipStatus"})
in
    SplitByUniqueDelimiter
```

---

## **Key Notes**
- **Delimiter**: Adjust the delimiter (`,`) in the `Combiner.CombineTextByDelimiter` function as needed.
- **Unpivoting**: The unpivoting step is optional and should only be used if your analysis requires converting columns into rows.
- **File Path**: Ensure the file path in the `Source` step points to the correct file.

---

## **Conclusion**
By following this guide, you can efficiently transform multiple header rows into a single, usable header row in Power Query. This process ensures your data is clean, structured, and ready for analysis.

