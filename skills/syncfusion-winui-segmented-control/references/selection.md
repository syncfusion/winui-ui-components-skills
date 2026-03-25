# Selection in WinUI Segmented Control

## Table of Contents
- [Overview](#overview)
- [SelectedIndex Property](#selectedindex-property)
- [SelectedItem Property](#selecteditem-property)
- [Selection Style Customization](#selection-style-customization)
- [Shadow Effects](#shadow-effects)
- [Selection Animations](#selection-animations)
- [Keyboard Navigation](#keyboard-navigation)
- [SelectionChanged Event](#selectionchanged-event)
- [Programmatic Selection](#programmatic-selection)
- [Best Practices](#best-practices)

## Overview

The Segmented Control provides comprehensive selection features including programmatic selection, custom styling, shadow effects, animations, and keyboard navigation. Selection is mutually exclusive—only one segment can be selected at a time.

## SelectedIndex Property

The `SelectedIndex` property controls which segment is currently selected based on its zero-based index.

### Basic Usage

```xaml
<syncfusion:SfSegmentedControl SelectedIndex="2">
    <x:String>Day</x:String>      <!-- Index 0 -->
    <x:String>Week</x:String>     <!-- Index 1 -->
    <x:String>Month</x:String>    <!-- Index 2 - Selected -->
    <x:String>Year</x:String>     <!-- Index 3 -->
</syncfusion:SfSegmentedControl>
```

### With Data Binding

```xaml
<syncfusion:SfSegmentedControl 
    ItemsSource="{Binding Periods}"
    DisplayMemberPath="Name"
    SelectedIndex="{Binding SelectedPeriodIndex, Mode=TwoWay}"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private int selectedPeriodIndex = 1;
    
    public int SelectedPeriodIndex
    {
        get { return selectedPeriodIndex; }
        set
        {
            selectedPeriodIndex = value;
            OnPropertyChanged(nameof(SelectedPeriodIndex));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

### Setting in Code

```csharp
// Set selection by index
segmentedControl.SelectedIndex = 2;

// Validate before setting
if (newIndex >= 0 && newIndex < segmentedControl.Items.Count)
{
    segmentedControl.SelectedIndex = newIndex;
}
```

## SelectedItem Property

Access the selected item object directly through the `SelectedItem` property.

### Reading Selected Item

```csharp
private void ProcessSelection()
{
    // For string collections
    string selectedPeriod = segmentedControl.SelectedItem as string;
    
    // For business objects
    var selectedModel = segmentedControl.SelectedItem as PeriodModel;
    if (selectedModel != null)
    {
        string name = selectedModel.Name;
        // Process the selected item
    }
}
```

### Binding SelectedItem

```xaml
<syncfusion:SfSegmentedControl 
    ItemsSource="{Binding Periods}"
    DisplayMemberPath="Name"
    SelectedItem="{Binding SelectedPeriod, Mode=TwoWay}"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private PeriodModel selectedPeriod;
    
    public ObservableCollection<PeriodModel> Periods { get; set; }
    
    public PeriodModel SelectedPeriod
    {
        get { return selectedPeriod; }
        set
        {
            selectedPeriod = value;
            OnPropertyChanged(nameof(SelectedPeriod));
            OnSelectionChanged();
        }
    }
    
    private void OnSelectionChanged()
    {
        // React to selection change
    }
}
```

## Selection Style Customization

Customize the appearance of the selected segment using the `SelectedSegmentStyle` property with `SelectedSegmentBorder` as the target type.

### Custom Background Color

```xaml
<Grid.Resources>
    <Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="customSelectionStyle">
        <Setter Property="Background" Value="#6C58EA"/>
    </Style>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    SelectedIndex="1"
    SelectedSegmentStyle="{StaticResource customSelectionStyle}">
    <x:String>Option 1</x:String>
    <x:String>Option 2</x:String>
    <x:String>Option 3</x:String>
</syncfusion:SfSegmentedControl>
```

### Multiple Properties

```xaml
<Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="advancedSelectionStyle">
    <Setter Property="Background" Value="#00B7C0"/>
    <Setter Property="CornerRadius" Value="8"/>
    <Setter Property="BorderThickness" Value="2"/>
    <Setter Property="BorderBrush" Value="#008B94"/>
</Style>
```

### Theme-Aware Selection Style

```xaml
<Grid.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="SelectionBackground" Color="#6C58EA"/>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Dark">
                <SolidColorBrush x:Key="SelectionBackground" Color="#9B8AFF"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
        
        <Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="themeAwareStyle">
            <Setter Property="Background" Value="{ThemeResource SelectionBackground}"/>
        </Style>
    </ResourceDictionary>
</Grid.Resources>
```

## Shadow Effects

Add depth to selected segments with shadow effects.

### Enabling Shadow

```xaml
<Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="shadowStyle">
    <Setter Property="Background" Value="#00B7C0"/>
    <Setter Property="HasShadow" Value="True"/>
    <Setter Property="ShadowColor" Value="#00B7C0"/>
    <Setter Property="CornerRadius" Value="4"/>
</Style>

<syncfusion:SfSegmentedControl 
    SelectedIndex="1"
    CornerRadius="4"
    SelectedSegmentStyle="{StaticResource shadowStyle}">
    <x:String>Small</x:String>
    <x:String>Medium</x:String>
    <x:String>Large</x:String>
</syncfusion:SfSegmentedControl>
```

### Custom Shadow Color

```xaml
<Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="purpleShadowStyle">
    <Setter Property="Background" Value="#9B59B6"/>
    <Setter Property="HasShadow" Value="True"/>
    <Setter Property="ShadowColor" Value="#8E44AD"/>
    <Setter Property="CornerRadius" Value="6"/>
</Style>
```

### Complete Shadow Example with Item Styling

```xaml
<syncfusion:SfSegmentedControl 
    Height="40"
    SelectedIndex="1"
    CornerRadius="4"
    BorderThickness="2"
    ItemBorderThickness="0"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Models}"
    SelectedSegmentStyle="{StaticResource shadowStyle}">
    
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="Margin" Value="3"/>
            <Setter Property="CornerRadius" Value="4"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
    
    <syncfusion:SfSegmentedControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary x:Key="Light">
                    <SolidColorBrush x:Key="SyncfusionSegmentedControlBackground" Color="#F2F2F2"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemBackground" Color="#F2F2F2"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemSelectedBackground" Color="#00B7C0"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemHoverBackground" Color="#5BDAE4"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemSelectedForeground" Color="White"/>
                </ResourceDictionary>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </syncfusion:SfSegmentedControl.Resources>
</syncfusion:SfSegmentedControl>
```

## Selection Animations

The Segmented Control supports slide animation for smooth selection transitions.

### SelectionAnimationType Enum

- **Slide** (default): Smooth sliding animation to selected segment
- **None**: Instant selection without animation

### Slide Animation (Default)

```xaml
<syncfusion:SfSegmentedControl 
    SelectedIndex="0"
    SelectionAnimationType="Slide">
    <x:String>Tab 1</x:String>
    <x:String>Tab 2</x:String>
    <x:String>Tab 3</x:String>
</syncfusion:SfSegmentedControl>
```

**When to use:** Default behavior provides smooth, modern transitions. Good for most scenarios.

### No Animation

```xaml
<syncfusion:SfSegmentedControl 
    SelectedIndex="0"
    SelectionAnimationType="None">
    <x:String>Tab 1</x:String>
    <x:String>Tab 2</x:String>
    <x:String>Tab 3</x:String>
</syncfusion:SfSegmentedControl>
```

**When to use:** Forms requiring instant feedback, rapid selection scenarios, or accessibility preferences.

### Conditional Animation

```csharp
// Enable/disable animation based on user preference
bool userPrefersReducedMotion = GetUserMotionPreference();
segmentedControl.SelectionAnimationType = userPrefersReducedMotion 
    ? SegmentSelectionAnimationType.None 
    : SegmentSelectionAnimationType.Slide;
```

## Keyboard Navigation

The Segmented Control provides full keyboard support for accessibility.

### Keyboard Behaviors

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus to the segmented control. Focus border appears on selected item (or first item if none selected). Pressing Tab again moves focus away. |
| **LeftArrow** | Moves focus to previous segment. Does not change selection—only navigates. If on first item, does nothing. |
| **RightArrow** | Moves focus to next segment. Does not change selection—only navigates. If on last item, does nothing. |
| **Enter** | Selects the currently focused segment. Changes SelectedIndex and triggers SelectionChanged event. |

### Focus Management Example

```xaml
<StackPanel>
    <TextBox Header="Name" Margin="0,0,0,10"/>
    
    <syncfusion:SfSegmentedControl 
        x:Name="prioritySelector"
        TabIndex="1"
        SelectedIndex="1">
        <x:String>Low</x:String>
        <x:String>Medium</x:String>
        <x:String>High</x:String>
    </syncfusion:SfSegmentedControl>
    
    <Button Content="Submit" Margin="0,10,0,0" TabIndex="2"/>
</StackPanel>
```

### Programmatic Focus

```csharp
// Set focus to segmented control
prioritySelector.Focus(FocusState.Programmatic);

// Or use keyboard focus
prioritySelector.Focus(FocusState.Keyboard);
```

## SelectionChanged Event

The `SelectionChanged` event fires when the selected segment changes, providing both old and new values.

### Event Signature

```csharp
public event EventHandler<SegmentSelectionChangedEventArgs> SelectionChanged;
```

### SegmentSelectionChangedEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `NewValue` | object | The newly selected item |
| `OldValue` | object | The previously selected item |

### Basic Event Handler

```xaml
<syncfusion:SfSegmentedControl 
    SelectionChanged="SegmentedControl_SelectionChanged"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Periods}"/>
```

```csharp
private void SegmentedControl_SelectionChanged(object sender, SegmentSelectionChangedEventArgs e)
{
    // For string items
    string newSelection = e.NewValue as string;
    string previousSelection = e.OldValue as string;
    
    Debug.WriteLine($"Changed from '{previousSelection}' to '{newSelection}'");
}
```

### With Business Objects

```csharp
private void SegmentedControl_SelectionChanged(object sender, SegmentSelectionChangedEventArgs e)
{
    var newItem = e.NewValue as PeriodModel;
    var oldItem = e.OldValue as PeriodModel;
    
    if (newItem != null)
    {
        LoadDataForPeriod(newItem.Name);
    }
    
    // Log the change
    if (oldItem != null && newItem != null)
    {
        Analytics.TrackEvent("PeriodChanged", new Dictionary<string, string>
        {
            ["From"] = oldItem.Name,
            ["To"] = newItem.Name
        });
    }
}
```

### Prevent Recursive Calls

```csharp
private bool isUpdatingSelection = false;

private void SegmentedControl_SelectionChanged(object sender, SegmentSelectionChangedEventArgs e)
{
    if (isUpdatingSelection) return;
    
    try
    {
        isUpdatingSelection = true;
        
        // Your selection logic here
        ProcessSelectionChange(e.NewValue);
    }
    finally
    {
        isUpdatingSelection = false;
    }
}
```

## Programmatic Selection

Change selection from code in various scenarios.

### Select by Index

```csharp
// Direct index assignment
segmentedControl.SelectedIndex = 2;

// With validation
private void SelectSegment(int index)
{
    if (index >= 0 && index < segmentedControl.Items.Count)
    {
        segmentedControl.SelectedIndex = index;
    }
}
```

### Select by Item

```csharp
// Find and select specific item
var targetPeriod = periods.FirstOrDefault(p => p.Name == "Month");
if (targetPeriod != null)
{
    segmentedControl.SelectedItem = targetPeriod;
}
```

### Cycle Through Options

```csharp
private void NextSegment()
{
    int currentIndex = segmentedControl.SelectedIndex;
    int nextIndex = (currentIndex + 1) % segmentedControl.Items.Count;
    segmentedControl.SelectedIndex = nextIndex;
}

private void PreviousSegment()
{
    int currentIndex = segmentedControl.SelectedIndex;
    int itemCount = segmentedControl.Items.Count;
    int previousIndex = (currentIndex - 1 + itemCount) % itemCount;
    segmentedControl.SelectedIndex = previousIndex;
}
```

### Conditional Selection

```csharp
private void UpdateSelectionBasedOnData(DataType dataType)
{
    switch (dataType)
    {
        case DataType.Daily:
            segmentedControl.SelectedIndex = 0;
            break;
        case DataType.Weekly:
            segmentedControl.SelectedIndex = 1;
            break;
        case DataType.Monthly:
            segmentedControl.SelectedIndex = 2;
            break;
        case DataType.Yearly:
            segmentedControl.SelectedIndex = 3;
            break;
    }
}
```

## Best Practices

### 1. Two-Way Binding for MVVM

```xaml
<syncfusion:SfSegmentedControl 
    ItemsSource="{Binding Periods}"
    SelectedIndex="{Binding SelectedIndex, Mode=TwoWay}"/>
```

**Why:** Keeps ViewModel synchronized with UI selection automatically.

### 2. Validate Indices

```csharp
private void SetSelection(int index)
{
    if (index >= 0 && index < segmentedControl.Items.Count)
    {
        segmentedControl.SelectedIndex = index;
    }
    else
    {
        Debug.WriteLine($"Invalid index: {index}");
    }
}
```

**Why:** Prevents ArgumentOutOfRangeException and unexpected behavior.

### 3. Handle Initial Selection

```xaml
<!-- Set reasonable default -->
<syncfusion:SfSegmentedControl SelectedIndex="0" ItemsSource="{Binding Items}"/>
```

**Why:** Ensures a clear initial state rather than no selection.

### 4. Use SelectionChanged for Side Effects

```csharp
private void SegmentedControl_SelectionChanged(object sender, SegmentSelectionChangedEventArgs e)
{
    // Trigger data refresh
    RefreshData();
    
    // Save preference
    SaveUserPreference(e.NewValue);
    
    // Update related UI
    UpdateChartType(e.NewValue);
}
```

**Why:** Centralized place for all selection-related logic.

### 5. Disable Animation for Forms

```xaml
<!-- For rapid selection in forms -->
<syncfusion:SfSegmentedControl SelectionAnimationType="None"/>
```

**Why:** Faster feedback for form inputs and accessibility.

### 6. Preserve Selection on Data Refresh

```csharp
private void RefreshData()
{
    int previousIndex = segmentedControl.SelectedIndex;
    
    // Reload ItemsSource
    segmentedControl.ItemsSource = GetUpdatedData();
    
    // Restore selection
    if (previousIndex >= 0 && previousIndex < segmentedControl.Items.Count)
    {
        segmentedControl.SelectedIndex = previousIndex;
    }
}
```

**Why:** Maintains user context when data updates.

### 7. Keyboard Accessibility

```xaml
<!-- Ensure proper tab order -->
<syncfusion:SfSegmentedControl TabIndex="1" IsTabStop="True"/>
```

**Why:** Keyboard users can navigate efficiently through your UI.

### 8. Visual Feedback

```xaml
<!-- Add shadow for better selected state visibility -->
<Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="visibleSelection">
    <Setter Property="HasShadow" Value="True"/>
    <Setter Property="Background" Value="{ThemeResource AccentFillColorDefaultBrush}"/>
</Style>
```

**Why:** Clear visual indication helps users understand current selection.
