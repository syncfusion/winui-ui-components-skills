# Sorting and Filtering in WinUI TreeGrid

## Table of Contents
- [Sorting](#sorting)
- [Filtering](#filtering)

## Sorting

Enable users to sort data by clicking column headers.

### Enable Sorting

```xaml
<treeGrid:SfTreeGrid AllowSorting="True" 
                    ItemsSource="{Binding Employees}" />
```

```csharp
sfTreeGrid.AllowSorting = true;
```

**Default behavior:** Click header to sort ascending, click again for descending, click third time to remove sort.

### Disable Sorting for Specific Column

```xaml
<treeGrid:TreeGridTextColumn MappingName="Notes" 
                            AllowSorting="False" />
```

### Sort Column Descriptions

Define initial sorting programmatically:

```csharp
sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "FirstName",
    SortDirection = ListSortDirection.Ascending
});
```

```xaml
<treeGrid:SfTreeGrid>
    <treeGrid:SfTreeGrid.SortColumnDescriptions>
        <grid:SortColumnDescription ColumnName="LastName" 
                                   SortDirection="Ascending" />
    </treeGrid:SfTreeGrid.SortColumnDescriptions>
</treeGrid:SfTreeGrid>
```

### Multi-Column Sorting

Sort by multiple columns (hold Ctrl while clicking headers):

```csharp
sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "Department",
    SortDirection = ListSortDirection.Ascending
});
sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "Salary",
    SortDirection = ListSortDirection.Descending
});
```

### Sort Click Action

Control sorting behavior:

```csharp
sfTreeGrid.SortClickAction = SortClickAction.DoubleClick;
```

| Action | Description |
|--------|-------------|
| **SingleClick** (default) | Sort on single click |
| **DoubleClick** | Sort on double click |

### Sorting Events

**SortColumnsChanging** - Before sort applied:

```csharp
sfTreeGrid.SortColumnsChanging += (sender, e) =>
{
    // Cancel sorting for specific column
    if (e.AddedItems[0].ColumnName == "ID")
    {
        e.Cancel = true;
        ShowMessage("Cannot sort by ID");
    }
};
```

**SortColumnsChanged** - After sort applied:

```csharp
sfTreeGrid.SortColumnsChanged += (sender, e) =>
{
    var sortInfo = sfTreeGrid.SortColumnDescriptions;
    StatusLabel.Text = $"Sorted by {sortInfo.Count} column(s)";
};
```

### Custom Sorting

Implement custom sort logic using `IComparer`:

```csharp
public class CustomSalaryComparer : IComparer<object>
{
    public int Compare(object x, object y)
    {
        var emp1 = x as Employee;
        var emp2 = y as Employee;
        
        // Custom logic: Sort by salary, but managers always first
        if (emp1.Title == "Manager" && emp2.Title != "Manager")
            return -1;
        if (emp1.Title != "Manager" && emp2.Title == "Manager")
            return 1;
            
        return emp1.Salary.CompareTo(emp2.Salary);
    }
}

// Apply custom comparer
var column = sfTreeGrid.Columns["Salary"];
column.Comparer = new CustomSalaryComparer();
```

### Clear Sorting

```csharp
sfTreeGrid.SortColumnDescriptions.Clear();
```

### Tri-State Sorting

Enable/disable the ability to clear sort by clicking header third time:

```csharp
sfTreeGrid.AllowTriStateSorting = true;  // Default
```

## Filtering

Enable Excel-style filtering UI.

### Enable Filtering

```xaml
<treeGrid:SfTreeGrid AllowFiltering="True" 
                    ItemsSource="{Binding Employees}" />
```

```csharp
sfTreeGrid.AllowFiltering = true;
```

**Result:** Filter icon appears in column headers. Click to open filter UI.

### Disable Filtering for Specific Column

```xaml
<treeGrid:TreeGridTextColumn MappingName="InternalNotes" 
                            AllowFiltering="False" />
```

### Filter Row

Show a dedicated filter row below headers:

```csharp
sfTreeGrid.FilterRowPosition = FilterRowPosition.FixedTop;
```

| Position | Description |
|----------|-------------|
| **None** (default) | No filter row |
| **FixedTop** | Filter row below headers |
| **Bottom** | Filter row at bottom |

### Filter Predicates

Define filters programmatically:

```csharp
// Add text filter
sfTreeGrid.FilterPredicates.Add(new FilterPredicate
{
    ColumnName = "FirstName",
    FilterType = FilterType.Contains,
    FilterValue = "John"
});

// Add numeric filter
sfTreeGrid.FilterPredicates.Add(new FilterPredicate
{
    ColumnName = "Salary",
    FilterType = FilterType.GreaterThan,
    FilterValue = 50000
});
```

### Filter Types

| FilterType | Description | Example |
|------------|-------------|---------|
| **Equals** | Exact match | FirstName = "John" |
| **NotEquals** | Not equal | Status != "Inactive" |
| **Contains** | Contains text | FirstName contains "son" |
| **StartsWith** | Begins with | FirstName starts with "J" |
| **EndsWith** | Ends with | Email ends with ".com" |
| **GreaterThan** | Numeric > | Salary > 50000 |
| **GreaterThanOrEqual** | Numeric >= | Age >= 18 |
| **LessThan** | Numeric < | Age < 65 |
| **LessThanOrEqual** | Numeric <= | Salary <= 100000 |

### Filter Behavior

**FilterBehavior** property controls how tree filters:

```csharp
sfTreeGrid.FilterBehavior = FilterBehavior.StronglyTyped;
```

| Behavior | Description |
|----------|-------------|
| **StronglyTyped** (default) | Filters entire row including parent/child |
| **StringTyped** | String-based filtering |

### Filtering Events

**FilterChanging** - Before filter applied:

```csharp
sfTreeGrid.FilterChanging += (sender, e) =>
{
    // Cancel filtering for specific column
    if (e.FilterPredicates[0].ColumnName == "ID")
    {
        e.Cancel = true;
        ShowMessage("Cannot filter by ID");
    }
    
    // Log filter action
    var filter = e.FilterPredicates[0];
    Log($"Filtering {filter.ColumnName} by {filter.FilterType}");
};
```

**FilterChanged** - After filter applied:

```csharp
sfTreeGrid.FilterChanged += (sender, e) =>
{
    var visibleCount = sfTreeGrid.View.Nodes.Count;
    StatusLabel.Text = $"{visibleCount} rows visible";
};
```

### Advanced Filter Options

**Filter Mode:**

```csharp
sfTreeGrid.FilterMode = FilterMode.Advanced;
// or
sfTreeGrid.FilterMode = FilterMode.Simple;
```

**Filter Delay:**

```csharp
// Set delay before filter applies (in ms)
sfTreeGrid.FilterDelay = 500;
```

### Multiple Filter Conditions

Combine multiple filters with AND/OR:

```csharp
var filterPredicates = new FilterPredicates();

// Add first condition
filterPredicates.Add(new FilterPredicate
{
    ColumnName = "Department",
    FilterType = FilterType.Equals,
    FilterValue = "Sales"
});

// Add second condition with AND logic
filterPredicates.Add(new FilterPredicate
{
    ColumnName = "Salary",
    FilterType = FilterType.GreaterThan,
    FilterValue = 60000,
    FilterOperator = FilterOperator.And
});

sfTreeGrid.FilterPredicates = filterPredicates;
```

### Clear Filters

```csharp
// Clear all filters
sfTreeGrid.ClearFilters();

// Or clear specific column filter
sfTreeGrid.FilterPredicates.Remove(
    sfTreeGrid.FilterPredicates.First(f => f.ColumnName == "Department")
);
```

### Custom Filter UI

Customize filter popup using events:

```csharp
sfTreeGrid.FilterItemsPopulating += (sender, e) =>
{
    // Customize filter items
    if (e.Column.MappingName == "Status")
    {
        // Show only specific filter options
        e.FilterItems = new List<string> { "Active", "Inactive", "Pending" };
    }
};
```

## Common Patterns

### Default Sort on Load

```csharp
private void LoadData()
{
    sfTreeGrid.ItemsSource = viewModel.Employees;
    
    // Apply default sort
    sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
    {
        ColumnName = "LastName",
        SortDirection = ListSortDirection.Ascending
    });
}
```

### Sort + Filter Combination

```csharp
// Filter for managers
sfTreeGrid.FilterPredicates.Add(new FilterPredicate
{
    ColumnName = "Title",
    FilterType = FilterType.Contains,
    FilterValue = "Manager"
});

// Then sort by salary
sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "Salary",
    SortDirection = ListSortDirection.Descending
});
```

### Save/Restore Filter State

```csharp
private List<FilterPredicate> savedFilters;

private void SaveFilters()
{
    savedFilters = new List<FilterPredicate>(sfTreeGrid.FilterPredicates);
}

private void RestoreFilters()
{
    sfTreeGrid.FilterPredicates.Clear();
    foreach (var filter in savedFilters)
    {
        sfTreeGrid.FilterPredicates.Add(filter);
    }
}
```

### Quick Filter TextBox

```xaml
<TextBox x:Name="FilterTextBox" 
         PlaceholderText="Search..."
         TextChanged="FilterTextBox_TextChanged" />
         
<treeGrid:SfTreeGrid x:Name="sfTreeGrid" />
```

```csharp
private void FilterTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    var searchText = FilterTextBox.Text;
    
    sfTreeGrid.FilterPredicates.Clear();
    
    if (!string.IsNullOrEmpty(searchText))
    {
        sfTreeGrid.FilterPredicates.Add(new FilterPredicate
        {
            ColumnName = "FirstName",
            FilterType = FilterType.Contains,
            FilterValue = searchText,
            FilterOperator = FilterOperator.Or
        });
        sfTreeGrid.FilterPredicates.Add(new FilterPredicate
        {
            ColumnName = "LastName",
            FilterType = FilterType.Contains,
            FilterValue = searchText,
            FilterOperator = FilterOperator.Or
        });
    }
}
```

### Batch Operations with Defer Refresh

```csharp
using (sfTreeGrid.View.DeferRefresh(TreeViewRefreshMode.NodeRefresh))
{
    // Add multiple sorts and filters
    sfTreeGrid.SortColumnDescriptions.Add(new SortColumnDescription
    {
        ColumnName = "Department",
        SortDirection = ListSortDirection.Ascending
    });
    
    sfTreeGrid.FilterPredicates.Add(new FilterPredicate
    {
        ColumnName = "Status",
        FilterType = FilterType.Equals,
        FilterValue = "Active"
    });
}
// Refresh happens once after using block
```

## Troubleshooting

**Sorting not working:**
- Ensure `AllowSorting = True`
- Check column's `AllowSorting` property
- Verify data property is comparable

**Filter UI not appearing:**
- Set `AllowFiltering = True`
- Check column's `AllowFiltering` property
- Ensure column header is visible

**Filters not applying:**
- Check `FilterBehavior` setting
- Verify `FilterPredicates` are added correctly
- Ensure filter values match data types

**Performance issues with sorting/filtering:**
- Use `DeferRefresh` for batch operations
- Implement virtualization for large datasets
- Consider on-demand loading (see node-features.md)
