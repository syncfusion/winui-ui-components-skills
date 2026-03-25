# Columns in WinUI DataGrid

Complete guide to column configuration, types, auto-generation, sizing, resizing, freezing, and manipulation in Syncfusion WinUI DataGrid.

## Table of Contents
- [Column Types](#column-types)
- [Defining Columns](#defining-columns)
- [Auto-Generating Columns](#auto-generating-columns)
- [Manually Defining Columns](#manually-defining-columns)
- [Column Manipulation](#column-manipulation)
- [Column Width and Sizing](#column-width-and-sizing)
- [Auto-Sizing Columns](#auto-sizing-columns)
- [Resizing Columns](#resizing-columns)
- [Drag and Drop Columns](#drag-and-drop-columns)
- [Freezing Columns](#freezing-columns)
- [Binding Column Properties](#binding-column-properties)
- [Common Scenarios](#common-scenarios)

## Column Types

SfDataGrid provides built-in column types for different data types. Choose the appropriate column type based on your data:

| Column Type | Data Type | Description |
|-------------|-----------|-------------|
| **GridTextColumn** | string, object, dynamic | Display string data |
| **GridNumericColumn** | double, int, decimal | Display and edit numeric data |
| **GridCheckBoxColumn** | bool | Display boolean as checkbox |
| **GridCheckBoxSelectorColumn** | - | Row selection via checkbox (not data-bound) |
| **GridDateColumn** | DateTimeOffset | Display and edit date values |
| **GridTimeColumn** | DateTimeOffset | Display and edit time values |
| **GridComboBoxColumn** | IEnumerable | Display dropdown for selection |
| **GridImageColumn** | ImageSource, string (URI) | Display images |
| **GridHyperlinkColumn** | Uri | Display clickable hyperlinks |
| **GridToggleSwitchColumn** | bool | Display boolean as ToggleSwitch |
| **GridTemplateColumn** | any | Custom template for any content |
| **GridUnboundColumn** | calculated | Display computed/unbound data |

## Defining Columns

You can define columns in two ways:
1. **Automatically** - Let DataGrid generate columns from data properties
2. **Manually** - Explicitly define each column

## Auto-Generating Columns

Enable automatic column generation using `AutoGenerateColumns` property (default is `true`).

```xml
<dataGrid:SfDataGrid x:Name="sfDataGrid"
                     AutoGenerateColumns="True"
                     ItemsSource="{Binding Orders}" />
```

### Auto-Generation Rules

Columns are generated based on property types:

| Property Type | Generated Column |
|---------------|------------------|
| string, object, dynamic | GridTextColumn |
| double, double? | GridNumericColumn |
| DateTimeOffset, DateTimeOffset? | GridDateColumn |
| Uri, Uri? | GridHyperlinkColumn |
| bool, bool? | GridCheckBoxColumn |
| Other types | GridTextColumn |

### AutoGenerateColumns Modes

Control auto-generation behavior with `AutoGenerateColumnsMode`:

| Mode | Behavior | When ItemsSource Changes |
|------|----------|--------------------------|
| **Reset** (default) | Generate columns from data object properties | Keeps manual columns, regenerates auto columns |
| **RetainOld** | Generate if not explicitly defined | Maintains existing columns and settings |
| **ResetAll** | Generate all columns fresh | Clears all columns including manual ones |
| **SmartReset** | Generate and retain data operations | Retains valid columns with their settings |
| **None** | No auto-generation | Keeps old columns |

```xml
<dataGrid:SfDataGrid AutoGenerateColumns="True"
                     AutoGenerateColumnsMode="RetainOld"
                     ItemsSource="{Binding Orders}" />
```

### Auto-Generate Custom Type Properties

Enable column generation for custom type properties:

```xml
<dataGrid:SfDataGrid AutoGenerateColumnsForCustomType="True"
                     AutoGenerateColumnsModeForCustomType="Both"
                     ItemsSource="{Binding Source}" />
```

**AutoGenerateColumnsModeForCustomType Options:**
- **Both** - Generates columns for custom type and its inner properties
- **Parent** - Only custom type column
- **Child** - Only inner properties of custom type

### Customize Auto-Generated Columns

Use `AutoGeneratingColumn` event to customize or cancel column generation:

```csharp
sfDataGrid.AutoGeneratingColumn += SfDataGrid_AutoGeneratingColumn;

private void SfDataGrid_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Cancel column generation for specific property
    if (e.Column.MappingName == "OrderID")
    {
        e.Cancel = true;
        return;
    }
    
    // Change column type
    if (e.Column.MappingName == "IsShipped" && e.Column is GridCheckBoxColumn)
    {
        e.Column = new GridTextColumn 
        { 
            MappingName = "IsShipped", 
            HeaderText = "Shipped Status" 
        };
    }
    
    // Customize column properties
    if (e.Column.MappingName == "CustomerID")
    {
        e.Column.AllowEditing = false;
        e.Column.AllowSorting = true;
        e.Column.AllowFiltering = true;
        e.Column.ColumnWidthMode = ColumnWidthMode.Auto;
        e.Column.Width = 150;
    }
    
    // Set custom templates
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderTemplate = Resources["customHeaderTemplate"] as DataTemplate;
        e.Column.CellTemplate = Resources["customCellTemplate"] as DataTemplate;
    }
}
```

### Data Annotations for Auto-Generation

Use data annotations to control auto-generation:

**Exclude Column:**
```csharp
using System.ComponentModel.DataAnnotations;

[Display(AutoGenerateField = false)]
public int OrderID { get; set; }
```

**Set as Editable:**
```csharp
[Editable(true)]
public decimal UnitPrice { get; set; }
```

**Customize Header Text:**
```csharp
[Display(Name = "Customer Name")]
public string CustomerName { get; set; }
```

**Change Column Order:**
```csharp
[Display(Order = 0)]
public string ShipCity { get; set; }

[Display(Order = 1)]
public int OrderID { get; set; }
```

## Manually Defining Columns

Define columns explicitly for complete control:

```xml
<dataGrid:SfDataGrid x:Name="sfDataGrid"
                     AutoGenerateColumns="False"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridTextColumn MappingName="OrderID" 
                                 HeaderText="Order ID" 
                                 Width="120"
                                 AllowEditing="False" />
        
        <dataGrid:GridTextColumn MappingName="CustomerName" 
                                 HeaderText="Customer" 
                                 Width="200" />
        
        <dataGrid:GridNumericColumn MappingName="UnitPrice" 
                                    HeaderText="Price"
                                    Width="150" />
        
        <dataGrid:GridDateColumn MappingName="OrderDate" 
                                 HeaderText="Order Date"
                                 Width="150" />
        
        <dataGrid:GridCheckBoxColumn MappingName="IsShipped" 
                                     HeaderText="Shipped"
                                     Width="100" />
        
        <dataGrid:GridComboBoxColumn MappingName="Country"
                                     HeaderText="Country"
                                     ItemsSource="{Binding Countries}"
                                     Width="150" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

**Code-Behind:**
```csharp
sfDataGrid.AutoGenerateColumns = false;

sfDataGrid.Columns.Add(new GridTextColumn 
{ 
    MappingName = "OrderID", 
    HeaderText = "Order ID", 
    Width = 120 
});

sfDataGrid.Columns.Add(new GridNumericColumn 
{ 
    MappingName = "UnitPrice", 
    HeaderText = "Price",
    Width = 150
});
```

## Column Manipulation

### Adding Columns at Runtime

```csharp
// Add to end
sfDataGrid.Columns.Add(new GridTextColumn 
{ 
    MappingName = "ShipAddress", 
    HeaderText = "Address" 
});

// Insert at specific position
sfDataGrid.Columns.Insert(0, new GridTextColumn 
{ 
    MappingName = "OrderID", 
    HeaderText = "ID" 
});
```

### Accessing Columns

```csharp
// By index
GridColumn column = sfDataGrid.Columns[1];

// By MappingName
GridColumn column = sfDataGrid.Columns["OrderID"];

// Check if exists
if (sfDataGrid.Columns.Contains("CustomerID"))
{
    var column = sfDataGrid.Columns["CustomerID"];
}
```

### Removing Columns

```csharp
// Remove all columns
sfDataGrid.Columns.Clear();

// Remove specific column
var column = sfDataGrid.Columns["OrderID"];
sfDataGrid.Columns.Remove(column);

// Remove by index
sfDataGrid.Columns.RemoveAt(0);

// Remove by MappingName
sfDataGrid.Columns.Remove(sfDataGrid.Columns["OrderID"]);
```

## Column Width and Sizing

### Setting Fixed Width

```xml
<dataGrid:GridTextColumn MappingName="OrderID" Width="150" />
```

### Column Width Mode

Use `ColumnWidthMode` to control how column widths are calculated:

| Mode | Description |
|------|-------------|
| **None** | Uses defined Width or default width |
| **Star** | Divides available width equally |
| **Auto** | Fits header and cell content (no truncation) |
| **AutoWithLastColumnFill** | Auto width + last column gets remaining width |
| **AutoLastColumnFill** | Auto width + last column gets max(auto width, remaining width) |
| **SizeToCells** | Fits cell content only |
| **SizeToHeader** | Fits header content only |

**Grid-Level:**
```xml
<dataGrid:SfDataGrid ColumnWidthMode="Auto"
                     ItemsSource="{Binding Orders}" />
```

**Column-Level (takes priority):**
```xml
<dataGrid:GridTextColumn MappingName="OrderID" 
                         ColumnWidthMode="SizeToHeader" />
```

### Star Width Example

```xml
<dataGrid:SfDataGrid ColumnWidthMode="Star"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <!-- Each column gets equal width -->
        <dataGrid:GridTextColumn MappingName="OrderID" />
        <dataGrid:GridTextColumn MappingName="Customer" />
        <dataGrid:GridTextColumn MappingName="Country" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

### Fill Remaining Width in Specific Column

```xml
<dataGrid:SfDataGrid ColumnWidthMode="Auto"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridTextColumn MappingName="OrderID" />
        <dataGrid:GridTextColumn MappingName="Customer" 
                                 ColumnWidthMode="AutoLastColumnFill" />
        <dataGrid:GridTextColumn MappingName="Country" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

### Min/Max Width

```xml
<dataGrid:GridTextColumn MappingName="CustomerName"
                         MinWidth="100"
                         MaxWidth="400"
                         ColumnWidthMode="Auto" />
```

## Auto-Sizing Columns

### Auto-Fit Based on Visible Rows

Improve performance by calculating column width only for visible rows:

```xml
<dataGrid:SfDataGrid ColumnWidthMode="Auto"
                     AutoFitRange="VisibleRows"
                     ItemsSource="{Binding Orders}" />
```

**AutoFitRange Options:**
- **AllRows** (default) - Calculate width for all rows
- **VisibleRows** - Calculate width only for visible rows (better performance)

### Refresh Auto-Size at Runtime

```csharp
// Refresh all columns
sfDataGrid.ColumnSizer.ResetAutoCalculationforAllColumns();
sfDataGrid.ColumnSizer.Refresh();

// Refresh specific column
var column = sfDataGrid.Columns["OrderID"];
sfDataGrid.ColumnSizer.ResetAutoCalculation(column);
sfDataGrid.ColumnSizer.Refresh();
```

### Reset Column Width for Auto-Sizing

```csharp
// Reset all column widths to allow auto-sizing
foreach (var column in sfDataGrid.Columns)
{
    if (!double.IsNaN(column.Width))
        column.Width = double.NaN;
}
sfDataGrid.ColumnSizer.Refresh();
```

### Customize Auto-Sizing Logic

```csharp
sfDataGrid.ColumnSizer = new CustomColumnSizer(sfDataGrid);

public class CustomColumnSizer : DataGridColumnSizer
{
    public CustomColumnSizer(SfDataGrid dataGrid) : base(dataGrid) { }
    
    protected override double CalculateCellWidth(GridColumn column, bool setWidth = true)
    {
        // Custom width calculation for GridComboBoxColumn
        if (column is GridComboBoxColumn comboColumn)
        {
            var source = comboColumn.ItemsSource;
            string maxText = string.Empty;
            
            foreach (var item in source)
            {
                string text = item.ToString();
                if (maxText.Length < text.Length)
                    maxText = text;
            }
            
            var size = MeasureText(new Size(column.Width, DataGrid.RowHeight), 
                                  maxText, column, null, GridQueryBounds.Width);
            return size.Width;
        }
        
        return base.CalculateCellWidth(column, setWidth);
    }
    
    protected override double CalculateHeaderWidth(GridColumn column, bool setWidth = true)
    {
        // Custom header width calculation
        return base.CalculateHeaderWidth(column, setWidth);
    }
}
```

### Font Settings for Auto-Sizing

**Grid-Level Font Settings:**
```csharp
sfDataGrid.ColumnSizer.FontSize = 12.0;
sfDataGrid.ColumnSizer.FontFamily = new FontFamily("Segoe UI");
sfDataGrid.ColumnSizer.Margin = new Thickness(9, 3, 1, 3);
sfDataGrid.ColumnSizer.SortIconWidth = 20;
sfDataGrid.ColumnSizer.FilterIconWidth = 20;
```

**Column-Level Font Settings:**
```csharp
var column = sfDataGrid.Columns["OrderID"];
DataGridColumnSizer.SetFontFamily(column, new FontFamily("Arial"));
DataGridColumnSizer.SetFontSize(column, 11.0);
DataGridColumnSizer.SetMargin(column, new Thickness(8, 2, 2, 2));
```

## Resizing Columns

Enable column resizing by dragging column header borders:

```xml
<dataGrid:SfDataGrid AllowResizingColumns="True"
                     ItemsSource="{Binding Orders}" />
```

### Disable Resizing for Specific Column

```xml
<dataGrid:GridTextColumn MappingName="OrderID" 
                         AllowResizing="False" />
```

### Hidden Column Resizing

Show and allow resizing of hidden columns:

```xml
<dataGrid:SfDataGrid AllowResizingHiddenColumns="True"
                     ItemsSource="{Binding Orders}" />
```

### ResizingColumns Event

```csharp
sfDataGrid.ResizingColumns += SfDataGrid_ResizingColumns;

private void SfDataGrid_ResizingColumns(object sender, ResizingColumnsEventArgs e)
{
    // Cancel resizing for specific column
    if (e.ColumnIndex == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Detect when resizing completes
    if (e.Reason == ColumnResizingReason.Resized)
    {
        var finalWidth = e.Width;
        Debug.WriteLine($"Column resized to {finalWidth}");
    }
}
```

## Drag and Drop Columns

Enable column reordering via drag-and-drop:

```xml
<dataGrid:SfDataGrid AllowDraggingColumns="True"
                     ItemsSource="{Binding Orders}" />
```

### Disable Dragging for Specific Column

```xml
<dataGrid:GridTextColumn MappingName="OrderID" 
                         AllowDragging="False" />
```

### ColumnDragging Event

```csharp
sfDataGrid.ColumnDragging += SfDataGrid_ColumnDragging;

private void SfDataGrid_ColumnDragging(object sender, QueryColumnDraggingEventArgs e)
{
    // e.From = source column index
    // e.To = target column index
    // e.Reason = QueryColumnDraggingReason (Dropping, DragStart, etc.)
    
    // Cancel dragging specific column
    if (sfDataGrid.Columns[e.From].MappingName == "OrderID" && 
        e.Reason == QueryColumnDraggingReason.Dropping)
    {
        e.Cancel = true;
    }
}
```

### Prevent Drag Between Frozen and Non-Frozen

```csharp
private void SfDataGrid_ColumnDragging(object sender, QueryColumnDraggingEventArgs e)
{
    if (e.Reason == QueryColumnDraggingReason.Dropping)
    {
        var frozenIndex = sfDataGrid.FrozenColumnsCount + 
                         sfDataGrid.ResolveToStartColumnIndex();
        
        // Prevent dragging from frozen to non-frozen
        if (e.From < frozenIndex && e.To >= frozenIndex)
            e.Cancel = true;
        
        // Prevent dragging from non-frozen to frozen
        if (e.From >= frozenIndex && e.To < frozenIndex)
            e.Cancel = true;
    }
}
```

### Custom Drag-Drop Controller

```csharp
sfDataGrid.ColumnDragDropController = new CustomDragDropController(sfDataGrid);

public class CustomDragDropController : DataGridColumnDragDropController
{
    public CustomDragDropController(SfDataGrid dataGrid) : base(dataGrid) { }
    
    protected override void PopupContentDroppedOnHeaderRow(int oldIndex, int newIndex)
    {
        // Custom drop logic
        base.PopupContentDroppedOnHeaderRow(oldIndex, newIndex);
    }
    
    protected override UIElement CreatePopupContent(GridColumn column)
    {
        // Custom popup appearance
        return base.CreatePopupContent(column);
    }
}
```

## Freezing Columns

Freeze columns at left or right side (like Excel):

```xml
<!-- Freeze first 2 columns from left -->
<dataGrid:SfDataGrid FrozenColumnsCount="2"
                     ItemsSource="{Binding Orders}" />

<!-- Freeze last 1 column from right -->
<dataGrid:SfDataGrid FrozenFooterColumnsCount="1"
                     ItemsSource="{Binding Orders}" />

<!-- Freeze both sides -->
<dataGrid:SfDataGrid FrozenColumnsCount="2"
                     FrozenFooterColumnsCount="1"
                     ItemsSource="{Binding Orders}" />
```

**Important:** Frozen column count must be less than the number of visible columns.

## Binding Column Properties

Bind column properties to ViewModel for MVVM:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool allowFiltering = true;
    
    public bool AllowFiltering
    {
        get => allowFiltering;
        set
        {
            allowFiltering = value;
            OnPropertyChanged(nameof(AllowFiltering));
        }
    }
}
```

```xml
<Page.Resources>
    <local:ViewModel x:Key="viewModel" />
</Page.Resources>

<dataGrid:SfDataGrid ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridTextColumn MappingName="OrderID"
                                 AllowFiltering="{Binding AllowFiltering, 
                                                 Source={StaticResource viewModel}}"
                                 AllowSorting="{Binding AllowSorting,
                                               Source={StaticResource viewModel}}" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

## Common Scenarios

### Scenario 1: Read-Only Grid with Custom Widths

```xml
<dataGrid:SfDataGrid AutoGenerateColumns="False"
                     AllowEditing="False"
                     ColumnWidthMode="None"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridTextColumn MappingName="OrderID" Width="100" />
        <dataGrid:GridTextColumn MappingName="Customer" Width="200" />
        <dataGrid:GridNumericColumn MappingName="Total" Width="150" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

### Scenario 2: Editable Grid with Auto-Sizing

```xml
<dataGrid:SfDataGrid AutoGenerateColumns="False"
                     AllowEditing="True"
                     ColumnWidthMode="Auto"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridTextColumn MappingName="OrderID" AllowEditing="False" />
        <dataGrid:GridTextColumn MappingName="Customer" />
        <dataGrid:GridNumericColumn MappingName="Quantity" />
        <dataGrid:GridDateColumn MappingName="OrderDate" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

### Scenario 3: Freeze Important Columns with Resizing

```xml
<dataGrid:SfDataGrid FrozenColumnsCount="2"
                     AllowResizingColumns="True"
                     AllowDraggingColumns="True"
                     ItemsSource="{Binding Orders}" />
```

### Scenario 4: Responsive Column Layout

```xml
<dataGrid:SfDataGrid ColumnWidthMode="Star"
                     ItemsSource="{Binding Orders}">
    <dataGrid:SfDataGrid.Columns>
        <!-- Equal distribution -->
        <dataGrid:GridTextColumn MappingName="OrderID" />
        <dataGrid:GridTextColumn MappingName="Customer" />
        <dataGrid:GridTextColumn MappingName="Country" />
    </dataGrid:SfDataGrid.Columns>
</dataGrid:SfDataGrid>
```

## Best Practices

1. **Set AutoGenerateColumns="False"** for production apps - manual column definition provides better control
2. **Use ColumnWidthMode="None"** with explicit widths for consistent layout
3. **Use AutoFitRange="VisibleRows"** with large datasets for better performance
4. **Freeze essential columns** (ID, Name) for better user experience
5. **Disable editing/sorting/filtering** on calculated or display-only columns
6. **Use GridTemplateColumn** for complex custom content
7. **Handle AutoGeneratingColumn** event to apply consistent styling
8. **Set MinWidth and MaxWidth** to prevent columns from becoming too narrow or wide
