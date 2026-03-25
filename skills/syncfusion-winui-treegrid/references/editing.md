# Editing in WinUI TreeGrid

## Table of Contents
- [Enable Editing](#enable-editing)
- [Edit Modes](#edit-modes)
- [Edit Events](#edit-events)
- [Column-Level Editing](#column-level-editing)
- [Programmatic Editing](#programmatic-editing)
- [Validation](#validation)
- [Editor Types](#editor-types)

## Enable Editing

Enable editing for the entire TreeGrid:

```xaml
<treeGrid:SfTreeGrid AllowEditing="True" 
                    ItemsSource="{Binding Employees}" />
```

```csharp
sfTreeGrid.AllowEditing = true;
```

When enabled, users can double-click cells or press F2 to enter edit mode.

## Edit Modes

Control when cells enter edit mode:

```csharp
sfTreeGrid.EditTrigger = EditTrigger.OnTap;
```

| EditTrigger | Behavior |
|------------|----------|
| **OnTap** | Single click enters edit mode |
| **OnDoubleTap** (default) | Double-click enters edit mode |

**Example:**
```xaml
<treeGrid:SfTreeGrid AllowEditing="True"
                    EditTrigger="OnTap" />
```

## Edit Events

### CurrentCellBeginEdit

Raised before a cell enters edit mode. Cancel editing or customize behavior.

```csharp
sfTreeGrid.CurrentCellBeginEdit += (sender, e) =>
{
    // Cancel editing for specific column
    if (e.Column.MappingName == "ID")
    {
        e.Cancel = true;
        return;
    }
    
    // Cancel editing for specific rows
    var employee = e.DataRow.RowData as Employee;
    if (employee.IsReadOnly)
    {
        e.Cancel = true;
    }
};
```

**Event Args Properties:**
- `e.Column` - Column being edited
- `e.DataRow` - Row data
- `e.Cancel` - Set to true to prevent editing

### CurrentCellEndEdit

Raised before changes are committed. Validate or cancel the edit.

```csharp
sfTreeGrid.CurrentCellEndEdit += (sender, e) =>
{
    // Validate the new value
    if (e.Column.MappingName == "Salary")
    {
        var salary = Convert.ToDouble(e.NewValue);
        if (salary < 0)
        {
            e.Cancel = true;
            // Show error message
            return;
        }
    }
};
```

**Event Args Properties:**
- `e.Column` - Column being edited
- `e.NewValue` - New value entered
- `e.OldValue` - Original value
- `e.Cancel` - Set to true to reject changes

### CurrentCellValueChanged

Raised after the cell value changes:

```csharp
sfTreeGrid.CurrentCellValueChanged += (sender, e) =>
{
    // Log changes
    var column = e.Column.MappingName;
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    
    Logger.Log($"Changed {column} from {oldValue} to {newValue}");
};
```

## Column-Level Editing

Control editing for individual columns:

```xaml
<!-- Allow editing this column -->
<treeGrid:TreeGridTextColumn MappingName="FirstName" 
                            AllowEditing="True" />

<!-- Disable editing this column -->
<treeGrid:TreeGridNumericColumn MappingName="ID" 
                               AllowEditing="False" />
```

```csharp
column.AllowEditing = false;  // Make column read-only
```

**Common pattern - Allow grid editing but protect key columns:**
```xaml
<treeGrid:SfTreeGrid AllowEditing="True">
    <treeGrid:SfTreeGrid.Columns>
        <treeGrid:TreeGridNumericColumn MappingName="ID" 
                                       AllowEditing="False" />
        <treeGrid:TreeGridTextColumn MappingName="FirstName" />
        <treeGrid:TreeGridTextColumn MappingName="LastName" />
        <treeGrid:TreeGridNumericColumn MappingName="Salary" />
    </treeGrid:SfTreeGrid.Columns>
</treeGrid:SfTreeGrid>
```

## Programmatic Editing

### Begin Edit Programmatically

```csharp
// Begin edit at row index and column index
sfTreeGrid.BeginEdit(rowIndex: 2, columnIndex: 1);

// Or get current cell
var currentCell = sfTreeGrid.SelectionController.CurrentCellManager.CurrentCell;
if (currentCell != null)
{
    sfTreeGrid.BeginEdit(currentCell.RowIndex, currentCell.ColumnIndex);
}
```

### End Edit and Commit

```csharp
// Commit current edit
sfTreeGrid.EndEdit();

// Commit with validation check
if (sfTreeGrid.CurrentCell != null)
{
    bool committed = sfTreeGrid.EndEdit();
    if (!committed)
    {
        // Edit was cancelled or validation failed
    }
}
```

### Cancel Edit

```csharp
// Cancel current edit without committing
sfTreeGrid.CancelEdit();
```

### Check if Cell is in Edit Mode

```csharp
if (sfTreeGrid.IsCurrentCellInEditing)
{
    // Cell is currently being edited
}
```

## Validation

### Data Annotation Validation

Use data annotations in your model:

```csharp
using System.ComponentModel.DataAnnotations;

public class Employee
{
    [Required(ErrorMessage = "First name is required")]
    [StringLength(50, ErrorMessage = "Maximum 50 characters")]
    public string FirstName { get; set; }
    
    [Range(0, double.MaxValue, ErrorMessage = "Salary must be positive")]
    public double Salary { get; set; }
    
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }
}
```

### Custom Validation in Events

```csharp
sfTreeGrid.CurrentCellEndEdit += (sender, e) =>
{
    if (e.Column.MappingName == "Email")
    {
        var email = e.NewValue?.ToString();
        if (!IsValidEmail(email))
        {
            e.Cancel = true;
            ShowErrorMessage("Please enter a valid email address");
        }
    }
    
    if (e.Column.MappingName == "Salary")
    {
        var employee = e.DataRow.RowData as Employee;
        var salary = Convert.ToDouble(e.NewValue);
        
        // Business rule: Manager salary must be > 50000
        if (employee.Title == "Manager" && salary <= 50000)
        {
            e.Cancel = true;
            ShowErrorMessage("Manager salary must exceed $50,000");
        }
    }
};

private bool IsValidEmail(string email)
{
    try
    {
        var addr = new System.Net.Mail.MailAddress(email);
        return addr.Address == email;
    }
    catch
    {
        return false;
    }
}
```

### Cell Validation

Implement `IDataErrorInfo` or `INotifyDataErrorInfo` in your model:

```csharp
using System.ComponentModel;

public class Employee : IDataErrorInfo
{
    public string FirstName { get; set; }
    public double Salary { get; set; }
    
    public string Error => null;
    
    public string this[string columnName]
    {
        get
        {
            string result = null;
            
            if (columnName == "FirstName")
            {
                if (string.IsNullOrWhiteSpace(FirstName))
                    result = "First name is required";
            }
            else if (columnName == "Salary")
            {
                if (Salary < 0)
                    result = "Salary cannot be negative";
            }
            
            return result;
        }
    }
}
```

## Editor Types

Each column type has its own editor:

### TreeGridTextColumn - TextBox

Default text editor:

```xaml
<treeGrid:TreeGridTextColumn MappingName="FirstName" 
                            MaxLength="50" />
```

### TreeGridNumericColumn - SfNumberBox

Numeric editor with constraints:

```xaml
<treeGrid:TreeGridNumericColumn MappingName="Salary"
                               MinValue="0"
                               MaxValue="10000000"
                               Step="1000"
                               NumberDecimalDigits="2" />
```

### TreeGridDateColumn - SfCalendarDatePicker

Date picker editor:

```xaml
<treeGrid:TreeGridDateColumn MappingName="HireDate"
                            MinDate="2000-01-01"
                            MaxDate="2030-12-31" />
```

### TreeGridTimeColumn - SfTimePicker

Time picker editor:

```xaml
<treeGrid:TreeGridTimeColumn MappingName="ShiftStartTime"
                            ClockIdentifier="12HourClock" />
```

### TreeGridCheckBoxColumn - CheckBox

Boolean editor (toggles on click):

```xaml
<treeGrid:TreeGridCheckBoxColumn MappingName="IsActive" />
```

### TreeGridComboBoxColumn - ComboBox

Dropdown selection:

```xaml
<treeGrid:TreeGridComboBoxColumn MappingName="Department"
                                ItemsSource="{Binding Departments}"
                                DisplayMemberPath="Name"
                                SelectedValuePath="ID"
                                IsEditable="True" />
```

### TreeGridTemplateColumn - Custom Editor

Provide custom edit template:

```xaml
<treeGrid:TreeGridTemplateColumn MappingName="Priority">
    <treeGrid:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Priority}" />
        </DataTemplate>
    </treeGrid:TreeGridTemplateColumn.CellTemplate>
    <treeGrid:TreeGridTemplateColumn.EditTemplate>
        <DataTemplate>
            <ComboBox ItemsSource="{Binding PriorityLevels}"
                     SelectedItem="{Binding Priority, Mode=TwoWay}" />
        </DataTemplate>
    </treeGrid:TreeGridTemplateColumn.EditTemplate>
</treeGrid:TreeGridTemplateColumn>
```

## Common Patterns

### Read-Only Rows

Make certain rows read-only based on data:

```csharp
sfTreeGrid.CurrentCellBeginEdit += (sender, e) =>
{
    var employee = e.DataRow.RowData as Employee;
    if (employee.Status == "Archived")
    {
        e.Cancel = true;
    }
};
```

### Conditional Column Editing

Allow editing only when conditions are met:

```csharp
sfTreeGrid.CurrentCellBeginEdit += (sender, e) =>
{
    var employee = e.DataRow.RowData as Employee;
    
    // Only managers can edit salary
    if (e.Column.MappingName == "Salary" && !CurrentUser.IsManager)
    {
        e.Cancel = true;
        ShowMessage("Only managers can edit salary");
    }
};
```

### Auto-Save on Edit

Automatically save changes to database:

```csharp
sfTreeGrid.CurrentCellEndEdit += async (sender, e) =>
{
    if (!e.Cancel)
    {
        var employee = e.DataRow.RowData as Employee;
        await SaveToDatabase(employee);
    }
};
```

### Track Changes

Monitor which cells have been edited:

```csharp
private HashSet<string> modifiedRows = new HashSet<string>();

sfTreeGrid.CurrentCellValueChanged += (sender, e) =>
{
    var employee = e.DataRow.RowData as Employee;
    modifiedRows.Add(employee.ID.ToString());
    
    // Mark row as modified (visual indicator)
    employee.IsModified = true;
};
```

## Keyboard Navigation

Default keyboard shortcuts for editing:

| Key | Action |
|-----|--------|
| **F2** | Begin edit current cell |
| **Enter** | Commit edit and move down |
| **Tab** | Commit edit and move right |
| **Shift+Tab** | Commit edit and move left |
| **Escape** | Cancel edit |

## Troubleshooting

**Editing not working:**
- Ensure `AllowEditing = True` on TreeGrid
- Check column's `AllowEditing` property
- Verify data model properties have public setters
- Check if `CurrentCellBeginEdit` event cancels editing

**Changes not persisting:**
- Implement `INotifyPropertyChanged` in data model
- Ensure two-way binding in templates
- Check `CurrentCellEndEdit` doesn't cancel

**Validation not showing:**
- Implement `IDataErrorInfo` or use data annotations
- Check validation logic in `CurrentCellEndEdit` event
- Ensure error messages are displayed

**Wrong editor appearing:**
- Verify column type matches data type
- Check if custom edit template is correctly defined
- Use `TreeGridTemplateColumn` for custom editors
