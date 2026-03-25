# Styling and Customization in WinUI TreeGrid

## Table of Contents
- [Conditional Styling](#conditional-styling)
- [GridLines Customization](#gridlines-customization)
- [UI Customization](#ui-customization)
- [Theme Integration](#theme-integration)

## Conditional Styling

Apply styles dynamically based on data values.

### QueryCellStyle Event

Customize individual cell appearance:

```csharp
sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    var node = sfTreeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    if (node != null)
    {
        var employee = node.Item as Employee;
        
        // Highlight high salaries
        if (e.Column.MappingName == "Salary")
        {
            var salary = employee.Salary;
            if (salary > 100000)
            {
                e.Style.Background = new SolidColorBrush(Colors.LightGreen);
                e.Style.Foreground = new SolidColorBrush(Colors.DarkGreen);
            }
        }
        
        // Highlight inactive status
        if (e.Column.MappingName == "Status" && employee.Status == "Inactive")
        {
            e.Style.Background = new SolidColorBrush(Colors.LightGray);
        }
        
        e.Handled = true;
    }
};
```

### QueryRowStyle Event

Customize entire row appearance:

```csharp
sfTreeGrid.QueryRowStyle += (sender, e) =>
{
    var node = sfTreeGrid.GetNodeAtRowIndex(e.RowIndex);
    if (node != null)
    {
        var employee = node.Item as Employee;
        
        // Highlight managers
        if (employee.Title.Contains("Manager"))
        {
            e.Style.Background = new SolidColorBrush(Colors.LightBlue);
            e.Style.FontWeight = FontWeights.Bold;
        }
        
        // Alternate row colors
        if (e.RowIndex % 2 == 0)
        {
            e.Style.Background = new SolidColorBrush(Colors.White);
        }
        else
        {
            e.Style.Background = new SolidColorBrush(Colors.WhiteSmoke);
        }
        
        e.Handled = true;
    }
};
```

### Style Properties Available

```csharp
e.Style.Background = new SolidColorBrush(Colors.Yellow);
e.Style.Foreground = new SolidColorBrush(Colors.Black);
e.Style.FontSize = 14;
e.Style.FontWeight = FontWeights.Bold;
e.Style.FontStyle = FontStyle.Italic;
e.Style.TextAlignment = TextAlignment.Right;
e.Style.BorderBrush = new SolidColorBrush(Colors.Red);
e.Style.BorderThickness = new Thickness(2);
```

### Priority-Based Cell Styling

```csharp
sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    if (e.Column.MappingName == "Priority")
    {
        var node = sfTreeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
        var task = node?.Item as TaskItem;
        
        if (task != null)
        {
            switch (task.Priority)
            {
                case "High":
                    e.Style.Background = new SolidColorBrush(Colors.Red);
                    e.Style.Foreground = new SolidColorBrush(Colors.White);
                    e.Style.FontWeight = FontWeights.Bold;
                    break;
                case "Medium":
                    e.Style.Background = new SolidColorBrush(Colors.Yellow);
                    break;
                case "Low":
                    e.Style.Background = new SolidColorBrush(Colors.LightGreen);
                    break;
            }
            
            e.Handled = true;
        }
    }
};
```

### Date-Based Styling

```csharp
sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    if (e.Column.MappingName == "DueDate")
    {
        var node = sfTreeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
        var task = node?.Item as TaskItem;
        
        if (task != null && task.DueDate.HasValue)
        {
            var daysRemaining = (task.DueDate.Value - DateTime.Now).Days;
            
            // Overdue
            if (daysRemaining < 0)
            {
                e.Style.Background = new SolidColorBrush(Colors.Red);
                e.Style.Foreground = new SolidColorBrush(Colors.White);
            }
            // Due soon (within 3 days)
            else if (daysRemaining <= 3)
            {
                e.Style.Background = new SolidColorBrush(Colors.Orange);
            }
            
            e.Handled = true;
        }
    }
};
```

## GridLines Customization

Control the visibility and appearance of grid lines.

### GridLinesVisibility Property

```csharp
sfTreeGrid.GridLinesVisibility = GridLinesVisibility.Both;
```

| Option | Description |
|--------|-------------|
| **None** | No grid lines |
| **Horizontal** | Only horizontal lines |
| **Vertical** | Only vertical lines |
| **Both** (default) | Both horizontal and vertical |

```xaml
<!-- No grid lines -->
<treeGrid:SfTreeGrid GridLinesVisibility="None" />

<!-- Only horizontal lines -->
<treeGrid:SfTreeGrid GridLinesVisibility="Horizontal" />

<!-- Only vertical lines -->
<treeGrid:SfTreeGrid GridLinesVisibility="Vertical" />

<!-- Both (default) -->
<treeGrid:SfTreeGrid GridLinesVisibility="Both" />
```

### Header Lines Visibility

```csharp
sfTreeGrid.HeaderLinesVisibility = GridLinesVisibility.Both;
```

### Customize Grid Line Color

```xaml
<treeGrid:SfTreeGrid>
    <treeGrid:SfTreeGrid.Resources>
        <SolidColorBrush x:Key="SyncfusionTreeGridGridLineBrush" 
                        Color="LightGray" />
    </treeGrid:SfTreeGrid.Resources>
</treeGrid:SfTreeGrid>
```

### Grid Line Thickness

```xaml
<treeGrid:SfTreeGrid GridLineStrokeThickness="2" />
```

## UI Customization

### Row Header

Show/hide row headers:

```xaml
<treeGrid:SfTreeGrid ShowRowHeader="True" 
                    RowHeaderWidth="30" />
```

```csharp
sfTreeGrid.ShowRowHeader = true;
sfTreeGrid.RowHeaderWidth = 30;
```

### Indent Column

Customize the tree structure indent column:

```csharp
sfTreeGrid.IndentColumnWidth = 30;  // Width of indent per level
sfTreeGrid.ExpanderColumnWidth = 24;  // Width of expander icon
```

### Customize Expander Icon

```xaml
<treeGrid:SfTreeGrid>
    <treeGrid:SfTreeGrid.Resources>
        <!-- Expanded icon -->
        <DataTemplate x:Key="ExpandedTemplate">
            <Path Data="M0,0 L4,4 L8,0 Z" 
                  Fill="Black" />
        </DataTemplate>
        
        <!-- Collapsed icon -->
        <DataTemplate x:Key="CollapsedTemplate">
            <Path Data="M0,0 L4,4 L0,8 Z" 
                  Fill="Black" />
        </DataTemplate>
    </treeGrid:SfTreeGrid.Resources>
</treeGrid:SfTreeGrid>
```

### Cell Padding

```xaml
<treeGrid:TreeGridTextColumn MappingName="FirstName">
    <treeGrid:TreeGridTextColumn.CellStyle>
        <Style TargetType="treeGrid:TreeGridCell">
            <Setter Property="Padding" Value="10,5" />
        </Style>
    </treeGrid:TreeGridTextColumn.CellStyle>
</treeGrid:TreeGridTextColumn>
```

### Selection Background

```xaml
<treeGrid:SfTreeGrid>
    <treeGrid:SfTreeGrid.Resources>
        <SolidColorBrush x:Key="SyncfusionTreeGridSelectionBackgroundBrush" 
                        Color="#FF4A90E2" />
        <SolidColorBrush x:Key="SyncfusionTreeGridSelectionForegroundBrush" 
                        Color="White" />
    </treeGrid:SfTreeGrid.Resources>
</treeGrid:SfTreeGrid>
```

### Current Cell Border

```xaml
<treeGrid:SfTreeGrid>
    <treeGrid:SfTreeGrid.Resources>
        <SolidColorBrush x:Key="SyncfusionTreeGridCurrentCellBorderBrush" 
                        Color="Blue" />
    </treeGrid:SfTreeGrid.Resources>
</treeGrid:SfTreeGrid>
```

### Custom Cell Template

Apply custom templates to entire columns:

```xaml
<treeGrid:TreeGridTemplateColumn MappingName="Status" 
                                HeaderText="Status">
    <treeGrid:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <Border Background="{Binding Status, Converter={StaticResource StatusToBrushConverter}}"
                    CornerRadius="12"
                    Padding="8,4"
                    HorizontalAlignment="Center">
                <TextBlock Text="{Binding Status}" 
                          Foreground="White"
                          FontSize="12" />
            </Border>
        </DataTemplate>
    </treeGrid:TreeGridTemplateColumn.CellTemplate>
</treeGrid:TreeGridTemplateColumn>
```

### Header Template Customization

```xaml
<treeGrid:TreeGridTextColumn MappingName="Salary" 
                            HeaderText="Annual Salary">
    <treeGrid:TreeGridTextColumn.HeaderTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Money" Margin="0,0,5,0" />
                <TextBlock Text="Annual Salary" 
                          FontWeight="Bold" 
                          VerticalAlignment="Center" />
            </StackPanel>
        </DataTemplate>
    </treeGrid:TreeGridTextColumn.HeaderTemplate>
</treeGrid:TreeGridTextColumn>
```

## Theme Integration

### Light and Dark Theme Support

TreeGrid automatically adapts to Windows theme:

```csharp
// TreeGrid respects system theme by default
// No additional code needed for basic theme support
```

### Custom Theme Resources

Override theme brushes:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Light Theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="TreeGridBackground" Color="White" />
                <SolidColorBrush x:Key="TreeGridForeground" Color="Black" />
                <SolidColorBrush x:Key="TreeGridHeaderBackground" Color="#F0F0F0" />
            </ResourceDictionary>
            
            <!-- Dark Theme -->
            <ResourceDictionary x:Key="Dark">
                <SolidColorBrush x:Key="TreeGridBackground" Color="#1E1E1E" />
                <SolidColorBrush x:Key="TreeGridForeground" Color="White" />
                <SolidColorBrush x:Key="TreeGridHeaderBackground" Color="#2D2D2D" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Syncfusion Theme Studio

Use Syncfusion Theme Studio for advanced customization:

1. Download Theme Studio from Syncfusion website
2. Customize colors and styles visually
3. Export generated XAML resource dictionary
4. Include in your application

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Themes/CustomTheme.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Common Patterns

### Zebra Striping (Alternating Rows)

```csharp
sfTreeGrid.QueryRowStyle += (sender, e) =>
{
    if (e.RowIndex == 0) return;  // Skip header
    
    if (e.RowIndex % 2 == 0)
    {
        e.Style.Background = new SolidColorBrush(Colors.White);
    }
    else
    {
        e.Style.Background = new SolidColorBrush(Color.FromArgb(255, 245, 245, 245));
    }
    
    e.Handled = true;
};
```

### Highlight Search Results

```csharp
private string _searchText = "";

sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    if (!string.IsNullOrEmpty(_searchText))
    {
        var cellValue = e.DisplayText?.ToString() ?? "";
        
        if (cellValue.Contains(_searchText, StringComparison.OrdinalIgnoreCase))
        {
            e.Style.Background = new SolidColorBrush(Colors.Yellow);
            e.Handled = true;
        }
    }
};

private void SearchTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    _searchText = SearchTextBox.Text;
    sfTreeGrid.InvalidateCells();  // Refresh styling
}
```

### Conditional Formatting with Multiple Conditions

```csharp
sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    var node = sfTreeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    if (node != null)
    {
        var employee = node.Item as Employee;
        
        // Multiple condition styling
        bool isManager = employee.Title.Contains("Manager");
        bool highSalary = employee.Salary > 100000;
        bool isActive = employee.Status == "Active";
        
        if (isManager && highSalary && isActive)
        {
            e.Style.Background = new SolidColorBrush(Colors.Gold);
            e.Style.FontWeight = FontWeights.Bold;
        }
        else if (!isActive)
        {
            e.Style.Foreground = new SolidColorBrush(Colors.Gray);
            e.Style.FontStyle = FontStyle.Italic;
        }
        
        e.Handled = true;
    }
};
```

### Heat Map Styling (Data Visualization)

```csharp
sfTreeGrid.QueryCellStyle += (sender, e) =>
{
    if (e.Column.MappingName == "Performance")
    {
        var node = sfTreeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
        var employee = node?.Item as Employee;
        
        if (employee != null)
        {
            var performance = employee.Performance;
            byte intensity = (byte)(255 * (performance / 100.0));
            
            // Red to Green gradient
            Color color = performance < 50 
                ? Color.FromArgb(255, 255, (byte)(intensity * 2), 0)
                : Color.FromArgb(255, (byte)(255 - (intensity - 128) * 2), 255, 0);
            
            e.Style.Background = new SolidColorBrush(color);
            e.Handled = true;
        }
    }
};
```

### Custom Borders for Grouping

```csharp
sfTreeGrid.QueryRowStyle += (sender, e) =>
{
    var node = sfTreeGrid.GetNodeAtRowIndex(e.RowIndex);
    if (node != null && node.Level == 0)
    {
        // Add thick border to root nodes
        e.Style.BorderThickness = new Thickness(0, 2, 0, 2);
        e.Style.BorderBrush = new SolidColorBrush(Colors.Navy);
        e.Handled = true;
    }
};
```

## Troubleshooting

**Styles not applying:**
- Set `e.Handled = true` in style events
- Check event is subscribed correctly
- Verify style properties are valid

**Grid lines not visible:**
- Check `GridLinesVisibility` property
- Verify grid line brush color contrasts with background
- Ensure `GridLineStrokeThickness` is > 0

**Theme not switching:**
- Verify theme resources are defined in `ThemeDictionaries`
- Check resource keys match Syncfusion theme keys
- Restart application after theme change

**Performance issues with styling:**
- Minimize complex logic in `QueryCellStyle`
- Cache brushes instead of creating new ones
- Use `InvalidateCells()` sparingly
