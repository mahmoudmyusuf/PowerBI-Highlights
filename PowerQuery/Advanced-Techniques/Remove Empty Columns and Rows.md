
# Power Query Script to Remove Empty Columns and Rows

This guide explains how to **manually** and **automatically** remove empty columns and rows in Power Query (M). Start with the manual steps to understand the process, then transition to the automatic script for efficiency.

---

<img src="/img/EmptyColumns.jpg" alt="Empty Columns Image" width="300" style="float: right; margin-left: 15px; margin-bottom: 15px;" />

## Manual Steps to Remove Empty Columns and Rows

### 1. Remove Empty Columns Manually
1. Load your data into Power Query.
2. Check the leftmost columns to identify if they are entirely empty or contain unnecessary data.
3. To remove a column:
   - Right-click the column header.
   - Select **Remove**.
4. Repeat for all empty or unnecessary columns.

### 2. Remove Empty Rows Manually
You have two options to identify and remove empty rows:

#### **Option 1: Use the "Remove Rows" Feature**
1. Go to the **Home** tab in Power Query.
2. Click on **Remove Rows** > **Remove Blank Rows**.
3. Manually confirm that the rows removed were indeed empty.

```m
let
    Source = Excel.Workbook(File.Contents("C:\Users\Mahmud\Downloads\assignment-3 -Clean Data 2.xlsx"), null, true),
    before_Sheet = Source{[Item="before",Kind="Sheet"]}[Data],
    #"Removed Columns" = Table.RemoveColumns(before_Sheet,{"Column1", "Column2", "Column3", "Column4"}),
    #"Removed Blank Rows" = Table.SelectRows(#"Removed Columns", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null})))
in
    #"Removed Blank Rows"
```

#### **Option 2: Transpose the Table**
1. Go to the **Transform** tab and click **Transpose**. This converts rows into columns and vice versa.
2. Check the leftmost columns (originally rows) to identify empty rows.
3. Remove the empty columns (originally rows).
4. Transpose the table back to its original structure.

```m
let
    Source = Excel.Workbook(File.Contents("C:\Users\Mahmud\Downloads\assignment-3 -Clean Data 2.xlsx"), null, true),
    before_Sheet = Source{[Item="before",Kind="Sheet"]}[Data],
    #"Removed Columns" = Table.RemoveColumns(before_Sheet,{"Column1", "Column2", "Column3", "Column4"}),
    #"Transposed Table" = Table.Transpose(#"Removed Columns"),
    #"Removed Columns1" = Table.RemoveColumns(#"Transposed Table",{"Column1", "Column2", "Column3"}),
    #"Transposed Table1" = Table.Transpose(#"Removed Columns1")
in
    #"Transposed Table1"
```
---

## Automatic Script to Remove Empty Columns and Rows

Once you’re comfortable with the manual process, use the following Power Query (M) script to **automatically** remove empty columns and rows.

### Power Query (M) Script

```m
let
    // Load the Excel file
    Source = Excel.Workbook(File.Contents("C:\Users\Mahmud\Downloads\assignment-3 -Clean Data 2.xlsx"), null, true),
    
    // Select the "YourTableName" 
    YourTableName = Source{[Item="before", Kind="Sheet"]}[Data],

    // Remove empty columns dynamically
    NonEmptyColumns = List.Select(Table.ColumnNames(YourTableName), 
        each List.NonNullCount(Table.Column(YourTableName, _)) > 0),
    
    CleanedTable = Table.SelectColumns(YourTableName, NonEmptyColumns),

    // Remove empty rows dynamically
    RemoveEmptyRows = Table.SelectRows(CleanedTable, 
        each List.NonNullCount(Record.FieldValues(_)) > 0)
    
in
    RemoveEmptyRows
```

### How to Use the Automatic Script
1. Open Power Query Editor.
2. Go to **Home** > **Advanced Editor**.
3. Replace the existing code with the script above.
4. Update the `Source` step to reference your data (e.g., replace `YourTableName` with your table name).
5. Click **Close & Apply** to apply the changes.

---

### Why Use the Automatic Script?
- **Saves Time**: No need to manually check each column or row.
- **Accuracy**: Ensures all empty columns and rows are removed consistently.
- **Scalability**: Works efficiently even with large datasets.
