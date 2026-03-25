# Customization in WinUI Rating

The WinUI Rating control offers extensive customization options to match your application's design and user experience requirements. This guide covers all customization features available.

## Table of Contents
- [Style Customization](#style-customization)
- [Item Size](#item-size)
- [Orientation](#orientation)
- [Read-Only Mode](#read-only-mode)
- [Clear Rating](#clear-rating)
- [Placeholder Value](#placeholder-value)
- [Combining Customizations](#combining-customizations)

## Style Customization

Customize the appearance of rated (filled) and unrated (empty) items using the `RatedItemStyle` and `UnratedItemStyle` properties.

### Basic Styling

**Properties:**
- `RatedItemStyle` - Style for selected/filled rating items
- `UnratedItemStyle` - Style for unselected/empty rating items

**XAML:**
```xml
<Page.Resources>
    <!-- Style for rated (filled) items -->
    <Style TargetType="Path" x:Key="ratedStyle">
        <Setter Property="Fill" Value="#FFD700"/>
        <Setter Property="Stroke" Value="#FFD700"/>
        <Setter Property="StrokeThickness" Value="1"/>
    </Style>
    
    <!-- Style for unrated (empty) items -->
    <Style TargetType="Path" x:Key="unratedStyle">
        <Setter Property="Fill" Value="LightGray"/>
        <Setter Property="Stroke" Value="LightGray"/>
        <Setter Property="StrokeThickness" Value="1"/>
    </Style>
</Page.Resources>

<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    RatedItemStyle="{StaticResource ratedStyle}"
    UnratedItemStyle="{StaticResource unratedStyle}"/>
```

**C#:**
```csharp
using Windows.UI;
using Microsoft.UI.Xaml.Media;
using Microsoft.UI.Xaml.Shapes;

// RatedItemStyle
Style ratedStyle = new Style(typeof(Path));

Setter fillSetter = new Setter();
fillSetter.Property = Path.FillProperty;
fillSetter.Value = new SolidColorBrush(Color.FromArgb(255, 255, 215, 0)); // Gold
ratedStyle.Setters.Add(fillSetter);

Setter strokeSetter = new Setter();
strokeSetter.Property = Path.StrokeProperty;
strokeSetter.Value = new SolidColorBrush(Color.FromArgb(255, 255, 215, 0));
ratedStyle.Setters.Add(strokeSetter);

Setter thicknessSetter = new Setter();
thicknessSetter.Property = Path.StrokeThicknessProperty;
thicknessSetter.Value = 1;
ratedStyle.Setters.Add(thicknessSetter);

// UnratedItemStyle
Style unratedStyle = new Style(typeof(Path));

Setter unratedFillSetter = new Setter();
unratedFillSetter.Property = Path.FillProperty;
unratedFillSetter.Value = new SolidColorBrush(Colors.LightGray);
unratedStyle.Setters.Add(unratedFillSetter);

Setter unratedStrokeSetter = new Setter();
unratedStrokeSetter.Property = Path.StrokeProperty;
unratedStrokeSetter.Value = new SolidColorBrush(Colors.LightGray);
unratedStyle.Setters.Add(unratedStrokeSetter);

Setter unratedThicknessSetter = new Setter();
unratedThicknessSetter.Property = Path.StrokeThicknessProperty;
unratedThicknessSetter.Value = 1;
unratedStyle.Setters.Add(unratedThicknessSetter);

// Apply styles
SfRating rating = new SfRating();
rating.Value = 3;
rating.ItemsCount = 5;
rating.RatedItemStyle = ratedStyle;
rating.UnratedItemStyle = unratedStyle;
```

### Custom Color Schemes

**Blue Theme:**
```xml
<Page.Resources>
    <Style TargetType="Path" x:Key="blueRatedStyle">
        <Setter Property="Fill" Value="#0078D4"/>
        <Setter Property="Stroke" Value="#0078D4"/>
    </Style>
    <Style TargetType="Path" x:Key="blueUnratedStyle">
        <Setter Property="Fill" Value="#E6E6E6"/>
        <Setter Property="Stroke" Value="#CCCCCC"/>
    </Style>
</Page.Resources>

<syncfusion:SfRating 
    Value="4"
    ItemsCount="5"
    RatedItemStyle="{StaticResource blueRatedStyle}"
    UnratedItemStyle="{StaticResource blueUnratedStyle}"/>
```

**Red Theme (for negative feedback):**
```xml
<Page.Resources>
    <Style TargetType="Path" x:Key="redRatedStyle">
        <Setter Property="Fill" Value="#E74C3C"/>
        <Setter Property="Stroke" Value="#C0392B"/>
    </Style>
    <Style TargetType="Path" x:Key="redUnratedStyle">
        <Setter Property="Fill" Value="Transparent"/>
        <Setter Property="Stroke" Value="#CCCCCC"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Page.Resources>

<syncfusion:SfRating 
    Value="2"
    ItemsCount="5"
    RatedItemStyle="{StaticResource redRatedStyle}"
    UnratedItemStyle="{StaticResource redUnratedStyle}"/>
```

**Theme-Adaptive Styling:**
```xml
<Page.Resources>
    <Style TargetType="Path" x:Key="accentRatedStyle">
        <Setter Property="Fill" Value="{ThemeResource SystemAccentColor}"/>
        <Setter Property="Stroke" Value="{ThemeResource SystemAccentColor}"/>
    </Style>
    <Style TargetType="Path" x:Key="subtleUnratedStyle">
        <Setter Property="Fill" Value="{ThemeResource CardBackgroundFillColorDefaultBrush}"/>
        <Setter Property="Stroke" Value="{ThemeResource ControlStrokeColorDefaultBrush}"/>
    </Style>
</Page.Resources>

<syncfusion:SfRating 
    Value="3"
    ItemsCount="5"
    RatedItemStyle="{StaticResource accentRatedStyle}"
    UnratedItemStyle="{StaticResource subtleUnratedStyle}"/>
```

## Item Size

Control the size of rating items using the `ItemSize` property.

**Property:** `ItemSize`  
**Type:** `double`  
**Default:** `24`

### Setting Item Size

**XAML:**
```xml
<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    ItemSize="50"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.ItemsCount = 5;
rating.ItemSize = 50;
```

### Common Size Guidelines

| Size | Use Case | Context |
|------|----------|---------|
| 16-20 | Small inline ratings | Lists, tables, compact views |
| 24-32 | Standard forms | Default, general purpose |
| 40-50 | Prominent ratings | Hero sections, main feedback |
| 60+ | Large displays | Kiosks, touch interfaces |

### Size Examples

**Small (16px) - In Lists:**
```xml
<ListView ItemsSource="{Binding Products}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                
                <TextBlock Grid.Column="0" Text="{Binding Name}"/>
                <syncfusion:SfRating 
                    Grid.Column="1"
                    Value="{Binding Rating}" 
                    ItemsCount="5"
                    ItemSize="16"
                    IsReadOnly="True"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

**Large (60px) - Feedback Form:**
```xml
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock 
        Text="How was your experience?" 
        FontSize="24" 
        FontWeight="Bold"
        HorizontalAlignment="Center"
        Margin="0,0,0,20"/>
    <syncfusion:SfRating 
        Value="0" 
        ItemsCount="5"
        ItemSize="60"/>
</StackPanel>
```

## Orientation

Change the layout direction using the `Orientation` property.

**Property:** `Orientation`  
**Type:** `Orientation` enum  
**Default:** `Horizontal`  
**Options:** `Horizontal`, `Vertical`

### Horizontal Orientation (Default)

**XAML:**
```xml
<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    Orientation="Horizontal"/>
```

### Vertical Orientation

**XAML:**
```xml
<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    Orientation="Vertical"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.ItemsCount = 5;
rating.Orientation = Orientation.Vertical;
```

### Use Cases for Vertical Orientation

**Sidebar Widget:**
```xml
<Border 
    BorderBrush="Gray" 
    BorderThickness="1" 
    Padding="15"
    Width="100">
    <StackPanel>
        <TextBlock Text="Rating" FontWeight="Bold" Margin="0,0,0,10"/>
        <syncfusion:SfRating 
            Value="4" 
            ItemsCount="5"
            Orientation="Vertical"
            ItemSize="30"
            HorizontalAlignment="Center"/>
    </StackPanel>
</Border>
```

## Read-Only Mode

Restrict user interaction and display ratings without allowing changes using the `IsReadOnly` property.

**Property:** `IsReadOnly`  
**Type:** `bool`  
**Default:** `false`

### Setting Read-Only Mode

**XAML:**
```xml
<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    IsReadOnly="True"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.ItemsCount = 5;
rating.IsReadOnly = true;
```

### When to Use Read-Only

**Display Average Ratings:**
```xml
<StackPanel Orientation="Horizontal" Spacing="10">
    <TextBlock Text="Average:" VerticalAlignment="Center"/>
    <syncfusion:SfRating 
        Value="4.3" 
        ItemsCount="5"
        Precision="Exact"
        IsReadOnly="True"
        ItemSize="20"/>
    <TextBlock 
        Text="(1,234 reviews)" 
        VerticalAlignment="Center"
        Foreground="Gray"/>
</StackPanel>
```

**Product Listing Cards:**
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <Image Grid.Row="0" Source="{Binding ProductImage}" Height="200"/>
    <TextBlock Grid.Row="1" Text="{Binding ProductName}" FontSize="16" Margin="0,10,0,5"/>
    <syncfusion:SfRating 
        Grid.Row="2"
        Value="{Binding Rating}" 
        ItemsCount="5"
        IsReadOnly="True"
        ItemSize="16"/>
</Grid>
```

**Dynamic Read-Only Based on Condition:**
```xml
<syncfusion:SfRating 
    Value="{Binding UserRating}"
    ItemsCount="5"
    IsReadOnly="{Binding HasUserAlreadyRated}"/>
```

## Clear Rating

Allow or prevent users from clearing their rating selection using the `IsClearEnabled` property.

**Property:** `IsClearEnabled`  
**Type:** `bool`  
**Default:** `true`  
**Note:** Only works with `Precision="Full"`

### Enabling Clear Functionality

**XAML:**
```xml
<syncfusion:SfRating 
    Value="3"
    ItemsCount="5" 
    IsClearEnabled="True"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.ItemsCount = 5;
rating.IsClearEnabled = true;
```

### Behavior

When `IsClearEnabled="True"`:
- User can click the first filled star to clear the entire rating
- Rating value returns to 0
- Useful for optional feedback

When `IsClearEnabled="False"`:
- User must choose a rating (cannot clear)
- Once set, rating can only be changed, not removed
- Useful for mandatory feedback

### Use Cases

**Optional Rating (Clear Enabled):**
```xml
<StackPanel>
    <TextBlock Text="Rate this item (optional):"/>
    <syncfusion:SfRating 
        x:Name="OptionalRating"
        Value="0"
        ItemsCount="5"
        IsClearEnabled="True"
        ValueChanged="OptionalRating_ValueChanged"/>
    <TextBlock 
        x:Name="OptionalRatingText"
        Text="Not rated"
        FontStyle="Italic"
        Margin="0,10,0,0"/>
</StackPanel>
```

```csharp
private void OptionalRating_ValueChanged(object sender, ValueChangedEventArgs e)
{
    if (e.NewValue == 0)
    {
        OptionalRatingText.Text = "Not rated";
    }
    else
    {
        OptionalRatingText.Text = $"Rating: {e.NewValue} stars";
    }
}
```

**Mandatory Rating (Clear Disabled):**
```xml
<StackPanel>
    <TextBlock Text="Rate this item (required):"/>
    <syncfusion:SfRating 
        Value="0"
        ItemsCount="5"
        IsClearEnabled="False"/>
    <TextBlock 
        Text="* Required field"
        Foreground="Red"
        FontSize="12"
        Margin="0,5,0,0"/>
</StackPanel>
```

## Placeholder Value

Display an initial value (like an average rating) before the user provides their own rating using the `PlaceholderValue` property.

**Property:** `PlaceholderValue`  
**Type:** `double`  
**Default:** `0`

### Setting Placeholder Value

**XAML:**
```xml
<syncfusion:SfRating 
    ItemsCount="5" 
    PlaceholderValue="3.5"
    Precision="Half"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
rating.PlaceholderValue = 3.5;
rating.Precision = RatingPrecision.Half;
```

### How It Works

- **Before user interaction:** Shows `PlaceholderValue` (typically average rating)
- **After user interaction:** Shows user's selected `Value`
- **Visual difference:** Placeholder appears lighter/different from actual rating

### Use Cases

**Show Average Before User Rates:**
```xml
<StackPanel Spacing="10">
    <TextBlock Text="Community average: 4.2 stars"/>
    <TextBlock Text="Your rating:"/>
    <syncfusion:SfRating 
        x:Name="UserRating"
        Value="0"
        PlaceholderValue="4.2"
        ItemsCount="5"
        Precision="Exact"
        ValueChanged="UserRating_ValueChanged"/>
</StackPanel>
```

```csharp
private void UserRating_ValueChanged(object sender, ValueChangedEventArgs e)
{
    if (e.NewValue > 0)
    {
        // User has provided their rating
        SaveUserRating(e.NewValue);
    }
}
```

**Product Page with Average:**
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <TextBlock 
        Grid.Row="0"
        Text="{Binding ProductName}" 
        FontSize="20" 
        FontWeight="Bold"/>
    
    <TextBlock 
        Grid.Row="1"
        Text="{Binding AverageRating, StringFormat='Average rating: {0:0.0} ({1} reviews)', TargetNullValue='No reviews yet'}"
        Margin="0,5,0,10"/>
    
    <StackPanel Grid.Row="2">
        <TextBlock Text="Rate this product:"/>
        <syncfusion:SfRating 
            Value="{Binding UserRating, Mode=TwoWay}"
            PlaceholderValue="{Binding AverageRating}"
            ItemsCount="5"
            Precision="Half"/>
    </StackPanel>
</Grid>
```

## Combining Customizations

Create sophisticated rating controls by combining multiple customization options.

### Premium Product Rating

```xml
<Page.Resources>
    <Style TargetType="Path" x:Key="goldRatedStyle">
        <Setter Property="Fill" Value="#FFD700"/>
        <Setter Property="Stroke" Value="#FFA500"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
    <Style TargetType="Path" x:Key="transparentUnratedStyle">
        <Setter Property="Fill" Value="Transparent"/>
        <Setter Property="Stroke" Value="#CCCCCC"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Page.Resources>

<StackPanel Spacing="15">
    <TextBlock 
        Text="Premium Product Rating" 
        FontSize="18" 
        FontWeight="Bold"/>
    
    <syncfusion:SfRating 
        Value="0"
        PlaceholderValue="4.5"
        ItemsCount="5"
        Precision="Half"
        ItemSize="45"
        RatedItemStyle="{StaticResource goldRatedStyle}"
        UnratedItemStyle="{StaticResource transparentUnratedStyle}"
        EnableToolTip="True"
        ToolTipFormat="0.0"
        IsClearEnabled="True"/>
</StackPanel>
```

### Compact List Item Rating

```xml
<syncfusion:SfRating 
    Value="{Binding Rating}"
    ItemsCount="5"
    Precision="Half"
    ItemSize="16"
    IsReadOnly="True"
    RatedItemStyle="{StaticResource compactRatedStyle}"
    UnratedItemStyle="{StaticResource compactUnratedStyle}"/>
```

### Large Touch-Friendly Rating

```xml
<syncfusion:SfRating 
    Value="0"
    ItemsCount="5"
    Precision="Full"
    ItemSize="70"
    IsClearEnabled="False"
    EnableToolTip="True"/>
```

## Best Practices

### Styling
- Keep rated items visually distinct from unrated items
- Use appropriate color contrast for accessibility
- Test styles in both light and dark themes
- Consider using theme resources for automatic adaptation

### Sizing
- Use consistent sizes across similar contexts
- Scale appropriately for touch vs mouse interfaces
- Ensure adequate spacing between items at all sizes

### Orientation
- Use horizontal for most cases (more familiar)
- Use vertical only when space is constrained horizontally
- Maintain consistent orientation within a view

### Read-Only
- Always set `IsReadOnly="True"` for display-only ratings
- Provide visual differentiation between editable and read-only
- Consider reduced opacity or size for read-only ratings

### Clear Functionality
- Enable clearing for optional ratings
- Disable clearing for required ratings
- Provide clear indication of rating requirement

### Placeholder Values
- Show community averages to guide expectations
- Update placeholder dynamically as averages change
- Ensure placeholder is visually distinct from user rating

## Summary

Key customization properties:
- **RatedItemStyle/UnratedItemStyle** - Visual appearance
- **ItemSize** - Control size (16-70px typical)
- **Orientation** - Layout direction
- **IsReadOnly** - Display vs input mode
- **IsClearEnabled** - Allow clearing ratings
- **PlaceholderValue** - Show initial/average value

Combine these properties to create rating controls perfectly suited to your application's needs.
